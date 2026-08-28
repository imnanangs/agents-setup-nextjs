# Safety & Security Guidelines

Pedoman keamanan standar industri, pencegahan kebocoran data (*data leak*), dan integritas sistem.

---

## 🔴 1. Critical Security Rules (Non-Negotiable)

1. **Secrets & Environment Variables**
   - **DILARANG KERAS** melakukan hardcode API keys, password, database connection strings, JWT secret, private keys, atau credentials ke dalam source code.
   - Gunakan selalu `process.env` dengan validasi environment (misalnya Zod / `@t3-oss/env-nextjs`).
   - Pastikan file `.env*` yang memuat rahasia telah masuk dalam `.gitignore`.
   - Pisahkan secara tegas antara env client-side (prefix `NEXT_PUBLIC_`) dan env server-side rahasia.

2. **Data Sanitization & Injection Prevention**
   - Sanitasi dan validasi semua input dari request (Query params, Body, Headers, Cookies).
   - Cegah **SQL Injection**: Gunakan ORM/Query Builder bertipe aman (Prisma, Drizzle, dll.) dengan parameterized query. Hindari raw string concatenation pada query database.
   - Cegah **XSS (Cross-Site Scripting)**: Hindari penggunaan `dangerouslySetInnerHTML` tanpa sanitasi ketat (seperti DOMPurify).

3. **Log & Privacy Protection**
   - Dilarang mencetak (*console.log/logger*) data sensitif seperti password, refresh token, credit card, atau PII (Personally Identifiable Information) pengguna ke stdout/production logs.

---

## 🛡️ 2. Validation & Authentication Safety

1. **Dual-Layer Validation**
   - Selalu terapkan schema validation (misal: Zod) di sisi **Server/API Handler/Server Action** terlepas dari validasi yang sudah ada di sisi Client (karena client validation bisa di-bypass).

2. **Authorization & RBAC (Role-Based Access Control)**
   - Jangan pernah mengandalkan UI hiding semata untuk keamanan.
   - Selalu verifikasi sesi (`Session`), kepemilikan data (`User ID match`), dan peran pengguna (`Role`) di setiap endpoint/Server Action sebelum menjalankan mutasi atau query database.

3. **CORS, CSRF & Rate Limiting**
   - Konfigurasikan header CORS secara ketat (hindari `Access-Control-Allow-Origin: *` pada endpoint sensitif).
   - Terapkan rate limiting pada endpoint publik seperti Login, Register, Password Reset, dan OTP.
