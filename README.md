# İş Sağlığı ve Güvenliği İçin Mobil Risk Algılama

Saha çalışanlarının akıllı telefonlarından sensör verisi toplayan, risk analizi yapan ve web panelinde canlı izleme sağlayan platform.

## Mimari

- **Mobil:** Flutter (ivmeölçer, GPS, mikrofon, ağ durumu)
- **Backend:** Node.js + Express + Socket.io
- **Veritabanı:** PostgreSQL + Prisma
- **Web Panel:** React + Vite + Recharts + Leaflet

## Gereksinimler

- Node.js 18+
- Docker & Docker Compose
- Flutter SDK 3.x (mobil için)

## Kurulum

### 1. Veritabanı

**Postgres.app (önerilen — Docker gerekmez):**

1. [Postgres.app](https://postgresapp.com/) uygulamasını açın (menü çubuğunda fil ikonu görünmeli)
2. Veritabanını oluşturun:
```bash
/Applications/Postgres.app/Contents/Versions/latest/bin/psql -d postgres -c "CREATE DATABASE isg_risk_db;"
```
3. `.env` dosyasında `DATABASE_URL` kullanıcı adınızı içermeli:
```
DATABASE_URL="postgresql://KULLANICI_ADINIZ@localhost:5432/isg_risk_db?schema=public"
```

**Alternatif — Docker:**
```bash
docker compose up -d
```

### 2. Backend

```bash
cd backend
npm install
npm run db:setup
npm run dev
```

Backend: http://localhost:3000

### 3. Web Panel

```bash
cd web
npm install
npm run dev
```

Web panel: http://localhost:5173

### 4. Mobil Uygulama

```bash
cd mobile
flutter pub get
flutter run
```

> Sunucu adresini uygulama içinden **Ayarlar** ekranından değiştirebilirsiniz. Android'de izleme başlatıldığında bildirim çubuğunda "ISG Risk — İzleniyor" görünür ve arka planda veri gönderimi devam eder.

### Telegram Bildirimleri (opsiyonel)

Kritik/yüksek alarmlar için Telegram Bot kurulumu:

1. [@BotFather](https://t.me/BotFather) ile bot oluşturun, token alın
2. Botu admin grubunuza ekleyin
3. `https://api.telegram.org/bot<TOKEN>/getUpdates` ile `chat_id` öğrenin
4. `.env` dosyasına ekleyin:
```
TELEGRAM_ENABLED=true
TELEGRAM_BOT_TOKEN=your_bot_token
TELEGRAM_CHAT_ID=your_chat_id
```

## Demo Hesapları

| Rol | E-posta | Şifre |
|-----|---------|-------|
| Admin | admin@isg.com | admin123 |
| Çalışan | calisan@isg.com | employee123 |

## API Özeti

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| POST | /api/auth/login | Giriş |
| PATCH | /api/auth/change-password | Şifre değiştir |
| POST | /api/auth/register | Kullanıcı oluştur (admin) |
| GET | /api/devices/locations | Cihaz konumları (harita) |
| POST | /api/devices/register | Cihaz kaydet |
| POST | /api/sensor-data | Sensör verisi gönder |
| GET | /api/sensor-data | Geçmiş sensör verisi |
| GET | /api/alarms | Alarm listesi |
| PATCH | /api/alarms/:id/resolve | Alarmı çöz |
| CRUD | /api/danger-zones | Tehlikeli bölge yönetimi |

Detaylı API dokümantasyonu: [docs/API.md](docs/API.md)

## Demo Senaryosu

1. Admin olarak web panele giriş yapın
2. Tehlikeli bölgeleri kontrol edin / yeni bölge ekleyin
3. Çalışan olarak mobil uygulamaya giriş yapın, izlemeyi başlatın
4. Telefonu sertçe sallayın → `hard_impact` alarmı
5. Tehlikeli bölgeye yaklaşın → `danger_zone_entry` alarmı
6. Web panelde canlı alarm ve grafikleri izleyin

## Proje Yapısı

```
proje/
├── backend/     # Node.js API + analiz + Socket.io
├── web/         # React dashboard
├── mobile/      # Flutter sensör uygulaması
├── docs/        # Teknik dokümantasyon
└── docker-compose.yml
```
