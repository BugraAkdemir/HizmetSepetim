<p align="center">
  <img src="./screenshots/logo.png" width="160" alt="logo">
</p>

<h1 align="center">HizmetSepetim</h1>
<p align="center">
  Modern Hizmet Marketplace Platformu • Final Sürüm 4.0.0
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-4.0.0-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/status-Final-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/platform-Android%20%7C%20Flutter-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/backend-Go%20Lang-00ADD8?style=for-the-badge">
</p>

---

## 📖 Bu Repo Hakkında

Bu, HizmetSepetim projesinin **dokümantasyon deposu**dur. Bu repo, HizmetSepetim platformunun tüm bileşenleri için kapsamlı dokümantasyon, sürüm geçmişi ve teknik kılavuzlar içerir.

> ⚠️ **Not:** Bu sadece dokümantasyon deposudur. Kaynak kodlar ayrı depolarda tutulmaktadır.

---

## 🏗️ Proje Yapısı

HizmetSepetim platformu dört ana bileşenden oluşur:

### 1. **API Backend** (`/hizmetSepetiApi`)
- **Teknoloji:** Go (Golang)
- **Veritabanı:** MySQL/MariaDB
- **Özellikler:** RESTful API, JWT Kimlik Doğrulama, Ödeme Entegrasyonu (Stripe), Cüzdan Sistemi, Destek Chat
- **Dokümantasyon:** [API Dokümantasyonu](./docs/api-setup-TR.md)

### 2. **Android Native Uygulama** (`/hizmetSepetiApp`)
- **Teknoloji:** Kotlin, Jetpack Compose
- **Mimari:** MVVM
- **Özellikler:** Material 3 Tasarım, Modern UI, Gerçek Zamanlı Bildirimler, Çevrimdışı Destek
- **Dokümantasyon:** [Android Uygulama Dokümantasyonu](./docs/android-app-setup-TR.md)

### 3. **Flutter Uygulaması** (`/hizmetsepetimapp_flutter`)
- **Teknoloji:** Flutter, Dart
- **Mimari:** Provider Pattern
- **Özellikler:** Çapraz Platform Desteği, Material Design, Güvenli Depolama
- **Dokümantasyon:** [Flutter Uygulama Dokümantasyonu](./docs/flutter-app-setup-TR.md)

### 4. **Web Uygulaması** (`/HizmetSepetimWeb`)
- **Teknoloji:** PHP, Bootstrap, JavaScript
- **Özellikler:** Public Website, Admin Paneli, Satıcı Paneli, 2FA Güvenliği
- **Dokümantasyon:** [Web Uygulama Dokümantasyonu](./docs/web-setup-TR.md)

---

## 📚 Dokümantasyon

### Hızlı Başlangıç Kılavuzları
- [API Kurulum Kılavuzu](./docs/api-setup-TR.md) - Go API'yi nasıl kurup çalıştırılır
- [Android Uygulama Kurulum Kılavuzu](./docs/android-app-setup-TR.md) - Android uygulamasını nasıl derlenir ve çalıştırılır
- [Flutter Uygulama Kurulum Kılavuzu](./docs/flutter-app-setup-TR.md) - Flutter uygulamasını nasıl derlenir ve çalıştırılır
- [Web Uygulama Kurulum Kılavuzu](./docs/web-setup-TR.md) - Web uygulamasını nasıl kurup dağıtılır

### Mimari & Tasarım
- [Mimari Genel Bakış](./docs/architecture-TR.md) - Sistem mimarisi ve tasarım desenleri
- [API Endpoint Referansı](./docs/api-endpoints-TR.md) - Tam API endpoint dokümantasyonu
- [Veritabanı Şeması](./docs/database-schema-TR.md) - Veritabanı yapısı ve ilişkileri

### Sürüm Geçmişi
- [Sürüm 4.0.0](./docs/v4.0.md) - En son sürüm notları
- [Sürüm 2.0.0](./docs/v2.0.0.md) - Önceki büyük sürüm
- [Sürüm 1.5.0](./docs/v1.5.0.md) - Özellik güncellemeleri
- [Sürüm 1.0.0](./docs/v1.0.0.md) - İlk sürüm

---

## 🚀 Hızlı Başlangıç

### Gereksinimler
- **API için:** Go 1.24+, MySQL 8.0+
- **Android Uygulama için:** Android Studio, JDK 11+, Android SDK 24+
- **Flutter Uygulama için:** Flutter SDK 3.10.4+, Dart 3.10.4+
- **Web Uygulama için:** PHP 7.4+, Apache/Nginx, MySQL 8.0+

### Başlangıç
1. İlgili repoları klonlayın:
   ```bash
   # API Reposu
   git clone <api-repo-url> hizmetSepetiApi

   # Android Uygulama Reposu
   git clone <android-repo-url> hizmetSepetiApp

   # Flutter Uygulama Reposu
   git clone <flutter-repo-url> hizmetsepetimapp_flutter

   # Web Uygulama Reposu
   git clone <web-repo-url> HizmetSepetimWeb
   ```

2. Her bileşen için `/docs` klasöründeki kurulum kılavuzlarını takip edin.

---

## 🛠️ Teknoloji Yığını

| Bileşen | Teknoloji |
|---------|-----------|
| **Backend API** | Go 1.24, Gin Framework, MySQL, JWT, Stripe |
| **Android Uygulama** | Kotlin, Jetpack Compose, Material 3, Retrofit, MVVM |
| **Flutter Uygulama** | Flutter 3.10.4, Dart, Dio, Provider, Secure Storage |
| **Web Uygulama** | PHP 7.4+, Bootstrap 5.3, JavaScript, MySQL |
| **Veritabanı** | MySQL 8.0 / MariaDB |
| **Kimlik Doğrulama** | JWT (JSON Web Tokens), 2FA (Admin Paneli) |
| **Ödeme** | Stripe Entegrasyonu |
| **Bildirimler** | Firebase Cloud Messaging (Android) |

---

## 📱 Özellikler

### Temel Özellikler
- ✅ Kullanıcı Kimlik Doğrulama & Kayıt
- ✅ Hizmet Kategorileri & Ürünler
- ✅ Randevu Sistemi
- ✅ Adres Yönetimi
- ✅ Sipariş Yönetimi
- ✅ Ödeme Entegrasyonu (Cüzdan & Kart)
- ✅ Promosyon Kodu Sistemi
- ✅ Destek Chat Sistemi
- ✅ Gerçek Zamanlı Bildirimler
- ✅ Satıcı Paneli
- ✅ Admin Paneli

### Platforma Özel Özellikler
- **Android:** Material 3 Tasarım, Arka Plan Görevleri için WorkManager
- **Flutter:** Çapraz Platform Desteği, Güvenli Token Depolama

---

## 📄 Lisans

Bu proje **CC BY-NC-ND 4.0 Lisansı** altında korunmaktadır.
© 2025 Buğra Akdemir. Tüm Hakları Saklıdır.

---

## 👨‍💻 Geliştirici

**Buğra Akdemir**
- Full-Stack Developer
- Teknolojiler: Kotlin, Go, Flutter, PHP, MySQL
- HizmetSepetim Kurucusu & Geliştiricisi

---

## 📞 Destek

Teknik sorular veya sorunlar için lütfen `/docs` klasöründeki dokümantasyona bakın veya geliştirme ekibiyle iletişime geçin.

---

<p align="center">
  <b>HizmetSepetim</b> • Modern Hizmet Marketplace Platformu • Sürüm 4.0.0 Final
</p>

