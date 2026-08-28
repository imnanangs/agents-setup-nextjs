# Panduan UI & Styling (CRITICAL)

Filosofi desain utama antarmuka pengguna pada proyek ini mengusung **Clean, Flat, Minimalist Design System** dengan border solid yang tegas.

---

## 🚫 1. Gaya yang DILARANG KERAS (Banned Styles)

- ❌ **Box Shadows**: **DILARANG KERAS** menggunakan bayangan seperti `shadow`, `shadow-sm`, `shadow-md`, `shadow-lg`, `shadow-xl`, `shadow-2xl`.
- ❌ **Glassmorphism**: **DILARANG KERAS** menggunakan efek blur/transparan seperti `backdrop-blur`, `bg-opacity-*` bertumpuk untuk efek kaca.
- ❌ **Gradients**: Hindari gradient (`bg-gradient-to-*`) kecuali jika secara eksplisit diminta oleh user.
- ❌ **Over-rounded Corners**: Hindari `rounded-full` atau `rounded-3xl` pada kartu/wadah (kecuali avatar profil atau badge pil kecil).

---

## 🟢 2. Gaya yang WAJIB Digunakan (Required Styles)

1. **Crisp Solid Borders**:
   - Selalu gunakan border solid yang tegas dan presisi untuk memisahkan kontainer, kartu, tabel, dan input.
   - Contoh kelas: `border border-border`, `border-2 border-slate-900`, `border-slate-200 dark:border-slate-800`.

2. **Border Radius Konsisten**:
   - Standar radius utama adalah **`rounded-lg`** (atau `rounded-md` untuk elemen kecil seperti tag/badge).

3. **Warna Flat & High Contrast**:
   - Gunakan latar belakang solid dan kontras tinggi untuk teks agar mudah dibaca.
   - Manfaatkan semantic CSS variables Tailwind (seperti `bg-background`, `text-foreground`, `bg-card`, `bg-muted`, `border-border`).

4. **Support Light & Dark Mode**:
   - Gunakan semantic color tokens Tailwind (`dark:` variants atau CSS variable-based styling) agar siap mendukung mode gelap dan terang tanpa hardcoded hex colors.
