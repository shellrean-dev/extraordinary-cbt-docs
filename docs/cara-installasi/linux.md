---
title: Installasi di Linux
---

# Panduan Installasi CBT di Linux

## Prasyarat

- Sistem operasi Linux (Ubuntu/Debian atau distribusi lainnya)
- Akses root/sudo
- License CBT yang didapat dari [ecosystem.ekstraordinary.com](https://ecosystem.ekstraordinary.com)

## Mendapatkan License dari Ecosystem

Sebelum menginstal CBT, Anda perlu mendapatkan license terlebih dahulu. Berikut adalah langkah-langkahnya:

1. **Buka website Ecosystem:**
   - Akses [ecosystem.ekstraordinary.com](https://ecosystem.ekstraordinary.com) di browser Anda

2. **Login atau Register:**
   - Jika sudah memiliki akun, login dengan email dan password Anda
   - Jika belum memiliki akun, "Daftar" untuk membuat akun baru

3. **Generate atau Copy License Key:**
   - Klik tombol "Generate License"
   - Copy license key yang muncul
   - Simpan license key di tempat yang aman

4. **Verifikasi License:**
   - Pastikan license key aktif dan valid
   - Periksa masa berlaku license

**Catatan:**
- License key diperlukan untuk mengaktifkan aplikasi CBT
- Setiap license key hanya bisa dipakai di satu server

## 1. Download dan Extract File CBT

1. **Download file CBT menggunakan wget:**
   - Buka [https://ekstraordinary.com/download](https://ekstraordinary.com/download) di browser
   - Klik kanan pada tombol Download dan pilih "Copy Link Address" (Salin Alamat Tautan)
   - Link akan dicopy ke clipboard Anda, misalnya: `https://ekstraordinary.com/download/4.7.0-linux.zip`

   ```bash
   # Pilih direktori tujuan, misalnya /var/www/cbt
   sudo mkdir -p /var/www/cbt
   cd /var/www/cbt
   
   # Download file menggunakan wget dengan link yang telah dicopy
   # Paste URL yang telah dicopy dari browser (contoh di bawah):
   wget "URL_YANG_TELAH_DICOPY_DARI_BROWSER"
   
   # Contoh:
   # wget "https://ekstraordinary.com/download/4.7.0-linux.zip"
   ```

2. **Ekstrak file yang telah didownload:**

   ```bash
   sudo apt install unzip -y  # Jika unzip belum terinstall
   sudo unzip <nama-file-yang-didownload>
   
   # Contoh:
   # sudo unzip 4.7.0-linux.zip
   ```

**Catatan Penting:**
- Anda dapat menggunakan direktori lain selain `/var/www/cbt`, misalnya `/opt/cbt`, `/home/user/cbt`, atau direktori pilihan Anda
- Format file download mungkin berupa:
  - `4.7.0-linux.zip` (format .zip)
  - `4.6.3-linux.zip` (format .zip)
  - `4.6.2-linux.zip` (format .zip)
- Pastikan nama file binary sesuai dengan arsitektur sistem Anda:
  - `main-amd64` untuk sistem x86_64/amd64
  - `main-arm64` untuk sistem ARM64
- Jika wget tidak tersedia, install dengan: `sudo apt install wget -y`
- Untuk file .tar.gz, pastikan tar sudah terinstall (biasanya sudah tersedia di sistem Linux)

## 2. Install PostgreSQL

```bash
# Update paket sistem
sudo apt update

# Install PostgreSQL dan tools tambahan
sudo apt install postgresql postgresql-contrib -y

# Verifikasi instalasi
sudo systemctl status postgresql
```

## 3. Setup Database PostgreSQL

1. Masuk ke akun postgres:

```bash
sudo -i -u postgres
```

2. Buka PostgreSQL CLI:

```bash
psql
```

3. Buat user database untuk CBT:

```sql
CREATE USER cbt_user WITH PASSWORD 'password_kuat_anda';
CREATE DATABASE cbt_db OWNER cbt_user;
GRANT ALL PRIVILEGES ON DATABASE cbt_db TO cbt_user;
\q
```

4. Keluar dari akun postgres:

```bash
exit
```

## 4. Setup Aplikasi CBT

1. Pastikan Anda berada di direktori CBT:

```bash
cd /var/www/cbt
```

2. Buat direktori storage:

```bash
sudo mkdir -p storage
sudo chmod -R 775 storage
```

**Catatan Storage:**
- Direktori storage akan digunakan untuk menyimpan file upload seperti gambar soal, dokumen, dan data ujian
- Pastikan direktori storage memiliki permission yang cukup untuk user yang menjalankan aplikasi (www-data atau user lainnya)
- Pastikan untuk mengubah `STORAGE_PATH` di file `.env` sesuai dengan path yang digunakan
- Direktori storage harus memiliki kapasitas yang cukup untuk menyimpan semua data ujian

## 5. Konfigurasi File .env

1. Edit file .env dengan text editor pilihan Anda:

```bash
sudo nano .env
```

3. Ubah konfigurasi berikut sesuai dengan setup Anda:

```env
# License yang didapat dari ecosystem.ekstraordinary.com
SERVER_SECRET_LICENSE_KEY=license_anda_di_sini

# Konfigurasi Database
DB_NAME=cbt_db
DB_USER=cbt_user
DB_PASS=password_kuat_anda
DB_HOST=localhost
DB_PORT=5432

# Path storage
STORAGE_PATH=/var/www/cbt/storage
```

**Catatan Konfigurasi .env:**
- Pastikan semua variabel di file `.env` diisi dengan benar sesuai dengan setup Anda
- `SERVER_SECRET_LICENSE_KEY` harus sesuai dengan license yang didapat dari ecosystem.ekstraordinary.com
- `STORAGE_PATH` harus mengarah ke direktori yang sudah dibuat dan memiliki permission yang tepat
- Untuk keamanan, gunakan password yang kuat untuk database (`DB_PASS`)

## 6. Menjalankan Aplikasi

1. Berikan permission eksekusi pada file binary:

```bash
sudo chmod +x main-amd64
```

2. Jalankan aplikasi:

```bash
./main-amd64
```

3. Jika berjalan dengan baik, aplikasi akan mengeluarkan output  Anda akan melihat output seperti:

```

  _____      _                           _ _                           ____ ____ _____ 
 | ____|_  _| |_ _ __ __ _  ___  _ __ __| (_)_ __   __ _ _ __ _   _   / ___| __ )_   _|
 |  _| \ \/ / __| '__/ _` |/ _ \| '__/ _` | | '_ \ / _` | '__| | | | | |   |  _ \ | |  
 | |___ >  <| |_| | | (_| | (_) | | | (_| | | | | | (_| | |  | |_| | | |___| |_) || |  
 |_____/_/\_\\__|_|  \__,_|\___/|_|  \__,_|_|_| |_|\__,_|_|   \__, |  \____|____/ |_|  
                                                              |___/                    

Author: shellrean <info@shellrean.id>
Version:  4.7.0-ROSETTA-RELEASE
```

4. Buka browser dan akses `http://localhost:9988` untuk memverifikasi instalasi.

## 7. Setup Systemd Service (Opsional)

Untuk menjalankan CBT sebagai service di background dan auto-start saat boot:

1. Buat file service systemd:

```bash
sudo nano /etc/systemd/system/cbt.service
```

2. Tambahkan konfigurasi berikut:

```ini
[Unit]
Description=CBT Application Service
After=network.target postgresql.service

[Service]
Type=simple
User=www-data
Group=www-data
WorkingDirectory=/var/www/cbt
ExecStart=/var/www/cbt/main-amd64
Restart=always
RestartSec=10
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=cbt

[Install]
WantedBy=multi-user.target
```

3. Reload systemd dan aktifkan service:

```bash
# Reload systemd daemon
sudo systemctl daemon-reload

# Aktifkan service agar auto-start saat boot
sudo systemctl enable cbt.service

# Jalankan service
sudo systemctl start cbt.service

# Cek status service
sudo systemctl status cbt.service
```

**Catatan Konfigurasi Systemd Service:**
- File service systemd (`/etc/systemd/system/cbt.service`) memungkinkan CBT berjalan otomatis saat boot
- Konfigurasi `User=www-data` dan `Group=www-data` dapat disesuaikan dengan user sistem Anda
- Jika menggunakan user lain, pastikan user tersebut memiliki permission untuk mengakses direktori CBT
- `Restart=always` memastikan aplikasi restart otomatis jika crash
- `RestartSec=10` memberi jeda 10 detik sebelum restart
- `WorkingDirectory=/var/www/cbt` harus sesuai dengan path instalasi CBT Anda
- `ExecStart=/var/www/cbt/main-amd64` harus sesuai dengan path dan nama file binary CBT
- Untuk melihat log: `sudo journalctl -u cbt.service -f`
- Untuk stop service: `sudo systemctl stop cbt.service`
- Untuk restart service: `sudo systemctl restart cbt.service`
- Untuk disable auto-start: `sudo systemctl disable cbt.service`


## 8. Troubleshooting

### PostgreSQL Connection Error
- Pastikan PostgreSQL berjalan: `sudo systemctl status postgresql`
- Periksa koneksi database: `sudo -u postgres psql -c "\l"`
- Verifikasi user dan password di file .env

### Permission Denied
- Pastikan user yang menjalankan service memiliki akses ke direktori CBT:
  ```bash
  sudo chown -R www-data:www-data /var/www/cbt
  sudo chmod -R 755 /var/www/cbt
  ```

### Port Already in Use
- Ubah `SERVER_PORT` di file .env ke port lain (misalnya 9988)
- Pastikan port tidak digunakan: `sudo netstat -tulpn | grep :9988`

### License Error
- Pastikan `SERVER_SECRET_LICENSE_KEY` di file .env sesuai dengan license dari ecosystem
- Verifikasi koneksi internet untuk validasi license

## 9. Verifikasi Instalasi

1. Cek apakah service berjalan:

```bash
sudo systemctl status cbt.service
```

2. Cek log aplikasi:

```bash
sudo journalctl -u cbt.service -f
```

3. Akses aplikasi melalui browser di `http://server-ip-anda:9988`

## Support

Jika mengalami masalah, kunjungi:
- Dokumentasi: [https://docs.ekstraordinary.com](https://docs.ekstraordinary.com)
- Telegram: [Telegram Group](https://t.me/+U6oCiWEC9Hi-u54k)
