# HizmetSepetim Android App - Setup Guide

## Overview

The HizmetSepetim Android application is a native Android app built with Kotlin and Jetpack Compose. It features a modern Material 3 design, MVVM architecture, and real-time notifications.

**Technology Stack:**
- Kotlin 2.2.21
- Jetpack Compose (Material 3)
- MVVM Architecture
- Retrofit for API calls
- DataStore for local storage
- WorkManager for background tasks
- Firebase Cloud Messaging for notifications

---

## Prerequisites

Before setting up the Android app, ensure you have:

- **Android Studio Hedgehog (2023.1.1) or later**
- **JDK 11 or higher**
- **Android SDK** (API Level 24+)
- **Git** - For cloning the repository
- **Android device or emulator** (API 24+)

---

## Installation Steps

### 1. Clone the Repository

```bash
git clone <android-repository-url> hizmetSepetiApp
cd hizmetSepetiApp
```

### 2. Open in Android Studio

1. Launch Android Studio
2. Select **File → Open**
3. Navigate to the `hizmetSepetiApp` folder
4. Click **OK**

Android Studio will automatically sync Gradle files and download dependencies.

### 3. Configure API Base URL

Edit the API base URL in your Retrofit client configuration:

**File:** `app/src/main/java/com/akdbt/hizmetsepetim/appData/RetrofitClient.kt`

```kotlin
private const val BASE_URL = "http://92.249.61.58:8080/"
```

Update this to match your API server URL.

### 4. Configure Firebase (Optional)

If you're using Firebase Cloud Messaging:

1. Download `google-services.json` from Firebase Console
2. Place it in `app/` directory
3. The plugin is already configured in `build.gradle.kts`

### 5. Build the Project

1. Click **Build → Make Project** (or press `Ctrl+F9` / `Cmd+F9`)
2. Wait for Gradle sync to complete
3. Resolve any dependency issues if they occur

### 6. Run the App

1. Connect an Android device via USB (with USB debugging enabled) or start an emulator
2. Click **Run → Run 'app'** (or press `Shift+F10` / `Ctrl+R`)
3. Select your device/emulator
4. The app will install and launch

---

## Project Structure

```
hizmetSepetiApp/
├── app/
│   ├── build.gradle.kts          # App-level Gradle config
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/com/akdbt/hizmetsepetim/
│   │   │   │   ├── MainActivity.kt           # Main entry point
│   │   │   │   ├── HizmetSepetimApp.kt      # Application class
│   │   │   │   ├── appData/                 # Data layer
│   │   │   │   │   ├── RetrofitClient.kt    # API client setup
│   │   │   │   │   ├── ApiService.kt        # API interface
│   │   │   │   │   └── Models.kt            # Data models
│   │   │   │   ├── gui/                     # UI screens
│   │   │   │   │   ├── HomeScreen.kt
│   │   │   │   │   ├── LoginScreen.kt
│   │   │   │   │   ├── SignUpScreen.kt
│   │   │   │   │   └── ...
│   │   │   │   ├── ui/                      # UI components
│   │   │   │   │   ├── theme/               # Theme configuration
│   │   │   │   │   └── ...
│   │   │   │   ├── utils/                   # Utilities
│   │   │   │   │   ├── DataStoreManager.kt  # Local storage
│   │   │   │   │   └── NetworkObserver.kt   # Network monitoring
│   │   │   │   └── workers/                # Background workers
│   │   │   │       └── NotificationWorker.kt
│   │   │   └── res/                         # Resources
│   │   │       ├── values/
│   │   │       └── ...
│   │   └── test/                            # Unit tests
│   └── google-services.json                 # Firebase config (if used)
├── gradle/
│   └── libs.versions.toml                   # Dependency versions
├── build.gradle.kts                         # Project-level Gradle config
└── settings.gradle.kts                      # Project settings
```

---

## Key Components

### 1. MainActivity

The main entry point that sets up navigation and initializes the app.

**Location:** `app/src/main/java/com/akdbt/hizmetsepetim/MainActivity.kt`

### 2. RetrofitClient

Handles API communication with JWT token management.

**Location:** `app/src/main/java/com/akdbt/hizmetsepetim/appData/RetrofitClient.kt`

### 3. DataStoreManager

Manages local data storage (user info, tokens).

**Location:** `app/src/main/java/com/akdbt/hizmetsepetim/utils/DataStoreManager.kt`

### 4. NotificationWorker

Background worker that fetches notifications every 15 minutes.

**Location:** `app/src/main/java/com/akdbt/hizmetsepetim/workers/NotificationWorker.kt`

---

## Configuration

### API Configuration

Update the base URL in `RetrofitClient.kt`:

```kotlin
private const val BASE_URL = "http://YOUR_API_SERVER:8080/"
```

### Build Configuration

**Version:** 4.0.0
**Version Code:** 400
**Min SDK:** 24 (Android 7.0)
**Target SDK:** 36 (Android 14+)
**Compile SDK:** 36

### Dependencies

Key dependencies are defined in `gradle/libs.versions.toml`:

- Jetpack Compose BOM: 2025.10.01
- Kotlin: 2.2.21
- Retrofit: 2.11.0
- Material 3
- Navigation Compose
- WorkManager
- Firebase Messaging

---

## Building for Release

### 1. Generate Signed APK

1. **Build → Generate Signed Bundle / APK**
2. Select **APK**
3. Create or select a keystore
4. Fill in keystore information
5. Select **release** build variant
6. Click **Finish**

### 2. Build Configuration

The release build is configured in `app/build.gradle.kts`:

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

### 3. ProGuard Rules

If you enable minification, add ProGuard rules in `app/proguard-rules.pro`:

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

## Features

### Implemented Features

- ✅ User Authentication (Login/Register)
- ✅ Service Categories & Products
- ✅ Appointment Booking
- ✅ Address Management
- ✅ Order History
- ✅ Wallet System
- ✅ Promo Code Redemption
- ✅ Profile Management
- ✅ Support Chat
- ✅ Real-time Notifications
- ✅ Offline Support (DataStore)

### UI Features

- Material 3 Design System
- Dark/Light Theme Support
- Smooth Animations
- Responsive Layout
- Error Handling UI
- Loading States
- Network Status Indicator

---

## Architecture

### MVVM Pattern

```
View (Compose UI)
    ↓
ViewModel (State Management)
    ↓
Repository / API Service
    ↓
API / Local Storage
```

### State Management

- **Local State:** `remember` and `mutableStateOf`
- **Global State:** ViewModel with LiveData/StateFlow
- **Persistence:** DataStore for user data and tokens

### Navigation

Uses Jetpack Navigation Compose:

```kotlin
NavHost(navController, startDestination = "home") {
    composable("home") { HomeScreen(navController) }
    composable("login") { LoginScreen(navController) }
    // ... more routes
}
```

---

## Testing

### Unit Tests

Run unit tests:

```bash
./gradlew test
```

### Instrumented Tests

Run on device/emulator:

```bash
./gradlew connectedAndroidTest
```

### Manual Testing Checklist

- [ ] Login/Register flow
- [ ] Category and product browsing
- [ ] Order creation
- [ ] Address management
- [ ] Wallet operations
- [ ] Notification reception
- [ ] Offline functionality

---

## Troubleshooting

### Build Errors

**Gradle Sync Failed:**
- Check internet connection
- Invalidate caches: **File → Invalidate Caches**
- Clean project: **Build → Clean Project**
- Rebuild: **Build → Rebuild Project**

**Dependency Issues:**
- Update Gradle wrapper: `./gradlew wrapper --gradle-version=8.5`
- Check `gradle/libs.versions.toml` for version conflicts

### Runtime Errors

**App Crashes on Launch:**
- Check API base URL is correct
- Verify network permissions in `AndroidManifest.xml`
- Check Logcat for error messages

**Network Errors:**
- Verify API server is running
- Check network security config for HTTP (if not using HTTPS)
- Verify CORS settings on API server

### Emulator Issues

**Slow Performance:**
- Enable hardware acceleration
- Use x86/x86_64 system images
- Allocate more RAM to emulator

---

## Network Configuration

### HTTP Support

For development with HTTP (non-HTTPS), the app includes network security config:

**File:** `app/src/main/res/xml/network_security_config.xml`

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

**Note:** Use HTTPS in production!

---

## Permissions

Required permissions in `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
```

---

## Background Tasks

### Notification Worker

The app uses WorkManager to fetch notifications every 15 minutes:

- **Interval:** 15 minutes
- **Constraints:** Network required
- **Location:** `workers/NotificationWorker.kt`

### Real-time Loop

Additional 20-second loop for instant updates (when app is active).

---

## Security Best Practices

1. **Never commit API keys** - Use BuildConfig or local.properties
2. **Use HTTPS in production** - Update base URL to HTTPS
3. **Secure token storage** - Tokens stored in DataStore (encrypted)
4. **Validate all inputs** - Sanitize user data
5. **ProGuard/R8** - Enable code obfuscation for release builds

---

## Support

For issues or questions:
- Check the main documentation: `/docs` folder
- Review API documentation: `api-setup.md`
- Contact the development team

---

## License

This project is protected under CC BY-NC-ND 4.0 License.
© 2025 Buğra Akdemir. All Rights Reserved.

