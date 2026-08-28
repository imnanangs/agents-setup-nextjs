# Arsitektur Next.js (App Router)

Pedoman arsitektur aplikasi Next.js modern menggunakan **App Router**, struktur direktori modular (*Feature-Driven*), serta strategi *colocation*.

---

## 🏛️ 1. App Router Standar

- **Wajib App Router**: Seluruh struktur routing menggunakan direktori `src/app/`. Dilarang menggunakan pola lama `pages/`.
- **Root Directory Layout**:
  ```text
  src/
  ├── app/                                 # Routing, layouts, dan HTTP endpoints
  │   ├── (auth)/                          # Route group untuk auth (login, register)
  │   ├── (dashboard)/                     # Route group untuk layout dashboard
  │   │   ├── layout.tsx                   # Layout dashboard dengan sidebar/navbar
  │   │   ├── page.tsx                     # Halaman dashboard utama
  │   │   └── users/
  │   │       ├── page.tsx                 # RSC Page
  │   │       ├── loading.tsx              # Suspense loading skeleton
  │   │       ├── error.tsx                # Error boundary
  │   │       └── _components/             # Komponen private khusus route users
  │   ├── api/                             # Route Handlers (Controller)
  │   │   └── users/
  │   │       └── route.ts
  │   ├── layout.tsx                       # Root layout (Fonts, ThemeProvider, QueryClient)
  │   ├── globals.css                      # Tailwind base & CSS variables
  │   ├── not-found.tsx                    # Halaman 404 global
  │   └── error.tsx                        # Global error boundary
  │
  ├── modules/                             # Domain/Feature-driven business logic
  │   └── users/
  │       ├── users.service.ts             # Backend business logic & DB query (Prisma/ORM)
  │       ├── users.api.ts                 # Frontend API client caller (via apiClient)
  │       ├── users.hooks.ts               # Custom React Query / mutation hooks
  │       ├── users.schema.ts              # Zod validation schemas
  │       ├── users.interface.ts           # Types & interfaces
  │       └── components/                  # Domain-specific reusable components (opsional)
  │
  ├── components/                          # Shared UI components (Global design system)
  │   └── ui/                              # Button, Input, Modal, Table, Badge
  │
  ├── lib/                                 # Shared libraries & utilities
  │   ├── api-client.ts                    # Centralized HTTP client wrapper
  │   └── utils.ts                         # cn() helper
  │
  └── types/                               # Global TypeScript definitions
  ```

---

## 📁 2. Prinsip Colocation & Route Groups

1. **Route Groups `(group)`**:
   - Gunakan tanda kurung `(auth)`, `(dashboard)`, `(marketing)` untuk mengelompokkan rute yang memiliki layout berbeda tanpa memengaruhi struktur path URL.

2. **Private Folders `_components/`**:
   - Jika suatu komponen hanya dipakai pada satu halaman tertentu (misal: `UserListTable` hanya dipakai di `/users`), letakkan di dalam folder `src/app/(dashboard)/users/_components/`.
   - Gunakan `src/components/ui/` hanya untuk komponen atomik yang reusable di seluruh aplikasi (Button, Input, Dialog).

3. **Colocation File Spesial**:
   - Selalu manfaatkan file konvensi Next.js di setiap level rute:
     - `loading.tsx`: Menampilkan instant loading state via React Suspense.
     - `error.tsx`: Menangkap runtime error pada level rute tanpa merusak seluruh aplikasi.
     - `not-found.tsx`: Menangani status 404 pada rute spesifik.
