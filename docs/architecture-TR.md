# HizmetSepetim - Mimari Genel Bakış

## Sistem Mimarisi

HizmetSepetim, birlikte çalışarak tam bir çözüm sunan üç ana bileşenden oluşan modern bir hizmet marketplace platformudur.

```
┌─────────────────┐     ┌─────────────────┐
│  Mobil İstemciler │     │  Web Uygulaması │
│  (Android/Flutter)│     │  (PHP/Admin/Satıcı)│
└────────┬────────┘     └────────┬────────┘
         │                      │
         │ HTTPS/REST API       │ Doğrudan DB Erişimi
         │                      │
┌────────▼──────────────────────▼────────┐
│   Go API Sunucusu │   PHP Web Uygulaması│
│   (Port 8080)   │   (Port 80/443)      │
└────────┬──────────────────────┬────────┘
         │                      │
         │ SQL Sorguları        │
         │                      │
┌────────▼──────────────────────▼────────┐
│   MySQL Veritabanı│                     │
│   (Port 3306)   │                      │
└────────────────────────────────────────┘
```

---

## Bileşen Genel Bakışı

### 1. Backend API (`/hizmetSepetiApi`)

**Teknoloji:** Go 1.24, Gin Framework
**Port:** 8080
**Veritabanı:** MySQL/MariaDB

**Sorumluluklar:**
- Kullanıcı kimlik doğrulama & yetkilendirme (JWT)
- İş mantığı & veri doğrulama
- Veritabanı işlemleri
- Ödeme işleme (Stripe entegrasyonu)
- Bildirim yönetimi
- Destek chat sistemi

**Ana Özellikler:**
- RESTful API tasarımı
- JWT tabanlı kimlik doğrulama
- CORS etkin
- Cüzdan sistemi
- Promosyon kodu sistemi
- Satıcı & admin endpoint'leri

---

### 2. Android Native Uygulama (`/hizmetSepetiApp`)

**Teknoloji:** Kotlin, Jetpack Compose
**Mimari:** MVVM
**Min SDK:** 24 (Android 7.0)

**Sorumluluklar:**
- Kullanıcı arayüzü & kullanıcı deneyimi
- API iletişimi
- Yerel veri depolama (DataStore)
- Arka plan görevleri (WorkManager)
- Push bildirimleri (Firebase)

**Ana Özellikler:**
- Material 3 tasarım
- Gerçek zamanlı bildirimler
- Çevrimdışı destek
- Arka plan senkronizasyonu
- Modern UI/UX

---

### 3. Flutter Uygulaması (`/hizmetsepetimapp_flutter`)

**Teknoloji:** Flutter, Dart
**Mimari:** Provider Pattern
**Platformlar:** Android, iOS

**Sorumluluklar:**
- Çapraz platform UI
- API iletişimi
- Güvenli token depolama
- Durum yönetimi
- Platforma özel özellikler

**Ana Özellikler:**
- Çapraz platform desteği
- Güvenli depolama
- Material Design
- Provider durum yönetimi

---

### 4. Web Uygulaması (`/HizmetSepetimWeb`)

**Teknoloji:** PHP 7.4+, Bootstrap 5.3
**Mimari:** Server-side PHP, MVC benzeri yapı
**Port:** 80/443 (HTTP/HTTPS)

**Sorumluluklar:**
- Public website (landing page)
- Admin panel yönetimi
- Satıcı panel yönetimi
- İçerik yönetimi
- İstatistik gösterimi

**Ana Özellikler:**
- Modern tasarımlı public website
- 2FA güvenlikli admin paneli
- Satıcı dashboard'u
- Destek ticket yönetimi
- Bildirim sistemi
- Promosyon kodu yönetimi

---

## Veri Akışı

### Kimlik Doğrulama Akışı

```
1. Kullanıcı kimlik bilgilerini girer
   ↓
2. Uygulama POST /login gönderir
   ↓
3. API kimlik bilgilerini doğrular
   ↓
4. API JWT token oluşturur
   ↓
5. Token güvenli şekilde saklanır (DataStore/SecureStorage)
   ↓
6. Token sonraki isteklerde dahil edilir
```

### Sipariş Oluşturma Akışı

```
1. Kullanıcı hizmet & randevu seçer
   ↓
2. Uygulama POST /create_order_with_payment gönderir
   ↓
3. API siparişi doğrular
   ↓
4. API ödemeyi işler (cüzdan/kart)
   ↓
5. API veritabanında sipariş oluşturur
   ↓
6. API sipariş onayı döner
   ↓
7. Uygulama başarı mesajı gösterir
```

### Bildirim Akışı

```
1. Admin bildirim oluşturur (API/Admin Panel üzerinden)
   ↓
2. Bildirim notification_queue tablosuna kaydedilir
   ↓
3. Android: WorkManager her 15 dakikada /notifications/get çağırır
   ↓
4. Flutter: Uygulama açıldığında /notifications/get çağırır
   ↓
5. Bildirim kullanıcıya gösterilir
   ↓
6. Bildirim gönderildi olarak işaretlenir
```

---

## Veritabanı Şeması

### Ana Tablolar

**users**
- Kullanıcı hesaplarını saklar (müşteriler, satıcılar, adminler)
- Alanlar: id, email, password (hashlenmiş), role, first_name, last_name, vb.

**categories**
- Hizmet kategorileri
- Alanlar: id, name, image_url

**products**
- Sunulan hizmetler/ürünler
- Alanlar: id, category_id, name, price, description, image_url

**orders**
- Müşteri siparişleri/randevuları
- Alanlar: id, user_id, seller_id, product_name, total_price, status, appointment_datetime, vb.

**addresses**
- Kullanıcı adresleri
- Alanlar: id, user_id, full_name, address_line, city, district, vb.

**wallets**
- Kullanıcı cüzdan bakiyeleri
- Alanlar: user_id, balance, updated_at

**wallet_transactions**
- Cüzdan işlem geçmişi
- Alanlar: id, user_id, amount, type, description, created_at

**support_conversations**
- Destek chat ticket'ları
- Alanlar: id, user_id, status, last_message_at

**support_messages**
- Destek chat mesajları
- Alanlar: id, conversation_id, sender_type, sender_id, message, created_at

---

## Güvenlik Mimarisi

### Kimlik Doğrulama

- **Yöntem:** JWT (JSON Web Tokens)
- **Token Süresi:** 7 gün
- **Depolama:**
  - Android: DataStore (şifrelenmiş)
  - Flutter: flutter_secure_storage (şifrelenmiş)

### Yetkilendirme

- **Rol tabanlı erişim kontrolü:**
  - `customer` - Normal kullanıcılar
  - `seller` - Hizmet sağlayıcılar
  - `admin` - Platform yöneticileri

### Veri Koruması

- Şifreler bcrypt ile hashlenir
- Süre sonlu JWT token'lar
- Hassas veriler için güvenli depolama
- Production'da HTTPS (önerilir)

---

## API Mimarisi

### Endpoint Yapısı

```
Public Endpoint'ler:
  /login
  /register
  /get_categories
  /get_products
  /get_product_detail

Kimlik Doğrulamalı Endpoint'ler:
  /get_addresses
  /create_order
  /wallet/balance
  /user/profile

Rol Tabanlı Endpoint'ler:
  /v2/seller/* (satıcı rolü gerekli)
  /admin/* (admin rolü gerekli)
```

### İstek/Yanıt Formatı

**İstek:**
```json
POST /login
Content-Type: application/json

{
  "email": "kullanici@example.com",
  "password": "sifre123"
}
```

**Yanıt:**
```json
{
  "success": true,
  "token": "jwt_token_buraya",
  "user": {
    "id": 1,
    "email": "kullanici@example.com",
    "role": "customer"
  }
}
```

---

## Mobil Uygulama Mimarisi

### Android (MVVM)

```
View (Compose UI)
    ↓
ViewModel (Durum Yönetimi)
    ↓
Repository / ApiService
    ↓
Retrofit Client
    ↓
API Sunucusu
```

**Ana Bileşenler:**
- `MainActivity` - Navigasyon & uygulama başlatma
- `RetrofitClient` - API client kurulumu
- `DataStoreManager` - Yerel depolama
- `NotificationWorker` - Arka plan görevleri

### Flutter (Provider)

```
UI Widget'ları
    ↓
Provider (Durum Yönetimi)
    ↓
ApiService
    ↓
Dio Client
    ↓
API Sunucusu
```

**Ana Bileşenler:**
- `main.dart` - Uygulama başlatma
- `api_service.dart` - API client
- `TokenStore` - Güvenli token depolama
- `UserStore` - Kullanıcı veri depolama

---

## Ödeme Mimarisi

### Ödeme Yöntemleri

1. **Cüzdan Ödemesi**
   - Kullanıcının cüzdan bakiyesi
   - Anında kesinti
   - İşlem kaydedilir

2. **Kart Ödemesi**
   - Stripe entegrasyonu
   - Güvenli ödeme işleme
   - Kart detayları güvenli saklanır

3. **Karma Ödeme**
   - Cüzdan + kart kombinasyonu
   - Önce cüzdan, sonra kart kullanılır

### Ödeme Akışı

```
1. Kullanıcı ödeme yöntemi seçer
   ↓
2. Uygulama toplamı hesaplar
   ↓
3. Uygulama ödeme isteği gönderir
   ↓
4. API ödeme tutarını doğrular
   ↓
5. API ödemeyi işler (cüzdan/kart)
   ↓
6. API sipariş oluşturur
   ↓
7. API onay döner
```

---

## Bildirim Sistemi

### Mimari

**Android:**
- WorkManager (15 dakikalık aralıklar)
- Firebase Cloud Messaging (push bildirimleri)
- Yerel bildirimler

**Flutter:**
- Uygulama açıldığında polling
- Yerel bildirimler
- Gelecek: Push bildirimleri desteği

### Bildirim Türleri

- Sipariş güncellemeleri
- Ödeme onayları
- Promosyon kodu uyarıları
- Destek mesajları
- Admin duyuruları

---

## Dağıtım Mimarisi

### Geliştirme

```
Yerel Geliştirme:
  - API: localhost:8080
  - Veritabanı: localhost:3306
  - Uygulamalar: Geliştirme derlemeleri
```

### Production

```
Production Sunucusu:
  - API: Production domain (port 8080)
  - Veritabanı: Production MySQL sunucusu
  - Uygulamalar: Release derlemeleri (Play Store/App Store)
```

### Önerilen Kurulum

- **API Sunucusu:** Ubuntu 22.04, systemd servisi
- **Veritabanı:** Yedeklemelerle MySQL 8.0+
- **Reverse Proxy:** Nginx (HTTPS için)
- **SSL Sertifikası:** Let's Encrypt
- **İzleme:** Log izleme, hata takibi

---

## Ölçeklenebilirlik Düşünceleri

### Veritabanı

- İndeks optimizasyonu
- Sorgu optimizasyonu
- Bağlantı havuzlama
- Okuma replikaları (gerekirse)

### API

- Durumsuz tasarım (JWT)
- Yatay ölçekleme mümkün
- Yük dengeleme hazır
- Uygun yerlerde önbellekleme

### Mobil Uygulamalar

- Verimli API çağrıları
- Yerel önbellekleme
- Arka plan senkronizasyonu
- Çevrimdışı destek

---

## Teknoloji Yığını Özeti

| Bileşen | Teknoloji |
|---------|-----------|
| **Backend** | Go 1.24, Gin, MySQL |
| **Android** | Kotlin, Jetpack Compose, MVVM |
| **Flutter** | Dart, Flutter, Provider |
| **Web** | PHP 7.4+, Bootstrap 5.3, JavaScript |
| **Auth** | JWT, 2FA (Admin Paneli) |
| **Ödeme** | Stripe |
| **Depolama (Android)** | DataStore |
| **Depolama (Flutter)** | Secure Storage, Shared Preferences |
| **Bildirimler** | WorkManager, FCM (Android) |

---

## Gelecek İyileştirmeler

### Potansiyel İyileştirmeler

- [ ] WebSockets ile gerçek zamanlı chat
- [ ] Flutter için push bildirimleri
- [ ] Gelişmiş önbellekleme stratejileri
- [ ] GraphQL API seçeneği
- [ ] Mikroservis mimarisi
- [ ] Docker konteynerleştirme
- [ ] Kubernetes orkestrasyonu
- [ ] Gelişmiş analitik

---

## Destek & Dokümantasyon

Detaylı dokümantasyon için:
- [API Kurulum Kılavuzu](./api-setup-TR.md)
- [Android Uygulama Kurulumu](./android-app-setup-TR.md)
- [Flutter Uygulama Kurulumu](./flutter-app-setup-TR.md)

---

## Lisans

Bu proje CC BY-NC-ND 4.0 Lisansı altında korunmaktadır.
© 2025 Buğra Akdemir. Tüm Hakları Saklıdır.

