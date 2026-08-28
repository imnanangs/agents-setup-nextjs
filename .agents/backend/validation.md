# Validasi Input (Validation Layer)

Validasi input adalah garis pertahanan pertama sistem. Dilarang memproses data mentah dari klien secara langsung. Seluruh request data **WAJIB** lolos validasi skema sebelum diteruskan ke Service Layer.

---

## 🛠️ 1. Library & Lokasi Skema

- **Standard Library**: Gunakan **Zod** (`zod`) untuk seluruh deklarasi skema validasi.
- **Lokasi File**: Simpan skema di file dedicated modul fitur terkait: `src/modules/[feature]/[feature].schema.ts`.
- **Inferensi Tipe**: Selalu export tipe TypeScript hasil inferensi dari skema Zod menggunakan `z.infer<typeof schema>` untuk digunakan di Service dan Controller.

---

## 🎯 2. Cakupan Validasi

Validasi harus mencakup seluruh bagian dari HTTP Request:

1. **Request Body (`req.json()`)**: Untuk payload mutasi data (`POST`, `PUT`, `PATCH`).
2. **Query Parameters (`req.nextUrl.searchParams`)**: Gunakan `z.coerce` untuk konversi otomatis tipe string query URL menjadi tipe yang sesuai (contoh: `z.coerce.number().default(1)`).
3. **Route Dynamic Parameters (`params`)**: Validasi identifier URL (contoh: validasi format UUID atau CUID pada `/api/users/[id]`).

---

## 🛡️ 3. Aturan Ketat Integritas Data

- **Pencegahan Mass Assignment**: Selalu gunakan `.strict()` pada skema mutasi agar request yang menyisipkan field terlarang (misal `role: "admin"`) langsung ditolak.
- **Sanitasi String**: Manfaatkan `.trim()`, `.toLowerCase()`, `.min()`, `.max()`, dan format helper bawaan Zod (`.email()`, `.url()`, `.uuid()`).

---

## 📋 4. Contoh Pola Implementasi Zod

```typescript
// src/modules/users/users.schema.ts
import { z } from "zod";

// Skema Buat Pengguna
export const createUserSchema = z
  .object({
    name: z
      .string({ required_error: "Nama wajib diisi" })
      .trim()
      .min(3, "Nama minimal harus 3 karakter")
      .max(100, "Nama maksimal 100 karakter"),
    email: z
      .string({ required_error: "Email wajib diisi" })
      .trim()
      .email("Format email tidak valid")
      .toLowerCase(),
    password: z
      .string({ required_error: "Password wajib diisi" })
      .min(8, "Password minimal 8 karakter"),
  })
  .strict();

// Skema Query Paginasi
export const getUsersQuerySchema = z.object({
  page: z.coerce.number().int().positive().default(1),
  limit: z.coerce.number().int().positive().max(100).default(10),
  search: z.string().trim().optional(),
});

// Inferensi Tipe TypeScript
export type CreateUserInput = z.infer<typeof createUserSchema>;
export type GetUsersQueryInput = z.infer<typeof getUsersQuerySchema>;
```
