# 🍿 Netflix Platformu Veritabanı

Bu proje, **Netflix benzeri bir platformun** veritabanı tasarımını göstermektedir.  
ER Diyagramı, kullanıcı hesapları, profiller, içerikler, türler, oyuncular ve izleme/etkileşim geçmişi gibi ilişkileri modellemektedir.  

---

## 📌 Varlıklar (Entities)

### 1. USER
- `USER_ID (PK)`
- `email`
- `name`
- `surname`
- `subscription_type` → Kullanıcının abonelik planı.

### 2. PROFILE
- `PROFILE_ID (PK)`
- `USER_ID (FK)`
- `age_group` → Çocuk, genç, yetişkin gibi sınıflandırmalar.

### 3. CONTENT
- `CONTENT_ID (PK)`
- `content_name`
- `year`
- `duration`
- `GENRE_ID (FK)`

### 4. GENRE
- `GENRE_ID (PK)`
- `genre_name`

### 5. ACTOR
- `ACTOR_ID (PK)`
- `name`
- `surname`

### 6. WATCHES (İzleme Geçmişi)
- `PROFILE_ID (FK)`
- `CONTENT_ID (FK)`
- `date` → İzleme tarihi.

### 7. INTERACTS (Etkileşimler)
- `PROFILE_ID (FK)`
- `CONTENT_ID (FK)`
- `interaction_type` → Örn. “Like”, “Dislike”, “Add to Watchlist”.
- `date`

### 8. FEATURES (İçerik–Oyuncu İlişkisi)
- `CONTENT_ID (FK)`
- `ACTOR_ID (FK)`

### 9. CATEGORIZES (İçerik–Tür İlişkisi)
- `GENRE_ID (FK)`
- `CONTENT_ID (FK)`

---

## 🔗 İlişkiler
- **USER → PROFILE** → 1–N  
- **PROFILE → WATCHES → CONTENT** → N–M  
- **PROFILE → INTERACTS → CONTENT** → N–M  
- **CONTENT → FEATURES → ACTOR** → N–M  
- **GENRE → CATEGORIZES → CONTENT** → 1–N  

---

## 📖 Senaryo

1. **Kullanıcı Kaydı ve Profiller**  
   - Kullanıcılar `USER` tablosuna eklenir.  
   - Her kullanıcı birden fazla `PROFILE` oluşturabilir. (ör. aile üyeleri için)  

2. **İçerikler**  
   - Filmler/diziler `CONTENT` tablosunda saklanır.  
   - Her içerik bir `GENRE` ile ilişkilidir ve birden fazla `ACTOR` içerebilir.  

3. **İzleme Geçmişi**  
   - Kullanıcı profilleri hangi içeriği, hangi tarihte izlediğini `WATCHES` tablosunda saklar.  

4. **Etkileşimler**  
   - Kullanıcılar içeriklere `like/dislike` bırakabilir veya `watchlist` ekleyebilir. (`INTERACTS`)  

5. **Oyuncular ve Türler**  
   - `FEATURES` tablosu bir içeriğin hangi oyunculara ait olduğunu gösterir.  
   - `CATEGORIZES` ilişkisi içerik–tür bağlantısını tanımlar.  

---

## 📊 ER Diyagramı
Aynı dizinden `Diagram.png` dosyasını açarak diyagramın görseline ulaşabilirsiniz.

---

## 🚀 Kullanım
Bu veritabanı tasarımı:  
- Netflix benzeri platformların temel işlevlerini (izleme geçmişi, kullanıcı profilleri, etkileşimler) modellemek için,  
- İçerik–oyuncu–tür bağlantılarının tutulmasında,  
- Kişiselleştirilmiş öneri sistemlerinin temel verilerini sağlamakta kullanılabilir.  

---

## 🛠️ Gelecek Geliştirmeler
- İçeriklere **puanlama sistemi** eklenebilir.  
- Kullanıcıların izleme süresi (kaç dakika izlendi) tutulabilir.  
- **Sezon–Bölüm yapısı** için ek tablolar (Series, Episodes) tanımlanabilir.  
- Çoklu cihaz desteği için “Device” tablosu eklenebilir.  
