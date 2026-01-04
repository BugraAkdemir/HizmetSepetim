<p align="center">
  <img src="./screenshots/logo.png" width="160" alt="logo">
</p>

<h1 align="center">HizmetSepetim</h1>
<p align="center">
  Modern Service Marketplace Platform • Final Version 4.0.0
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-4.0.0-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/status-Final-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/platform-Android%20%7C%20Flutter-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/backend-Go%20Lang-00ADD8?style=for-the-badge">
</p>

---

## 📖 About This Repository

This is the **documentation repository** for the HizmetSepetim project. This repository contains comprehensive documentation, version history, and technical guides for all components of the HizmetSepetim platform.

> ⚠️ **Note:** This is a documentation-only repository. Source code is maintained in separate repositories.

---

## 🏗️ Project Structure

The HizmetSepetim platform consists of four main components:

### 1. **API Backend** (`/hizmetSepetiApi`)
- **Technology:** Go (Golang)
- **Database:** MySQL/MariaDB
- **Features:** RESTful API, JWT Authentication, Payment Integration (Stripe), Wallet System, Support Chat
- **Documentation:** [API Documentation](./docs/api-setup.md)

### 2. **Android Native App** (`/hizmetSepetiApp`)
- **Technology:** Kotlin, Jetpack Compose
- **Architecture:** MVVM
- **Features:** Material 3 Design, Modern UI, Real-time Notifications, Offline Support
- **Documentation:** [Android App Documentation](./docs/android-app-setup.md)

### 3. **Flutter App** (`/hizmetsepetimapp_flutter`)
- **Technology:** Flutter, Dart
- **Architecture:** Provider Pattern
- **Features:** Cross-platform Support, Material Design, Secure Storage
- **Documentation:** [Flutter App Documentation](./docs/flutter-app-setup.md)

### 4. **Web Application** (`/HizmetSepetimWeb`)
- **Technology:** PHP, Bootstrap, JavaScript
- **Features:** Public Website, Admin Panel, Seller Panel, 2FA Security
- **Documentation:** [Web Application Documentation](./docs/web-setup.md)

---

## 📚 Documentation

### Quick Start Guides
- [API Setup Guide](./docs/api-setup.md) - How to set up and run the Go API
- [Android App Setup Guide](./docs/android-app-setup.md) - How to build and run the Android app
- [Flutter App Setup Guide](./docs/flutter-app-setup.md) - How to build and run the Flutter app
- [Web Application Setup Guide](./docs/web-setup.md) - How to set up and deploy the web application

### Architecture & Design
- [Architecture Overview](./docs/architecture.md) - System architecture and design patterns
- [API Endpoints Reference](./docs/api-endpoints.md) - Complete API endpoint documentation
- [Database Schema](./docs/database-schema.md) - Database structure and relationships

### Version History
- [Version 4.0.0](./docs/v4.0.md) - Latest version release notes
- [Version 2.0.0](./docs/v2.0.0.md) - Previous major version
- [Version 1.5.0](./docs/v1.5.0.md) - Feature updates
- [Version 1.0.0](./docs/v1.0.0.md) - Initial release

---

## 🚀 Quick Start

### Prerequisites
- **For API:** Go 1.24+, MySQL 8.0+
- **For Android App:** Android Studio, JDK 11+, Android SDK 24+
- **For Flutter App:** Flutter SDK 3.10.4+, Dart 3.10.4+
- **For Web App:** PHP 7.4+, Apache/Nginx, MySQL 8.0+

### Getting Started
1. Clone the respective repositories:
   ```bash
   # API Repository
   git clone <api-repo-url> hizmetSepetiApi

   # Android App Repository
   git clone <android-repo-url> hizmetSepetiApp

   # Flutter App Repository
   git clone <flutter-repo-url> hizmetsepetimapp_flutter

   # Web Application Repository
   git clone <web-repo-url> HizmetSepetimWeb
   ```

2. Follow the setup guides in the `/docs` folder for each component.

---

## 🛠️ Technology Stack

| Component | Technology |
|-----------|-----------|
| **Backend API** | Go 1.24, Gin Framework, MySQL, JWT, Stripe |
| **Android App** | Kotlin, Jetpack Compose, Material 3, Retrofit, MVVM |
| **Flutter App** | Flutter 3.10.4, Dart, Dio, Provider, Secure Storage |
| **Web Application** | PHP 7.4+, Bootstrap 5.3, JavaScript, MySQL |
| **Database** | MySQL 8.0 / MariaDB |
| **Authentication** | JWT (JSON Web Tokens), 2FA (Admin Panel) |
| **Payment** | Stripe Integration |
| **Notifications** | Firebase Cloud Messaging (Android) |

---

## 📱 Features

### Core Features
- ✅ User Authentication & Registration
- ✅ Service Categories & Products
- ✅ Appointment Booking System
- ✅ Address Management
- ✅ Order Management
- ✅ Payment Integration (Wallet & Card)
- ✅ Promo Code System
- ✅ Support Chat System
- ✅ Real-time Notifications
- ✅ Seller Dashboard
- ✅ Admin Panel

### Platform-Specific Features
- **Android:** Material 3 Design, WorkManager for Background Tasks
- **Flutter:** Cross-platform Support, Secure Token Storage

---

## 📄 License

This project is protected under the **CC BY-NC-ND 4.0 License**.
© 2025 Buğra Akdemir. All Rights Reserved.

---

## 👨‍💻 Developer

**Buğra Akdemir**
- Full-Stack Developer
- Technologies: Kotlin, Go, Flutter, PHP, MySQL
- Founder & Developer of HizmetSepetim

---

## 📞 Support

For technical questions or issues, please refer to the documentation in the `/docs` folder or contact the development team.

---

<p align="center">
  <b>HizmetSepetim</b> • Modern Service Marketplace Platform • Version 4.0.0 Final
</p>
