# HizmetSepetim Web - Setup Guide

## Overview

The HizmetSepetim Web application is a PHP-based web platform that includes a public website, admin panel, and seller panel. It serves as the management interface for the HizmetSepetim platform.

**Technology Stack:**
- PHP 7.4+
- MySQL/MariaDB
- Bootstrap 5.3
- Vanilla JavaScript
- SCSS/CSS

---

## Prerequisites

Before setting up the web application, ensure you have:

- **PHP 7.4 or higher** - [Download PHP](https://www.php.net/downloads)
- **MySQL 8.0+** or **MariaDB 10.5+**
- **Apache** or **Nginx** web server
- **Composer** (optional, for dependencies)
- **Git** - For cloning the repository

---

## Installation Steps

### 1. Clone the Repository

```bash
git clone <web-repository-url> HizmetSepetimWeb
cd HizmetSepetimWeb
```

### 2. Configure Database Connection

#### For Admin Panel

Edit `/var/config/config.php` (or create if it doesn't exist):

```php
<?php
// Database configuration
$DB_HOST = 'localhost';
$DB_NAME = 'bugradev_hizmetsepetim_db';
$DB_USER = 'root';
$DB_PASS = 'your_password_here';

// Create connection
$conn = new mysqli($DB_HOST, $DB_USER, $DB_PASS, $DB_NAME);

// Security
$PEPPER = 'your_pepper_secret_here'; // For password hashing
?>
```

#### For Seller Panel

Edit `seller/config.php`:

```php
<?php
define('DB_HOST', 'localhost');
define('DB_NAME', 'bugradev_hizmetsepetim_db');
define('DB_USER', 'root');
define('DB_PASS', 'your_password_here');

// Admin seller credentials
define('ADMIN_SELLER_USER', 'admin');
define('ADMIN_SELLER_PASS', 'your_password_here');
define('ADMIN_SELLER_SESSION', 'hs_admin_seller');
?>
```

### 3. Set Up Web Server

#### Apache Configuration

Create a virtual host configuration:

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

#### Nginx Configuration

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

### 4. Set Permissions

```bash
chmod -R 755 HizmetSepetimWeb
chown -R www-data:www-data HizmetSepetimWeb
```

### 5. Configure Admin Panel

#### Create Admin User

Access the database and create an admin user:

```sql
INSERT INTO admin_users (username, password_hash)
VALUES ('admin', '$2y$10$hashed_password_here');
```

**Note:** Use `password_hash()` function in PHP to generate the hash.

#### Set Up 2FA (Two-Factor Authentication)

1. Generate a secret for Google Authenticator
2. Store it in `admin_2fa` table
3. Admin users will need to enter 2FA code after login

### 6. Access the Application

- **Public Website:** `http://your-domain/`
- **Admin Panel:** `http://your-domain/admins/`
- **Seller Panel:** `http://your-domain/seller/`

---

## Project Structure

```
HizmetSepetimWeb/
├── admins/                    # Admin panel
│   ├── index.php             # Admin dashboard
│   ├── adminStatics.php      # Statistics page
│   ├── admin/                # Admin sub-panels
│   │   ├── support/          # Support ticket management
│   │   ├── notifications/    # Notification management
│   │   └── ...
│   ├── backends/             # Backend scripts
│   └── images/               # Admin images
├── seller/                    # Seller panel
│   ├── index.php             # Seller dashboard
│   ├── login.php             # Seller login
│   ├── orders_pending.php    # Pending orders
│   ├── orders_assign.php     # Assign orders
│   ├── payouts.php           # Payout requests
│   ├── config.php             # Seller config
│   ├── db.php                 # Database connection
│   └── auth.php               # Authentication
├── pages/                     # Public pages
│   ├── about.php              # About page
│   ├── api-docs.php           # API documentation
│   ├── developer.php          # Developer page
│   └── ...
├── widgets/                   # Reusable components
│   ├── navbar.php             # Navigation bar
│   ├── footer.php             # Footer
│   └── roadmap.php            # Roadmap widget
├── assets/                    # Static assets
│   ├── css/                   # Stylesheets
│   ├── js/                    # JavaScript files
│   └── img/                   # Images
├── errors/                    # Error pages
│   ├── 404.php
│   ├── 500.php
│   └── 503.php
├── contracts/                 # Legal pages
│   ├── privacy.php
│   └── terms.php
└── index.php                  # Main landing page
```

---

## Key Features

### Public Website

- **Landing Page** - Modern, responsive homepage
- **About Page** - Platform information
- **API Documentation** - API endpoint documentation
- **Contact Form** - Integrated with API
- **Statistics** - Platform statistics display
- **FAQ Section** - Frequently asked questions

### Admin Panel

- **Dashboard** - Overview of platform statistics
- **Order Management** - View and manage orders
- **Seller Management** - Manage sellers and payouts
- **Support System** - Handle support tickets
- **Notification System** - Send global notifications
- **Promo Code Management** - Create and manage promo codes
- **Maintenance Mode** - Enable/disable maintenance mode
- **2FA Security** - Two-factor authentication

### Seller Panel

- **Dashboard** - Seller overview
- **Order Management** - View and complete orders
- **Payout Requests** - Request payouts
- **Statistics** - Seller-specific statistics

---

## Configuration

### Database Tables

The web application uses the same database as the API. Key tables:

- `admin_users` - Admin user accounts
- `admin_2fa` - 2FA secrets
- `admin_login_attempts` - Login attempt tracking
- `orders` - Orders/appointments
- `seller_payout_requests` - Payout requests
- `support_conversations` - Support tickets
- `support_messages` - Support messages
- `notification_queue` - Notifications
- `promo_codes` - Promotional codes

### Security Features

1. **2FA Authentication** - Google Authenticator support
2. **Brute Force Protection** - Login attempt limiting
3. **Session Security** - IP and User-Agent validation
4. **Password Hashing** - bcrypt with pepper
5. **SQL Injection Prevention** - Prepared statements

---

## API Integration

The web application integrates with the Go API:

**API Base URL:** `http://92.249.61.58:8080/`

**Endpoints Used:**
- `POST /contact` - Contact form submission
- `GET /maintenance` - Check maintenance mode
- Various admin endpoints for statistics

---

## Development

### Local Development

1. Set up local PHP server:
```bash
php -S localhost:8000
```

2. Access at `http://localhost:8000`

### Styling

- Main stylesheet: `assets/css/style.scss`
- Compile SCSS to CSS (if using SCSS)
- Bootstrap 5.3 included via CDN

### JavaScript

- Main script: `assets/js/app.js`
- Handles animations, form submissions, etc.
- Vanilla JavaScript (no frameworks)

---

## Deployment

### Production Setup

1. **Use HTTPS** - Set up SSL certificate
2. **Update Database Credentials** - Use production database
3. **Set Proper Permissions** - Restrict file access
4. **Enable Error Logging** - Configure PHP error logs
5. **Set Up Backups** - Regular database backups

### Security Checklist

- [ ] Change default admin credentials
- [ ] Use strong passwords
- [ ] Enable HTTPS
- [ ] Restrict file permissions
- [ ] Update PHP to latest version
- [ ] Disable error display in production
- [ ] Set up firewall rules
- [ ] Regular security updates

---

## Troubleshooting

### Common Issues

**"Database connection failed":**
- Check database credentials in config files
- Verify MySQL service is running
- Check database exists

**"Session errors":**
- Check PHP session directory permissions
- Verify session configuration in php.ini

**"404 errors":**
- Check web server configuration
- Verify .htaccess file (Apache)
- Check file permissions

**"Admin login not working":**
- Verify admin user exists in database
- Check password hash format
- Clear browser cookies/sessions

---

## Maintenance

### Regular Tasks

1. **Database Backups** - Daily automated backups
2. **Log Monitoring** - Check error logs regularly
3. **Security Updates** - Keep PHP and dependencies updated
4. **Performance Monitoring** - Monitor server resources

### Backup Script Example

```bash
#!/bin/bash
mysqldump -u root -p bugradev_hizmetsepetim_db > backup_$(date +%Y%m%d).sql
```

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

