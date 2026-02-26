# DCL (Data Control Language) — SQL Server’da Yetkilendirme (Kısa Konu Anlatımı)

Bu bölüm **DCL** komutlarını (özellikle `GRANT / DENY / REVOKE`) ve bunların SQL Server’daki **LOGIN/USER/ROLE** yapısıyla ilişkisini hızlıca oturtmak içindir.

---

## 1) DCL Nedir?
**DCL (Data Control Language)**, veritabanında **kim ne yapabilir?** sorusunu yöneten komut grubudur.

DCL ile:
- Kullanıcıya/role yetki verilir
- Yetki engellenir
- Daha önce verilen/engellenen yetki geri alınır

SQL Server’da DCL’nin temel komutları:
- `GRANT`
- `DENY`
- `REVOKE`

---

## 2) SQL Server’da kimlik yapısı: Login vs User

### LOGIN (Server-level)
- SQL Server instance’a giriş kimliği
- Sunucu seviyesinde tanımlanır (`master`)

### USER (Database-level)
- Bir veritabanı içindeki kimlik
- Northwind gibi her DB’de ayrı oluşturulur

✅ Kısa cümle:
> **Login sunucuya sokar, User veritabanında ne yapacağını belirler.**

**Önemli:**  
Login var, ama ilgili DB’de User yoksa → o DB’de işlem yapamazsın (kimlik eşleşmesi yoktur).

---

## 3) “Yetki” neye verilir? (Securable mantığı)
Yetkiler bir **hedef nesne** üzerine verilir. En yaygınları:

- **DATABASE**: Veritabanına bağlanma vb.
- **SCHEMA**: `dbo` şeması gibi toplu kontrol
- **OBJECT**: tablo, view, procedure, function

Örnek işlemler:
- Tablo/View: `SELECT, INSERT, UPDATE, DELETE`
- Proc/Function: `EXECUTE`

---

## 4) GRANT / DENY / REVOKE (DCL’nin kalbi)

### 4.1 GRANT — “İzin ver”
Bir kullanıcı/role için belirli bir işlemi **serbest bırakır**.

Örnek:
```sql
GRANT SELECT ON dbo.Customers TO [kullanici];
```

Anlamı:
> `[kullanici]` artık `dbo.Customers` tablosunda `SELECT` yapabilir.

---

### 4.2 DENY — “Yasak koy”
Belirli bir işlemi **özellikle engeller**.

Örnek:
```sql
DENY SELECT ON dbo.Employees TO [kullanici];
```

Anlamı:
> `[kullanici]` artık `dbo.Employees` tablosunda `SELECT` yapamaz.

⚠️ Kritik nokta (dersin en önemli kısmı):
- Kullanıcı role üzerinden yetki alıyor olsa bile, birçok senaryoda **DENY baskındır**.
- Yani “rol izin verdi” diye düşünürken DENY bunu durdurabilir.

---

### 4.3 REVOKE — “Kararı geri al (nötrle)”
`REVOKE`, daha önce verilmiş `GRANT` veya konulmuş `DENY` kararını **geri alır**.

Örnek:
```sql
REVOKE SELECT ON dbo.Employees FROM [kullanici];
```

Anlamı:
> Bu kullanıcı için `dbo.Employees` üzerindeki “SELECT ile ilgili özel karar” kaldırıldı.

✅ En net yorum:
- REVOKE = “Benim koyduğum özel hükmü kaldır.”
- Sonuçta kullanıcı izinli mi?
  - Eğer başka yerden (ör. rol) izin geliyorsa → izinli olur.
  - Başka yerden izin gelmiyorsa → yine yetkisiz kalır.

---

## 5) Rol (Role) ve DCL ilişkisi
Rol, yetkileri “paket” halinde yönetmek için vardır.

Örnek rol:
- `db_datareader` → veritabanındaki çoğu tabloda `SELECT`

Kullanıcıyı role eklemek:
```sql
ALTER ROLE db_datareader ADD MEMBER [kullanici];
```

Rolün faydası:
- Tek tek her tabloya GRANT vermek yerine
- Bir kez role ekleyip yönetmek

---

## 6) Öncelik mantığı (akılda kalacak kural)
Bir kullanıcı bir yetkiyi farklı yerlerden alabilir:
- Direkt kullanıcıya verilmiş GRANT
- Role üzerinden gelen yetki
- Üstten gelen (schema/database) yetkiler

Bu durumda genel pratik kural:
1) **DENY varsa**, çoğu senaryoda **engeller**
2) DENY yoksa ve bir yerden **GRANT geliyorsa**, izin vardır
3) `REVOKE` tek başına izin vermez; sadece “özel kararı” kaldırır

> Ders labında özellikle şu görülecek:
> - Rol SELECT verirken, DENY ile tek tabloyu kapatacağız
> - REVOKE ile DENY’yi kaldırınca rol tekrar etkili olacak

---

## 7) Mini örnek senaryo (mantık oturtma)
- Kullanıcı role girer (`db_datareader`) → birçok tabloda SELECT yapabilir
- Sonra `Employees` için `DENY SELECT` konur → Employees’ı okuyamaz
- Sonra `REVOKE SELECT` ile DENY kaldırılır → role sayesinde Employees tekrar okunur

Bu, DCL’nin gerçek dünyadaki en net kullanım örneklerinden biridir.

---

## 8) Labdan önce bilmeniz gereken 2 şey
1) DCL komutları genelde “yetki veren” (admin) hesapla çalıştırılır.  
2) Etkiyi görmek için en iyi yöntem: SSMS’te **iki bağlantı** açmak:
   - Admin bağlantısı (GRANT/DENY/REVOKE)
   - Öğrenci bağlantısı (test SELECT’ler)

---