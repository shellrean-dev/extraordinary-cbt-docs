# Error Ketika Login

Berikut adalah beberapa masalah yang sering terjadi saat login beserta solusinya.

## Email dan Password Default Exo

Jika Anda mencoba login untuk pertama kali atau sedang dalam pengembangan, gunakan kredensial default berikut:

- **Email**: `admin@shellrean.id`
- **Password**: `criticalpassword`

## Error 1: Permission Denied for Sequence `personal_access_token_id_seq`

**Gejala:**
```
pq: permission denied for sequence personal_access_token_id_seq
```

**Penyebab:**
User database yang digunakan pada konfigurasi `.env` tidak memiliki akses ke tabel atau sequence yang diperlukan.

**Solusi (Pilih salah satu):**
1. ** Berikan permission ke user tersebut**:
   ```sql
   -- Berikan akses ke semua tabel, schema, dan sequence
   GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO nama_user;
   GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO nama_user;
   ```

2. **Jadikan user sebagai owner schema**:
   ```sql
   ALTER SCHEMA public OWNER TO nama_user;
   ```

3. **Gunakan user root (postgres)**:
   Ubah konfigurasi di file `.env` untuk menggunakan user `postgres`:
   ```
   DB_USER=postgres
   DB_PASS=password_postgres
   ```

**Catatan:** Ganti `nama_user` dengan username database yang Anda gunakan di konfigurasi `.env`.

--- 

*Dokumen ini akan diperbarui jika ditemukan error login lainnya.*
