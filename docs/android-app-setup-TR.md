# HizmetSepetim Android Uygulaması - Kurulum Kılavuzu

## Genel Bakış

HizmetSepetim Android uygulaması, Kotlin ve Jetpack Compose ile geliştirilmiş native bir Android uygulamasıdır. Modern Material 3 tasarımı, MVVM mimarisi ve gerçek zamanlı bildirimler içerir.

**Teknoloji Yığını:**
- Kotlin 2.2.21
- Jetpack Compose (Material 3)
- MVVM Mimarisi
- API çağrıları için Retrofit
- Yerel depolama için DataStore
- Arka plan görevleri için WorkManager
- Bildirimler için Firebase Cloud Messaging

---

## Gereksinimler

Android uygulamasını kurmadan önce aşağıdakilerin yüklü olduğundan emin olun:

- **Android Studio Hedgehog (2023.1.1) veya daha yeni**
- **JDK 11 veya daha yüksek**
- **Android SDK** (API Seviyesi 24+)
- **Git** - Repoyu klonlamak için
- **Android cihaz veya emülatör** (API 24+)

---

## Kurulum Adımları

### 1. Repoyu Klonlayın

```bash
git clone <android-repository-url> hizmetSepetiApp
cd hizmetSepetiApp
```

### 2. Android Studio'da Açın

1. Android Studio'yu başlatın
2. **File → Open** seçin
3. `hizmetSepetiApp` klasörüne gidin
4. **OK** tıklayın

Android Studio otomatik olarak Gradle dosyalarını senkronize edecek ve bağımlılıkları indirecektir.

### 3. API Base URL'ini Yapılandırın

Retrofit client yapılandırmanızda API base URL'ini düzenleyin:

**Dosya:** `app/src/main/java/com/akdbt/hizmetsepetim/appData/RetrofitClient.kt`

```kotlin
private const val BASE_URL = "http://92.249.61.58:8080/"
```

Bunu API sunucu URL'inize göre güncelleyin.

### 4. Firebase'i Yapılandırın (Opsiyonel)

Firebase Cloud Messaging kullanıyorsanız:

1. Firebase Console'dan `google-services.json` dosyasını indirin
2. `app/` dizinine yerleştirin
3. Plugin zaten `build.gradle.kts` dosyasında yapılandırılmıştır

### 5. Projeyi Derleyin

1. **Build → Make Project** tıklayın (veya `Ctrl+F9` / `Cmd+F9` tuşlarına basın)
2. Gradle senkronizasyonunun tamamlanmasını bekleyin
3. Herhangi bir bağımlılık sorunu varsa çözün

### 6. Uygulamayı Çalıştırın

1. USB ile bir Android cihaz bağlayın (USB hata ayıklama etkin) veya bir emülatör başlatın
2. **Run → Run 'app'** tıklayın (veya `Shift+F10` / `Ctrl+R` tuşlarına basın)
3. Cihazınızı/emülatörünüzü seçin
4. Uygulama yüklenecek ve başlatılacaktır

---

## Proje Yapısı

```
hizmetSepetiApp/
├── app/
│   ├── build.gradle.kts          # App seviyesi Gradle yapılandırması
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/akdbt/hizmetsepetim/
│   │   │   │   ├── MainActivity.kt           # Ana giriş noktası
│   │   │   │   ├── HizmetSepetimApp.kt      # Application sınıfı
│   │   │   │   ├── appData/                 # Veri katmanı
│   │   │   │   │   ├── RetrofitClient.kt    # API client kurulumu
│   │   │   │   │   ├── ApiService.kt        # API arayüzü
│   │   │   │   │   └── Models.kt            # Veri modelleri
│   │   │   │   ├── gui/                     # UI ekranları
│   │   │   │   │   ├── HomeScreen.kt
│   │   │   │   │   ├── LoginScreen.kt
│   │   │   │   │   ├── SignUpScreen.kt
│   │   │   │   │   └── ...
│   │   │   │   ├── ui/                      # UI bileşenleri
│   │   │   │   │   ├── theme/               # Tema yapılandırması
│   │   │   │   │   └── ...
│   │   │   │   ├── utils/                   # Yardımcılar
│   │   │   │   │   ├── DataStoreManager.kt  # Yerel depolama
│   │   │   │   │   └── NetworkObserver.kt   # Ağ izleme
│   │   │   │   └── workers/                # Arka plan worker'ları
│   │   │   │       └── NotificationWorker.kt
│   │   │   └── res/                         # Kaynaklar
│   │   │       ├── values/
│   │   │       └── ...
│   │   └── test/                            # Birim testleri
│   └── google-services.json                 # Firebase yapılandırması (kullanılıyorsa)
├── gradle/
│   └── libs.versions.toml                   # Bağımlılık sürümleri
├── build.gradle.kts                         # Proje seviyesi Gradle yapılandırması
└── settings.gradle.kts                      # Proje ayarları
```

---

## Ana Bileşenler

### 1. MainActivity

Navigasyonu kurup uygulamayı başlatan ana giriş noktası.

**Konum:** `app/src/main/java/com/akdbt/hizmetsepetim/MainActivity.kt`

### 2. RetrofitClient

JWT token yönetimi ile API iletişimini yönetir.

**Konum:** `app/src/main/java/com/akdbt/hizmetsepetim/appData/RetrofitClient.kt`

### 3. DataStoreManager

Yerel veri depolamayı yönetir (kullanıcı bilgileri, token'lar).

**Konum:** `app/src/main/java/com/akdbt/hizmetsepetim/utils/DataStoreManager.kt`

### 4. NotificationWorker

Her 15 dakikada bir bildirimleri getiren arka plan worker'ı.

**Konum:** `app/src/main/java/com/akdbt/hizmetsepetim/workers/NotificationWorker.kt`

---

## Yapılandırma

### API Yapılandırması

`RetrofitClient.kt` dosyasındaki base URL'i güncelleyin:

```kotlin
private const val BASE_URL = "http://API_SUNUCUNUZ:8080/"
```

### Derleme Yapılandırması

**Sürüm:** 4.0.0
**Sürüm Kodu:** 400
**Min SDK:** 24 (Android 7.0)
**Hedef SDK:** 36 (Android 14+)
**Derleme SDK:** 36

### Bağımlılıklar

Ana bağımlılıklar `gradle/libs.versions.toml` dosyasında tanımlanmıştır:

- Jetpack Compose BOM: 2025.10.01
- Kotlin: 2.2.21
- Retrofit: 2.11.0
- Material 3
- Navigation Compose
- WorkManager
- Firebase Messaging

---

## Release İçin Derleme

### 1. İmzalı APK Oluşturun

1. **Build → Generate Signed Bundle / APK**
2. **APK** seçin
3. Bir keystore oluşturun veya seçin
4. Keystore bilgilerini doldurun
5. **release** build variant'ını seçin
6. **Finish** tıklayın

### 2. Derleme Yapılandırması

Release derlemesi `app/build.gradle.kts` dosyasında yapılandırılmıştır:

```kotlin
buildTypes {
    release {
        isMinifyEnabled = false
        proguardFiles(
            getDefaultProguardFile("proguard-android-optimize.txt"),
            "proguard-rules.pro"
        )
    }
}
```

### 3. ProGuard Kuralları

Minification'ı etkinleştirirseniz, `app/proguard-rules.pro` dosyasına ProGuard kuralları ekleyin:

```proguard
# Retrofit
-keepattributes Signature, InnerClasses
-keepattributes *Annotation*
-keep class retrofit2.** { *; }
-keep class okhttp3.** { *; }
-keep class okio.** { *; }

# Gson
-keepattributes Signature
-keepattributes *Annotation*
-keep class sun.misc.Unsafe { *; }
-keep class com.google.gson.** { *; }
```

---

## Özellikler

### Uygulanan Özellikler

- ✅ Kullanıcı Kimlik Doğrulama (Giriş/Kayıt)
- ✅ Hizmet Kategorileri & Ürünler
- ✅ Randevu Sistemi
- ✅ Adres Yönetimi
- ✅ Sipariş Geçmişi
- ✅ Cüzdan Sistemi
- ✅ Promosyon Kodu Kullanımı
- ✅ Profil Yönetimi
- ✅ Destek Chat
- ✅ Gerçek Zamanlı Bildirimler
- ✅ Çevrimdışı Destek (DataStore)

### UI Özellikleri

- Material 3 Tasarım Sistemi
- Koyu/Açık Tema Desteği
- Yumuşak Animasyonlar
- Duyarlı Düzen
- Hata Yönetimi UI
- Yükleme Durumları
- Ağ Durumu Göstergesi

---

## Mimari

### MVVM Deseni

```
View (Compose UI)
    ↓
ViewModel (Durum Yönetimi)
    ↓
Repository / API Service
    ↓
API / Yerel Depolama
```

### Durum Yönetimi

- **Yerel Durum:** `remember` ve `mutableStateOf`
- **Global Durum:** LiveData/StateFlow ile ViewModel
- **Kalıcılık:** Kullanıcı verileri ve token'lar için DataStore

### Navigasyon

Jetpack Navigation Compose kullanır:

```kotlin
NavHost(navController, startDestination = "home") {
    composable("home") { HomeScreen(navController) }
    composable("login") { LoginScreen(navController) }
    // ... daha fazla route
}
```

---

## Test Etme

### Birim Testleri

Birim testlerini çalıştırın:

```bash
./gradlew test
```

### Enstrümantasyon Testleri

Cihaz/emülatörde çalıştırın:

```bash
./gradlew connectedAndroidTest
```

### Manuel Test Kontrol Listesi

- [ ] Giriş/Kayıt akışı
- [ ] Kategori ve ürün tarama
- [ ] Sipariş oluşturma
- [ ] Adres yönetimi
- [ ] Cüzdan işlemleri
- [ ] Bildirim alma
- [ ] Çevrimdışı işlevsellik

---

## Sorun Giderme

### Derleme Hataları

**Gradle Senkronizasyonu Başarısız:**
- İnternet bağlantısını kontrol edin
- Önbellekleri temizleyin: **File → Invalidate Caches**
- Projeyi temizleyin: **Build → Clean Project**
- Yeniden derleyin: **Build → Rebuild Project**

**Bağımlılık Sorunları:**
- Gradle wrapper'ı güncelleyin: `./gradlew wrapper --gradle-version=8.5`
- Sürüm çakışmaları için `gradle/libs.versions.toml` dosyasını kontrol edin

### Çalışma Zamanı Hataları

**Uygulama Başlatıldığında Çöküyor:**
- API base URL'inin doğru olduğunu kontrol edin
- `AndroidManifest.xml` dosyasındaki ağ izinlerini doğrulayın
- Hata mesajları için Logcat'i kontrol edin

**Ağ Hataları:**
- API sunucusunun çalıştığını doğrulayın
- HTTP için ağ güvenlik yapılandırmasını kontrol edin (HTTPS kullanmıyorsanız)
- API sunucusundaki CORS ayarlarını doğrulayın

### Emülatör Sorunları

**Yavaş Performans:**
- Donanım hızlandırmayı etkinleştirin
- x86/x86_64 sistem görüntülerini kullanın
- Emülatöre daha fazla RAM ayırın

---

## Ağ Yapılandırması

### HTTP Desteği

HTTP (HTTPS olmayan) ile geliştirme için, uygulama ağ güvenlik yapılandırması içerir:

**Dosya:** `app/src/main/res/xml/network_security_config.xml`

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" />
        </trust-anchors>
    </base-config>
</network-security-config>
```

**Not:** Production'da HTTPS kullanın!

---

## İzinler

`AndroidManifest.xml` dosyasında gerekli izinler:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```

---

## Arka Plan Görevleri

### Bildirim Worker'ı

Uygulama, her 15 dakikada bir bildirimleri getirmek için WorkManager kullanır:

- **Aralık:** 15 dakika
- **Kısıtlamalar:** Ağ gerekli
- **Konum:** `workers/NotificationWorker.kt`

### Gerçek Zamanlı Döngü

Anında güncellemeler için ek 20 saniyelik döngü (uygulama aktifken).

---

## Güvenlik En İyi Uygulamaları

1. **Asla API anahtarlarını commit etmeyin** - BuildConfig veya local.properties kullanın
2. **Production'da HTTPS kullanın** - Base URL'i HTTPS'e güncelleyin
3. **Güvenli token depolama** - Token'lar DataStore'da saklanır (şifrelenmiş)
4. **Tüm girdileri doğrulayın** - Kullanıcı verilerini temizleyin
5. **ProGuard/R8** - Release derlemeleri için kod karıştırmayı etkinleştirin

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

