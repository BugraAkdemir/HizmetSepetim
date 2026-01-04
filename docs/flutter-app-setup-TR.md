# HizmetSepetim Flutter Uygulaması - Kurulum Kılavuzu

## Genel Bakış

HizmetSepetim Flutter uygulaması, Flutter ve Dart ile geliştirilmiş çapraz platform bir mobil uygulamadır. Android uygulamasıyla aynı işlevselliği sağlar ancak hem Android hem de iOS platformlarını destekler.

**Teknoloji Yığını:**
- Flutter SDK 3.10.4+
- Dart 3.10.4+
- HTTP istekleri için Dio
- Durum yönetimi için Provider
- Güvenli token depolama için flutter_secure_storage
- Yerel depolama için shared_preferences

---

## Gereksinimler

Flutter uygulamasını kurmadan önce aşağıdakilerin yüklü olduğundan emin olun:

- **Flutter SDK 3.10.4 veya daha yeni** - [Flutter Kurulum](https://flutter.dev/docs/get-started/install)
- **Dart SDK 3.10.4+** (Flutter ile birlikte gelir)
- **Android Studio** veya **VS Code** (Flutter eklentileri ile)
- **Android SDK** (Android geliştirme için)
- **Xcode** (iOS geliştirme için, sadece macOS)
- **Git** - Repoyu klonlamak için

---

## Kurulum Adımları

### 1. Repoyu Klonlayın

```bash
git clone <flutter-repository-url> hizmetsepetimapp_flutter
cd hizmetsepetimapp_flutter
```

### 2. Bağımlılıkları Yükleyin

```bash
flutter pub get
```

Bu komut `pubspec.yaml` dosyasında tanımlı tüm gerekli paketleri indirecektir.

### 3. API Base URL'ini Yapılandırın

Servis dosyanızdaki API base URL'ini düzenleyin:

**Dosya:** `lib/appData/api_service.dart`

```dart
const String baseUrl = "http://92.249.61.58:8080/";
```

Bunu API sunucu URL'inize göre güncelleyin.

### 4. Uygulamayı Çalıştırın

#### Android için:

```bash
flutter run
```

Veya listeden bir Android cihaz/emülatör seçin.

#### iOS için (sadece macOS):

```bash
flutter run -d ios
```

Xcode'un yüklü olduğundan ve iOS simülatörünün çalıştığından emin olun.

### 5. Release İçin Derleyin

#### Android APK:

```bash
flutter build apk --release
```

APK `build/app/outputs/flutter-apk/app-release.apk` konumunda oluşturulacaktır.

#### Android App Bundle:

```bash
flutter build appbundle --release
```

Google Play Store dağıtımı için.

#### iOS (sadece macOS):

```bash
flutter build ios --release
```

---

## Proje Yapısı

```
hizmetsepetimapp_flutter/
├── lib/
│   ├── main.dart                    # Uygulama giriş noktası
│   ├── appData/
│   │   └── api_service.dart         # API servisi & modeller
│   ├── gui/
│   │   ├── main_layout.dart         # Ana layout wrapper
│   │   ├── home_screen.dart         # Ana ekran
│   │   ├── login_screen.dart        # Giriş ekranı
│   │   ├── signup_screen.dart       # Kayıt ekranı
│   │   └── ...                      # Diğer ekranlar
│   ├── utils/
│   │   ├── auth_state.dart          # Global kimlik doğrulama durumu
│   │   ├── token_store.dart         # JWT token yönetimi
│   │   └── user_store.dart          # Kullanıcı veri yönetimi
│   └── theme/
│       └── colors.dart              # Tema renkleri
├── assets/                          # Görseller, fontlar, vb.
│   └── icon/
│       └── app_icon.png
├── pubspec.yaml                     # Bağımlılıklar & yapılandırma
└── README.md                        # Proje dokümantasyonu
```

---

## Ana Bileşenler

### 1. main.dart

Uygulamayı başlatan ve mevcut kimlik doğrulamayı kontrol eden giriş noktası.

**Konum:** `lib/main.dart`

### 2. api_service.dart

Otomatik JWT token enjeksiyonu ile tüm API iletişimini yönetir.

**Konum:** `lib/appData/api_service.dart`

### 3. TokenStore

`flutter_secure_storage` kullanarak JWT token'ların güvenli depolamasını yönetir.

**Konum:** `lib/utils/token_store.dart`

### 4. UserStore

`shared_preferences` kullanarak kullanıcı oturum verilerini yönetir.

**Konum:** `lib/utils/user_store.dart`

---

## Yapılandırma

### API Yapılandırması

`lib/appData/api_service.dart` dosyasındaki base URL'i güncelleyin:

```dart
const String baseUrl = "http://API_SUNUCUNUZ:8080/";
```

### Uygulama Yapılandırması

**Sürüm:** 1.9.9
**Build Numarası:** 199
**Minimum SDK:** Android 21+, iOS 11.0+

### Bağımlılıklar

`pubspec.yaml` dosyasındaki ana bağımlılıklar:

```yaml
dependencies:
  flutter:
    sdk: flutter
  dio: ^5.9.0                    # HTTP client
  flutter_secure_storage: ^9.0.0  # Güvenli depolama
  shared_preferences: ^2.2.2      # Yerel depolama
  provider: ^6.0.5                # Durum yönetimi
  cupertino_icons: ^1.0.8         # iOS ikonları
```

---

## Durum Yönetimi

### Provider Deseni

Uygulama durum yönetimi için Provider kullanır:

- **authState:** Global kimlik doğrulama durumu (`ValueNotifier<bool>`)
- **userSession:** Mevcut kullanıcı oturumu (`ValueNotifier<UserSession?>`)

### Yerel Durum

Bireysel ekranlar yerel UI durumu için `StatefulWidget` ile `setState` kullanır.

### Kalıcılık

- **JWT Token'lar:** `flutter_secure_storage` ile güvenli şekilde saklanır
- **Kullanıcı Verileri:** `shared_preferences` ile saklanır

---

## API Entegrasyonu

### Base URL

Varsayılan: `http://92.249.61.58:8080/`

### Kimlik Doğrulama

Tüm kimlik doğrulamalı istekler header'da JWT token içerir:

```dart
options.headers["Authorization"] = "Bearer $token";
```

### Endpoint'ler

Uygulama aşağıdaki ana endpoint'leri kullanır:

- `GET /get_categories` - Kategori listesi
- `GET /get_products?category_id={id}` - Ürün listesi
- `GET /get_product_detail?id={id}` - Ürün detayı
- `POST /register` - Kullanıcı kaydı
- `POST /login` - Giriş
- `GET /get_addresses` - Adres listesi
- `POST /add_address` - Adres ekle
- `GET /get_addons` - Ek hizmetler
- `POST /create_order_with_payment` - Sipariş oluştur
- `GET /get_orders` - Sipariş/randevu listesi
- `GET /wallet/balance` - Cüzdan bakiyesi
- `GET /wallet/transactions` - İşlem geçmişi
- `POST /redeem_promo` - Promosyon kodu kullan
- `POST /update_profile` - Profili güncelle

---

## Tasarım Sistemi

### Renk Paleti

`lib/theme/colors.dart` dosyasında tanımlanmıştır:

- **Primary:** `#2A9D8F` (Teal)
- **Background:** `#F2F6F5` (Açık gri)
- **Text Dark:** `#0F172A`
- **Text Soft:** `#64748B`

### UI Özellikleri

- Material Design
- Gradient alt navigasyon çubuğu
- Kart tabanlı düzen (20px border-radius)
- Zarif gölgeler
- Yumuşak animasyonlar

---

## Release İçin Derleme

### Android

#### İmzalı APK Oluşturun

1. Bir keystore oluşturun (yoksa):

```bash
keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
```

2. `android/key.properties` dosyasını yapılandırın:

```properties
storePassword=<şifre>
keyPassword=<şifre>
keyAlias=upload
storeFile=<keystore-yolu>
```

3. `android/app/build.gradle` dosyasını keystore'u kullanacak şekilde güncelleyin.

4. Derleyin:

```bash
flutter build apk --release
```

#### App Bundle Oluşturun

```bash
flutter build appbundle --release
```

### iOS (sadece macOS)

1. Xcode'da `ios/Runner.xcworkspace` dosyasını açın
2. İmzalama ve yetenekleri yapılandırın
3. Derleyin:

```bash
flutter build ios --release
```

---

## Özellikler

### Uygulanan Özellikler

- ✅ Kullanıcı Kimlik Doğrulama (Giriş/Kayıt)
- ✅ Hizmet Kategorileri & Ürünler
- ✅ Ürün Detayları
- ✅ Randevu Sistemi
- ✅ Adres Yönetimi
- ✅ Sipariş Geçmişi
- ✅ Cüzdan Sistemi
- ✅ Promosyon Kodu Kullanımı
- ✅ Profil Yönetimi
- ✅ Ödeme Entegrasyonu (Cüzdan & Kart)

### Platform Desteği

| Platform | Durum |
|----------|-------|
| Android | ✅ Tam Destekleniyor |
| iOS | 🎯 Hedef Platform |

---

## Test Etme

### Testleri Çalıştırın

```bash
flutter test
```

### Cihazda Çalıştırın

```bash
flutter run
```

### Hot Reload

Uygulama çalışırken terminalde `r` tuşuna basın.

### Hot Restart

Büyük `R` tuşuna basın.

---

## Sorun Giderme

### Derleme Hataları

**"Cihaz bulunamadı":**
- Bir Android emülatör başlatın veya bir cihaz bağlayın
- `flutter devices` çalıştırarak mevcut cihazları görün

**"Gradle derleme başarısız":**
- Projeyi temizleyin: `flutter clean`
- Bağımlılıkları tekrar alın: `flutter pub get`
- Yeniden derleyin: `flutter build apk`

**iOS derleme hataları (sadece macOS):**
- Xcode'un yüklü ve güncel olduğundan emin olun
- `ios/` dizininde `pod install` çalıştırın
- Xcode'da workspace'i açın ve imzalamayı kontrol edin

### Çalışma Zamanı Hataları

**Ağ hataları:**
- API sunucusunun çalıştığını doğrulayın
- `api_service.dart` dosyasındaki base URL'i kontrol edin
- API sunucusundaki CORS ayarlarını doğrulayın

**Token depolama hataları:**
- Platforma özel depolama izinlerini kontrol edin
- Android için, `minSdkVersion`'ın 18+ olduğundan emin olun

---

## Platforma Özel Yapılandırma

### Android

**Minimum SDK:** 21 (Android 5.0)
**Hedef SDK:** 33+

**İzinler** (`android/app/src/main/AndroidManifest.xml` dosyasında):

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### iOS

**Minimum Sürüm:** 11.0
**Hedef Sürüm:** 14.0+

**Info.plist** yapılandırması ağ istekleri için gerekli olabilir.

---

## Güvenlik En İyi Uygulamaları

1. **Asla secret'ları commit etmeyin** - Environment variable'lar veya config dosyaları kullanın
2. **Production'da HTTPS kullanın** - Base URL'i HTTPS'e güncelleyin
3. **Güvenli token depolama** - Token'lar `flutter_secure_storage` ile saklanır
4. **Tüm girdileri doğrulayın** - Kullanıcı verilerini temizleyin
5. **Kod karıştırma** - Release derlemeleri için etkinleştirin:

```bash
flutter build apk --release --obfuscate --split-debug-info=./debug-info
```

---

## Geliştirme İş Akışı

### 1. Değişiklik Yapın

`lib/` dizinindeki dosyaları düzenleyin.

### 2. Hot Reload

Dosyaları kaydedin ve uygulama otomatik olarak yeniden yüklenecektir (veya `r` tuşuna basın).

### 3. Test Edin

Mümkünse hem Android hem de iOS'ta test edin.

### 4. Release Derleyin

Dağıtıma hazır olduğunuzda release sürümünü derleyin.

---

## Destek

Sorunlar veya sorular için:
- Ana dokümantasyona bakın: `/docs` klasörü
- API dokümantasyonunu inceleyin: `api-setup.md`
- Geliştirme ekibiyle iletişime geçin

---

## Lisans

Bu proje CC BY-NC-ND 4.0 Lisansı altında korunmaktadır.
© 2025 Buğra Akdemir. Tüm Hakları Saklıdır.

