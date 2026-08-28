# Panduan Komponen React & Next.js

Pedoman arsitektur dan penulisan komponen frontend menggunakan pola modern React Server Components (RSC) dan Client Components di Next.js App Router.

---

## 🏗️ 1. Server Components vs Client Components

- **Server Components (Default)**:
  - Komponen secara default adalah React Server Component (RSC) tanpa direktif `'use client'`.
  - Gunakan RSC untuk data fetching (melalui API Service layer / Data Access Layer), akses server, dan rendering konten statis demi performa optimal serta bundle size minimal.
- **Client Components (`'use client'`)**:
  - Gunakan `'use client'` hanya pada komponen daun (*leaf components*) yang membutuhkan:
    - State (`useState`, `useReducer`).
    - Lifecycle & Effects (`useEffect`, `useLayoutEffect`).
    - Event listener (`onClick`, `onChange`, `onSubmit`).
    - Browser APIs (`window`, `localStorage`, geolocation).
    - Custom hooks atau form management (React Hook Form, TanStack Query).

---

## 🧩 2. Konvensi Penulisan Komponen

1. **Functional Components & TypeScript**:
   - Selalu gunakan functional components dengan deklarasi eksplisit props interface.
   - Hindari `React.FC` jadul, definisikan tipe props langsung pada parameter fungsi.
   ```tsx
   interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
     variant?: "primary" | "secondary" | "outline" | "danger";
     isLoading?: boolean;
   }

   export function Button({
     variant = "primary",
     isLoading,
     children,
     className,
     ...props
   }: ButtonProps) {
     return (
       <button
         className={cn(
           "rounded-lg border px-4 py-2 font-medium transition-colors",
           variant === "primary" && "border-slate-900 bg-slate-900 text-white dark:border-slate-100 dark:bg-slate-100 dark:text-slate-900",
           variant === "outline" && "border-border bg-transparent hover:bg-muted",
           className
         )}
         disabled={isLoading || props.disabled}
         {...props}
       >
         {isLoading ? "Memuat..." : children}
       </button>
     );
   }
   ```

2. **Exports & Penamaan**:
   - Gunakan **Named Exports** (`export function MyComponent() {}`) untuk komponen UI reusable di `src/components/`.
   - Gunakan **Default Exports** (`export default function Page() {}`) hanya untuk file rute halaman App Router (`page.tsx`, `layout.tsx`, `loading.tsx`, `error.tsx`, `not-found.tsx`).

3. **Komposisi & `cn()` Utility**:
   - Selalu gunakan helper `cn()` (`clsx` + `tailwind-merge`) untuk menggabungkan class Tailwind dengan aman tanpa konflik class.

4. **Integrasi Data & API di Komponen**:
   - **DILARANG KERAS** memanggil `fetch()` mentah secara inline di dalam komponen.
   - Di Server Component: panggil fungsi API Service (`await userApi.getUsers()`).
   - Di Client Component: gunakan custom query/mutation hook (`useUsers()`, `useCreateUser()`) yang terhubung ke Service Layer.

5. **Struktur File Komponen**:
   - Komponen UI global: `src/components/ui/` (Button, Input, Modal, Table, Badge).
   - Komponen fitur spesifik: `src/modules/[feature]/components/` atau `src/app/(routes)/[feature]/_components/`.
