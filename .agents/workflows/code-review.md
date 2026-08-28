# Prosedur Code Review (Code Review Checklist)

Daftar periksa kualitas (*quality checklist*) sebelum kode dimerge ke branch utama atau dideploy ke production.

---

## 📋 Checklist Tinjauan Kode

### 1. Kualitas & Gaya Kode (Code Quality)
- [ ] **TypeScript Strict**: Tidak ada tipe data `any` tersembunyi atau bypass `@ts-ignore` tanpa alasan arsitektural yang sah.
- [ ] **Penamaan Deskriptif**: Variabel, fungsi, file, dan types menggunakan nama yang jelas dan konsisten.
- [ ] **DRY (Don't Repeat Yourself)**: Tidak ada kode duplikat yang dapat digantikan dengan komponen/utilitas bersama.
- [ ] **Single Responsibility**: Setiap fungsi atau komponen hanya fokus pada satu tugas.

### 2. Frontend & Desain Antarmuka
- [ ] **Flat & Minimalist**: Mengikuti aturan styling tanpa box-shadow, tanpa glassmorphism, menggunakan border solid dan `rounded-lg`.
- [ ] **Pemisahan RSC vs Client**: Direktif `'use client'` hanya ditaruh pada komponen interaktif di daun pohon hierarki.
- [ ] **Integrasi API Terstandarisasi**: Tidak ada `fetch()` mentah secara inline di dalam page/komponen; selalu melalui API Service / Hooks layer.
- [ ] **Aksesibilitas**: Formulir memiliki label yang tepat, tombol interaktif memiliki `aria-label`, dan navigasi keyboard berfungsi dengan baik.

### 3. Backend, Database & Keamanan
- [ ] **Arsitektur Modular**: Route handler ramping, seluruh logika bisnis berada di `src/modules/[feature]/[feature].service.ts`.
- [ ] **Validasi Skema (Zod)**: Semua input (body, params, query) telah divalidasi dan menggunakan `.strict()`.
- [ ] **Format Respon Standar**: Output API menggunakan struktur `{ status, message, data, meta }`.
- [ ] **Keamanan**: Tidak ada hardcoded credentials, otorisasi RBAC dan kepemilikan resource dicek di Service Layer, query database bebas dari risiko N+1 & SQL Injection.
- [ ] **Error Handling**: Error ditangkap secara terpusat, dan error internal (500) disanitasi di production.