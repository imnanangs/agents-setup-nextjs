# 🤖 Panduan Sistem AI Agent (AI Agent System Guidelines)

Selamat datang di direktori konfigurasi `.agents`. File ini berfungsi sebagai indeks utama dan manual instruksi bagi seluruh asisten coding AI (AI Coding Assistants) dalam proyek ini.

---

## 🎯 Tujuan (Purpose)
Memastikan seluruh kode yang dihasilkan konsisten, aman, terstruktur rapi, mengikuti standar arsitektur industri (*best practices*), serta mudah dipelihara pada repositori ini.

---

## 📁 Indeks Basis Pengetahuan (Knowledge Base Index)

1. **[Panduan Inti (`/core`)](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/core)**:
   - [`behavior.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/core/behavior.md): Peran, pola pikir *Senior Full-Stack*, dan standar kualitas AI.
   - [`communication.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/core/communication.md): Format output kode, diff changes, dan komunikasi ringkas.
   - [`safety.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/core/safety.md): Aturan keamanan, pencegahan kebocoran data (*secrets/PII*), dan sanitasi input.
   - [`workflow.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/core/workflow.md): Alur kerja terstruktur 4 tahap (*Analyze, Plan, Execute, Verify*).
   - [`git.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/core/git.md): Konvensi Conventional Commits, tipe pesan commit, strategi branching, dan atomic commits.

2. **[Arsitektur Next.js (`/nextjs`)](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs)**:
   - [`architecture.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/architecture.md): Struktur folder App Router modern & colocation.
   - [`app-router.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/app-router.md): Hirarki file spesial (`page`, `layout`, `loading`, `error`, `not-found`).
   - [`server-client.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/server-client.md): Strategi React Server Components (RSC) vs Client Components (`'use client'`).
   - [`data-fetching.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/data-fetching.md): Standarisasi integrasi API berlapis (*Centralized HTTP Client & API Service layer*).
   - [`images-assets.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/images-assets.md): Optimasi aset, `next/image`, dan `next/font`.
   - [`metadata-seo.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/metadata-seo.md): Optimasi SEO, dynamic metadata, Open Graph, sitemap & robots.
   - [`performance.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/performance.md): Core Web Vitals (LCP, INP, CLS) & dynamic imports.
   - [`rendering.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/rendering.md): Strategi rendering (SSG, SSR, ISR, Streaming Suspense).
   - [`routing.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/nextjs/routing.md): Komponen `<Link>`, navigasi programmatic, dan query params.

3. **[Konvensi TypeScript (`/typescript`)](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/typescript)**:
   - [`conventions.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/typescript/conventions.md): Strict compiler mode, aturan penamaan, union strings, dan larangan `any`.
   - [`interfaces.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/typescript/interfaces.md): Panduan pemilihan `interface` vs `type` alias.
   - [`type-safety.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/typescript/type-safety.md): End-to-end type safety, type guards, exhaustive checks (`never`), dan utility types.

4. **[Frontend Design System (`/frontend`)](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/frontend)**:
   - [`ui.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/frontend/ui.md): Desain Flat, Minimalist, solid crisp borders, tanpa box shadow/glassmorphism.
   - [`components.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/frontend/components.md): Konvensi penulisan komponen, named exports, dan helper `cn()`.
   - [`forms.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/frontend/forms.md): Integrasi React Hook Form, Zod resolver, dan integrasi API service layer.
   - [`responsive.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/frontend/responsive.md): Pendekatan Mobile-First dan adaptasi layout responsif.
   - [`animations.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/frontend/animations.md): Transisi halus mikro (150ms-300ms) & Framer Motion.
   - [`accessibility.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/frontend/accessibility.md): Standar WCAG 2.1 AA, keyboard focus rings, semantic HTML5, dan ARIA attributes.

5. **[Backend & API (`/backend`)](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend)**:
   - [`README.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/README.md): Gambaran umum sistem backend dan persona arsitek.
   - [`api.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/api.md): Format response JSON standar `{ status, message, data, meta }` & HTTP status codes.
   - [`architecture.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/architecture.md): Arsitektur Feature-Driven modular (`src/modules/[feature]/`).
   - [`authentication.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/authentication.md): Autentikasi JWT, password hashing (Argon2/Bcrypt), dan proteksi HttpOnly cookie.
   - [`authorization.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/authorization.md): Otorisasi RBAC dan validasi Resource Ownership di Service Layer.
   - [`database.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/database.md): Manajemen database, pooling serverless, indexing, dan soft-delete.
   - [`error-handling.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/error-handling.md): Custom `AppError`, penanganan terpusat, dan sanitasi error 500.
   - [`performance.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/performance.md): Pencegahan N+1, seleksi kolom (*lean queries*), paginasi wajib, dan caching.
   - [`security.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/security.md): Strategi *Defense in Depth*, rate limiting, CORS ketat, dan sanitasi.
   - [`validation.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/backend/validation.md): Skema validasi Zod untuk request body, query params, dan route params.

6. **[Standard Operating Procedures (`/workflows`)](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows)**:
   - [`new-feature.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows/new-feature.md): Alur pembuatan fitur baru dari database ke UI.
   - [`new-api.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows/new-api.md): Langkah membuat endpoint API modular.
   - [`new-page.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows/new-page.md): Langkah membuat halaman rute App Router.
   - [`new-component.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows/new-component.md): Langkah membuat komponen UI reusable.
   - [`bug-fix.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows/bug-fix.md): 5 langkah diagnosis dan perbaikan bug.
   - [`refactor.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows/refactor.md): Langkah refactoring inkremental dan pembersihan kode.
   - [`code-review.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows/code-review.md): Checklist tinjauan kualitas sebelum merge/deploy.
   - [`new-project.md`](file:///home/nanang-supriatna/Application/NextJs/boilerplate/.agents/workflows/new-project.md): Setup dan inisialisasi awal proyek baru.
