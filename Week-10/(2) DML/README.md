# 📝 DML (Data Manipulation Language) – Veri İşleme Komutları

DML komutları, veritabanındaki verileri **eklemek, güncellemek, silmek, kopyalamak ve değiştirmek** için kullanılır.  
Bu bölümde SQL Server üzerinde kullanılan temel DML yapıları ele alınmıştır.

---

# ⭐ 1. DECLARE ve SET – Değişken Tanımlama ve Değer Atama

SQL Server’da değişken tanımlamak için **DECLARE**, değer atamak için **SET** kullanılır.

## 🧩 DECLARE – Değişken Tanımlama
\`\`\`sql
DECLARE @yas INT;
DECLARE @ad NVARCHAR(50);
DECLARE @ortalama FLOAT;
\`\`\`

## 🧩 SET – Değer Atama
\`\`\`sql
SET @yas = 25;
SET @ad = 'Deniz';
SET @ortalama = 84.5;
\`\`\`

## ✔ Kullanım Senaryosu
\`\`\`sql
DECLARE @altFiyat INT;
SET @altFiyat = 50;

SELECT * FROM Urunler
WHERE BirimFiyati > @altFiyat;
\`\`\`
Bu sorgu, bir değişkene bağlı dinamik filtreleme yapmaya olanak tanır.
---

# ⭐ 2. SELECT INTO – Veriden Yeni Tablo Oluşturma

`SELECT INTO`, sorgu sonucundan **yeni bir tablo oluşturur**.

## 🧩 Tam tabloyu kopyalama
\`\`\`sql
SELECT *
INTO Urunler_Yedek
FROM Urunler;
\`\`\`

## 🧩 Filtrelenmiş kopyalama
\`\`\`sql
SELECT UrunAdi, BirimFiyati
INTO PahaliUrunler
FROM Urunler
WHERE BirimFiyati > 100;
\`\`\`


---

# ⭐ 3. INSERT INTO – Veri Ekleme

INSERT komutu iki şekilde yazılabilir.

---

## 🧩 A) Tüm sütunlara sırayla ekleme
\`\`\`sql
INSERT INTO Musteriler
VALUES ('A101', 'ABC Market', 'İzmir', 'Türkiye');
\`\`\`

⚠ *VALUES sırası, tablodaki sütun sırasıyla birebir aynı olmalıdır.*

---

## 🧩 B) Sadece belirli sütunlara ekleme (Önerilen yöntem)
\`\`\`sql
INSERT INTO Musteriler (MusteriID, SirketAdi, Sehir)
VALUES ('A102', 'XYZ Ltd', 'Ankara');
\`\`\`

---

## 🧩 NULL Değeri Bırakabilme
\`\`\`sql
INSERT INTO Musteriler (MusteriID, SirketAdi, Sehir, Ulke)
VALUES ('A103', 'Tekno Bilgisayar', NULL, 'Türkiye');
\`\`\`

---

# ⭐ 4. UPDATE – Veri Güncelleme

Veriyi değiştirmek için kullanılır.  
**Mutlaka WHERE kullanılmalıdır!**

## 🧩 Tek alan güncelleme
\`\`\`sql
UPDATE Musteriler
SET Sehir = 'İzmir'
WHERE MusteriID = 'A102';
\`\`\`

## ⚠ WHERE yoksa tehlikelidir
\`\`\`sql
UPDATE Urunler SET BirimFiyati = 10;
\`\`\`

---

# ⭐ 5. DELETE – Satır Silme

DELETE, tablodan satır siler.

## 🧩 Satır silme
\`\`\`sql
DELETE FROM Musteriler
WHERE MusteriID = 'A103';
\`\`\`

## ⚠ WHERE yoksa tüm tablo silinir
\`\`\`sql
DELETE FROM Siparisler;
\`\`\`

---

# ⭐ 6. TRUNCATE TABLE – Tabloyu Hızlı Boşaltma

TRUNCATE TABLE, bir tablodaki **tüm veriyi çok hızlı siler**.

## 🧩 Örnek
\`\`\`sql
TRUNCATE TABLE SiparisDetaylari;
\`\`\`

## ✔ Özellikleri

- Çok hızlıdır  
- Minimal loglama yapar  
- Identity (kimlik) değerini sıfırlar  
- WHERE kullanılamaz  
- Satır satır değil toplu siler  
- Geri alma DELETE’e göre daha zordur  

---

# ⭐ 7. DELETE vs TRUNCATE – Karşılaştırma Tablosu

| Özellik | DELETE | TRUNCATE |
|--------|--------|----------|
| Loglama | Satır satır | Minimal |
| Hız | Yavaş | Çok hızlı |
| WHERE | Var | Yok |
| Identity Reset | Hayır | Evet |
| Satır Silme | Bireysel | Toplu |
| FK varsa | Engelleyebilir | Genelde çalışmaz |
| Geri alma | Var | Zor/İmkânsız |

---

# ⭐ Özet

- **DECLARE / SET:** SQL değişkenleri  
- **SELECT INTO:** Veriyi başka tabloya kopyalar  
- **INSERT INTO:** Veri ekler (sıra çok önemlidir!)  
- **UPDATE:** Veriyi değiştirir  
- **DELETE:** Satır siler  
- **TRUNCATE:** Tüm tabloyu hızlıca boşaltır  

