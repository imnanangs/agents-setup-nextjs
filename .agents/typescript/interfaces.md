# Interface vs Type Alias (Interfaces)

Pedoman penentuan penggunaan `interface` dan `type` dalam mendefinisikan struktur data di aplikasi.

---

## 🎯 1. Kapan Menggunakan `interface`?

Gunakan **`interface`** untuk:
1. **Definisi Bentuk Objek / Domain Model**:
   ```typescript
   export interface User {
     id: string;
     name: string;
     email: string;
     role: UserRole;
     createdAt: Date;
   }
   ```
2. **Deklarasi Props Komponen React**:
   ```typescript
   export interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
     variant?: "primary" | "outline" | "danger";
     isLoading?: boolean;
   }
   ```
3. **Ekstensi Struktur Objek (Inheritance via `extends`)**:
   ```typescript
   export interface AdminUser extends User {
     permissions: string[];
     department: string;
   }
   ```

---

## 🧩 2. Kapan Menggunakan `type` Alias?

Gunakan **`type`** untuk:
1. **Union Types & String Literals**:
   ```typescript
   export type Status = "idle" | "loading" | "success" | "error";
   export type InputType = "text" | "password" | "email" | "number";
   ```
2. **Intersection Types**:
   ```typescript
   export type WithAuditFields<T> = T & {
     createdAt: string;
     updatedAt: string;
   };
   ```
3. **Tuple & Tipe Primitif Kustom**:
   ```typescript
   export type Coordinates = [number, number];
   export type ID = string | number;
   ```
4. **Inferensi Tipe dari Skema Validasi (Zod)**:
   ```typescript
   export type CreateUserInput = z.infer<typeof createUserSchema>;
   ```

---

## 🔒 3. Readonly & Immutability

Gunakan `readonly` pada properti yang tidak boleh dimutasi setelah inisialisasi:
```typescript
export interface AppConfig {
  readonly apiUrl: string;
  readonly timeoutMs: number;
}
```