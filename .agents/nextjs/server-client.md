# Server Components vs Client Components

Pedoman penentuan dan pemisahan yang optimal antara **React Server Components (RSC)** dan **Client Components** di Next.js App Router.

---

## 🎯 1. Filosofi Pembagian Peran

```mermaid
flowchart TD
    subgraph Server Boundary (RSC - Default)
        A[Layout / Page] --> B[Static Content & Data Fetching]
        B --> C[API Services / DAL]
    end
    subgraph Client Boundary ('use client')
        D[Interactive Buttons & Forms]
        E[Modals & Dropdowns]
        F[Hooks / Browser State]
    end
    B -->|Meneruskan props| D
    B -->|Meneruskan props| E
```

---

## 🟢 2. Kapan Menggunakan Server Components (RSC)?

Jadikan komponen sebagai **Server Component (tanpa `'use client'`)** secara *default* saat:
- Melakukan pengambilan data (*data fetching*) awal halaman melalui API Service / Data Access Layer.
- Mengakses backend resources secara langsung (Database, API keys rahasia).
- Mengurangi ukuran JavaScript bundle yang dikirim ke browser (zero bundle size).
- Merender konten statis, markup artikel, atau layout dasar.

---

## 🔵 3. Kapan Menggunakan Client Components (`'use client'`)?

Gunakan direktif `'use client'` di baris paling pertama file hanya saat:
- Menangani event interaktif (`onClick`, `onChange`, `onSubmit`, `onKeyDown`).
- Menggunakan React State & Lifecycle (`useState`, `useReducer`, `useEffect`, `useLayoutEffect`).
- Menggunakan Custom Hooks (seperti TanStack Query `useQuery`, `useForm`, `useSearchParams`, `usePathname`).
- Mengakses Browser APIs (`window`, `document`, `localStorage`, navigator).

---

## 💡 4. Best Practices Komposisi

1. **Dorong Client Components ke Daun Terbawah (*Push to Leaves*)**:
   - Jangan jadikan seluruh halaman `page.tsx` menjadi `'use client'`.
   - Buat halaman `page.tsx` tetap sebagai Server Component, dan pisahkan bagian interaktifnya (misal: tombol like, form modal, search bar) ke dalam komponen `'use client'` terpisah di `_components/`.

2. **Passing Server Components sebagai `children` ke Client Component**:
   - Untuk membungkus Server Component di dalam Client Wrapper (seperti Context Provider atau Animasi Wrapper), teruskan Server Component sebagai prop `children`:
   ```tsx
   // ClientWrapper.tsx ('use client')
   export function ClientWrapper({ children }: { children: React.ReactNode }) {
     return <div className="p-4">{children}</div>;
   }
   ```
