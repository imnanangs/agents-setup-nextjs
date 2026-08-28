# Performa & Optimasi Backend

Pedoman penulisan kode backend dengan efisiensi tinggi, penggunaan sumber daya yang hemat, dan respons waktu cepat.

---

## 🚀 1. Mencegah N+1 Query Problem

- **Dilarang Query di dalam Loop**: Jangan pernah mengeksekusi query database di dalam `Array.map()`, `forEach()`, atau `for...of`.
- **Eager Loading & Joins**: Ambil seluruh relasi data dalam 1 query terpadu menggunakan fitur relasi ORM (misal: `include` / `with` pada Prisma/Drizzle) atau `JOIN` SQL eksplisit.

---

## 🔍 2. Optimasi Pemilihan Kolom (Lean Queries)

- **Hindari Pola "SELECT *"**: Jangan mengambil semua kolom bila hanya sebagian data yang dibutuhkan oleh frontend.
- **Eksplisit Seleksi Kolom**: Gunakan properti `select` pada ORM untuk mereduksi *memory footprint*, payload transfer data, dan waktu serialisasi JSON.

---

## 📑 3. Wajib Paginasi (Pagination by Default)

- **Larangan Unbound Queries**: Semua endpoint yang mengembalikan daftar/list data **WAJIB** menerapkan limit dan offset/cursor paginasi secara *default*.
- **Parameter Paginasi Standar**:
  - `page`: Nomor halaman aktif (default: 1).
  - `limit`: Jumlah item per halaman (default: 10, maksimal dibatasi misal: 100 untuk mencegah DoS query).
- Kembalikan informasi metadata paginasi lengkap pada field `meta` (`page`, `limit`, `total`, `total_pages`).

---

## ⚡ 4. Strategi Caching

1. **Next.js Caching & Revalidation**:
   - Manfaatkan `unstable_cache` atau tag-based revalidation (`revalidateTag`) untuk data publik yang jarang berubah (seperti daftar kategori, konfigurasi sistem, FAQ).
2. **In-Memory / Redis Caching**:
   - Gunakan Redis untuk cache hasil kalkulasi berat, aggregasi laporan, atau session data berfrekuensi tinggi.
