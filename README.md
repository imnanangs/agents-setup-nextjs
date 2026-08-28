# 🤖 Next.js AI Agents Guidelines & Ruleset

Kumpulan pedoman (*guidelines*), aturan (*rules*), arsitektur standar, dan alur kerja (*SOP/workflows*) untuk **AI Coding Assistant** (Antigravity, Cursor, GitHub Copilot, Claude Dev/Cline, dll.) dalam membangun aplikasi **Next.js (App Router)** berskala produksi yang bersih, konsisten, aman, dan berperforma tinggi.

---

## 🎯 Tujuan Repositori

Repositori ini dirancang sebagai basis pengetahuan (*knowledge base*) bagi AI Agent agar:
1. **Konsisten**: Mengikuti arsitektur *Modular / Feature-Driven* tanpa campur aduk antar domain.
2. **Type-Safe**: Menerapkan validasi skema ketat (Zod + TypeScript strict mode) dari database ke UI.
3. **Best Practices**: Menerapkan arsitektur data-fetching berlapis, Server Components (RSC), optimasi performa Core Web Vitals, dan standar keamanan *Defense-in-Depth*.
4. **Terstandar**: Memiliki SOP baku untuk setiap aktivitas (*New Feature, Bug Fix, New API, New Page, Refactoring, Code Review*).

---

## 📁 Struktur Direktori `.agents/`

```text
.agents/
├── AGENTS.md                   # 🌟 Indeks Utama & Entry Point AI Agent
│
├── core/                       # Panduan Prinsip Dasar & Standar Kualitas
│   ├── behavior.md             # Pola pikir Senior Full-Stack & standar kode AI
│   ├── communication.md        # Format response ringkas & git diff preview
│   ├── git.md                  # Conventional Commits, branching, & atomic commit
│   ├── safety.md               # Keamanan rahasia (secrets), PII, & sanitasi input
│   └── workflow.md             # Siklus kerja 4 tahap (Analyze, Plan, Execute, Verify)
│
├── nextjs/                     # Konvensi & Arsitektur Next.js (App Router)
│   ├── app-router.md           # Konvensi file spesial (page, layout, loading, error)
│   ├── architecture.md         # Struktur folder modular & colocation
│   ├── data-fetching.md        # Arsitektur integrasi API berlapis (apiClient & services)
│   ├── images-assets.md        # Optimasi next/image & next/font
│   ├── metadata-seo.md         # Dynamic metadata, Open Graph, sitemap & robots
│   ├── performance.md          # Core Web Vitals (LCP, INP, CLS) & dynamic import
│   ├── rendering.md            # Pola rendering (SSG, SSR, ISR, Streaming Suspense)
│   ├── routing.md              # Navigasi Link, router push, & query params
│   └── server-client.md        # Aturan RSC vs Client Components ('use client')
│
├── typescript/                 # Standar Penulisan TypeScript
│   ├── conventions.md          # Strict mode, naming conventions, larangan 'any'
│   ├── interfaces.md           # Panduan 'interface' vs 'type' alias
│   └── type-safety.md          # Type guards, utility types, & exhaustive check
│
├── frontend/                   # Standar Desain Sistem & UI
│   ├── accessibility.md        # WCAG 2.1 AA, keyboard navigation, & semantic HTML
│   ├── animations.md           # Transisi halus mikro (150-300ms) & Framer Motion
│   ├── components.md           # Konvensi komponen UI, named exports, & cn() helper
│   ├── forms.md                # React Hook Form + Zod resolver + integrasi API
│   ├── responsive.md           # Pendekatan Mobile-First & breakpoint responsif
│   └── ui.md                   # Filosofi Flat & Minimalist UI, solid borders
│
├── backend/                    # Arsitektur Backend & Route Handlers
│   ├── README.md               # Gambaran umum layer backend
│   ├── api.md                  # Standarisasi format response JSON & HTTP codes
│   ├── architecture.md         # Feature-Driven modular (Controller, Service, Schema)
│   ├── authentication.md       # JWT auth, password hashing, & HttpOnly cookie
│   ├── authorization.md        # RBAC & kepemilikan resource di Service layer
│   ├── database.md             # Pooling connection ORM (Prisma/Drizzle) & indexing
│   ├── error-handling.md       # Custom AppError & penanganan error terpusat
│   ├── performance.md          # Pencegahan N+1, lean select queries, & pagination
│   ├── security.md             # Rate limit, CORS ketat, CSRF, & sanitasi
│   └── validation.md           # Validasi Zod pada body, query, & route params
│
└── workflows/                  # Standard Operating Procedures (SOP)
    ├── new-feature.md          # SOP pembuatan fitur baru end-to-end (DB ke UI)
    ├── new-api.md              # SOP pembuatan endpoint API Route Handler
    ├── new-page.md             # SOP pembuatan halaman rute baru
    ├── new-component.md        # SOP pembuatan komponen UI reusable
    ├── bug-fix.md              # SOP 5 langkah diagnosis & perbaikan bug
    ├── refactor.md             # SOP refactoring kode secara inkremental
    ├── code-review.md          # Checklist tinjauan kualitas sebelum merge
    └── new-project.md          # Panduan inisialisasi awal proyek baru
```

---

## 🏛️ Konvensi Arsitektur Modul (`src/modules/[feature]/`)

Setiap domain fitur diisolasi dalam satu folder modular dengan format **Flat & Eksplisit berdasarkan Peran**:

```text
src/modules/[feature]/
├── [feature].service.ts    # [BACKEND] Logika bisnis & query database (ORM)
├── [feature].api.ts        # [FRONTEND] HTTP caller via apiClient (fetch wrapper)
├── [feature].hooks.ts      # [FRONTEND] Custom React Query / mutation hooks
├── [feature].schema.ts     # Zod validation schemas (mutasi & query)
├── [feature].interface.ts  # Types & TypeScript interfaces
└── components/             # Komponen UI spesifik domain fitur (opsional)
```

---

## 🚀 Cara Penggunaan

### 1. Pasang pada Proyek Next.js Baru / Eksisting

Cukup salin direktori `.agents/` ke root proyek Next.js Anda:

```bash
# Clone repositori ruleset ini
git clone git@github.com:imnanangs/agents-setup-nextjs.git temp-agents

# Pindahkan folder .agents ke root proyek Anda
cp -r temp-agents/.agents /path/to/your-nextjs-project/

# Hapus temporary clone
rm -rf temp-agents
```

### 2. Hubungkan ke Konfigurasi AI Agent

- **Antigravity / AI Rules**: Arahkan context/rule sistem ke `.agents/AGENTS.md`.
- **Cursor IDE (`.cursorrules`)**: Tambahkan referensi ke file `.agents/AGENTS.md`.
- **GitHub Copilot (`.github/copilot-instructions.md`)**: Sertakan isi instruksi dari `.agents/AGENTS.md`.

---

## 🛠️ Perintah / Command AI yang Tersedia

Saat berinteraksi dengan AI Coding Assistant, Anda dapat menggunakan prompt / perintah ringkas berikut:

| Perintah Prompt | Deskripsi | Dokumen Rujukan |
| :--- | :--- | :--- |
| `buat fitur baru [nama]` | Membangun fitur lengkap dari skema DB, API backend, API client, hooks, hingga halaman UI | `.agents/workflows/new-feature.md` |
| `buat api baru [endpoint]` | Membuat Route Handler, validasi Zod schema, dan Service logic | `.agents/workflows/new-api.md` |
| `buat page baru [rute]` | Membuat Server Component page lengkap dengan SEO metadata, loading, & error state | `.agents/workflows/new-page.md` |
| `buat komponen [nama]` | Membuat komponen UI modular yang type-safe & accessible | `.agents/workflows/new-component.md` |
| `fix bug [deskripsi]` | Menjalankan 5 langkah diagnosis root cause tanpa mengubah kode yang tidak relevan | `.agents/workflows/bug-fix.md` |
| `refactor [modul/file]` | Melakukan restrukturisasi kode inkremental tanpa memecah fungsionalitas yang ada | `.agents/workflows/refactor.md` |
| `review code` | Menjalankan audit checklist keamanan, TypeScript strictness, dan arsitektur | `.agents/workflows/code-review.md` |

---

## 📄 Lisensi
MIT License © 2026 Nanang Supriatna
