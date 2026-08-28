# Konvensi TypeScript (TypeScript Conventions)

Pedoman standar penulisan kode TypeScript yang bersih, konsisten, dan mudah dibaca di seluruh repositori.

---

## ⚙️ 1. Pengaturan Compiler & Strict Mode

- Proyek ini berjalan dengan pengaturan **`strict: true`** di `tsconfig.json`.
- **Dilarang Menonaktifkan Strict Check**: Jangan gunakan compiler flags yang melonggarkan tipe (seperti `noImplicitAny: false`).
- Jangan gunakan komentar `@ts-ignore` atau `@ts-nocheck` kecuali ada batasan library pihak ketiga yang tidak terdokumentasi (wajib menyertakan komentar penjelasan alasan penggunaan).

---

## 🏷️ 2. Aturan Penamaan (Naming Conventions)

1. **Interfaces & Types**: Gunakan **PascalCase** (contoh: `UserProfile`, `CreateOrderInput`, `ApiResponse<T>`).
2. **Generics**: Gunakan huruf kapital tunggal deskriptif atau PascalCase (contoh: `T`, `TData`, `TResponse`).
3. **Enums & Const Enums**: Prioritaskan **Union of Strings** daripada `enum` TypeScript tradisional:
   ```typescript
   // Direkomendasikan (Tree-shakeable, zero runtime JS)
   export type UserRole = "admin" | "merchant" | "customer";

   // Hindari enum runtime berat
   // enum UserRole { Admin = "ADMIN", ... }
   ```
4. **Variabel & Fungsi**: Gunakan **camelCase** (contoh: `getUserById`, `isLoading`).
5. **Konstanta Global**: Gunakan **SCREAMING_SNAKE_CASE** (contoh: `DEFAULT_PAGE_LIMIT = 10`).

---

## 🚫 3. Larangan Penggunaan `any`

- **Gunakan `unknown`**: Jika tipe data benar-benar belum diketahui (misal respons eksternal atau parsing JSON), gunakan tipe `unknown` dan lakukan *type narrowing* (menggunakan `typeof`, `instanceof`, atau validasi Zod).
- Hindari *type assertion* berlebih (`as MyType`) jika tipe tersebut dapat diinferensikan secara alami oleh TypeScript.
