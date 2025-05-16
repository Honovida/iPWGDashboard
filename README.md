
# 📡 iPWGDashboard  

**مدیریت پیشرفته و حرفه‌ای سرورهای WireGuard VPN همراه با داشبورد گرافیکی و API قدرتمند.**

---

## 📖 فهرست مطالب

- [ویژگی‌ها](#ویژگی‌ها)  
- [نصب سریع](#نصب-سریع)  
- [نیازمندی‌ها](#نیازمندی‌ها)  
- [راهنمای استفاده](#راهنمای-استفاده)  
- [API Endpoints](#api-endpoints)  
- [پشتیبان‌گیری و بازیابی](#پشتیبان‌گیری-و-بازیابی)  
- [ساختار پروژه](#ساختار-پروژه)  
- [فعال‌سازی SSL](#فعال‌سازی-ssl)  
- [لایسنس](#لایسنس)  

---

## 📋 ویژگی‌ها

- ✅ مدیریت کامل سرور WireGuard  
- ✅ تعریف و مدیریت کاربران (Peers)  
- ✅ تعیین محدودیت پهنای باند و زمان برای هر کاربر  
- ✅ تولید QR Code تنظیمات کاربران  
- ✅ مانیتورینگ زنده منابع سیستم (CPU, RAM, Network)  
- ✅ ارسال هشدار از طریق تلگرام  
- ✅ پشتیبان‌گیری و بازیابی تنظیمات کاربران و سرور  
- ✅ نصب و راه‌اندازی سریع تنها با یک دستور  
- ✅ رابط کاربری مدرن، واکنش‌گرا و مبتنی بر Bootstrap 5  

---

## 🚀 نصب سریع

```bash
apt update && apt install git -y && git clone https://github.com/iPmartNetwork/iPWGDashboard && cd iPWGDashboard && chmod +x install.sh && ./install.sh

```

---

## 🛠️ نیازمندی‌ها

- سیستم عامل: Ubuntu 20.04/22.04  
- Python 3.8+  
- WireGuard  
- Nginx (برای SSL و Reverse Proxy)  
- Certbot (برای صدور گواهی SSL)  

---

## 📚 راهنمای استفاده

### اجرای داشبورد:

```bash
cd iPWGDashboard
./install.sh  # نصب مجدد یا بروزرسانی
python3 src/dashboard.py
```

### آدرس پیش‌فرض داشبورد:

```
https://<your-server-ip>:10086
```

---

## 📡 API Endpoints  

### 📌 افزودن کاربر جدید  

```http
POST /api/add_peer
```

| پارامتر | توضیح             |
|---------|-------------------|
| name    | نام کاربر          |
| limit   | محدودیت حجم (MB)   |
| time    | محدودیت زمان (دقیقه) |

**مثال:**

```bash
curl -X POST https://<server>/api/add_peer -d "name=user1&limit=5000&time=1440"
```

---

### 📌 دریافت لیست کاربران  

```http
GET /api/peers_status
```

**مثال:**

```bash
curl https://<server>/api/peers_status
```

---

### 📌 حذف کاربر  

```http
POST /api/remove_peer
```

| پارامتر | توضیح    |
|---------|----------|
| name    | نام کاربر |

---

## 📦 پشتیبان‌گیری و بازیابی  

### پشتیبان‌گیری:

```bash
python3 src/Utilities.py --backup
```

### بازیابی تنظیمات:

```bash
python3 src/Utilities.py --restore /path/to/backup_file.json
```

---

## 📂 ساختار پروژه  

```
iPWGDashboard/
├── install.sh           # اسکریپت نصب سریع
└── src/
    ├── dashboard.py     # فایل اصلی اجرای داشبورد (Backend)
    ├── Utilities.py     # ابزارهای کمکی (Backup, Restore, Monitoring)
    ├── gunicorn.conf.py # تنظیمات گونی‌کورن برای اجرای بهینه
    └── certbot.ini      # تنظیمات صدور گواهی SSL
```

### 📄 توضیحات فایل‌ها:

- **install.sh**: نصب خودکار پیش‌نیازها و راه‌اندازی پروژه  
- **dashboard.py**: پیاده‌سازی Backend و API با استفاده از Flask  
- **Utilities.py**: شامل توابع پشتیبان‌گیری، بازیابی و مدیریت منابع سیستم  
- **gunicorn.conf.py**: بهینه‌سازی اجرای برنامه برای محیط‌های تولیدی  
- **certbot.ini**: تنظیمات لازم برای Certbot جهت صدور گواهی SSL  

---

## 🔒 فعال‌سازی SSL  

SSL به صورت خودکار از طریق Certbot فعال می‌شود. همچنین می‌توانید به صورت دستی تنظیمات Nginx را به شکل زیر انجام دهید:

```nginx
server {
    listen 443 ssl;
    server_name your-domain.com;

    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:10086;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## 📬 اطلاع‌رسانی تلگرام  

برای فعال کردن هشدارهای تلگرام، Bot Token و Chat ID را در فایل `src/certbot.ini` تنظیم کنید.

---

## 📖 لایسنس  

این پروژه تحت لایسنس **MIT** ارائه می‌شود. استفاده، توزیع و تغییر آن کاملاً آزاد است.
