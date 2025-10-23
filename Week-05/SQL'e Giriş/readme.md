# 📘 SQL’e Giriş

## 🎯 1. SQL Nedir?

**SQL (Structured Query Language)**, yani *Yapılandırılmış Sorgulama Dili*, ilişkisel veritabanlarıyla etkileşim kurmak için kullanılan standart bir dildir.  
SQL sayesinde verileri **tanımlar (DDL)**, **sorgular (DQL)** ve **işler (DML)** hale getirebiliriz.  

SQL ilk olarak **1970’li yıllarda IBM** tarafından geliştirilen “SEQUEL” isimli dilden türemiştir.  
Bugün **SQL Server**, **MySQL**, **PostgreSQL**, **Oracle**, **SQLite** gibi birçok sistem SQL dilini temel alır.

---

## 🖥️ 1.1 SQL Server ve SSMS Nedir?

Bu derste SQL komutlarını çalıştırmak için **Microsoft SQL Server** kullanacağız.  
SQL Server, Microsoft tarafından geliştirilen güçlü bir **veritabanı yönetim sistemi (RDBMS)**’dir.  
Veritabanlarını oluşturmak, yönetmek ve sorgulamak için kullanılır.

Veritabanını yönetmek için **SQL Server Management Studio (SSMS)** adlı yardımcı bir program kullanırız.  
SSMS, SQL sorgularını yazmak, tabloları görüntülemek, verileri düzenlemek ve yedekleme işlemleri yapmak için **grafiksel bir arayüz** sağlar.

📹 **SSMS Kurulum Videosu:**  
👉 [SQL Server ve SSMS Kurulumu (YouTube)](https://www.youtube.com/watch?v=AO0u0D6pKP4)

---

## ⚙️ 1.2 SSMS Kurulum Sonrası Giriş Ekranı

Kurulum tamamlandıktan sonra SSMS’i açtığınızda karşınıza **Server Connection (Sunucu Bağlantısı)** ekranı gelir.

Bu ekranda 3 önemli alan bulunur:

### 🔸 **Server Type**
Burada genellikle **Database Engine** seçeneği kalır (varsayılan olarak gelir).  
Bu, veritabanı motoruna bağlanacağımız anlamına gelir.

### 🔸 **Server Name**
Sunucu adıdır. SSMS’in hangi SQL Server örneğine bağlanacağını belirtir.

- Eğer SQL Server’ı **kendi bilgisayarınıza** kurduysanız:
  ```
  .\SQLEXPRESS
  ```
  veya sadece
  ```
  .
  ```
  yazabilirsiniz.

  > `.` veya `localhost` kendi bilgisayarınız anlamına gelir.

- Eğer özel bir isimle kurduysanız (örneğin `SQL2022`), o adı yazmalısınız:
  ```
  .\SQL2022
  ```

- Uzak bir sunucuya bağlanacaksanız o sunucunun IP adresi veya ismi yazılır:
  ```
  192.168.1.100\SQLEXPRESS
  ```

---

### 🔸 **Authentication (Kimlik Doğrulama Türü)**

SSMS’e bağlanırken iki farklı kimlik doğrulama yöntemi kullanabilirsiniz:

| Authentication Türü | Açıklama |
|----------------------|-----------|
| **Windows Authentication** | Bilgisayarınızda oturum açtığınız Windows hesabını kullanarak giriş yapar. (Şifre istemez.) |
| **SQL Server Authentication** | SSMS için özel bir kullanıcı adı ve şifre kullanılır. (Genellikle `sa` kullanıcısı ile giriş yapılır.) |

> 💡 Eğer ilk defa kullanıyorsanız **Windows Authentication** seçili kalsın.  
> SQL Authentication daha sonra şifreli kullanıcılar oluşturulduğunda kullanılır.

---

### 🔸 **Username ve Password Alanları**

- Eğer “Windows Authentication” seçiliyse, **username** ve **password** alanları **pasif** olur.
- Eğer “SQL Server Authentication” seçerseniz:
  - `Login:` kısmına genelde `sa` yazılır.
  - `Password:` kısmına, SQL Server kurulumunda belirlediğiniz şifre girilir.

🔹 Eğer kullanıcı adını `./` olarak yazarsanız, bu da **“bu bilgisayardaki yerel sunucu”** anlamına gelir.

Örnek:
```
Server Name: ./SQLEXPRESS
Authentication: Windows Authentication
```

veya:
```
Server Name: .
Authentication: SQL Server Authentication
Login: sa
Password: (Kurulumda belirlediğiniz şifre)
```

---

## 🧠 1.3 SQL’in Temel Özellikleri

- SQL komutları **büyük/küçük harf duyarlı değildir**.  
  `SELECT` ile `select` aynı işlemi yapar. Ancak okunabilirlik açısından büyük harf tercih edilir.

- Komutlar genellikle **noktalı virgül ( ; )** ile sonlandırılır.  
  Bu zorunlu değildir ama birden fazla sorgu yazarken ayırıcı olarak kullanılabilir.

- SQL ifadeleri, veritabanı sistemine göre küçük farklılıklar gösterebilir.  
  Örneğin:  
  - `TOP` ifadesi SQL Server’da,  
  - `LIMIT` ifadesi MySQL’de kullanılır.

---

## 📊 1.4 SQL ve Veritabanı Mantığı

Bir veritabanını bir **Excel çalışma kitabı** gibi düşünebiliriz:  
- Her sayfa = **Tablo (Table)**  
- Her sütun = **Alan (Field)**  
- Her satır = **Kayıt (Record)**  

Ancak SQL veritabanları bundan çok daha güçlüdür:  
- Farklı tablolar birbiriyle ilişkilendirilebilir,  
- Karmaşık sorgular yazılabilir,  
- Binlerce satırda işlem yapılabilir.

---

## 🧱 1.5 SQL Komut Grupları

SQL dili temelde üç ana gruba ayrılır:

| Tür | Açıklama |
|-----|-----------|
| **DDL (Data Definition Language)** | Veritabanı yapısını tanımlar ve değiştirir. (ör. tablo oluşturma, silme) |
| **DQL (Data Query Language)** | Verileri sorgular ve raporlar. (ör. filtreleme, sıralama, gruplama) |
| **DML (Data Manipulation Language)** | Veriler üzerinde işlem yapar. (ör. veri ekleme, silme, güncelleme) |

---

🔹 **NOT:**  
SQL sorgularını çalıştırırken her sorgunun sonunda `;` (noktalı virgül) kullanımı zorunlu değildir.  
İsterseniz kullanmayabilirsiniz; ancak birden fazla sorgu arka arkaya çalıştıracaksanız `;` kullanmak iyi bir pratiktir.

---

## 🧩 1.6 SQL Veri Tipleri (Data Types)

SQL’de her sütun belirli bir **veri tipine (data type)** sahiptir.  
Veri tipi, o sütunda hangi türde veri saklanabileceğini belirler (örneğin sayı mı, metin mi, tarih mi).

Aşağıda bu kısmı anlatmak için henüz görmediğiniz komutları kullandım burada yabancı gelen komutlara çok takılmayın, dikkat etmeniz gereken kısım veri tiplerinin kullanımı olmalı.

Veri tipleri 4 ana grupta incelenir:

---

### 🔹 1️⃣ Sayısal (Numeric) Veri Tipleri

| Veri Tipi | Açıklama | Örnek |
|------------|-----------|--------|
| `INT` | Tam sayılar için kullanılır | `100`, `-45` |
| `BIGINT` | Çok büyük tam sayılar için | `5000000000` |
| `FLOAT` | Ondalıklı sayılar (yaklaşık değer) | `3.14` |
| `DECIMAL(p,s)` | Ondalıklı sayılar (kesin değer) | `DECIMAL(6,2)` → `1234.56` |
| `SMALLINT` | Küçük tam sayılar (2 byte) | `150` |

> 💡 Örnek:
> ```sql
> CREATE TABLE OrnekSayi (
>     UrunID INT,
>     Fiyat DECIMAL(5,2),
>     VergiOrani FLOAT
> );
> ```

---

### 🔹 2️⃣ Metin (String) Veri Tipleri

| Veri Tipi | Açıklama | Örnek |
|------------|-----------|--------|
| `CHAR(n)` | Sabit uzunlukta metin | `'ABC'` (CHAR(5) → `'ABC  '`) |
| `VARCHAR(n)` | Değişken uzunlukta metin | `'Ahmet'` |
| `NVARCHAR(n)` | Unicode karakterler için (Türkçe karakterleri destekler) | `'İzmir'` |
| `TEXT` | Uzun metinler (eski sürüm) | `'Bu bir açıklamadır.'` |

> 💡 **NOT:**  
> SQL Server’da metin ifadeleri **tek tırnak (‘ ’)** içinde yazılır.  
> C, Java veya Python’daki gibi **çift tırnak (“ ”)** kullanılmaz.  
> ```sql
> SELECT 'Merhaba SQL!' AS Mesaj;
> ```

> 🧠 **Unicode Karakterler:**  
> Eğer tablo Türkçe karakter içerecekse (ğ, ü, ş, ç, ö, ı gibi) sütun tipi **NVARCHAR** olmalıdır.

---

### 🔹 3️⃣ Tarih ve Saat (Date & Time) Veri Tipleri

| Veri Tipi | Açıklama | Örnek |
|------------|-----------|--------|
| `DATE` | Yalnızca tarih bilgisi tutar | `'2025-10-23'` |
| `TIME` | Yalnızca saat bilgisi tutar | `'14:35:00'` |
| `DATETIME` | Tarih + saat bilgisi tutar | `'2025-10-23 14:35:00'` |
| `SMALLDATETIME` | Daha düşük doğrulukta tarih/saat | `'2025-10-23 14:30'` |
| `YEAR()` / `MONTH()` / `DAY()` | Tarih parçalama fonksiyonları | `YEAR(GETDATE())` → `2025` |

> 💡 **NOT:**  
> Tarihler **‘YYYY-MM-DD’** biçiminde ve **tek tırnak içinde** yazılmalıdır. (Türkçe karşılığı YYYY-AA-GG => Y: Yıl => A: Ay => G: Gün) 
> ```sql
> INSERT INTO Calisanlar (Ad, Soyad, IseBaslamaTarihi)
> VALUES ('Ahmet', 'Demir', '2023-04-15');
> ```

---

### 🔹 4️⃣ Mantıksal (Boolean) Veri Tipi

| Veri Tipi | Açıklama | Örnek |
|------------|-----------|--------|
| `BIT` | 0 veya 1 değerleri alır (false / true) | `1` veya `0` |

> 💡 `BIT` genellikle **durum** veya **aktif/pasif** bilgisi için kullanılır:
> ```sql
> CREATE TABLE Ogrenciler (
>     OgrenciID INT,
>     Ad NVARCHAR(50),
>     AktifMi BIT
> );
> 
> INSERT INTO Ogrenciler VALUES (1, 'Ali', 1);
> ```

---

### 🔹 5️⃣ Diğer Sık Kullanılan Tipler

| Veri Tipi | Açıklama |
|------------|-----------|
| `MONEY` | Para birimi (4 ondalığa kadar) |
| `UNIQUEIDENTIFIER` | Eşsiz kimlik (GUID) |
| `VARBINARY` | Görsel veya dosya verisi |
| `IMAGE` | Eski sürüm görsel verisi (kullanımı önerilmez) |

---

## 🧪 Örnek Uygulama

Aşağıdaki tablo, farklı veri tiplerinin nasıl kullanıldığını gösterir:

```sql
CREATE TABLE Urunler (
    UrunID INT PRIMARY KEY,
    UrunAdi NVARCHAR(100),
    BirimFiyati DECIMAL(6,2),
    StokMiktari INT,
    UretimTarihi DATE,
    AktifMi BIT
);

INSERT INTO Urunler (UrunID, UrunAdi, BirimFiyati, StokMiktari, UretimTarihi, AktifMi)
VALUES (1, 'Filtre Kahve', 120.50, 50, '2024-06-12', 1);
```

> 💡 Bu örnekte:  
> - Metin için **NVARCHAR**,  
> - Fiyat için **DECIMAL**,  
> - Tarih için **DATE**,  
> - Durum bilgisi için **BIT** kullanılmıştır.

---

> 📘 *Artık temel veri tiplerini biliyorsunuz. :)*



---

## 🧩 2. SQL Komut Türleri Genel Bakış

| Tür | Açıklama | Örnek Komutlar |
|-----|-----------|----------------|
| **DDL** (Data Definition Language) | Veritabanı nesnelerini tanımlar veya değiştirir | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| **DQL** (Data Query Language) | Verileri sorgulamak için kullanılır | `SELECT`, `WHERE`, `ORDER BY DESC`, `ORDER BY ASC`, `TOP`, `AND`, `OR`, `IN`, `NOT`, `HAVING`, `GROUP BY`, ... |
| **DML** (Data Manipulation Language) | Veriler üzerinde işlem yapar | `INSERT INTO`, `UPDATE`, `DELETE` |

---

> 📘 *Bir sonraki bölümde SQL’in yapı tanımlama kısmı olan **DDL (Data Definition Language)** konusuna geçilecektir.*
