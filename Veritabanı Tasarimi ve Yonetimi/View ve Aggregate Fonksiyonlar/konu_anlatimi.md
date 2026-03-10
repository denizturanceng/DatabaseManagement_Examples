# MSSQL – Aggregate Functions ve View

Bu dokümanda SQL Server'da veri analizi yaparken sık kullanılan **Aggregate Functions (Toplulaştırma Fonksiyonları)** ve **View** kavramları anlatılmaktadır.

---

# 1. Aggregate Functions (Toplulaştırma Fonksiyonları)

Aggregate Functions, bir tablodaki **birden fazla satırı alıp tek bir sonuç üreten fonksiyonlardır**.

Bu fonksiyonlar genellikle şu soruların cevaplarını bulmak için kullanılır:

- Tabloda kaç kayıt var?
- Toplam satış ne kadar?
- Ortalama fiyat nedir?
- En pahalı veya en ucuz ürün hangisi?

Bu tür işlemler veri analizi ve raporlama için oldukça önemlidir.

SQL Server'da en sık kullanılan Aggregate Functions şunlardır:

- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()

---

# COUNT()

COUNT fonksiyonu **bir tabloda kaç kayıt olduğunu saymak için kullanılır.**

Genellikle kayıt sayısını bulmak veya belirli bir alanda kaç veri olduğunu görmek için kullanılır.

COUNT fonksiyonunun üç farklı kullanımı vardır.

### COUNT(*)

Tablodaki **tüm satırları sayar.**

NULL değerler dahil tüm kayıtları sayar.

### COUNT(column)

Belirli bir kolonda **NULL olmayan kayıtları sayar.**

### COUNT(DISTINCT column)

Belirli bir kolondaki **tekrar etmeyen (benzersiz) değerleri sayar.**

---

# SUM()

SUM fonksiyonu **sayısal değerlerin toplamını hesaplar.**

Genellikle satış miktarları, fiyatlar veya stok miktarları gibi sayısal veriler üzerinde kullanılır.

SUM fonksiyonu yalnızca **sayısal veri tipleri ile çalışır.**

---

# AVG()

AVG fonksiyonu **ortalama değer hesaplamak için kullanılır.**

Bir kolondaki sayısal değerlerin ortalamasını verir.

Örneğin:

- ortalama ürün fiyatı
- ortalama sipariş tutarı
- ortalama satış miktarı

gibi hesaplamalar yapılabilir.

---

# MIN()

MIN fonksiyonu **bir kolondaki en küçük değeri bulur.**

Sayısal verilerde kullanılabildiği gibi tarih veya metin alanlarında da kullanılabilir.

Örneğin:

- en düşük fiyat
- en erken sipariş tarihi

---

# MAX()

MAX fonksiyonu **bir kolondaki en büyük değeri bulur.**

Örneğin:

- en pahalı ürün
- en yüksek satış miktarı
- en son sipariş tarihi

gibi bilgiler elde edilebilir.

---

# GROUP BY

Aggregate Functions çoğu zaman **GROUP BY ile birlikte kullanılır.**

GROUP BY, tablodaki verileri **belirli bir kolona göre gruplar.**

Her grup için ayrı ayrı hesaplama yapılmasını sağlar.

Örneğin:

- her müşterinin kaç siparişi var
- her kategoride kaç ürün var
- her şehirde kaç müşteri var

Bu tür analizler GROUP BY ile yapılır.

GROUP BY kullanıldığında:

- SELECT kısmındaki normal kolonlar
- GROUP BY içinde belirtilmelidir.

---

# HAVING

HAVING komutu, **GROUP BY ile oluşan gruplar üzerinde filtreleme yapmak için kullanılır.**

Normalde filtreleme için **WHERE** kullanılır.

Ancak WHERE, aggregate sonuçları filtreleyemez.

Bu nedenle grup sonuçlarını filtrelemek için **HAVING kullanılır.**

---

# Aggregate Functions Örnekleri

### Örnek 1 – Toplam Sipariş Sayısı

```sql
SELECT COUNT(*) AS ToplamSiparis
FROM Orders
```

Bu sorgu Orders tablosundaki toplam sipariş sayısını verir.

---

### Örnek 2 – Ortalama Ürün Fiyatı

```sql
SELECT AVG(UnitPrice) AS OrtalamaFiyat
FROM Products
```

Bu sorgu ürünlerin ortalama fiyatını hesaplar.

---

### Örnek 3 – En Pahalı Ürün

```sql
SELECT MAX(UnitPrice) AS EnPahaliUrun
FROM Products
```

Bu sorgu en yüksek fiyatlı ürünü bulur.

---

### Örnek 4 – Her Müşterinin Sipariş Sayısı

```sql
SELECT CustomerID, COUNT(OrderID) AS SiparisSayisi
FROM Orders
GROUP BY CustomerID
```

Bu sorgu her müşterinin kaç sipariş verdiğini gösterir.

---

### Örnek 5 – Her Kategoride Kaç Ürün Var

```sql
SELECT CategoryID, COUNT(ProductID) AS UrunSayisi
FROM Products
GROUP BY CategoryID
```

Bu sorgu her kategorideki ürün sayısını gösterir.

---

### Örnek 6 – 5'ten Fazla Sipariş Veren Müşteriler

```sql
SELECT CustomerID, COUNT(OrderID) AS SiparisSayisi
FROM Orders
GROUP BY CustomerID
HAVING COUNT(OrderID) > 5
```

Bu sorgu yalnızca 5'ten fazla sipariş veren müşterileri listeler.

---

# 2. View

# View Neden Kullanılır?

View konusunu anlamak için önce bir problem düşünelim.

Bir şirketin veritabanında çalışan bilgilerini tutan bir tablo olduğunu düşünelim.

| EmployeeID | Name   | Department | Salary |
|------------|--------|-----------|--------|
| 1          | Ali    | Sales     | 45000  |
| 2          | Ayşe   | HR        | 52000  |
| 3          | Mehmet | IT        | 60000  |
| 4          | Zeynep | Sales     | 48000  |

Bu tabloyu satış departmanındaki çalışanların da görebildiğini düşünelim.

Ancak satış çalışanlarının **maaş bilgilerini görmesini istemiyoruz.**

Eğer tabloyu doğrudan sorgularsak:

```sql
SELECT *
FROM Employees
```

sonuç şu şekilde olacaktır:

| EmployeeID | Name   | Department | Salary |
|------------|--------|-----------|--------|
| 1          | Ali    | Sales     | 45000  |
| 2          | Ayşe   | HR        | 52000  |
| 3          | Mehmet | IT        | 60000  |
| 4          | Zeynep | Sales     | 48000  |

Bu durumda maaş bilgileri de görünür.

Bu her zaman istenen bir durum değildir.

---

# View ile Çözüm

View kullanarak yalnızca görmek istediğimiz bilgileri içeren bir yapı oluşturabiliriz.

```sql
CREATE VIEW SalesEmployees
AS
SELECT EmployeeID, Name, Department
FROM Employees
WHERE Department = 'Sales'
```

Bu komut veritabanında bir **view oluşturur.**

---

# View Kullanımı

View oluşturulduktan sonra tablo gibi sorgulanabilir.

```sql
SELECT *
FROM SalesEmployees
```

Sonuç:

| EmployeeID | Name   | Department |
|------------|--------|-----------|
| 1          | Ali    | Sales     |
| 4          | Zeynep | Sales     |

Görüldüğü gibi maaş bilgisi görünmemektedir.

---

# Önemli Nokta

View aslında **veritabanında saklanan bir SELECT sorgusudur.**

Başka bir deyişle:

View = Kaydedilmiş SELECT sorgusu

Örneğin şu sorguyu düşünelim:

```sql
SELECT CustomerID, COUNT(OrderID)
FROM Orders
GROUP BY CustomerID
```

Bu sorguyu sürekli yazmak yerine bir View olarak kaydedebiliriz.

```sql
CREATE VIEW CustomerOrderSummary
AS
SELECT CustomerID, COUNT(OrderID)
FROM Orders
GROUP BY CustomerID
```

Daha sonra sadece şu sorguyu yazmamız yeterlidir:

```sql
SELECT *
FROM CustomerOrderSummary
```

Bu sayede karmaşık sorguları tekrar tekrar yazmak zorunda kalmayız.

# View Oluşturma

Bir View oluşturmak için **CREATE VIEW** komutu kullanılır.

Genel yapı:

```sql
CREATE VIEW ViewAdi
AS
SQL Sorgusu
```

---

# View Güncelleme

Bir View değiştirilmek istendiğinde **ALTER VIEW** komutu kullanılır.

---

# View Silme

Bir View silinmek istendiğinde **DROP VIEW** komutu kullanılır.

---

# View Örnekleri

### Örnek 1 – Müşteri Sipariş Sayısı View

```sql
CREATE VIEW MusteriSiparisSayilari
AS
SELECT CustomerID, COUNT(OrderID) AS SiparisSayisi
FROM Orders
GROUP BY CustomerID
```

---

### View Kullanımı

```sql
SELECT *
FROM MusteriSiparisSayilari
```

---

### Örnek 2 – Ürün Fiyat Bilgileri View

```sql
CREATE VIEW UrunFiyatBilgileri
AS
SELECT ProductName, UnitPrice
FROM Products
```

---

### View Sorgulama

```sql
SELECT *
FROM UrunFiyatBilgileri
```

---

### Örnek 3 – Ortalama Kategori Fiyatı View

```sql
CREATE VIEW KategoriOrtalamaFiyat
AS
SELECT CategoryID, AVG(UnitPrice) AS OrtalamaFiyat
FROM Products
GROUP BY CategoryID
```

---

### View Kullanımı

```sql
SELECT *
FROM KategoriOrtalamaFiyat
```

---

# Genel Özet

Aggregate Functions veri analizi yapmak için kullanılır.

Temel fonksiyonlar:

- COUNT()
- SUM()
- AVG()
- MIN()
- MAX()

Bu fonksiyonlar çoğunlukla **GROUP BY** ile birlikte kullanılır.

View ise:

Karmaşıklığı Gizlemek (Basitleştirme): Birçok tablonun birleştiği (JOIN) veya karmaşık hesaplamaların yapıldığı uzun sorguları tek bir "sanal tabloya" dönüştürür. Yazılım ekibi uzun sorgular yazmak yerine sadece SELECT * FROM vw_OzetTablo diyerek veriyi kolayca çeker.

Güvenlik ve Veri İzolasyonu: Kullanıcılara ana tablonun (örneğin tüm maaş listesinin) tamamına erişim vermek yerine, sadece görmeleri gereken satır ve sütunları içeren bir View veririz. Ana veriyi kilitler, sadece View'u dışa açarız.

Yeniden Kullanılabilirlik (Reusability): Aynı uzun SQL sorgusunu projenin farklı yerlerinde tekrar tekrar yazmak yerine, veritabanında bir kez View olarak yaratır ve her yerden onu çağırırız. Merkezi bir yönetim sağlar ve kod tekrarını önler.
