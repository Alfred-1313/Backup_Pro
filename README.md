```md
# Backup_Pro (Backup & Transfer)

یک ابزار ساده و منویی برای **بکاپ + انتقال (Transfer)** پنل‌ها، همراه با **گزارش تلگرام** و **زمان‌بندی کرون**.

## نصب / اجرا سریع

```bash
sudo bash -c "$(curl -sL https://github.com/Mehrdad11228/Backup_Pro/raw/main/Backup-Transfor.sh)"
پنل‌های پشتیبانی‌شده
Marzneshin

Pasarguard

Marzban

X-ui (فقط کانفیگ و سرتیفیکیت)

این اسکریپت چه کاری انجام می‌دهد؟
نصب پیش‌نیازها (zip/tar/7z، کلاینت دیتابیس‌ها، sshpass، rsync و…)

نمایش منوی اصلی:

نصب بکاپ‌گیر

حذف بکاپ‌گیر

اجرای بکاپ دستی

انتقال بکاپ به سرور دیگر

ساخت اسکریپت بکاپ اختصاصی هر پنل داخل /root/

تنظیم زمان‌بندی بکاپ با crontab

ارسال فایل بکاپ + گزارش به Telegram

Backup (Flow)
text
Copy code
Backup
├─ Detect Panel & DB Type
├─ Copy Required Paths
├─ (Optional) Database Dump
├─ Compress (zip/tgz/7z/tar/gzip)
└─ Send to Telegram + Cleanup
Transfer (Flow)
text
Copy code
Transfer
├─ Select Panel
├─ (Optional) DB Dump on Source
├─ Copy Data to Temp Folder
├─ Remote Cleanup (delete old paths)
├─ Rsync to Remote (sshpass)
└─ Restart Panel on Remote (nohup) + Report
محتوای بکاپ (برای هر پنل)
Marzneshin
مسیرها:

/etc/opt/marzneshin/

/var/lib/marzneshin/

/var/lib/marznode/ (فقط فایل‌های لازم)

دیتابیس:

SQLite: داخل فایل‌ها موجود است

MySQL/MariaDB: تلاش برای dump با اطلاعات docker-compose.yml

Marzban
مسیرها:

/opt/marzban/

/var/lib/marzban/

دیتابیس:

SQLite: داخل فایل‌ها موجود است

MySQL/MariaDB: dump از روی .env (متغیر MYSQL_PASSWORD)

Pasarguard
مسیرها:

/opt/pasarguard/

/opt/pg-node/

/var/lib/pasarguard/

/var/lib/pg-node/

دیتابیس:

SQLite: داخل فایل‌ها موجود است

MySQL/MariaDB: dump از SQLALCHEMY_DATABASE_URL

PostgreSQL: dump با docker exec pg_dump (تشخیص خودکار کانتینر postgres)

X-ui
مسیرها:

/etc/x-ui/

/root/cert/

نکات مهم
اسکریپت باید با root اجرا شود (sudo).

تلگرام ممکن است فایل‌های حجیم را قبول نکند؛ اگر حجم بیشتر از 50MB باشد هشدار می‌دهد.

در حالت Transfer، مسیرهای مقصد روی سرور ریموت قبل از انتقال پاکسازی و جایگزین می‌شوند.

فایل‌های ساخته‌شده
اسکریپت‌های بکاپ:

/root/marzneshin_backup.sh

/root/marzban_backup.sh


- 🇺🇸 English: [README.en.md](./README.en.md)

/root/pasarguard_backup.sh

/root/x-ui_backup.sh
