# 🧩 1. DQL (Data Query Language)

**DQL (Data Query Language)**, veritabanındaki **verileri sorgulamak, filtrelemek ve analiz etmek** için kullanılan SQL komut grubudur.  
Veritabanı yapısını değil, **verilerin kendisini okur (read-only)**.  
Bu bölümde `SELECT` temelli sorgularla Northwind Türkçe veritabanı üzerinde örnekler yapacağız.

---

## 🏢 1️⃣ Northwind Türkçe Veritabanına Genel Bakış

Northwind, bir toptan satış yapan firmanın müşterilerini, siparişlerini, çalışanlarını ve ürünlerini içeren örnek bir ticari veritabanıdır.

| Tablo Adı | Açıklama |
|------------|-----------|
| **Musteriler** | Müşteri bilgilerini tutar (şirket adı, iletişim, ülke, şehir vb.) |
| **Calisanlar** | Şirket çalışanlarını listeler |
| **Tedarikciler** | Ürün sağlayan firmaları içerir |
| **Urunler** | Satılan ürünlerin fiyat, stok, kategori ve tedarikçi bilgilerini içerir |
| **Siparisler** | Her müşteriye ait siparişleri tutar |
| **SiparisDetaylari** | Siparişe ait ürün, fiyat ve miktar bilgilerini içerir |
| **Nakliyeciler** | Nakliyeci firmaların bilgilerini içerir |

---

## 🔍 2️⃣ SELECT – Veri Sorgulama
```sql
SELECT * FROM Musteriler;
```
Tüm sütunları listeler.  
Belirli sütunlar için:
```sql
SELECT SirketAdi, Sehir, Ulke FROM Musteriler;
```

---

## ⚙️ 3️⃣ WHERE – Koşul Belirtme
```sql
SELECT * FROM Musteriler
WHERE Ulke = 'Türkiye' AND Sehir = 'İzmir';
```
> `AND`, `OR`, `IN`, `BETWEEN` gibi operatörlerle koşullar yazılabilir.

---

## 🔤 4️⃣ LIKE – Desen Arama
```sql
SELECT * FROM Musteriler
WHERE SirketAdi LIKE 'A%';  -- A ile başlayan
```
| Desen | Açıklama |
|--------|-----------|
| `%A` | A ile biten |
| `%an%` | içinde “an” geçen |
| `_a%` | ikinci harfi “a” olan |

---

## 📊 5️⃣ ORDER BY – Sıralama
```sql
SELECT SirketAdi, Ulke FROM Musteriler
ORDER BY Ulke ASC;   -- Artan
```
```sql
SELECT * FROM Urunler
ORDER BY BirimFiyati DESC;  -- Azalan
```

---

## 🔢 6️⃣ DISTINCT – Tekil (Unique) Kayıtlar
```sql
SELECT DISTINCT Ulke FROM Musteriler;
```

---

## 🔝 7️⃣ TOP – İlk N Kaydı Getirmek
```sql
SELECT TOP 10 * FROM Urunler;
```

---

## 🧮 8️⃣ AGGREGATE Fonksiyonlar
| Fonksiyon | Açıklama | Örnek |
|------------|-----------|--------|
| `COUNT()` | Kayıt sayısını verir | `SELECT COUNT(*) FROM Urunler;` |
| `SUM()` | Toplam değeri verir | `SELECT SUM(StokMiktari) FROM Urunler;` |
| `AVG()` | Ortalama değeri verir | `SELECT AVG(BirimFiyati) FROM Urunler;` |
| `MAX()` | En yüksek değeri verir | `SELECT MAX(BirimFiyati) FROM Urunler;` |
| `MIN()` | En düşük değeri verir | `SELECT MIN(BirimFiyati) FROM Urunler;` |

---

## 🧩 9️⃣ GROUP BY – Gruplama (Henüz göstermedim. 21.10.2025 tarihli derste anlatacağım. Fakat siz isterseniz önden öğrenebilirsiniz :D )
```sql
SELECT KategoriID, COUNT(*) AS UrunSayisi
FROM Urunler
GROUP BY KategoriID;
```

---

## 🔎 1️⃣0️⃣ HAVING – Gruplar Üzerinde Filtreleme (Henüz göstermedim. 21.10.2025 tarihli derste anlatacağım. Fakat siz isterseniz önden öğrenebilirsiniz :D )
```sql
SELECT KategoriID, COUNT(*) AS UrunSayisi
FROM Urunler
GROUP BY KategoriID
HAVING COUNT(*) > 5;
```

---

## ✂️ 1️⃣1️⃣ STRING (Metin) Fonksiyonları (Derste dediğim gibi bu string fonksiyonlarını kurcalayıp öğrenebilirsiniz buradan :D )

SQL Server’da metin (string) ifadeleri üzerinde işlem yapmak için kullanılan bazı temel fonksiyonlar aşağıdadır.  
Örnekler Northwind veritabanı üzerinden verilmiştir.

| Fonksiyon | Açıklama | Örnek |
|------------|-----------|--------|
| `LEN()` | Metin uzunluğunu verir | `SELECT SirketAdi, LEN(SirketAdi) AS Uzunluk FROM Musteriler;` |
| `UPPER()` | Tüm harfleri büyük yapar | `SELECT UPPER(SirketAdi) FROM Musteriler;` |
| `LOWER()` | Tüm harfleri küçük yapar | `SELECT LOWER(SirketAdi) FROM Musteriler;` |
| `LEFT()` | Soldan belirli sayıda karakter alır | `SELECT LEFT(SirketAdi, 3) FROM Musteriler;` |
| `RIGHT()` | Sağdan belirli sayıda karakter alır | `SELECT RIGHT(SirketAdi, 4) FROM Musteriler;` |
| `SUBSTRING()` | Belirli aralıktaki karakterleri döndürür | `SELECT SUBSTRING(SirketAdi, 2, 5) FROM Musteriler;` |
| `REPLACE()` | Belirli bir ifadeyi değiştirir | `SELECT REPLACE(SirketAdi, 'A', '*') FROM Musteriler;` |
| `CONCAT()` | Metinleri birleştirir | `SELECT CONCAT(Ad, ' ', Soyad) AS CalisanAdi FROM Calisanlar;` |
| `LTRIM()` / `RTRIM()` | Boşlukları kırpar | `SELECT LTRIM(RTRIM(SirketAdi)) FROM Musteriler;` |

> 💡 **Uygulama:**  
> “Musteriler” tablosunda şirket adlarını tamamen büyük harfli görmek için:  
> ```sql
> SELECT UPPER(SirketAdi) AS BuyukHarfSirketAdi FROM Musteriler;
> ```

---

## 📅 1️⃣2️⃣ DATE (Tarih) Fonksiyonları (Derste dediğim gibi bu date fonksiyonlarını kurcalayıp öğrenebilirsiniz buradan :D )


Tarih ve saat verilerini analiz etmek için SQL Server’da birçok tarih fonksiyonu bulunur.

| Fonksiyon | Açıklama | Örnek |
|------------|-----------|--------|
| `GETDATE()` | Sistemdeki anlık tarih ve saati verir | `SELECT GETDATE();` |
| `YEAR()` | Yılı döndürür | `SELECT YEAR(SiparisTarihi) AS Yil FROM Siparisler;` |
| `MONTH()` | Ay bilgisini döndürür | `SELECT MONTH(SiparisTarihi) AS Ay FROM Siparisler;` |
| `DAY()` | Gün bilgisini döndürür | `SELECT DAY(SiparisTarihi) AS Gun FROM Siparisler;` |
| `DATEPART()` | Tarihin belirli bir parçasını alır | `SELECT DATEPART(QUARTER, SiparisTarihi) AS Ceyrek FROM Siparisler;` |
| `DATEDIFF()` | İki tarih arasındaki farkı verir | `SELECT DATEDIFF(DAY, SiparisTarihi, SevkTarihi) AS TeslimSuresi FROM Siparisler;` |
| `DATEADD()` | Belirli süre ekler | `SELECT DATEADD(DAY, 7, SiparisTarihi) AS TahminiTeslim FROM Siparisler;` |
| `FORMAT()` | Tarihi özel biçimde yazar | `SELECT FORMAT(SiparisTarihi, 'dd.MM.yyyy') FROM Siparisler;` |

> 💡 **Uygulama:**  
> Siparişlerin tahmini teslim tarihini bulmak için:  
> ```sql
> SELECT SiparisID, SiparisTarihi, DATEADD(DAY, 7, SiparisTarihi) AS TahminiTeslim
> FROM Siparisler;
> ```

---

## 🎯 1️⃣3️⃣ Uygulama Önerileri

Aşağıdaki sorguları Northwind Türkçe veritabanında çalıştırarak DQL pratiği yapabilirsiniz:

1️⃣ **Türkiye’deki müşterilerin adını ve şehirlerini listeleyin.**
```sql
SELECT SirketAdi, Sehir FROM Musteriler WHERE Ulke = 'Türkiye';
```

2️⃣ **Ürünlerin ortalama fiyatının üzerindekileri bulun.**
```sql
SELECT UrunAdi, BirimFiyati
FROM Urunler
WHERE BirimFiyati > (SELECT AVG(BirimFiyati) FROM Urunler);
```

3️⃣ **Her ülkeye göre müşteri sayısını bulun.**
```sql
SELECT Ulke, COUNT(*) AS MusteriSayisi
FROM Musteriler
GROUP BY Ulke;
```

4️⃣ **Kategori bazında toplam ürün sayısını hesaplayın (5’ten fazla olanları gösterin).**
```sql
SELECT KategoriID, COUNT(*) AS UrunSayisi
FROM Urunler
GROUP BY KategoriID
HAVING COUNT(*) > 5;
```

5️⃣ **Şirket adını büyük harfe çevirip uzunluğunu gösterin.**
```sql
SELECT UPPER(SirketAdi) AS BuyukHarfSirketAdi, LEN(SirketAdi) AS Uzunluk
FROM Musteriler;
```

6️⃣ **Siparişlerin kaç gün içinde sevk edildiğini bulun.**
```sql
SELECT SiparisID, DATEDIFF(DAY, SiparisTarihi, SevkTarihi) AS TeslimSuresi
FROM Siparisler;
```

7️⃣ **En yüksek fiyata sahip ilk 5 ürünü listeleyin.**
```sql
SELECT TOP 5 UrunAdi, BirimFiyati
FROM Urunler
ORDER BY BirimFiyati DESC;
```

---

> 💡 **Özet:**  
> - DQL veritabanındaki **verileri okuma ve analiz etme** dilidir.  
> - `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `DISTINCT`, `LIKE`, `TOP` ifadeleriyle zenginleştirilir.  
> - **STRING** ve **DATE** fonksiyonları, verileri biçimlendirmek ve analiz etmek için çok önemlidir.  
> - Tüm sorguları Northwind Türkçe veritabanında deneyebilirsiniz.
