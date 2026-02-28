# Tidak Bisa Menjalankan App

## 1. Error HTTP-CODE: 0

Jika Anda mengalami error dengan pesan "HTTP-CODE: 0", ini biasanya menunjukkan bahwa server Anda tidak terhubung dengan ekosistem.

**Solusi:**
- Periksa koneksi internet server Anda
- Pastikan server dapat mengakses ekosistem CBT
- Cek firewall atau proxy yang mungkin memblokir koneksi

## 2. Berapa Banyak Maksimal License Key?

Maksimal license key yang dapat aktif bersamaan adalah **4 license**.

## 3. Apakah Setelah 4 Bisa Tetap Pakai CBT?

Setelah memiliki 4 license aktif, jika ada license yang sudah expired, maka akan muncul slot tambahan untuk generate license key baru. Jadi, 4 adalah total maksimal license yang aktif bersamaan pada waktu yang sama.

## 4. Apakah 1 License Bisa Dipakai Lebih Dari 2 Server?

**Tidak bisa.** Satu license hanya dapat digunakan untuk satu server. Setiap server memerlukan license yang berbeda.

## Troubleshooting Lainnya

### Cek Secret Key
Jika error bukan HTTP-CODE: 0, periksa secret key Anda:
- Pastikan secret key valid dan belum expired
- Verifikasi bahwa secret key sesuai dengan server yang digunakan
- Cek format secret key apakah sudah benar

### Cek Konfigurasi Server
- Pastikan port yang digunakan tidak bentrok dengan aplikasi lain
- Verifikasi bahwa semua service yang diperlukan sudah berjalan
- Cek log aplikasi untuk detail error yang lebih spesifik

### Cek Versi Aplikasi
- Pastikan Anda menggunakan versi aplikasi yang kompatibel
- Update aplikasi ke versi terbaru jika diperlukan
- Verifikasi kompatibilitas dengan sistem operasi server

### Kontak Support
Jika masalah tetap berlanjut setelah mencoba semua solusi di atas, hubungi tim support dengan menyertakan:
- Screenshot error yang muncul
- Log aplikasi
- Detail konfigurasi server
- Versi aplikasi yang digunakan