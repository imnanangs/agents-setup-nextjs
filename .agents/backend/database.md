# Panduan Database & ORM

Pedoman pengelolaan basis data, migrasi skema, koneksi pool, dan optimasi query pada aplikasi.

---

## 🗄️ 1. Pilihan Database & Koneksi

- **Primary Databases**: PostgreSQL / MySQL.
- **Connection Pooling**:
  - Pada lingkungan serverless / Next.js Edge / Serverless Functions, pastikan menggunakan connection pooler (seperti Supabase Transaction Pooler, Neon serverless driver, atau Prisma Accelerate / pgBouncer) untuk mencegah habisnya batas koneksi database.
  - Gunakan pola *singleton* saat menginisialisasi database client (Prisma/Drizzle) di lingkungan pengembangan untuk mencegah multiple instance saat Hot Module Replacement (HMR).

---

## 📐 2. Migrasi Skema & Integritas Data

1. **Aturan Migrasi**:
   - Selalu buat dan jalankan migrasi resmi (contoh: `prisma migrate dev` atau `drizzle-kit generate`) ketika mengubah struktur skema database.
   - Jangan pernah mengubah skema database secara manual di production tanpa script migrasi tercatat.

2. **Foreign Keys & Cascading**:
   - Definisikan *foreign key constraint* secara eksplisit untuk menjaga integritas relasi antar tabel.
   - Gunakan aturan `ON DELETE CASCADE` atau `ON DELETE SET NULL` secara bijak sesuai aturan domain bisnis.

3. **Soft Delete vs Hard Delete**:
   - Untuk entitas penting (User, Transaction, Order), gunakan kolom `deletedAt` (Soft Delete) agar data historis tetap terlacak dan dapat dipulihkan jika diperlukan.

---

## ⚡ 3. Optimasi Query & Indexing

- **Indexing**:
  - Tambahkan index pada kolom yang sering digunakan pada filter `WHERE`, pengurutan `ORDER BY`, dan relasi `FOREIGN KEY`.
  - Gunakan `UNIQUE INDEX` untuk mencegah duplikasi data penting (contoh: `email`, `username`, `slug`).
- **Pencegahan N+1 Query**:
  - Gunakan *eager loading* (relational join) untuk mengambil relasi data alih-alih melakukan query database berulang di dalam looping.
- **Selective Query**:
  - Hindari mengambil semua kolom secara sembarangan. Gunakan klausa `select` untuk hanya mengambil kolom yang dibutuhkan oleh API.
