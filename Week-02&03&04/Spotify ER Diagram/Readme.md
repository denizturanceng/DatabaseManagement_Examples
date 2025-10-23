# 🎵 Spotify Platformu Veritabanı

Bu proje, **Spotify benzeri bir müzik platformunun** veritabanı tasarımını göstermektedir.  
ER Diyagramı, kullanıcılar, şarkılar, sanatçılar, albümler, çalma listeleri ve kullanıcı etkileşimleri arasındaki ilişkileri modellemektedir.  

---

## 📌 Varlıklar (Entities)

### 1. USER
- `USER_ID (PK)`
- `email`
- `password`
- `subscription_type`

### 2. PLAYLISTS
- `PLAYLIST_ID (PK)`
- `createdUserID (FK)`
- `name`
- `date`
- `duration`

### 3. SONGS
- `SONG_ID (PK)`
- `SINGER_ID (FK)`
- `ALBUM_ID (FK)`
- `name`
- `genre`
- `duration`

### 4. SINGER (ARTIST)
- `SINGER_ID (PK)`
- `name`
- `attribute` (opsiyonel)

### 5. ALBUMS
- `ALBUM_ID (PK)`
- `SINGER_ID (FK)`
- `name`
- `genre`
- `date`
- `duration`

### 6. PLAYLISTSONGS
- `PLAYLIST_ID (FK)`
- `SONG_ID (FK)`

### 7. USER_SONG_INTERACTIONS
- `INTERACTION_ID (PK)`
- `USER_ID (FK)`
- `SONG_ID (FK)`
- `interaction_type` → Like, Dislike, Follow vb.
- `date`

---

## 🔗 İlişkiler
- **USER → PLAYLISTS** → 1–N (bir kullanıcı birden çok playlist oluşturabilir)  
- **PLAYLISTS → SONGS** → N–M (çözüm: `PLAYLISTSONGS`)  
- **USER → SONGS** → N–M (çözüm: `USER_SONG_INTERACTIONS`)  
- **SINGER → SONGS** → 1–N (bir sanatçının birçok şarkısı olabilir)  
- **SINGER → ALBUMS** → 1–N (bir sanatçının birçok albümü olabilir)  
- **ALBUMS → SONGS** → 1–N (bir albümde birçok şarkı bulunabilir)  

---

## 📖 Senaryo

1. **Kullanıcı Kaydı**  
   - Kullanıcılar `USER` tablosuna eklenir.  
   - Kullanıcıların abonelik tipi (Free, Premium) saklanır.  

2. **Çalma Listeleri (Playlists)**  
   - Her kullanıcı birden fazla `PLAYLIST` oluşturabilir.  
   - Playlist içinde hangi şarkıların bulunduğu `PLAYLISTSONGS` tablosu üzerinden takip edilir.  

3. **Şarkılar ve Sanatçılar**  
   - Her `SONG` bir `SINGER` tarafından söylenir.  
   - Şarkılar aynı zamanda bir `ALBUM` içerisinde yer alır.  

4. **Etkileşimler**  
   - Kullanıcılar şarkıları `like`, `dislike` edebilir veya `follow` edebilir.  
   - Bu bilgiler `USER_SONG_INTERACTIONS` tablosunda tutulur.  

---

## 📊 ER Diyagramı
Aynı dizinden `Diagram.png` dosyasını açarak diyagramın görseline ulaşabilirsiniz.

---

## 🚀 Kullanım
Bu veritabanı tasarımı:  
- Spotify benzeri müzik platformlarının temel fonksiyonlarını modellemek için,  
- Kullanıcı–şarkı–albüm–playlist ilişkilerinin yönetilmesinde,  
- Öneri sistemleri ve kullanıcı etkileşimlerinin analiz edilmesinde kullanılabilir.  

---

## 🛠️ Gelecek Geliştirmeler
- **Collaborative playlists**: birden fazla kullanıcının playlist düzenleyebilmesi.  
- **Dinleme geçmişi (Listening History)** tablosu eklenebilir.  
- **Çalma sırası (Queue)** desteği eklenebilir.  
- Şarkıların **streaming sayısı** ayrı bir tabloda takip edilebilir.  
