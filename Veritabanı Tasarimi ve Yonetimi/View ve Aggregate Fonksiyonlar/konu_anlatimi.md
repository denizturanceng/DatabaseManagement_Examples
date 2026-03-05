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

View, veritabanında **saklanan bir SQL sorgusudur.**

Başka bir deyişle:

Bir sorguyu veritabanına kaydederiz ve daha sonra **tablo gibi kullanabiliriz.**

View'ler fiziksel olarak veri tutmazlar.  
Sadece içinde yazılan sorgunun sonucunu gösterirler.

---

# View Kullanma Amaçları

View'ler şu durumlarda kullanılır:

### Karmaşık sorguları basitleştirmek

Bazı sorgular çok fazla JOIN içerebilir ve oldukça uzun olabilir.  
Bu sorguları tekrar tekrar yazmak yerine bir View içine koyabiliriz.

---

### Güvenlik sağlamak

Bazen bir tablonun tüm kolonlarını kullanıcıya göstermek istemeyebiliriz.  
View kullanarak yalnızca belirli kolonları gösterebiliriz.

---

### Tekrar kullanılabilir sorgular oluşturmak

Sık kullanılan rapor sorguları View olarak kaydedilebilir.

---

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

- SQL sorgularını veritabanında saklamayı sağlar
- karmaşık sorguları basitleştirir
- tablo gibi kullanılabilir.