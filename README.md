# 🛡️ ابزار جامع Backup_Pro
**مدیریت هوشمند بک‌آپ و انتقال سرور به سرور پنل‌های VPN**

[🌐 Switch to English Documentation](readme.en.md)

اسکریپت **Backup_Pro** راهکاری سبک و سریع برای خودکارسازی بک‌آپ‌گیری (ارسال به تلگرام) و مهاجرت (انتقال کامل به سرور جدید) است. این ابزار به طور هوشمند دیتابیس و فایل‌های پیکربندی را شناسایی و مدیریت می‌کند.

### 📋 پنل‌های تحت پشتیبانی و مسیرها
| نام پنل | مسیرهای مورد استفاده (Backup/Transfer) |
| :--- | :--- |
| **Marzban** | `/opt/marzban` , `/var/lib/marzban` |
| **Marzneshin** | `/etc/opt/marzneshin` , `/var/lib/marznode` , `/var/lib/marzneshin` |
| **Pasarguard** | `/opt/pasarguard` , `/opt/pg-node` , `/var/lib/pasarguard` , `/var/lib/pg-node` |
| **X-UI** | `/etc/x-ui` , `/root/cert/` |

### 🚀 نصب و اجرا
```bash
sudo bash -c "$(curl -sL [https://github.com/Mehrdad11228/Backup_Pro/raw/main/Backup-Transfor.sh](https://github.com/Mehrdad11228/Backup_Pro/raw/main/Backup-Transfor.sh))"
