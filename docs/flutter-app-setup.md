# HizmetSepetim Flutter App - Setup Guide

## Overview

The HizmetSepetim Flutter application is a cross-platform mobile app built with Flutter and Dart. It provides the same functionality as the Android app but supports both Android and iOS platforms.

**Technology Stack:**
- Flutter SDK 3.10.4+
- Dart 3.10.4+
- Dio for HTTP requests
- Provider for state management
- flutter_secure_storage for secure token storage
- shared_preferences for local storage

---

## Prerequisites

Before setting up the Flutter app, ensure you have:

- **Flutter SDK 3.10.4 or later** - [Install Flutter](https://flutter.dev/docs/get-started/install)
- **Dart SDK 3.10.4+** (included with Flutter)
- **Android Studio** or **VS Code** with Flutter extensions
- **Android SDK** (for Android development)
- **Xcode** (for iOS development, macOS only)
- **Git** - For cloning the repository

---

## Installation Steps

### 1. Clone the Repository

```bash
git clone <flutter-repository-url> hizmetsepetimapp_flutter
cd hizmetsepetimapp_flutter
```

### 2. Install Dependencies

```bash
flutter pub get
```

This will download all required packages defined in `pubspec.yaml`.

### 3. Configure API Base URL

Edit the API base URL in your service file:

**File:** `lib/appData/api_service.dart`

```dart
const String baseUrl = "http://92.249.61.58:8080/";
```

Update this to match your API server URL.

### 4. Run the App

#### For Android:

```bash
flutter run
```

Or select an Android device/emulator from the list.

#### For iOS (macOS only):

```bash
flutter run -d ios
```

Ensure you have Xcode installed and iOS simulator running.

### 5. Build for Release

#### Android APK:

```bash
flutter build apk --release
```

The APK will be generated in `build/app/outputs/flutter-apk/app-release.apk`

#### Android App Bundle:

```bash
flutter build appbundle --release
```

For Google Play Store distribution.

#### iOS (macOS only):

```bash
flutter build ios --release
```

---

## Project Structure

```
hizmetsepetimapp_flutter/
├── lib/
│   ├── main.dart                    # App entry point
│   ├── appData/
│   │   └── api_service.dart         # API service & models
│   ├── gui/
│   │   ├── main_layout.dart         # Main layout wrapper
│   │   ├── home_screen.dart         # Home screen
│   │   ├── login_screen.dart        # Login screen
│   │   ├── signup_screen.dart       # Signup screen
│   │   └── ...                      # Other screens
│   ├── utils/
│   │   ├── auth_state.dart          # Global auth state
│   │   ├── token_store.dart         # JWT token management
│   │   └── user_store.dart          # User data management
│   └── theme/
│       └── colors.dart              # Theme colors
├── assets/                          # Images, fonts, etc.
│   └── icon/
│       └── app_icon.png
├── pubspec.yaml                     # Dependencies & config
└── README.md                        # Project documentation
```

---

## Key Components

### 1. main.dart

The entry point that initializes the app and checks for existing authentication.

**Location:** `lib/main.dart`

### 2. api_service.dart

Handles all API communication with automatic JWT token injection.

**Location:** `lib/appData/api_service.dart`

### 3. TokenStore

Manages secure storage of JWT tokens using `flutter_secure_storage`.

**Location:** `lib/utils/token_store.dart`

### 4. UserStore

Manages user session data using `shared_preferences`.

**Location:** `lib/utils/user_store.dart`

---

## Configuration

### API Configuration

Update the base URL in `lib/appData/api_service.dart`:

```dart
const String baseUrl = "http://YOUR_API_SERVER:8080/";
```

### App Configuration

**Version:** 1.9.9
**Build Number:** 199
**Minimum SDK:** Android 21+, iOS 11.0+

### Dependencies

Key dependencies in `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  dio: ^5.9.0                    # HTTP client
  flutter_secure_storage: ^9.0.0  # Secure storage
  shared_preferences: ^2.2.2      # Local storage
  provider: ^6.0.5                # State management
  cupertino_icons: ^1.0.8         # iOS icons
```

---

## State Management

### Provider Pattern

The app uses Provider for state management:

- **authState:** Global authentication state (`ValueNotifier<bool>`)
- **userSession:** Current user session (`ValueNotifier<UserSession?>`)

### Local State

Individual screens use `StatefulWidget` with `setState` for local UI state.

### Persistence

- **JWT Tokens:** Stored securely with `flutter_secure_storage`
- **User Data:** Stored with `shared_preferences`

---

## API Integration

### Base URL

Default: `http://92.249.61.58:8080/`

### Authentication

All authenticated requests include JWT token in header:

```dart
options.headers["Authorization"] = "Bearer $token";
```

### Endpoints

The app uses the following main endpoints:

- `GET /get_categories` - Categories list
- `GET /get_products?category_id={id}` - Products list
- `GET /get_product_detail?id={id}` - Product detail
- `POST /register` - User registration
- `POST /login` - Login
- `GET /get_addresses` - Addresses list
- `POST /add_address` - Add address
- `GET /get_addons` - Additional services
- `POST /create_order_with_payment` - Create order
- `GET /get_orders` - Orders/appointments list
- `GET /wallet/balance` - Wallet balance
- `GET /wallet/transactions` - Transaction history
- `POST /redeem_promo` - Redeem promo code
- `POST /update_profile` - Update profile

---

## Design System

### Color Palette

Defined in `lib/theme/colors.dart`:

- **Primary:** `#2A9D8F` (Teal)
- **Background:** `#F2F6F5` (Light gray)
- **Text Dark:** `#0F172A`
- **Text Soft:** `#64748B`

### UI Features

- Material Design
- Gradient bottom navigation bar
- Card-based layout (20px border-radius)
- Subtle shadows
- Smooth animations

---

## Building for Release

### Android

#### Generate Signed APK

1. Create a keystore (if you don't have one):

```bash
keytool -genkey -v -keystore ~/upload-keystore.jks -keyalg RSA -keysize 2048 -validity 10000 -alias upload
```

2. Configure `android/key.properties`:

```properties
storePassword=<password>
keyPassword=<password>
keyAlias=upload
storeFile=<path-to-keystore>
```

3. Update `android/app/build.gradle` to use the keystore.

4. Build:

```bash
flutter build apk --release
```

#### Generate App Bundle

```bash
flutter build appbundle --release
```

### iOS (macOS only)

1. Open `ios/Runner.xcworkspace` in Xcode
2. Configure signing & capabilities
3. Build:

```bash
flutter build ios --release
```

---

## Features

### Implemented Features

- ✅ User Authentication (Login/Register)
- ✅ Service Categories & Products
- ✅ Product Details
- ✅ Appointment Booking
- ✅ Address Management
- ✅ Order History
- ✅ Wallet System
- ✅ Promo Code Redemption
- ✅ Profile Management
- ✅ Payment Integration (Wallet & Card)

### Platform Support

| Platform | Status |
|----------|--------|
| Android | ✅ Fully Supported |
| iOS | 🎯 Target Platform |

---

## Testing

### Run Tests

```bash
flutter test
```

### Run on Device

```bash
flutter run
```

### Hot Reload

Press `r` in the terminal while the app is running for hot reload.

### Hot Restart

Press `R` (capital) for hot restart.

---

## Troubleshooting

### Build Errors

**"No devices found":**
- Start an Android emulator or connect a device
- Run `flutter devices` to see available devices

**"Gradle build failed":**
- Clean the project: `flutter clean`
- Get dependencies again: `flutter pub get`
- Rebuild: `flutter build apk`

**iOS build errors (macOS only):**
- Ensure Xcode is installed and updated
- Run `pod install` in `ios/` directory
- Open workspace in Xcode and check signing

### Runtime Errors

**Network errors:**
- Verify API server is running
- Check base URL in `api_service.dart`
- Verify CORS settings on API server

**Token storage errors:**
- Check platform-specific storage permissions
- For Android, ensure `minSdkVersion` is 18+

---

## Platform-Specific Configuration

### Android

**Minimum SDK:** 21 (Android 5.0)
**Target SDK:** 33+

**Permissions** (in `android/app/src/main/AndroidManifest.xml`):

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

### iOS

**Minimum Version:** 11.0
**Target Version:** 14.0+

**Info.plist** configuration may be required for network requests.

---

## Security Best Practices

1. **Never commit secrets** - Use environment variables or config files
2. **Use HTTPS in production** - Update base URL to HTTPS
3. **Secure token storage** - Tokens stored with `flutter_secure_storage`
4. **Validate all inputs** - Sanitize user data
5. **Code obfuscation** - Enable for release builds:

```bash
flutter build apk --release --obfuscate --split-debug-info=./debug-info
```

---

## Development Workflow

### 1. Make Changes

Edit files in `lib/` directory.

### 2. Hot Reload

Save files and the app will automatically reload (or press `r`).

### 3. Test

Test on both Android and iOS if possible.

### 4. Build Release

Build release version when ready to deploy.

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

