# Autentikasi (Authentication)

Pedoman implementasi autentikasi menggunakan **JWT (JSON Web Tokens)** dan manajemen session berbasis standar keamanan modern.

---

## 🔐 1. Struktur Modul Autentikasi

Autentikasi dikelola sebagai fitur modul mandiri di `src/modules/auth/`:

```text
src/
├── app/
│   └── api/
│       └── auth/
│           ├── login/route.ts           # Endpoint Login
│           ├── register/route.ts        # Endpoint Register
│           ├── refresh/route.ts         # Endpoint Refresh Token
│           ├── logout/route.ts          # Endpoint Logout
│           └── me/route.ts              # Endpoint Cek Profil User Aktif
│
└── modules/
    └── auth/
        ├── auth.service.ts              # Hashing, token generation, verifikasi credential
        ├── auth.schema.ts               # Skema Zod login, register, forgot-password
        └── auth.interface.ts            # Tipe token payload & auth context
```

---

## 🛡️ 2. Aturan Keamanan Autentikasi

1. **Password Hashing**:
   - **DILARANG** menyimpan password dalam bentuk *plain text*.
   - Gunakan algoritma hashing aman seperti **Argon2** atau **Bcrypt** (work factor minimal 10-12) di dalam `auth.service.ts`.

2. **Penyimpanan Token & Pengiriman**:
   - **Web / Browser Clients**: Simpan JWT di dalam cookie dengan konfigurasi:
     - `httpOnly: true` (mencegah pencurian token via XSS).
     - `secure: process.env.NODE_ENV === 'production'` (hanya lewat HTTPS).
     - `sameSite: 'lax'` atau `'strict'` (mencegah serangan CSRF).
     - `path: '/'`
   - **Mobile / External Clients**: Kembalikan token di dalam response `data` untuk disertakan pada header `Authorization: Bearer <token>`.

3. **Short-Lived Access Token & Refresh Token**:
   - Access Token memiliki masa aktif singkat (misal: 15–30 menit).
   - Refresh Token memiliki masa aktif lebih panjang (misal: 7–30 hari) dan disimpan terenkripsi/hash di database untuk mendukung *token revocation* saat logout.

---

## 🚦 3. Proteksi Route via Middleware (`middleware.ts`)

Gunakan **Next.js Middleware** untuk memeriksa autentikasi secara global sebelum request mencapai Route Handler:

- Ambil token dari cookie (`request.cookies.get('token')`) atau header `Authorization`.
- Verifikasi validitas dan masa berlaku token menggunakan library seperti `jose` (kompatibel dengan Edge runtime).
- Jika valid, inject context user (misal: `x-user-id`, `x-user-role`, `x-user-email`) ke dalam request header untuk dibaca di Route Handler.
- Jika tidak valid atau token hilang pada protected route, langsung kembalikan response `401 Unauthorized` berformat standar.
