# Keamanan Backend (Backend Security)

Menerapkan strategi pertahanan berlapis (*Defense in Depth*) pada setiap level: Edge/Middleware, Controller, dan Service Layer.

---

## 🛡️ 1. Lapisan Pertahanan (Defense in Depth)

1. **Lapisan Edge / Middleware (`middleware.ts`)**:
   - Memfilter traffic tidak valid sebelum menyentuh server backend.
   - Pengecekan awal integritas token/session dan verifikasi header.
   - Rate limiting dan deteksi bot mencurigakan.

2. **Lapisan Controller (Route Handlers)**:
   - Validasi ketat seluruh payload input (Zod Schema).
   - Otorisasi berbasis role (RBAC).

3. **Lapisan Service**:
   - Validasi integritas bisnis dan kepemilikan data (*Resource Ownership*).
   - Sanitasi data sebelum masuk query ORM/Database.

---

## 🚦 2. Rate Limiting & Proteksi Brute-Force

- **Endpoint Publik Sensitif**: Terapkan limitasi request yang ketat pada route seperti `/api/auth/login`, `/api/auth/register`, `/api/auth/forgot-password`, dan endpoint pengiriman OTP/SMS.
- **Implementasi**: Gunakan Redis Edge Rate Limiting (seperti `@upstash/ratelimit` / Redis) berbasis IP address atau Identifier pengguna.
- Kembalikan status HTTP `429 Too Many Requests` saat batasan terlampaui.

---

## 🌐 3. Konfigurasi CORS & Security Headers

- **Strict CORS**: Jangan gunakan `Access-Control-Allow-Origin: *` di production pada API yang menangani data kredensial/cookie.
- **Security Headers**: Pastikan aplikasi menyertakan security headers standar (X-Content-Type-Options: nosniff, X-Frame-Options: DENY, Referrer-Policy: strict-origin-when-cross-origin, Content-Security-Policy).

---

## 🔒 4. Pencegahan Vulnerability Umum

- **SQL Injection**: Cegah dengan ORM parameterized query (Prisma / Drizzle). Jangan pernah menggunakan string template langsung (`SELECT * FROM users WHERE id = '${id}'`).
- **Mass Assignment Vulnerability**: Gunakan Zod `.strict()` pada skema create/update untuk menolak field tak terduga (seperti `role`, `isAdmin`, atau `isVerified` yang dikirim penyerang).
