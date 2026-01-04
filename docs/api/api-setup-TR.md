# HizmetSepetim API - Kurulum Kılavuzu

## Genel Bakış

HizmetSepetim API, Go (Golang) ile geliştirilmiş bir RESTful backend servisidir. Kimlik doğrulama, sipariş yönetimi, ödeme işleme, cüzdan sistemi ve destek chat işlevselliği sağlar.

**Teknoloji Yığını:**
- Go 1.24.0
- Gin Web Framework
- MySQL/MariaDB
- JWT Kimlik Doğrulama
- Stripe Ödeme Entegrasyonu

---

## Gereksinimler

API'yi kurmadan önce aşağıdakilerin yüklü olduğundan emin olun:

- **Go 1.24+** - [Go İndir](https://golang.org/dl/)
- **MySQL 8.0+** veya **MariaDB 10.5+**
- **Git** - Repoyu klonlamak için
- **Metin Editörü/IDE** - VS Code, GoLand veya tercih ettiğiniz editör

---

## Kurulum Adımları

### 1. Repoyu Klonlayın

```bash
git clone <api-repository-url> hizmetSepetiApi
cd hizmetSepetiApi
```

### 2. Bağımlılıkları Yükleyin

```bash
go mod download
```

Bu komut `go.mod` dosyasında tanımlı tüm gerekli Go paketlerini indirecektir.

### 3. Veritabanı Kurulumu

#### Veritabanı Oluşturun

```sql
CREATE DATABASE bugradev_hizmetsepetim_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

#### Veritabanı Şemasını İçe Aktarın

Eğer SQL dump dosyanız varsa:

```bash
mysql -u root -p bugradev_hizmetsepetim_db < databasebackup/11Ara2025.sql
```

Veya yedekten geri yükleyin:

```bash
mysql -u root -p bugradev_hizmetsepetim_db < before_restore.sql
```

### 4. Veritabanı Bağlantısını Yapılandırın

`api.go` dosyasını düzenleyin ve veritabanı bağlantı stringini güncelleyin:

```go
dsn := "root:SIFRENIZ@tcp(localhost:3306)/bugradev_hizmetsepetim_db?charset=utf8mb4&parseTime=true&loc=Local"
```

Değiştirin:
- `root` → MySQL kullanıcı adınız
- `SIFRENIZ` → MySQL şifreniz
- `localhost:3306` → Farklıysa veritabanı host ve port

### 5. JWT Secret'ı Yapılandırın

`api.go` dosyasındaki JWT secret anahtarını güncelleyin:

```go
var jwtSecret = []byte("GIZLI_ANAHTARINIZ_BURAYA")
```

**Önemli:** Production'da güçlü, rastgele bir secret anahtar kullanın!

### 6. Stripe'ı Yapılandırın (Opsiyonel)

Ödemeler için Stripe kullanıyorsanız, Stripe API anahtarınızı ayarlayın:

```go
stripe.Key = "sk_test_STRIPE_GIZLI_ANAHTARINIZ"
```

### 7. Uploads Klasörünü Oluşturun

Dosya yüklemeleri (avatarlar, vb.) için bir klasör oluşturun:

```bash
mkdir uploads
chmod 755 uploads
```

### 8. API'yi Çalıştırın

```bash
go run api.go
```

API varsayılan olarak `8080` portunda başlayacaktır.

Şunu görmelisiniz:
```
MySQL bağlantısı başarılı! 🚀
API 8080 portunda çalışıyor...
```

---

## Proje Yapısı

```
hizmetSepetiApi/
├── api.go                 # Tüm handler'ları içeren ana API dosyası
├── go.mod                  # Go modül bağımlılıkları
├── go.sum                  # Bağımlılık checksum'ları
├── databasebackup/          # SQL yedek dosyaları
│   ├── 09Ara2025.sql
│   └── 11Ara2025.sql
└── before_restore.sql      # Veritabanı geri yükleme scripti
```

---

## API Endpoint'leri

### Public Endpoint'ler

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| GET | `/` | Sağlık kontrolü |
| POST | `/login` | Kullanıcı girişi |
| POST | `/register` | Kullanıcı kaydı |
| GET | `/get_categories` | Tüm kategorileri getir |
| GET | `/get_products?category_id={id}` | Kategoriye göre ürünleri getir |
| GET | `/get_product_detail?id={id}` | Ürün detaylarını getir |
| GET | `/get_addons` | Mevcut ek hizmetleri getir |
| GET | `/maintenance` | Bakım modunu kontrol et |
| POST | `/contact` | İletişim formu gönderimi |

### Kimlik Doğrulama Gerektiren Endpoint'ler (JWT Token Gerekli)

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| GET | `/get_addresses` | Kullanıcı adreslerini getir |
| POST | `/add_address` | Yeni adres ekle |
| POST | `/update_address` | Adresi güncelle |
| POST | `/delete_address` | Adresi sil |
| GET | `/get_orders` | Kullanıcı siparişlerini getir |
| POST | `/create_order` | Sipariş oluştur |
| POST | `/create_order_with_payment` | Ödemeli sipariş oluştur |
| GET | `/wallet/balance` | Cüzdan bakiyesini getir |
| GET | `/wallet/transactions` | Cüzdan işlem geçmişini getir |
| POST | `/redeem_promo` | Promosyon kodu kullan |
| GET | `/user/profile` | Kullanıcı profilini getir |
| POST | `/user/profile/update` | Profili güncelle |
| POST | `/user/profile/avatar` | Avatarı güncelle |
| POST | `/support/start` | Destek konuşması başlat |
| GET | `/support/messages?conversation_id={id}` | Mesajları getir |
| POST | `/support/send` | Destek mesajı gönder |

### Satıcı Endpoint'leri

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| GET | `/v2/seller/orders` | Satıcı siparişlerini getir |
| POST | `/v2/seller/orders/{id}/complete` | Siparişi tamamla |
| GET | `/v2/seller/wallet/summary` | Cüzdan özetini getir |
| GET | `/v2/seller/payouts` | Ödeme taleplerini getir |
| POST | `/v2/seller/payouts` | Ödeme talebi oluştur |

### Admin Endpoint'leri

| Method | Endpoint | Açıklama |
|--------|----------|----------|
| GET | `/admin/support/list` | Destek ticket'larını listele |
| GET | `/admin/support/messages` | Destek mesajlarını getir |
| POST | `/admin/support/send` | Admin mesajı gönder |
| POST | `/admin/support/close` | Destek ticket'ını kapat |

---

## Kimlik Doğrulama

API, kimlik doğrulama için JWT (JSON Web Tokens) kullanır.

### Giriş İsteği

```json
POST /login
Content-Type: application/json

{
  "email": "kullanici@example.com",
  "password": "sifre123"
}
```

### Giriş Yanıtı

```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "email": "kullanici@example.com",
    "role": "customer"
  }
}
```

### Token Kullanımı

Token'ı `Authorization` header'ında gönderin:

```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## Veritabanı Şeması

### Ana Tablolar

- `users` - Kullanıcı hesapları
- `categories` - Hizmet kategorileri
- `products` - Hizmetler/ürünler
- `orders` - Siparişler/randevular
- `addresses` - Kullanıcı adresleri
- `wallets` - Kullanıcı cüzdan bakiyeleri
- `wallet_transactions` - Cüzdan işlem geçmişi
- `promo_codes` - Promosyon kodları
- `support_conversations` - Destek ticket'ları
- `support_messages` - Destek mesajları
- `seller_payout_requests` - Satıcı ödeme talepleri

---

## Ortam Yapılandırması

Production için environment variable'ları kullanmayı düşünün:

```go
// os.Getenv kullanım örneği
dsn := os.Getenv("DB_DSN")
jwtSecret = []byte(os.Getenv("JWT_SECRET"))
```

`.env` dosyası oluşturun (git'te takip edilmez):

```
DB_DSN=root:sifre@tcp(localhost:3306)/hizmetsepetim_db
JWT_SECRET=gizli-anahtar-buraya
STRIPE_KEY=sk_test_...
```

---

## Production'da Çalıştırma

### Binary Derleme

```bash
go build -o hizmetsepetim-api api.go
```

### systemd Kullanımı (Linux)

`/etc/systemd/system/hizmetsepetim-api.service` oluşturun:

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

Servisi başlatın:

```bash
sudo systemctl enable hizmetsepetim-api
sudo systemctl start hizmetsepetim-api
```

### Docker Kullanımı (Opsiyonel)

`Dockerfile` oluşturun:

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

Derleyin ve çalıştırın:

```bash
docker build -t hizmetsepetim-api .
docker run -p 8080:8080 hizmetsepetim-api
```

---

## Sorun Giderme

### Veritabanı Bağlantı Sorunları

- MySQL'in çalıştığını doğrulayın: `sudo systemctl status mysql`
- Bağlantı stringindeki kimlik bilgilerini kontrol edin
- Veritabanının var olduğundan emin olun
- Port 3306 için firewall kurallarını kontrol edin

### Port Zaten Kullanımda

`api.go` dosyasında portu değiştirin:

```go
http.ListenAndServe(":8081", nil) // Farklı port kullan
```

### CORS Sorunları

CORS varsayılan olarak etkindir. Origin'leri kısıtlamak istiyorsanız, `EnableCORS` fonksiyonunu değiştirin:

```go
w.Header().Set("Access-Control-Allow-Origin", "https://yourdomain.com")
```

---

## Test Etme

### Login Endpoint'ini Test Edin

```bash
curl -X POST http://localhost:8080/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"sifre"}'
```

### Kimlik Doğrulamalı Endpoint'i Test Edin

```bash
curl -X GET http://localhost:8080/get_addresses \
  -H "Authorization: Bearer TOKEN_BURAYA"
```

---

## Güvenlik En İyi Uygulamaları

1. **Asla secret'ları commit etmeyin** - Environment variable'ları kullanın
2. **Production'da HTTPS kullanın** - SSL/TLS kurun
3. **Tüm girdileri doğrulayın** - Kullanıcı verilerini temizleyin
4. **Rate limiting** - API endpoint'leri için rate limiting uygulayın
5. **SQL Injection Önleme** - Her zaman parametreli sorgular kullanın (zaten uygulanmış)
6. **JWT Süresi** - Token'lar 7 gün sonra sona erer (yapılandırılabilir)

---

## Destek

Sorunlar veya sorular için:
- Ana dokümantasyona bakın: `/docs` klasörü
- API endpoint dokümantasyonunu inceleyin: `api-endpoints.md`
- Geliştirme ekibiyle iletişime geçin

---

## Lisans

Bu proje CC BY-NC-ND 4.0 Lisansı altında korunmaktadır.
© 2025 Buğra Akdemir. Tüm Hakları Saklıdır.

