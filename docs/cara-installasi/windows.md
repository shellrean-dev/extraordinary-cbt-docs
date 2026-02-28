---
title: Installasi di Windows
---

# Panduan Installasi CBT di Windows

## Prasyarat

- Sistem operasi Windows 10 atau 11 (64-bit)
- Akses administrator untuk install software
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

1. **Download file CBT:**
   - Buka [https://ekstraordinary.com/download](https://ekstraordinary.com/download) di browser
   - Klik tombol "Download for Windows" atau link download yang sesuai

2. **Extract file yang telah didownload:**
   - Klik kanan pada file ZIP yang didownload
   - Pilih "Extract All..."
   - Pilih folder tujuan, misalnya `D:\CBT` atau `C:\CBT`
   - Klik "Extract"

**Catatan Penting:**
- Anda dapat menggunakan direktori lain sesuai preferensi Anda
- Format file download biasanya berupa `.zip`
- Pastikan folder tujuan memiliki cukup ruang penyimpanan

## 2. Install PostgreSQL

1. **Download PostgreSQL Installer:**
   - Buka [https://www.postgresql.org/download/windows/](https://www.postgresql.org/download/windows/)
   - Download installer versi terbaru untuk Windows

2. **Jalankan Installer:**
   - Double-click file installer (misal: `postgresql-15-windows-x64.exe`)
   - Ikuti wizard instalasi dengan klik "Next"
   - **Penting:** Catat password yang Anda buat untuk user `postgres`
   - Biarkan port default `5432`
   - Selesaikan instalasi

3. **Verifikasi Instalasi:**
   - Cari "pgAdmin 4" di Start Menu
   - Buka pgAdmin untuk memastikan PostgreSQL terinstall dengan baik

## 3. Setup Database PostgreSQL

1. **Buka pgAdmin:**
   - Cari "pgAdmin 4" di Start Menu dan buka aplikasi
   - Masukkan password yang dibuat saat instalasi

2. **Buat Database:**
   - Di panel kiri, klik kanan pada "Databases"
   - Pilih "Create" → "Database"
   - Isi:
     - **Database:** `cbt_db`
     - **Owner:** `postgres`
   - Klik "Save"

## 4. Import Data SQL

1. **Cari file SQL:**
   - Di folder CBT yang sudah diextract, cari file `exo-dump-master.sql`

2. **Import di pgAdmin:**
   - Di pgAdmin, klik kanan pada database `cbt_db`
   - Pilih "Query Tool"
   - Klik icon "Open File" (folder)
   - Pilih file `exo-dump-master.sql`
   - Klik icon "Execute" (petir) atau tekan F5
   - Tunggu sampai proses import selesai

## 5. Setup Aplikasi CBT

1. **Buat direktori storage:**
   - Di folder CBT (misal: `D:\CBT`)
   - Klik kanan → "New" → "Folder"
   - Beri nama folder `storage`

2. **Berikan permission (jika perlu):**
   - Klik kanan pada folder `storage`
   - Pilih "Properties" → "Security"
   - Pastikan user memiliki permission "Modify"

## 6. Konfigurasi File .env

1. **Edit file .env:**
   - Di folder CBT, cari file `.env`
   - Klik kanan → "Open with" → "Notepad" atau text editor lain

2. **Ubah konfigurasi berikut:**

```env
# License yang didapat dari ecosystem.ekstraordinary.com
SERVER_SECRET_LICENSE_KEY=license_anda_di_sini

# Konfigurasi Database
DB_NAME=cbt_db
DB_USER=postgres
DB_PASS=password_yang_dibuat_saat_install_postgres
DB_HOST=localhost
DB_PORT=5432

# Path storage (sesuaikan dengan lokasi folder CBT)
STORAGE_PATH=D:\CBT\storage

# Port aplikasi
SERVER_PORT=9988
```

**Catatan Konfigurasi .env:**
- Pastikan semua variabel di file `.env` diisi dengan benar
- `SERVER_SECRET_LICENSE_KEY` harus sesuai dengan license dari ecosystem.ekstraordinary.com
- `DB_PASS` harus sesuai dengan password PostgreSQL yang dibuat saat instalasi
- `STORAGE_PATH` harus mengarah ke folder `storage` yang sudah dibuat
- Sesuaikan `STORAGE_PATH` jika folder CBT berada di lokasi lain

## 7. Menjalankan Aplikasi

1. **Cari file executable:**
   - Di folder CBT, cari file `main.exe` atau `main`

2. **Jalankan aplikasi:**
   - Double-click file `main.exe`
   - Jika berjalan dengan baik, akan muncul command prompt dengan output:

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

3. **Verifikasi instalasi:**
   - Buka browser (Chrome, Firefox, Edge)
   - Akses `http://localhost:9988`
   - Aplikasi CBT seharusnya sudah bisa diakses

## 8. Setup Windows Service (Opsional)

Untuk menjalankan CBT sebagai service di Windows:

1. **Download NSSM (Non-Sucking Service Manager):**
   - Download dari https://nssm.cc/download
   - Extract ke folder tertentu

2. **Install sebagai service:**
   - Buka Command Prompt sebagai Administrator
   - Navigasi ke folder NSSM
   - Jalankan perintah:

```cmd
nssm install CBTService
```

3. **Konfigurasi service:**
   - Di window NSSM yang muncul:
     - **Path:** Browse ke `D:\CBT\main.exe` (sesuaikan lokasi)
     - **Startup directory:** `D:\CBT` (folder CBT)
   - Klik "Install service"

4. **Kelola service:**
   - Buka "Services" (Windows + R, ketik `services.msc`)
   - Cari service "CBTService"
   - Klik kanan → "Start"
   - Untuk auto-start: Klik kanan → "Properties" → Set "Startup type" ke "Automatic"

**Catatan Service:**
- Service memungkinkan CBT berjalan otomatis saat Windows startup
- Untuk melihat log: Buka Event Viewer → Windows Logs → Application
- Untuk stop service: `services.msc` → CBTService → Stop
- Untuk uninstall: `nssm remove CBTService`

## 9. Troubleshooting

### PostgreSQL Connection Error
- Pastikan PostgreSQL service berjalan: 
  - Windows + R → `services.msc` → Cari "postgresql"
  - Pastikan status "Running"
- Periksa password PostgreSQL di file `.env`

### Permission Denied
- Pastikan folder CBT dan `storage` memiliki permission yang cukup
- Coba jalankan `main.exe` sebagai Administrator

### Port Already in Use
- Ubah `SERVER_PORT` di file `.env` ke port lain (misalnya 9999)
- Restart aplikasi
- Akses dengan port baru: `http://localhost:9999`

### License Error
- Pastikan `SERVER_SECRET_LICENSE_KEY` di file `.env` sesuai dengan license dari ecosystem
- Verifikasi koneksi internet untuk validasi license
- Pastikan tidak ada spasi di awal/akhir license key

### Tidak Bisa Diakses dari Browser
- Pastikan aplikasi masih berjalan (lihat di taskbar)
- Coba browser lain
- Clear cache browser: `Ctrl + Shift + Delete`

### Akses dari Jaringan Tidak Bisa
1. **Cari IP Address komputer:**
   - Buka Command Prompt
   - Ketik `ipconfig`
   - Catat IPv4 Address (misal: `192.168.1.100`)

2. **Konfigurasi Firewall:**
   - Buka Windows Defender Firewall
   - Pilih "Allow an app through firewall"
   - Klik "Change settings" → "Allow another app"
   - Browse ke `D:\CBT\main.exe`
   - Centang "Private" dan "Public"
   - Klik "OK"

3. **Akses dari device lain:**
   - Pastikan dalam satu jaringan
   - Akses `http://192.168.1.100:9988` (ganti dengan IP komputer Anda)

## 10. Verifikasi Instalasi

1. **Cek apakah aplikasi berjalan:**
   - Lihat di taskbar atau Task Manager
   - Command prompt harus tetap terbuka dengan output CBT

2. **Akses aplikasi:**
   - Buka browser di komputer server: `http://localhost:9988`
   - Buka browser di device lain: `http://[IP_SERVER]:9988`

3. **Test fungsi dasar:**
   - Login dengan credential default
   - Buat user baru
   - Buat soal ujian sederhana

## Support

Jika mengalami masalah, kunjungi:
- Dokumentasi: [https://docs.ekstraordinary.com](https://docs.ekstraordinary.com)
- Telegram: [Telegram Group](https://t.me/+U6oCiWEC9Hi-u54k)