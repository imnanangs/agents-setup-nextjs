# Arsitektur Backend (Feature-Driven / Modular)

Menerapkan **Modular / Feature-Driven Architecture** untuk memastikan kohesi tinggi, *separation of concerns*, dan kemudahan pemeliharaan kode.

---

## 🧱 1. Struktur Layer Utama

1. **Route Handlers (`src/app/api/[feature]/route.ts`)**:
   - Berperan sebagai **HTTP Controller**.
   - Hanya bertanggung jawab untuk:
     - Menerima `NextRequest`.
     - Mengekstrak query params, route params, atau request body.
     - Memvalidasi skema input dengan Zod (`[feature].schema.ts`).
     - Memanggil fungsi di **Service Layer**.
     - Mengembalikan `NextResponse` berformat standar.
   - Harus ramping (*thin controller*, idealnya < 50 baris).

2. **Feature Modules (`src/modules/[feature]/`)**:
   - Membungkus seluruh logika dan kebutuhan spesifik satu domain fitur (contoh: `users`, `products`, `auth`, `transactions`).
   - `[feature].service.ts`: Berisi seluruh *business logic* dan query database.
   - `[feature].schema.ts`: Skema validasi Zod untuk request input & DTO.
   - `[feature].interface.ts`: Deklarasi Type / Interface TypeScript internal domain.

3. **Shared / Common Layer (`src/common/` atau `src/lib/`)**:
   - Memuat utilitas yang dipakai lintas modul (contoh: formatter response API, custom error class, JWT helper, middleware auth, database client instance).

---

## 📂 2. Konvensi Struktur Direktori

```text
src/
├── app/
│   └── api/
│       └── users/
│           ├── route.ts                 # Endpoint: GET (list), POST (create)
│           └── [id]/
│               └── route.ts             # Endpoint: GET (detail), PATCH (update), DELETE
│
├── modules/
│   └── users/                           # Feature Module: Users
│       ├── users.service.ts             # Logika bisnis & Query Database
│       ├── users.schema.ts              # Skema validasi Zod
│       └── users.interface.ts           # Types/Interfaces TypeScript
│
└── common/                              # Kode bersama (Cross-cutting)
    ├── errors/
    │   └── app-error.ts                 # Custom AppError / ApiError class
    ├── utils/
    │   └── response.util.ts             # Helper response standar (success/error)
    └── db/
        └── index.ts                     # Inisialisasi Prisma Client / Drizzle instance
```

---

## 🚫 3. Aturan Arsitektur yang Dilarang

- ❌ **Dilarang** memanggil database langsung (ORM/SQL) di dalam Route Handler `route.ts`.
- ❌ **Dilarang** menaruh validasi parsing logika manual tanpa Zod schema.
- ❌ **Dilarang** mencampur logika antar fitur tanpa batas domain yang jelas (gunakan shared service jika dibutuhkan komunikasi antar modul).
