# HizmetSepetim Web - Kurulum Kılavuzu

## Genel Bakış

HizmetSepetim Web uygulaması, public website, admin paneli ve satıcı paneli içeren PHP tabanlı bir web platformudur. HizmetSepetim platformu için yönetim arayüzü görevi görür.

**Teknoloji Yığını:**
- PHP 7.4+
- MySQL/MariaDB
- Bootstrap 5.3
- Vanilla JavaScript
- SCSS/CSS

---

## Gereksinimler

Web uygulamasını kurmadan önce aşağıdakilerin yüklü olduğundan emin olun:

- **PHP 7.4 veya daha yüksek** - [PHP İndir](https://www.php.net/downloads)
- **MySQL 8.0+** veya **MariaDB 10.5+**
- **Apache** veya **Nginx** web sunucusu
- **Composer** (opsiyonel, bağımlılıklar için)
- **Git** - Repoyu klonlamak için

---

## Kurulum Adımları

### 1. Repoyu Klonlayın

```bash
git clone <web-repository-url> HizmetSepetimWeb
cd HizmetSepetimWeb
```

### 2. Veritabanı Bağlantısını Yapılandırın

#### Admin Paneli İçin

`/var/config/config.php` dosyasını düzenleyin (yoksa oluşturun):

```php
<?php
// Veritabanı yapılandırması
$DB_HOST = 'localhost';
$DB_NAME = 'bugradev_hizmetsepetim_db';
$DB_USER = 'root';
$DB_PASS = 'sifreniz_buraya';

// Bağlantı oluştur
$conn = new mysqli($DB_HOST, $DB_USER, $DB_PASS, $DB_NAME);

// Güvenlik
$PEPPER = 'pepper_gizli_anahtar_buraya'; // Şifre hashleme için
?>
```

#### Satıcı Paneli İçin

`seller/config.php` dosyasını düzenleyin:

```php
<?php
define('DB_HOST', 'localhost');
define('DB_NAME', 'bugradev_hizmetsepetim_db');
define('DB_USER', 'root');
define('DB_PASS', 'sifreniz_buraya');

// Admin satıcı kimlik bilgileri
define('ADMIN_SELLER_USER', 'admin');
define('ADMIN_SELLER_PASS', 'sifreniz_buraya');
define('ADMIN_SELLER_SESSION', 'hs_admin_seller');
?>
```

### 3. Web Sunucusunu Kurun

#### Apache Yapılandırması

Virtual host yapılandırması oluşturun:

```apache
<VirtualHost *:80>
    ServerName hizmetsepetim.local
    DocumentRoot /path/to/HizmetSepetimWeb

    <Directory /path/to/HizmetSepetimWeb>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

#### Nginx Yapılandırması

```nginx
server {
    listen 80;
    server_name hizmetsepetim.local;
    root /path/to/HizmetSepetimWeb;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }
}
```

### 4. İzinleri Ayarlayın

```bash
chmod -R 755 HizmetSepetimWeb
chown -R www-data:www-data HizmetSepetimWeb
```

### 5. Admin Paneli Yapılandırın

#### Admin Kullanıcı Oluşturun

Veritabanına erişin ve bir admin kullanıcı oluşturun:

```sql
INSERT INTO admin_users (username, password_hash)
VALUES ('admin', '$2y$10$hashlenmis_sifre_buraya');
```

**Not:** Hash oluşturmak için PHP'de `password_hash()` fonksiyonunu kullanın.

#### 2FA (İki Faktörlü Kimlik Doğrulama) Kurulumu

1. Google Authenticator için bir secret oluşturun
2. Bunu `admin_2fa` tablosuna kaydedin
3. Admin kullanıcılar girişten sonra 2FA kodu girmek zorunda kalacak

### 6. Uygulamaya Erişin

- **Public Website:** `http://your-domain/`
- **Admin Paneli:** `http://your-domain/admins/`
- **Satıcı Paneli:** `http://your-domain/seller/`

---

## Proje Yapısı

```
HizmetSepetimWeb/
├── admins/                    # Admin paneli
│   ├── index.php             # Admin dashboard
│   ├── adminStatics.php      # İstatistik sayfası
│   ├── admin/                # Admin alt panelleri
│   │   ├── support/          # Destek ticket yönetimi
│   │   ├── notifications/    # Bildirim yönetimi
│   │   └── ...
│   ├── backends/             # Backend script'leri
│   └── images/               # Admin görselleri
├── seller/                    # Satıcı paneli
│   ├── index.php             # Satıcı dashboard
│   ├── login.php             # Satıcı girişi
│   ├── orders_pending.php    # Bekleyen siparişler
│   ├── orders_assign.php     # Sipariş atama
│   ├── payouts.php           # Ödeme talepleri
│   ├── config.php             # Satıcı yapılandırması
│   ├── db.php                 # Veritabanı bağlantısı
│   └── auth.php               # Kimlik doğrulama
├── pages/                     # Public sayfalar
│   ├── about.php              # Hakkında sayfası
│   ├── api-docs.php           # API dokümantasyonu
│   ├── developer.php          # Geliştirici sayfası
│   └── ...
├── widgets/                   # Yeniden kullanılabilir bileşenler
│   ├── navbar.php             # Navigasyon çubuğu
│   ├── footer.php             # Footer
│   └── roadmap.php            # Roadmap widget'ı
├── assets/                    # Statik dosyalar
│   ├── css/                   # Stil dosyaları
│   ├── js/                    # JavaScript dosyaları
│   └── img/                   # Görseller
├── errors/                    # Hata sayfaları
│   ├── 404.php
│   ├── 500.php
│   └── 503.php
├── contracts/                 # Yasal sayfalar
│   ├── privacy.php
│   └── terms.php
└── index.php                  # Ana landing sayfası
```

---

## Ana Özellikler

### Public Website

- **Landing Page** - Modern, responsive ana sayfa
- **Hakkında Sayfası** - Platform bilgileri
- **API Dokümantasyonu** - API endpoint dokümantasyonu
- **İletişim Formu** - API ile entegre
- **İstatistikler** - Platform istatistikleri gösterimi
- **SSS Bölümü** - Sıkça sorulan sorular

### Admin Paneli

- **Dashboard** - Platform istatistikleri genel bakışı
- **Sipariş Yönetimi** - Siparişleri görüntüleme ve yönetme
- **Satıcı Yönetimi** - Satıcıları ve ödemeleri yönetme
- **Destek Sistemi** - Destek ticket'larını yönetme
- **Bildirim Sistemi** - Global bildirimler gönderme
- **Promosyon Kodu Yönetimi** - Promosyon kodları oluşturma ve yönetme
- **Bakım Modu** - Bakım modunu etkinleştirme/devre dışı bırakma
- **2FA Güvenliği** - İki faktörlü kimlik doğrulama

### Satıcı Paneli

- **Dashboard** - Satıcı genel bakışı
- **Sipariş Yönetimi** - Siparişleri görüntüleme ve tamamlama
- **Ödeme Talepleri** - Ödeme talepleri oluşturma
- **İstatistikler** - Satıcıya özel istatistikler

---

## Yapılandırma

### Veritabanı Tabloları

Web uygulaması API ile aynı veritabanını kullanır. Ana tablolar:

- `admin_users` - Admin kullanıcı hesapları
- `admin_2fa` - 2FA secret'ları
- `admin_login_attempts` - Giriş denemesi takibi
- `orders` - Siparişler/randevular
- `seller_payout_requests` - Ödeme talepleri
- `support_conversations` - Destek ticket'ları
- `support_messages` - Destek mesajları
- `notification_queue` - Bildirimler
- `promo_codes` - Promosyon kodları

### Güvenlik Özellikleri

1. **2FA Kimlik Doğrulama** - Google Authenticator desteği
2. **Brute Force Koruması** - Giriş denemesi sınırlama
3. **Oturum Güvenliği** - IP ve User-Agent doğrulama
4. **Şifre Hashleme** - Pepper ile bcrypt
5. **SQL Injection Önleme** - Hazırlanmış ifadeler

---

## API Entegrasyonu

Web uygulaması Go API ile entegre edilmiştir:

**API Base URL:** `http://92.249.61.58:8080/`

**Kullanılan Endpoint'ler:**
- `POST /contact` - İletişim formu gönderimi
- `GET /maintenance` - Bakım modunu kontrol etme
- İstatistikler için çeşitli admin endpoint'leri

---

## Geliştirme

### Yerel Geliştirme

1. Yerel PHP sunucusunu kurun:
```bash
php -S localhost:8000
```

2. `http://localhost:8000` adresinden erişin

### Stil

- Ana stil dosyası: `assets/css/style.scss`
- SCSS'yi CSS'ye derleyin (SCSS kullanıyorsanız)
- Bootstrap 5.3 CDN üzerinden dahil edilmiştir

### JavaScript

- Ana script: `assets/js/app.js`
- Animasyonları, form gönderimlerini vb. yönetir
- Vanilla JavaScript (framework yok)

---

## Dağıtım

### Production Kurulumu

1. **HTTPS Kullanın** - SSL sertifikası kurun
2. **Veritabanı Kimlik Bilgilerini Güncelleyin** - Production veritabanını kullanın
3. **Uygun İzinleri Ayarlayın** - Dosya erişimini kısıtlayın
4. **Hata Günlüğünü Etkinleştirin** - PHP hata loglarını yapılandırın
5. **Yedeklemeleri Kurun** - Düzenli veritabanı yedeklemeleri

### Güvenlik Kontrol Listesi

- [ ] Varsayılan admin kimlik bilgilerini değiştirin
- [ ] Güçlü şifreler kullanın
- [ ] HTTPS'i etkinleştirin
- [ ] Dosya izinlerini kısıtlayın
- [ ] PHP'yi en son sürüme güncelleyin
- [ ] Production'da hata görüntülemeyi devre dışı bırakın
- [ ] Firewall kurallarını ayarlayın
- [ ] Düzenli güvenlik güncellemeleri

---

## Sorun Giderme

### Yaygın Sorunlar

**"Veritabanı bağlantısı başarısız":**
- Config dosyalarındaki veritabanı kimlik bilgilerini kontrol edin
- MySQL servisinin çalıştığını doğrulayın
- Veritabanının var olduğunu kontrol edin

**"Oturum hataları":**
- PHP oturum dizini izinlerini kontrol edin
- php.ini'deki oturum yapılandırmasını doğrulayın

**"404 hataları":**
- Web sunucusu yapılandırmasını kontrol edin
- .htaccess dosyasını doğrulayın (Apache)
- Dosya izinlerini kontrol edin

**"Admin girişi çalışmıyor":**
- Veritabanında admin kullanıcının var olduğunu doğrulayın
- Şifre hash formatını kontrol edin
- Tarayıcı çerezlerini/oturumlarını temizleyin

---

## Bakım

### Düzenli Görevler

1. **Veritabanı Yedeklemeleri** - Günlük otomatik yedeklemeler
2. **Log İzleme** - Hata loglarını düzenli kontrol edin
3. **Güvenlik Güncellemeleri** - PHP ve bağımlılıkları güncel tutun
4. **Performans İzleme** - Sunucu kaynaklarını izleyin

### Yedekleme Script Örneği

```bash
#!/bin/bash
mysqldump -u root -p bugradev_hizmetsepetim_db > backup_$(date +%Y%m%d).sql
```

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

