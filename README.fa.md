# Backup Pro

Quick install / نصب سریع

```bash
sudo bash -c "$(curl -sL https://github.com/Mehrdad11228/Backup_Pro/raw/main/Backup-Transfor.sh)"
```

Manual run / اجرای دستی

```bash
curl -fsSL -o Backup-Transfor.sh https://github.com/Mehrdad11228/Backup_Pro/raw/main/Backup-Transfor.sh
chmod +x Backup-Transfor.sh
sudo ./Backup-Transfor.sh
```

---

## English

### Overview
Backup Pro is an interactive Bash script that sets up scheduled backups and transfer workflows for **Marzneshin**, **Pasarguard**, **X-ui**, and **Marzban**.  
It detects the database type, collects panel files, optionally dumps the database, compresses the backup, and sends it to Telegram.  
It also supports transferring backups to a remote server and restarting the remote panel service.

### Features
- Pretty terminal UI with step-by-step prompts
- Auto-detects DB type from `.env` or `docker-compose.yml`
- Generates per-panel backup script in `/root` and schedules it with `cron`
- Multiple compression formats: `zip`, `tgz`, `7z`, `tar`, `gzip/gz`
- Telegram report + file upload
- Transfer mode with `rsync + sshpass` and optional DB dump
- Auto-cleanup after backup

### Compatibility / Requirements
- Debian/Ubuntu (apt-based)
- Run as **root**
- Internet access for apt + Telegram API + optional X-ui install
- Tools used: `zip`, `tar`, `gzip`, `7z`, `rsync`, `sshpass`, `mysqldump`, `curl`
- Docker required for **PostgreSQL** dump in Pasarguard
- Remote server: SSH access + panel restart command available

### Panels & Data Paths
| Panel | Data paths backed up | DB detection source | DB handling |
|---|---|---|---|
| Marzneshin | `/etc/opt/marzneshin`, `/var/lib/marzneshin`, `/var/lib/marznode` (only `xray_config.json`) | `/etc/opt/marzneshin/docker-compose.yml` | SQLite: files, MySQL/MariaDB: `mysqldump` |
| Marzban | `/opt/marzban`, `/var/lib/marzban` | `/opt/marzban/.env` | SQLite: files, MySQL/MariaDB: `mysqldump` |
| Pasarguard | `/opt/pasarguard`, `/opt/pg-node`, `/var/lib/pasarguard`, `/var/lib/pg-node` | `/opt/pasarguard/.env` | SQLite: files, MySQL/MariaDB: `mysqldump`, PostgreSQL: `pg_dump` (docker) |
| X-ui | `/etc/x-ui`, `/root/cert` | N/A | No DB dump |

Note: Transfer mode may also copy MySQL/PostgreSQL data directories (Pasarguard) and optionally a DB dump folder.

### Script Flow (Tree Diagram)
```
Backuper Menu
|-- Install Backuper
|   |-- Select panel (Marzneshin / Pasarguard / X-ui / Marzban)
|   |-- Telegram token + chat id
|   |-- Compression type
|   |-- Caption (optional)
|   |-- Schedule (minutes / hours)
|   |-- Detect DB type
|   |-- Create /root/<panel>_backup.sh
|   |-- Add cron
|   `-- Run first backup
|-- Remove Backuper
|   |-- Delete /root/*_backup.sh
|   |-- Remove /root/backuper_*
|   `-- Clean cron entries
|-- Run Script
|   `-- Run existing backup script
|-- Transfer Backup
|   |-- Validate local panel folders
|   |-- Remote credentials
|   |-- Optional DB dump
|   |-- Rsync data to remote
|   `-- Restart remote service
`-- Exit
```

### Usage
1. Run the install command above.  
2. Choose **Install Backuper** from the menu.  
3. Enter Telegram token, Chat ID, compression type, caption, and schedule.  
4. The script creates `/root/<panel>_backup.sh`, adds cron, and runs the first backup.

### Transfer Mode Notes
- **Remote target paths are deleted** before copying.
- Optional DB dump transfer; for Pasarguard it can also copy MySQL/PostgreSQL data directories.
- Remote restart command is triggered (`marzneshin`, `marzban`, `pasarguard`, `x-ui`).

### Files Created
- `/root/<panel>_backup.sh`
- `/root/backuper_<panel>/backup_<timestamp>.<ext>` (temporary)
- Root cron entry for scheduled runs

### Telegram Notes
- Sends HTML-formatted report + archive
- Warns when backup is > 50 MB (Telegram limits may block large files)

### Important Notes
- Script runs `apt update && apt upgrade` automatically at startup.
- Transfer uses `sshpass` (password-based SSH). SSH keys are safer but not implemented yet.

---

## فارسی

### معرفی
Backup Pro یک اسکریپت تعاملی Bash است که برای پنل‌های **Marzneshin**، **Pasarguard**، **X-ui** و **Marzban**  
بکاپ زمان‌بندی‌شده و انتقال داده راه‌اندازی می‌کند. نوع دیتابیس را تشخیص می‌دهد، فایل‌ها را جمع‌آوری می‌کند،  
در صورت نیاز از دیتابیس خروجی می‌گیرد، آرشیو می‌کند و برای تلگرام ارسال می‌کند.  
همچنین قابلیت انتقال به سرور دیگر و ری‌استارت سرویس مقصد را دارد.

### قابلیت‌ها
- رابط کاربری ترمینال با مراحل واضح
- تشخیص خودکار دیتابیس از `.env` یا `docker-compose.yml`
- ساخت اسکریپت بکاپ اختصاصی در `/root` و زمان‌بندی با `cron`
- چند نوع فشرده‌سازی: `zip`, `tgz`, `7z`, `tar`, `gzip/gz`
- گزارش و ارسال فایل در تلگرام
- انتقال بکاپ با `rsync + sshpass` و خروجی دیتابیس اختیاری
- پاکسازی خودکار بعد از بکاپ

### پیش‌نیازها
- Debian/Ubuntu (apt)
- اجرا با **دسترسی روت**
- اینترنت برای apt و تلگرام و نصب اختیاری X-ui
- ابزارهای موردنیاز: `zip`, `tar`, `gzip`, `7z`, `rsync`, `sshpass`, `mysqldump`, `curl`
- برای بکاپ PostgreSQL در Pasarguard نیاز به Docker
- روی سرور مقصد: دسترسی SSH و دستورهای restart پنل

### پنل‌ها و مسیرها
| پنل | مسیرهای بکاپ | منبع تشخیص دیتابیس | نحوه بکاپ دیتابیس |
|---|---|---|---|
| Marzneshin | `/etc/opt/marzneshin`, `/var/lib/marzneshin`, `/var/lib/marznode` (فقط `xray_config.json`) | `/etc/opt/marzneshin/docker-compose.yml` | SQLite: فایل‌ها، MySQL/MariaDB: `mysqldump` |
| Marzban | `/opt/marzban`, `/var/lib/marzban` | `/opt/marzban/.env` | SQLite: فایل‌ها، MySQL/MariaDB: `mysqldump` |
| Pasarguard | `/opt/pasarguard`, `/opt/pg-node`, `/var/lib/pasarguard`, `/var/lib/pg-node` | `/opt/pasarguard/.env` | SQLite: فایل‌ها، MySQL/MariaDB: `mysqldump`, PostgreSQL: `pg_dump` (docker) |
| X-ui | `/etc/x-ui`, `/root/cert` | N/A | بدون خروجی دیتابیس |

نکته: در حالت انتقال ممکن است مسیرهای دیتابیس MySQL/PostgreSQL هم کپی شوند (Pasarguard) و پوشه Dump نیز ارسال شود.

### نمودار شاخه‌ای
```
منوی اصلی Backuper
|-- نصب بکاپر
|   |-- انتخاب پنل (Marzneshin / Pasarguard / X-ui / Marzban)
|   |-- وارد کردن توکن تلگرام و Chat ID
|   |-- انتخاب نوع فشرده‌سازی
|   |-- کپشن (اختیاری)
|   |-- زمان‌بندی (دقیقه / ساعت)
|   |-- تشخیص نوع دیتابیس
|   |-- ساخت /root/<panel>_backup.sh
|   |-- افزودن کرون‌جاب
|   `-- اجرای اولین بکاپ
|-- حذف بکاپر
|   |-- حذف /root/*_backup.sh
|   |-- حذف /root/backuper_*
|   `-- پاکسازی کرون‌جاب‌ها
|-- اجرای دستی اسکریپت
|   `-- اجرای بکاپ موجود
|-- انتقال بکاپ
|   |-- بررسی مسیرهای محلی
|   |-- گرفتن اطلاعات سرور مقصد
|   |-- خروجی دیتابیس (اختیاری)
|   |-- انتقال با rsync
|   `-- ری‌استارت سرویس مقصد
`-- خروج
```

### نحوه استفاده
1. دستور نصب بالا را اجرا کنید.  
2. از منو گزینه **Install Backuper** را انتخاب کنید.  
3. توکن تلگرام، Chat ID، نوع فشرده‌سازی، کپشن و زمان‌بندی را وارد کنید.  
4. اسکریپت فایل `/root/<panel>_backup.sh` می‌سازد، کرون‌جاب اضافه می‌کند و اولین بکاپ را اجرا می‌کند.

### نکات انتقال
- مسیرهای مقصد **قبل از انتقال پاک می‌شوند**.
- ارسال Dump دیتابیس اختیاری است؛ برای Pasarguard ممکن است دیتای MySQL/PostgreSQL هم کپی شود.
- دستور ری‌استارت سرویس مقصد اجرا می‌شود (`marzneshin`, `marzban`, `pasarguard`, `x-ui`).

### فایل‌های ایجادشده
- `/root/<panel>_backup.sh`
- `/root/backuper_<panel>/backup_<timestamp>.<ext>` (موقت)
- کرون‌جاب روت برای اجرای زمان‌بندی‌شده

### نکات تلگرام
- گزارش HTML و فایل آرشیو ارسال می‌شود
- برای فایل بزرگ‌تر از 50MB هشدار ارسال می‌شود

### نکات مهم
- در شروع اجرا `apt update && apt upgrade` انجام می‌شود.
- انتقال با `sshpass` و پسورد انجام می‌شود؛ برای امنیت بیشتر بهتر است از SSH key استفاده شود (فعلاً در اسکریپت پیاده نشده).

