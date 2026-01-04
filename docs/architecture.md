# HizmetSepetim - Architecture Overview

## System Architecture

HizmetSepetim is a modern service marketplace platform consisting of three main components that work together to provide a complete solution.

```
┌─────────────────┐     ┌─────────────────┐
│  Mobile Clients │     │  Web Application│
│  (Android/Flutter)│     │  (PHP/Admin/Seller)│
└────────┬────────┘     └────────┬────────┘
         │                      │
         │ HTTPS/REST API       │ Direct DB Access
         │                      │
┌────────▼──────────────────────▼────────┐
│   Go API Server │   PHP Web App        │
│   (Port 8080)   │   (Port 80/443)      │
└────────┬──────────────────────┬────────┘
         │                      │
         │ SQL Queries          │
         │                      │
┌────────▼──────────────────────▼────────┐
│   MySQL Database│                      │
│   (Port 3306)   │                      │
└────────────────────────────────────────┘
```

---

## Component Overview

### 1. Backend API (`/hizmetSepetiApi`)

**Technology:** Go 1.24, Gin Framework
**Port:** 8080
**Database:** MySQL/MariaDB

**Responsibilities:**
- User authentication & authorization (JWT)
- Business logic & data validation
- Database operations
- Payment processing (Stripe integration)
- Notification management
- Support chat system

**Key Features:**
- RESTful API design
- JWT-based authentication
- CORS enabled
- Wallet system
- Promo code system
- Seller & admin endpoints

---

### 2. Android Native App (`/hizmetSepetiApp`)

**Technology:** Kotlin, Jetpack Compose
**Architecture:** MVVM
**Min SDK:** 24 (Android 7.0)

**Responsibilities:**
- User interface & user experience
- API communication
- Local data storage (DataStore)
- Background tasks (WorkManager)
- Push notifications (Firebase)

**Key Features:**
- Material 3 design
- Real-time notifications
- Offline support
- Background sync
- Modern UI/UX

---

### 3. Flutter App (`/hizmetsepetimapp_flutter`)

**Technology:** Flutter, Dart
**Architecture:** Provider Pattern
**Platforms:** Android, iOS

**Responsibilities:**
- Cross-platform UI
- API communication
- Secure token storage
- State management
- Platform-specific features

**Key Features:**
- Cross-platform support
- Secure storage
- Material Design
- Provider state management

---

### 4. Web Application (`/HizmetSepetimWeb`)

**Technology:** PHP 7.4+, Bootstrap 5.3
**Architecture:** Server-side PHP, MVC-like structure
**Port:** 80/443 (HTTP/HTTPS)

**Responsibilities:**
- Public website (landing page)
- Admin panel management
- Seller panel management
- Content management
- Statistics display

**Key Features:**
- Public website with modern design
- Admin panel with 2FA security
- Seller dashboard
- Support ticket management
- Notification system
- Promo code management

---

## Data Flow

### Authentication Flow

```
1. User enters credentials
   ↓
2. App sends POST /login
   ↓
3. API validates credentials
   ↓
4. API generates JWT token
   ↓
5. Token stored securely (DataStore/SecureStorage)
   ↓
6. Token included in subsequent requests
```

### Order Creation Flow

```
1. User selects service & appointment
   ↓
2. App sends POST /create_order_with_payment
   ↓
3. API validates order
   ↓
4. API processes payment (wallet/card)
   ↓
5. API creates order in database
   ↓
6. API returns order confirmation
   ↓
7. App displays success message
```

### Notification Flow

```
1. Admin creates notification (via API/Admin Panel)
   ↓
2. Notification stored in notification_queue table
   ↓
3. Android: WorkManager polls /notifications/get every 15 min
   ↓
4. Flutter: App polls /notifications/get on app open
   ↓
5. Notification displayed to user
   ↓
6. Notification marked as sent
```

---

## Database Schema

### Core Tables

**users**
- Stores user accounts (customers, sellers, admins)
- Fields: id, email, password (hashed), role, first_name, last_name, etc.

**categories**
- Service categories
- Fields: id, name, image_url

**products**
- Services/products offered
- Fields: id, category_id, name, price, description, image_url

**orders**
- Customer orders/appointments
- Fields: id, user_id, seller_id, product_name, total_price, status, appointment_datetime, etc.

**addresses**
- User addresses
- Fields: id, user_id, full_name, address_line, city, district, etc.

**wallets**
- User wallet balances
- Fields: user_id, balance, updated_at

**wallet_transactions**
- Wallet transaction history
- Fields: id, user_id, amount, type, description, created_at

**support_conversations**
- Support chat tickets
- Fields: id, user_id, status, last_message_at

**support_messages**
- Support chat messages
- Fields: id, conversation_id, sender_type, sender_id, message, created_at

---

## Security Architecture

### Authentication

- **Method:** JWT (JSON Web Tokens)
- **Token Expiration:** 7 days
- **Storage:**
  - Android: DataStore (encrypted)
  - Flutter: flutter_secure_storage (encrypted)

### Authorization

- **Role-based access control:**
  - `customer` - Regular users
  - `seller` - Service providers
  - `admin` - Platform administrators

### Data Protection

- Passwords hashed with bcrypt
- JWT tokens with expiration
- Secure storage for sensitive data
- HTTPS in production (recommended)

---

## API Architecture

### Endpoint Structure

```
Public Endpoints:
  /login
  /register
  /get_categories
  /get_products
  /get_product_detail

Authenticated Endpoints:
  /get_addresses
  /create_order
  /wallet/balance
  /user/profile

Role-based Endpoints:
  /v2/seller/* (seller role required)
  /admin/* (admin role required)
```

### Request/Response Format

**Request:**
```json
POST /login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "success": true,
  "token": "jwt_token_here",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "customer"
  }
}
```

---

## Mobile App Architecture

### Android (MVVM)

```
View (Compose UI)
    ↓
ViewModel (State Management)
    ↓
Repository / ApiService
    ↓
Retrofit Client
    ↓
API Server
```

**Key Components:**
- `MainActivity` - Navigation & app initialization
- `RetrofitClient` - API client setup
- `DataStoreManager` - Local storage
- `NotificationWorker` - Background tasks

### Flutter (Provider)

```
UI Widgets
    ↓
Provider (State Management)
    ↓
ApiService
    ↓
Dio Client
    ↓
API Server
```

**Key Components:**
- `main.dart` - App initialization
- `api_service.dart` - API client
- `TokenStore` - Secure token storage
- `UserStore` - User data storage

---

## Payment Architecture

### Payment Methods

1. **Wallet Payment**
   - User's wallet balance
   - Immediate deduction
   - Transaction logged

2. **Card Payment**
   - Stripe integration
   - Secure payment processing
   - Card details stored securely

3. **Mixed Payment**
   - Combination of wallet + card
   - Wallet used first, then card

### Payment Flow

```
1. User selects payment method
   ↓
2. App calculates total
   ↓
3. App sends payment request
   ↓
4. API validates payment amount
   ↓
5. API processes payment (wallet/card)
   ↓
6. API creates order
   ↓
7. API returns confirmation
```

---

## Notification System

### Architecture

**Android:**
- WorkManager (15-minute intervals)
- Firebase Cloud Messaging (push notifications)
- Local notifications

**Flutter:**
- Polling on app open
- Local notifications
- Future: Push notifications support

### Notification Types

- Order updates
- Payment confirmations
- Promo code alerts
- Support messages
- Admin announcements

---

## Deployment Architecture

### Development

```
Local Development:
  - API: localhost:8080
  - Database: localhost:3306
  - Apps: Development builds
```

### Production

```
Production Server:
  - API: Production domain (port 8080)
  - Database: Production MySQL server
  - Apps: Release builds (Play Store/App Store)
```

### Recommended Setup

- **API Server:** Ubuntu 22.04, systemd service
- **Database:** MySQL 8.0+ with backups
- **Reverse Proxy:** Nginx (for HTTPS)
- **SSL Certificate:** Let's Encrypt
- **Monitoring:** Log monitoring, error tracking

---

## Scalability Considerations

### Database

- Index optimization
- Query optimization
- Connection pooling
- Read replicas (if needed)

### API

- Stateless design (JWT)
- Horizontal scaling possible
- Load balancing ready
- Caching where appropriate

### Mobile Apps

- Efficient API calls
- Local caching
- Background sync
- Offline support

---

## Technology Stack Summary

| Component | Technology |
|-----------|-----------|
| **Backend** | Go 1.24, Gin, MySQL |
| **Android** | Kotlin, Jetpack Compose, MVVM |
| **Flutter** | Dart, Flutter, Provider |
| **Web** | PHP 7.4+, Bootstrap 5.3, JavaScript |
| **Auth** | JWT, 2FA (Admin Panel) |
| **Payment** | Stripe |
| **Storage (Android)** | DataStore |
| **Storage (Flutter)** | Secure Storage, Shared Preferences |
| **Notifications** | WorkManager, FCM (Android) |

---

## Future Enhancements

### Potential Improvements

- [ ] Real-time chat with WebSockets
- [ ] Push notifications for Flutter
- [ ] Advanced caching strategies
- [ ] GraphQL API option
- [ ] Microservices architecture
- [ ] Docker containerization
- [ ] Kubernetes orchestration
- [ ] Advanced analytics

---

## Support & Documentation

For detailed documentation:
- [API Setup Guide](./api-setup.md)
- [Android App Setup](./android-app-setup.md)
- [Flutter App Setup](./flutter-app-setup.md)

---

## License

This project is protected under CC BY-NC-ND 4.0 License.
© 2025 Buğra Akdemir. All Rights Reserved.

