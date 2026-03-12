# MSSQL VIEW Uygulamaları (Northwind Türkçe)

Bu uygulamada **SQL Server VIEW yapısı** Northwind Türkçe veritabanı üzerinden öğrenilecektir.

VIEW, bir sorgunun veritabanında **isim verilmiş hali** gibidir.  
Uzun ve karmaşık sorguları tekrar tekrar yazmak yerine VIEW oluşturup tablo gibi kullanabiliriz.

---

# 1. VIEW Nedir?

VIEW aslında veritabanında saklanan bir **SELECT sorgusudur**.

Avantajları:

- Uzun sorguları tekrar yazmayı önler
- JOIN karmaşasını gizler
- Veri erişimini kolaylaştırır
- Güvenlik amacıyla bazı kolonları gizleyebilir

VIEW oluşturulduktan sonra şu şekilde kullanılır:

```sql
SELECT * FROM view_adi
```

---

# 2. Northwind Türkçe Veritabanı

Bu uygulamada aşağıdaki tablolar kullanılacaktır:

- Urunler
- Kategoriler
- Musteriler
- Siparisler
- Calisanlar

---

# 3. Örnek 1 — Ürün ve Kategori Bilgisi

Amaç: Ürünlerin hangi kategoriye ait olduğunu görmek.

Normalde şöyle bir JOIN yazmamız gerekir:

```sql
SELECT 
u.UrunID,
u.UrunAdi,
k.KategoriAdi,
u.BirimFiyati
FROM Urunler u
INNER JOIN Kategoriler k
ON u.KategoriID = k.KategoriID
```

Bu sorguyu VIEW olarak kaydedelim.

```sql
CREATE VIEW vw_UrunKategoriListesi
AS
SELECT 
u.UrunID,
u.UrunAdi,
k.KategoriAdi,
u.BirimFiyati,
u.StokMiktari
FROM Urunler u
INNER JOIN Kategoriler k
ON u.KategoriID = k.KategoriID
```

Kullanımı:

```sql
SELECT * FROM vw_UrunKategoriListesi
```

---

# 4. Örnek 2 — Pahalı Ürünleri Listeleme

Amaç: Fiyatı yüksek ürünleri kolayca görebilmek.

```sql
CREATE VIEW vw_PahaliUrunler
AS
SELECT
UrunID,
UrunAdi,
BirimFiyati,
StokMiktari
FROM Urunler
WHERE BirimFiyati > 50
```

Kullanımı:

```sql
SELECT * FROM vw_PahaliUrunler
```

---

# 5. Örnek 3 — Sipariş Özeti

Amaç: Sipariş, müşteri ve çalışan bilgilerini birlikte görmek.

Normal sorgu:

```sql
SELECT
s.SiparisID,
m.MusteriAdiSirketi,
c.Adi + ' ' + c.Soyadi AS CalisanAdSoyad,
s.SiparisTarihi,
s.SevkTarihi
FROM Siparisler s
INNER JOIN Musteriler m
ON s.MusteriID = m.MusteriID
INNER JOIN Calisanlar c
ON s.CalisanID = c.CalisanID
```

VIEW oluşturalım:

```sql
CREATE VIEW vw_SiparisOzeti
AS
SELECT
s.SiparisID,
m.MusteriAdiSirketi,
c.Adi + ' ' + c.Soyadi AS CalisanAdSoyad,
s.SiparisTarihi,
s.SevkTarihi
FROM Siparisler s
INNER JOIN Musteriler m
ON s.MusteriID = m.MusteriID
INNER JOIN Calisanlar c
ON s.CalisanID = c.CalisanID
```

Kullanım:

```sql
SELECT * FROM vw_SiparisOzeti
```

---

# 6. Örnek 4 — Her Müşterinin Sipariş Sayısı

Amaç: Hangi müşterinin kaç sipariş verdiğini görmek.

```sql
CREATE VIEW vw_MusteriSiparisSayilari
AS
SELECT
m.MusteriID,
m.MusteriAdiSirketi,
COUNT(s.SiparisID) AS SiparisSayisi
FROM Musteriler m
LEFT JOIN Siparisler s
ON m.MusteriID = s.MusteriID
GROUP BY
m.MusteriID,
m.MusteriAdiSirketi
```

Kullanım:

```sql
SELECT * FROM vw_MusteriSiparisSayilari
```

---

# 7. Örnek 5 — Stok Azalan Ürünler

Amaç: Stok miktarı azalan ürünleri hızlı görmek.

```sql
CREATE VIEW vw_AzStokluUrunler
AS
SELECT
UrunID,
UrunAdi,
StokMiktari
FROM Urunler
WHERE StokMiktari < 20
```

Kullanım:

```sql
SELECT * FROM vw_AzStokluUrunler
```

---

# 8. VIEW Silme

Bir VIEW silmek için:

```sql
DROP VIEW view_adi
```

Örnek:

```sql
DROP VIEW vw_AzStokluUrunler
```

---

# 9. Uygulama Soruları

Aşağıdaki VIEW'leri kendiniz oluşturunuz.

### Soru 1

Ürün adı, kategori adı ve stok miktarını gösteren bir VIEW oluşturunuz.

---

### Soru 2

Fiyatı 100’den büyük ürünleri gösteren bir VIEW oluşturunuz.

---

### Soru 3

Her çalışanın aldığı sipariş sayısını gösteren bir VIEW oluşturunuz.

---

### Soru 4

Sipariş ID, müşteri adı ve sipariş tarihini gösteren bir VIEW oluşturunuz.

---

# 10. Sonuç

VIEW yapısı sayesinde:

- Uzun sorgular tekrar yazılmaz
- JOIN işlemleri kolaylaşır
- Veri erişimi basitleşir

VIEW'ler veritabanında **sorguların organize edilmesini sağlar**.