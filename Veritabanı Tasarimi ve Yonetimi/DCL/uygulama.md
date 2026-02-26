# MSSQL Yetkilendirme Labı: LOGIN / USER / GRANT – DENY – REVOKE (Northwind)

Bu labda SQL Server yetkilendirme mantığını **tamamen SQL komutlarıyla** (arayüzden tıklama yok) uygulayarak öğreneceksiniz.

## Hedefler
- **LOGIN** (server-level) ve **USER** (database-level) farkını görmek
- `GRANT / DENY / REVOKE` davranışını gerçek sorgularla test etmek
- Rol (`db_datareader`) + `DENY` çakışmasını gözlemlemek
- Yetkileri sistem tablolarından doğrulamak

> Veritabanı: **Northwind**  
> Not: Sizde tablo adları Türkçe ise (`Musteriler`, `Siparisler` vb.) aşağıdaki `Customers/Orders/Employees` adlarını uyarlayın.

---

## 0) Kendi adınızı seçin (gerçek verilerle)
Bu labda herkes **kendi ad-soyadıyla** bir login/user oluşturacak.

Aşağıdaki yerlerde:
- `AD_SOYAD_LOGIN` → kendi login adınız
- `AD_SOYAD_USER`  → kendi user adınız

Örnek:
- Login: `ali.kaya`
- User : `ali.kaya`

> Kurallar:
- Türkçe karakter kullanmayın (`ş,ğ,ı,ö,ç,ü` yerine `s,g,i,o,c,u`)
- Boşluk kullanmayın
- Nokta (`.`) veya alt çizgi (`_`) kullanabilirsiniz

Bu README’de örnek olarak **`ali.kaya`** kullanıyorum. Siz kendi adınızla değiştirin.

---

## 1) (Opsiyonel) Baştan temiz başlamak
Daha önce aynı isimlerle oluşturduysanız “zaten var” hatası almamak için çalıştırın.

```sql
-- Önce DB tarafı
USE Northwind;
DROP USER IF EXISTS [ali.kaya];
GO

-- Sonra server tarafı
USE master;
DROP LOGIN IF EXISTS [ali.kaya];
GO
```

---

## 2) LOGIN oluştur (Server-level)
```sql
USE master;
GO
CREATE LOGIN [ali.kaya]
WITH PASSWORD = 'P@ssw0rd!123', CHECK_POLICY = OFF;
GO
```

### Kontrol
```sql
SELECT name, type_desc
FROM sys.server_principals
WHERE name = 'ali.kaya';
GO
```

---

## 3) Northwind’de USER oluştur (Database-level)
```sql
USE Northwind;
GO
CREATE USER [ali.kaya] FOR LOGIN [ali.kaya];
GO

GRANT CONNECT TO [ali.kaya];
GO
```

### Kontrol
```sql
USE Northwind;
GO
SELECT name, type_desc
FROM sys.database_principals
WHERE name = 'ali.kaya';
GO
```

---

## 4) İki bağlantı penceresi açın (ÖNEMLİ)
Labın mantığı “anında etki”yi görmek:

- **Pencere A (Admin bağlantısı):** Yetkileri verip kaldıracağınız pencere  
- **Pencere B (Öğrenci bağlantısı):** Login olarak `ali.kaya` ile bağlanıp test yapacağınız pencere

Öğrenci bağlantısında:
- Login: `ali.kaya`
- Password: `P@ssw0rd!123`

> Yanlışlıkla her şeyi admin penceresinde test ederseniz hata görmezsiniz ve lab boşa gider.

---

## 5) Yetki yokken SELECT dene (hata beklenir)
Aşağıyı **öğrenci bağlantısında** çalıştırın:

```sql
USE Northwind;
GO
SELECT TOP 5 * FROM dbo.Customers;
GO
```

Beklenen: **permission denied** benzeri hata.

---

## 6) GRANT: Tek tabloya SELECT izni ver
Aşağıyı **admin bağlantısında** çalıştırın:

```sql
USE Northwind;
GO
GRANT SELECT ON dbo.Customers TO [ali.kaya];
GO
```

### Test (öğrenci bağlantısı)
```sql
USE Northwind;
GO
SELECT TOP 5 * FROM dbo.Customers;
GO
```

Beklenen: Çalışır.

---

## 7) Rol ile geniş okuma yetkisi ver (db_datareader)
Aşağıyı **admin bağlantısında** çalıştırın:

```sql
USE Northwind;
GO
ALTER ROLE db_datareader ADD MEMBER [ali.kaya];
GO
```

### Test (öğrenci bağlantısı)
```sql
USE Northwind;
GO
SELECT TOP 5 * FROM dbo.Orders;
GO
```

Beklenen: Çalışır.

---

## 8) DENY: Rol olsa bile engelle (DENY baskın mı?)
Aşağıyı **admin bağlantısında** çalıştırın:

```sql
USE Northwind;
GO
DENY SELECT ON dbo.Employees TO [ali.kaya];
GO
```

### Test (öğrenci bağlantısı)
```sql
USE Northwind;
GO
SELECT TOP 5 * FROM dbo.Employees;
GO
```

Beklenen: Hata.  
> Kritik mesaj: Rol SELECT verse bile **DENY** birçok durumda baskındır.

---

## 9) REVOKE: Yasağı kaldır (REVOKE izin mi verir?)
Aşağıyı **admin bağlantısında** çalıştırın:

```sql
USE Northwind;
GO
REVOKE SELECT ON dbo.Employees FROM [ali.kaya];
GO
```

### Test (öğrenci bağlantısı)
```sql
USE Northwind;
GO
SELECT TOP 5 * FROM dbo.Employees;
GO
```

Beklenen: Çalışır.  
> Çünkü `REVOKE` burada “izin vermedi”; sadece **DENY’yi kaldırdı**.  
> İzin rol üzerinden (db_datareader) geldi.

---

## 10) Rolü çıkar → GRANT’ı kaldır → farkı gör
### 10.1 Rolü çıkar
Admin bağlantısı:
```sql
USE Northwind;
GO
ALTER ROLE db_datareader DROP MEMBER [ali.kaya];
GO
```

Öğrenci bağlantısı (Customers hâlâ çalışabilir; çünkü daha önce GRANT vermiştik):
```sql
USE Northwind;
GO
SELECT TOP 5 * FROM dbo.Customers;
GO
```

### 10.2 GRANT’ı kaldır (REVOKE)
Admin bağlantısı:
```sql
USE Northwind;
GO
REVOKE SELECT ON dbo.Customers FROM [ali.kaya];
GO
```

Öğrenci bağlantısı (hata beklenir):
```sql
USE Northwind;
GO
SELECT TOP 5 * FROM dbo.Customers;
GO
```

---

## 11) “Kimde hangi yetki var?” (kanıt)
Admin bağlantısı:
```sql
USE Northwind;
GO
SELECT 
    dp.name AS principal_name,
    perm.permission_name,
    perm.state_desc,
    OBJECT_NAME(perm.major_id) AS object_name
FROM sys.database_permissions perm
JOIN sys.database_principals dp 
  ON perm.grantee_principal_id = dp.principal_id
WHERE dp.name = 'ali.kaya'
ORDER BY object_name, permission_name;
GO
```

Rol üyelikleri:
```sql
USE Northwind;
GO
SELECT r.name AS role_name, m.name AS member_name
FROM sys.database_role_members drm
JOIN sys.database_principals r 
  ON drm.role_principal_id = r.principal_id
JOIN sys.database_principals m 
  ON drm.member_principal_id = m.principal_id
WHERE m.name = 'ali.kaya';
GO
```

---

## 12) Teslim Soruları (kısa)
1) Login var ama User yoksa ne olur?  
2) `DENY` neden `db_datareader` rolünü “ezdi”?  
3) `REVOKE` burada tam olarak neyi kaldırdı? “izin mi verdi” yoksa başka bir şey mi yaptı?  
4) Rol ile yetkilendirmenin (tek tek GRANT’a göre) avantajı nedir?

---

## 13) (Opsiyonel) Temizlik
Ders bitince kaldırmak isterseniz:

```sql
USE Northwind;
DROP USER IF EXISTS [ali.kaya];
GO

USE master;
DROP LOGIN IF EXISTS [ali.kaya];
GO
```