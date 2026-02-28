---
title: Cara Update CBT
---

# Panduan Update CBT

## Prasyarat Update

- Sistem operasi yang sama dengan instalasi sebelumnya (Linux/Windows)
- Akses root/sudo untuk Linux atau Administrator untuk Windows
- License CBT yang masih aktif dan valid
- Backup database dan file penting sebelum melakukan update

## Informasi Umum Update

### Update Bertahap
**PENTING:** Update harus dilakukan secara bertahap. Tidak bisa langsung dari versi lama ke versi yang sangat jauh.

**Contoh update yang benar:**
- Dari versi 4.4.0 → 4.5.0 → 4.6.0 → 4.7.0
- **Bukan:** Dari versi 4.4.0 langsung ke 4.7.0

### License Key
- License key yang sama bisa digunakan untuk update
- Tidak perlu generate license baru selama:
  - Tidak ganti mesin/server
  - License belum expired
  - Tidak melebihi limit pengguna

### File SQL Update
- Setiap versi mungkin memiliki file SQL update yang perlu dijalankan
- File SQL biasanya memiliki format: `update_dari_[versi-lama]_ke_[versi-baru].sql`
- **Contoh:** `update_dari_4.5.0_ke_4.6.0.sql`
- Hanya jalankan file SQL yang sesuai dengan versi asal Anda
- Jika tidak ada file SQL untuk versi Anda, berarti tidak perlu menjalankan SQL update

## 1. Persiapan Sebelum Update

### Backup Data Penting
Sebelum melakukan update, pastikan untuk melakukan backup data berikut:

1. **Backup Database PostgreSQL**
2. **Backup File Konfigurasi (.env)**
3. **Backup File Binary Lama**
4. **Backup Direktori Storage** (jika diperlukan)

## 2. Update di Linux

### 2.1 Persiapan Linux

1. **Stop Service CBT:**
   ```bash
   # Jika menggunakan systemd service
   sudo systemctl stop cbt.service
   
   # Atau jika berjalan manual
   pkill main-amd64
   ```

2. **Backup Database PostgreSQL:**
   ```bash
   # Masuk sebagai user postgres
   sudo -i -u postgres
   
   # Backup database
   pg_dump cbt_db > /path/backup/cbt_backup_$(date +%Y%m%d).sql
   
   # Keluar dari user postgres
   exit
   ```

3. **Backup File Konfigurasi:**
   ```bash
   # Backup file .env
   cp /var/www/cbt/.env /path/backup/.env_backup_$(date +%Y%m%d)
   
   # Backup direktori storage (jika diperlukan)
   tar -czf /path/backup/storage_backup_$(date +%Y%m%d).tar.gz /var/www/cbt/storage/
   ```

### 2.2 Download Versi Terbaru untuk Linux

1. **Download file CBT terbaru:**
   - Buka [https://ekstraordinary.com/download](https://ekstraordinary.com/download) di browser
   - Download versi terbaru untuk Linux

   ```bash
   # Pilih direktori download, misalnya /tmp
   cd /tmp
   
   # Download file menggunakan wget
   wget "https://ekstraordinary.com/download/versi-terbaru-linux.zip"
   
   # Contoh:
   # wget "https://ekstraordinary.com/download/4.7.0-linux.zip"
   ```

2. **Ekstrak file yang telah didownload:**
   ```bash
   sudo unzip versi-terbaru-linux.zip -d /tmp/cbt-new
   
   # Contoh:
   # sudo unzip 4.7.0-linux.zip -d /tmp/cbt-new
   ```

### 2.3 Proses Update Linux

1. **Timpa File Binary Lama:**
   ```bash
   # Backup file binary lama
   cp /var/www/cbt/main-amd64 /var/www/cbt/main-amd64.backup
   
   # Copy file binary baru
   cp /tmp/cbt-new/main-amd64 /var/www/cbt/
   
   # Berikan permission eksekusi
   sudo chmod +x /var/www/cbt/main-amd64
   ```

2. **Update File SQL (Jika Diperlukan):**
   ```bash
   # Jalankan file SQL update jika ada
   sudo -i -u postgres psql cbt_db < /tmp/cbt-new/update_dari_4.5.0_ke_4.6.0.sql
   ```

3. **Pertahankan Konfigurasi:**
   - File `.env` dan konfigurasi lainnya biasanya tidak berubah
   - Pastikan license key di file `.env` masih valid

### 2.4 Verifikasi Update Linux

1. **Jalankan Aplikasi:**
   ```bash
   cd /var/www/cbt
   ./main-amd64
   ```

2. **Setup Kembali Systemd Service:**
   ```bash
   # Restart service
   sudo systemctl daemon-reload
   sudo systemctl restart cbt.service
   
   # Cek status service
   sudo systemctl status cbt.service
   
   # Cek log aplikasi
   sudo journalctl -u cbt.service -f
   ```

## 3. Update di Windows

### 3.1 Persiapan Windows

1. **Stop Service CBT:**
   - Buka "Services" (Windows + R, ketik `services.msc`)
   - Cari service "CBTService" atau service CBT Anda
   - Klik kanan → "Stop"

2. **Backup Database PostgreSQL:**
   - Buka pgAdmin 4
   - Klik kanan pada database `cbt_db`
   - Pilih "Backup..."
   - Pilih lokasi backup
   - Klik "Backup"

3. **Backup File Konfigurasi:**
   - Copy file `.env` ke lokasi backup
   - Copy folder `storage` ke lokasi backup (jika diperlukan)

### 3.2 Download Versi Terbaru untuk Windows

1. **Download file CBT terbaru:**
   - Buka [https://ekstraordinary.com/download](https://ekstraordinary.com/download) di browser
   - Download versi terbaru untuk Windows

2. **Extract file yang telah didownload:**
   - Klik kanan pada file ZIP yang didownload
   - Pilih "Extract All..."
   - Pilih folder tujuan, misalnya `D:\CBT-new`

### 3.3 Proses Update Windows

1. **Timpa File Binary Lama:**
   - Backup file `main.exe` lama (rename menjadi `main.exe.backup`)
   - Copy file `main.exe` baru dari folder extract ke folder CBT lama
   - Contoh: Copy dari `D:\CBT-new\main.exe` ke `D:\CBT\main.exe`

2. **Update File SQL (Jika Diperlukan):**
   - Buka pgAdmin 4
   - Klik kanan pada database `cbt_db`
   - Pilih "Query Tool"
   - Klik icon "Open File" (folder)
   - Pilih file SQL update (misal: `update_dari_4.5.0_ke_4.6.0.sql`)
   - Klik icon "Execute" (petir) atau tekan F5

3. **Pertahankan Konfigurasi:**
   - File `.env` dari instalasi lama tetap digunakan
   - Pastikan license key di file `.env` masih valid

### 3.4 Verifikasi Update Windows

1. **Jalankan Aplikasi:**
   - Double-click file `main.exe` di folder CBT
   - Pastikan aplikasi berjalan tanpa error

2. **Setup Kembali Windows Service:**
   - Buka Command Prompt sebagai Administrator
   - Navigasi ke folder NSSM (jika menggunakan NSSM)
   - Restart service:
     ```cmd
     nssm restart CBTService
     ```
   - Atau start manual melalui `services.msc`

## 4. Troubleshooting Update

### 4.1 Troubleshooting Linux

**Error "Version Mismatch":**
- Pastikan update dilakukan secara bertahap
- Periksa apakah ada file SQL yang terlewat

**Database Connection Error:**
- Periksa konfigurasi database di file `.env`
- Pastikan PostgreSQL service berjalan: `sudo systemctl status postgresql`
- Verifikasi user dan password database

**Rollback ke Versi Sebelumnya:**
```bash
# Stop service
sudo systemctl stop cbt.service

# Restore file binary lama
cp /var/www/cbt/main-amd64.backup /var/www/cbt/main-amd64

# Restore database dari backup
sudo -i -u postgres psql cbt_db < /path/backup/cbt_backup.sql

# Start service kembali
sudo systemctl start cbt.service
```

### 4.2 Troubleshooting Windows

**License Error Setelah Update:**
- Pastikan `SERVER_SECRET_LICENSE_KEY` di file `.env` masih sama
- License tetap bisa digunakan selama tidak ganti mesin
- Jika ganti mesin, perlu generate license baru di ecosystem

**PostgreSQL Connection Error:**
- Pastikan PostgreSQL service berjalan: 
  - Windows + R → `services.msc` → Cari "postgresql"
  - Pastikan status "Running"
- Periksa password PostgreSQL di file `.env`

**Port Already in Use:**
- Ubah `SERVER_PORT` di file `.env` ke port lain (misalnya 9999)
- Restart aplikasi
- Akses dengan port baru: `http://localhost:9999`

## 5. Best Practices Update

1. **Selalu Backup Sebelum Update:**
   - Database
   - File konfigurasi (.env)
   - File binary lama

2. **Update di Waktu Low Traffic:**
   - Lakukan update saat tidak ada ujian berjalan
   - Beri tahu pengguna tentang maintenance

3. **Test di Environment Staging:**
   - Jika memungkinkan, test update di server staging terlebih dahulu

4. **Dokumentasi Versi:**
   - Catat versi sebelum dan sesudah update
   - Dokumentasi perubahan yang signifikan

5. **Monitor Setelah Update:**
   - Pantau log aplikasi selama 24 jam pertama
   - Periksa performa dan stabilitas sistem

## 6. Verifikasi Akhir

### Untuk Linux:
1. **Versi Aplikasi:**
   ```bash
   ./main-amd64 --version
   ```

2. **Status Service:**
   ```bash
   sudo systemctl status cbt.service
   ```

### Untuk Windows:
1. **Versi Aplikasi:**
   - Jalankan `main.exe` dan lihat versi di output

2. **Status Service:**
   - Buka `services.msc` dan cek status CBTService

### Untuk Semua Platform:
3. **Akses Aplikasi:**
   - Buka browser dan akses `http://localhost:9988` (atau port yang digunakan)
   - Login dan test semua modul penting

4. **Data Integrity:**
   - Periksa apakah semua data masih utuh
   - Verifikasi jumlah soal, ujian, dan pengguna

## 7. Support dan Dokumentasi

Jika mengalami masalah saat update:
- Dokumentasi: [https://docs.ekstraordinary.com](https://docs.ekstraordinary.com)
- Telegram: [Telegram Group](https://t.me/+U6oCiWEC9Hi-u54k)
- Email: support@ekstraordinary.com

### Catatan Versi
- Selalu periksa changelog versi terbaru
- Perhatikan fitur baru dan perubahan yang mungkin mempengaruhi workflow
- Update minor biasanya aman, update major mungkin memerlukan penyesuaian

Dengan mengikuti panduan ini, proses update CBT seharusnya berjalan lancar. Selalu ingat untuk backup data penting sebelum melakukan update apapun.