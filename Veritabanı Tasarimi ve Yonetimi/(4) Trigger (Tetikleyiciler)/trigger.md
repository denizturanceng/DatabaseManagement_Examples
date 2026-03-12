# MSSQL TRIGGER Uygulamaları (Northwind Türkçe)

Bu uygulamada **SQL Server TRIGGER yapısı** Northwind Türkçe veritabanı üzerinden öğrenilecektir.

Trigger, bir tablo üzerinde **INSERT, UPDATE veya DELETE işlemi gerçekleştiğinde otomatik çalışan SQL kodudur.**

---

# 1. TRIGGER Nedir?

Trigger, veritabanında belirli bir olay gerçekleştiğinde otomatik çalışan bir mekanizmadır.

Trigger şu durumlarda çalışabilir:

- INSERT (yeni kayıt eklendiğinde)
- UPDATE (kayıt güncellendiğinde)
- DELETE (kayıt silindiğinde)

Trigger sayesinde:

- Veri hataları engellenebilir
- Log kayıtları tutulabilir
- İş kuralları uygulanabilir
- Otomatik işlemler yapılabilir

---

# 2. Trigger Çalışma Mantığı

SQL Server trigger'larda iki özel tablo bulunur.

## inserted tablosu

Yeni eklenen veya güncellenen kayıtları içerir.

## deleted tablosu

Silinen veya güncellemeden önceki eski kayıtları içerir.

UPDATE işlemlerinde:

- eski veri → deleted
- yeni veri → inserted

---

# 3. Örnek 1 — Ürün Fiyat Değişimini Loglama

Amaç: Bir ürünün fiyatı değiştiğinde eski ve yeni fiyatın kaydedilmesi.

Önce log tablosu oluşturalım.

```sql
CREATE TABLE UrunFiyatLog
(
LogID INT IDENTITY(1,1) PRIMARY KEY,
UrunID INT,
EskiFiyat MONEY,
YeniFiyat MONEY,
DegisiklikTarihi DATETIME
)
```

Trigger oluşturma:

```sql
CREATE TRIGGER trg_UrunFiyatDegisimi
ON Urunler
AFTER UPDATE
AS
BEGIN
INSERT INTO UrunFiyatLog (UrunID, EskiFiyat, YeniFiyat, DegisiklikTarihi)
SELECT
d.UrunID,
d.BirimFiyati,
i.BirimFiyati,
GETDATE()
FROM deleted d
INNER JOIN inserted i
ON d.UrunID = i.UrunID
WHERE d.BirimFiyati <> i.BirimFiyati
END
```

Test etmek için:

```sql
UPDATE Urunler
SET BirimFiyati = BirimFiyati + 10
WHERE UrunID = 1
```

Sonucu görmek için:

```sql
SELECT * FROM UrunFiyatLog
```

---

# 4. Örnek 2 — Geçersiz Fiyatı Engelleme

Amaç: Ürün fiyatının 0 veya negatif olmasını engellemek.

```sql
CREATE TRIGGER trg_GecersizFiyatKontrol
ON Urunler
AFTER INSERT, UPDATE
AS
BEGIN

IF EXISTS (
SELECT *
FROM inserted
WHERE BirimFiyati <= 0
)

BEGIN
RAISERROR ('Birim fiyat 0 veya negatif olamaz.',16,1)
ROLLBACK TRANSACTION
END

END
```

Test:

```sql
UPDATE Urunler
SET BirimFiyati = 0
WHERE UrunID = 2
```

Bu işlem hata vermelidir.

---

# 5. Örnek 3 — Silinen Ürünleri Kaydetme

Amaç: Silinen ürünleri başka bir tabloda saklamak.

Önce log tablosu oluşturulur.

```sql
CREATE TABLE SilinenUrunlerLog
(
LogID INT IDENTITY(1,1) PRIMARY KEY,
UrunID INT,
UrunAdi NVARCHAR(100),
SilinmeTarihi DATETIME
)
```

Trigger:

```sql
CREATE TRIGGER trg_SilinenUrunleriKaydet
ON Urunler
AFTER DELETE
AS
BEGIN

INSERT INTO SilinenUrunlerLog (UrunID, UrunAdi, SilinmeTarihi)

SELECT
UrunID,
UrunAdi,
GETDATE()

FROM deleted

END
```

Test:

```sql
DELETE FROM Urunler
WHERE UrunID = 3
```

Sonucu görmek için:

```sql
SELECT * FROM SilinenUrunlerLog
```

---

# 6. Örnek 4 — Aşırı Fiyat Girişini Engelleme

Amaç: Ürün fiyatının çok yüksek girilmesini önlemek.

```sql
CREATE TRIGGER trg_AsiriFiyatKontrol
ON Urunler
AFTER INSERT, UPDATE
AS
BEGIN

IF EXISTS (
SELECT *
FROM inserted
WHERE BirimFiyati > 1000
)

BEGIN
RAISERROR ('Birim fiyat 1000 den büyük olamaz.',16,1)
ROLLBACK TRANSACTION
END

END
```

Test:

```sql
UPDATE Urunler
SET BirimFiyati = 2000
WHERE UrunID = 1
```

---

# 7. Trigger Silme

Bir trigger silmek için:

```sql
DROP TRIGGER trigger_adi
```

Örnek:

```sql
DROP TRIGGER trg_AsiriFiyatKontrol
```

---

# 8. Uygulama Soruları

Aşağıdaki trigger'ları kendiniz yazınız.

### Soru 1

Urunler tablosunda bir kayıt silindiğinde silinen ürün bilgilerini bir log tablosuna kaydeden trigger yazınız.

---

### Soru 2

Urunler tablosuna eklenen bir ürünün stok miktarı negatif olursa işlemi iptal eden trigger yazınız.

---

### Soru 3

Bir ürünün fiyatı değiştiğinde eski fiyat ve yeni fiyatı log tablosuna kaydeden trigger yazınız.

---

### Soru 4

Siparisler tablosuna yeni bir sipariş eklendiğinde sipariş tarihi ve müşteri ID bilgisini bir log tablosuna kaydeden trigger yazınız.

---

# 9. Sonuç

Trigger sayesinde:

- Veri değişiklikleri kontrol edilebilir
- Hatalı veri girişleri engellenebilir
- Sistem otomatik kayıt tutabilir

Trigger yapısı veritabanlarında **iş kurallarını otomatik uygulamak için kullanılır.**