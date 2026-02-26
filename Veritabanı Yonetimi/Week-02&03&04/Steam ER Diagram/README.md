# 🎮 Steam Platformu Veritabanı

Bu proje, **Steam platformunun** veritabanı tasarımını göstermektedir.  
ER Diyagramı, kullanıcı profilleri, oyunlar, oyun türleri, geliştiriciler, arkadaşlıklar ve değerlendirme ilişkilerini modellemektedir.
Bir kişi birden dazla steam hesabı oluşturabilir fakat bunu yaparken her hesaba ayrı bir mail adresi tanımlayacağı için sistem her ehsabı sanki farklı bir kişi gibi görür. (Sabahki grupta player ve profile ayrılmıştı, bu örnekte her bir profile ayrı bir kişi olarak görülür.)

---

## 📌 Varlıklar (Entities)

### 1. PLAYER_PROFILES
- `PROFILE_ID (PK)`
- `login_username` → Hesap girişi için kullanılan isim (account username).
- `profile_username` → Oyunlarda ve arkadaş listesinde görünen isim (display name).
- `password`
- `email`
- `profile_level` → Genel hesap seviyesi.

### 2. PLAYER_PROFILE_GAMES
- `PROFILE_ID (FK)`
- `GAME_ID (FK)`
- `profile_level` → Oyuna özgü seviye/ilerleme.
- `playing_time`
- `bought_date`

### 3. GAME
- `GAME_ID (PK)`
- `DEVELOPER_ID (FK)`
- `GENRE_ID (FK)`
- `game_level`

### 4. DEVELOPER
- `DEVELOPER_ID (PK)`
- `name`

### 5. GENRES
- `GENRE_ID (PK)`
- `genre_name`

### 6. RATINGS
- `RATING_ID (PK)`
- `PROFILE_ID (FK)` → Yorumu yapan kullanıcı.
- `GAME_ID (FK)` → Hangi oyun için.
- `comment`
- `point`

### 7. FRIENDSHIPS
- `FRIENDSHIP_ID (PK)`
- `REQ_SND_PRF_ID (FK)` → İsteği gönderen kullanıcı.
- `REQ_GET_PRF_ID (FK)` → İsteği alan kullanıcı.
- `REQ_SEND_DATE`
- `REQ_ACC_DATE`
- `REQ_STATUS`

---

## 🔗 İlişkiler
- **DEVELOPER → GAME** → 1–N  
- **GENRE → GAME** → 1–N  
- **PLAYER_PROFILES → PLAYER_PROFILE_GAMES → GAME** → N–M  
- **PLAYER_PROFILES → RATINGS → GAME** → N–M  
- **PLAYER_PROFILES → FRIENDSHIPS → PLAYER_PROFILES** → N–M  

---

## 📖 Senaryo

1. **Kullanıcı Kaydı**:  
   Yeni oyuncular `PLAYER_PROFILES` tablosuna eklenir.  
   - `login_username`: giriş için gizli hesap adı.  
   - `profile_username`: oyunlarda görünen ad.  

2. **Oyunlar**:  
   - `GAME` tablosu her oyunu temsil eder.  
   - `DEVELOPER` ve `GENRES` tablolarına bağlıdır.  

3. **Kullanıcı-Oyun İlişkisi**:  
   - `PLAYER_PROFILE_GAMES` tablosu hangi oyuncunun hangi oyuna sahip olduğunu gösterir.  
   - Oyun süresi, seviye ve satın alma tarihi saklanır.  

4. **Değerlendirme**:  
   - Oyuncular, `RATINGS` tablosu üzerinden oyunlara puan ve yorum bırakır.  

5. **Arkadaşlık**:  
   - Oyuncular, `FRIENDSHIPS` tablosu ile birbirine istek gönderir.  
   - Kabul/red durumu, gönderim ve onay tarihleri kayıt altına alınır.  

---

## 📊 ER Diyagramı
Aynı dizinden Diagram.jpg dosyasını açarak çizilen diagrama ulaşabilirsiniz.

---

## 🚀 Kullanım
Bu veritabanı tasarımı:
- Oyun platformu geliştirme projelerinde,
- Kullanıcı–oyun ilişkilerinin modellenmesinde,
- Sosyal özelliklerin (arkadaşlık, değerlendirme) eklenmesinde örnek alınabilir.

---

## 🛠️ Gelecek Geliştirmeler
- Oyuna özel takma ad desteği (isteğe bağlı `profile_username` alanı `PLAYER_PROFILE_GAMES` tablosuna taşınabilir).  
- Arkadaşlık ilişkilerinde çift yönlü kayıtların engellenmesi için ek kısıtlamalar.  
- Oyun içi başarımlar (achievements) için yeni bir tablo eklenebilir.  

---
