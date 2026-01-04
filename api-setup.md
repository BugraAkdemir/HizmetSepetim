# HizmetSepetim API - Setup Guide

## Overview

The HizmetSepetim API is a RESTful backend service built with Go (Golang). It provides authentication, order management, payment processing, wallet system, and support chat functionality.

**Technology Stack:**
- Go 1.24.0
- Gin Web Framework
- MySQL/MariaDB
- JWT Authentication
- Stripe Payment Integration

---

## Prerequisites

Before setting up the API, ensure you have the following installed:

- **Go 1.24+** - [Download Go](https://golang.org/dl/)
- **MySQL 8.0+** or **MariaDB 10.5+**
- **Git** - For cloning the repository
- **Text Editor/IDE** - VS Code, GoLand, or any preferred editor

---

## Installation Steps

### 1. Clone the Repository

```bash
git clone <api-repository-url> hizmetSepetiApi
cd hizmetSepetiApi
```

### 2. Install Dependencies

```bash
go mod download
```

This will download all required Go packages defined in `go.mod`.

### 3. Database Setup

#### Create Database

```sql
CREATE DATABASE bugradev_hizmetsepetim_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

#### Import Database Schema

If you have a SQL dump file:

```bash
mysql -u root -p bugradev_hizmetsepetim_db < databasebackup/11Ara2025.sql
```

Or restore from backup:

```bash
mysql -u root -p bugradev_hizmetsepetim_db < before_restore.sql
```

### 4. Configure Database Connection

Edit `api.go` and update the database connection string:

```go
dsn := "root:YOUR_PASSWORD@tcp(localhost:3306)/bugradev_hizmetsepetim_db?charset=utf8mb4&parseTime=true&loc=Local"
```

Replace:
- `root` with your MySQL username
- `YOUR_PASSWORD` with your MySQL password
- `localhost:3306` with your database host and port if different

### 5. Configure JWT Secret

Update the JWT secret key in `api.go`:

```go
var jwtSecret = []byte("YOUR_SECRET_KEY_HERE")
```

**Important:** Use a strong, random secret key in production!

### 6. Configure Stripe (Optional)

If you're using Stripe for payments, set your Stripe API key:

```go
stripe.Key = "sk_test_YOUR_STRIPE_SECRET_KEY"
```

### 7. Create Uploads Directory

Create a directory for file uploads (avatars, etc.):

```bash
mkdir uploads
chmod 755 uploads
```

### 8. Run the API

```bash
go run api.go
```

The API will start on port `8080` by default.

You should see:
```
MySQL bağlantısı başarılı! 🚀
API 8080 portunda çalışıyor...
```

---

## Project Structure

```
hizmetSepetiApi/
├── api.go                 # Main API file with all handlers
├── go.mod                  # Go module dependencies
├── go.sum                  # Dependency checksums
├── databasebackup/         # SQL backup files
│   ├── 09Ara2025.sql
│   └── 11Ara2025.sql
└── before_restore.sql      # Database restore script
```

---

## API Endpoints

### Public Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Health check |
| POST | `/login` | User login |
| POST | `/register` | User registration |
| GET | `/get_categories` | Get all categories |
| GET | `/get_products?category_id={id}` | Get products by category |
| GET | `/get_product_detail?id={id}` | Get product details |
| GET | `/get_addons` | Get available addons |
| GET | `/maintenance` | Check maintenance mode |
| POST | `/contact` | Contact form submission |

### Authenticated Endpoints (Require JWT Token)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/get_addresses` | Get user addresses |
| POST | `/add_address` | Add new address |
| POST | `/update_address` | Update address |
| POST | `/delete_address` | Delete address |
| GET | `/get_orders` | Get user orders |
| POST | `/create_order` | Create order |
| POST | `/create_order_with_payment` | Create order with payment |
| GET | `/wallet/balance` | Get wallet balance |
| GET | `/wallet/transactions` | Get wallet transactions |
| POST | `/redeem_promo` | Redeem promo code |
| GET | `/user/profile` | Get user profile |
| POST | `/user/profile/update` | Update profile |
| POST | `/user/profile/avatar` | Update avatar |
| POST | `/support/start` | Start support conversation |
| GET | `/support/messages?conversation_id={id}` | Get messages |
| POST | `/support/send` | Send support message |

### Seller Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/v2/seller/orders` | Get seller orders |
| POST | `/v2/seller/orders/{id}/complete` | Complete order |
| GET | `/v2/seller/wallet/summary` | Get wallet summary |
| GET | `/v2/seller/payouts` | Get payout requests |
| POST | `/v2/seller/payouts` | Create payout request |

### Admin Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/admin/support/list` | List support tickets |
| GET | `/admin/support/messages` | Get support messages |
| POST | `/admin/support/send` | Send admin message |
| POST | `/admin/support/close` | Close support ticket |

---

## Authentication

The API uses JWT (JSON Web Tokens) for authentication.

### Login Request

```json
POST /login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

### Login Response

```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "role": "customer"
  }
}
```

### Using the Token

Include the token in the `Authorization` header:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## Database Schema

### Key Tables

- `users` - User accounts
- `categories` - Service categories
- `products` - Services/products
- `orders` - Orders/appointments
- `addresses` - User addresses
- `wallets` - User wallet balances
- `wallet_transactions` - Wallet transaction history
- `promo_codes` - Promotional codes
- `support_conversations` - Support tickets
- `support_messages` - Support messages
- `seller_payout_requests` - Seller payout requests

---

## Environment Configuration

For production, consider using environment variables:

```go
// Example using os.Getenv
dsn := os.Getenv("DB_DSN")
jwtSecret = []byte(os.Getenv("JWT_SECRET"))
```

Create a `.env` file (not tracked in git):

```
DB_DSN=root:password@tcp(localhost:3306)/hizmetsepetim_db
JWT_SECRET=your-secret-key-here
STRIPE_KEY=sk_test_...
```

---

## Running in Production

### Build Binary

```bash
go build -o hizmetsepetim-api api.go
```

### Using systemd (Linux)

Create `/etc/systemd/system/hizmetsepetim-api.service`:

```ini
[Unit]
Description=HizmetSepetim API
After=network.target mysql.service

[Service]
Type=simple
User=www-data
WorkingDirectory=/path/to/hizmetSepetiApi
ExecStart=/path/to/hizmetsepetim-api
Restart=always

[Install]
WantedBy=multi-user.target
```

Start the service:

```bash
sudo systemctl enable hizmetsepetim-api
sudo systemctl start hizmetsepetim-api
```

### Using Docker (Optional)

Create `Dockerfile`:

```dockerfile
FROM golang:1.24-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go build -o api api.go

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/api .
EXPOSE 8080
CMD ["./api"]
```

Build and run:

```bash
docker build -t hizmetsepetim-api .
docker run -p 8080:8080 hizmetsepetim-api
```

---

## Troubleshooting

### Database Connection Issues

- Verify MySQL is running: `sudo systemctl status mysql`
- Check credentials in connection string
- Ensure database exists
- Check firewall rules for port 3306

### Port Already in Use

Change the port in `api.go`:

```go
http.ListenAndServe(":8081", nil) // Use different port
```

### CORS Issues

CORS is enabled by default. If you need to restrict origins, modify `EnableCORS` function:

```go
w.Header().Set("Access-Control-Allow-Origin", "https://yourdomain.com")
```

---

## Testing

### Test Login Endpoint

```bash
curl -X POST http://localhost:8080/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password"}'
```

### Test Authenticated Endpoint

```bash
curl -X GET http://localhost:8080/get_addresses \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

---

## Security Best Practices

1. **Never commit secrets** - Use environment variables
2. **Use HTTPS in production** - Set up SSL/TLS
3. **Validate all inputs** - Sanitize user data
4. **Rate limiting** - Implement rate limiting for API endpoints
5. **SQL Injection Prevention** - Always use parameterized queries (already implemented)
6. **JWT Expiration** - Tokens expire after 7 days (configurable)

---

## Support

For issues or questions:
- Check the main documentation: `/docs` folder
- Review API endpoint documentation: `api-endpoints.md`
- Contact the development team

---

## License

This project is protected under CC BY-NC-ND 4.0 License.
© 2025 Buğra Akdemir. All Rights Reserved.

