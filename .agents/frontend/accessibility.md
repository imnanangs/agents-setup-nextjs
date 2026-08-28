# Panduan Aksesibilitas (Web Accessibility - a11y)

Pedoman memastikan aplikasi dapat diakses dengan mudah oleh semua pengguna, termasuk pengguna dengan disabilitas, pembaca layar (*screen readers*), dan navigasi keyboard penuh (standar **WCAG 2.1 AA**).

---

## ⌨️ 1. Navigasi Keyboard Penuh

- **Tab Order yang Logis**: Elemen interaktif (`<a>`, `<button>`, `<input>`, `<select>`, `<textarea>`) harus dapat dijangkau menggunakan tombol `Tab` dalam urutan logis.
- **Focus Rings Jelas**: Jangan pernah menghapus outline fokus (`outline-none`) tanpa memberikan visual focus indicator pengganti yang jelas!
  ```html
  <!-- Contoh Focus Ring Tegas -->
  <button class="focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-slate-900 focus-visible:ring-offset-2 dark:focus-visible:ring-slate-100">
    Aksi
  </button>
  ```
- **Aksi Keyboard Standar**:
  - Tombol: `Enter` dan `Space` untuk memicu aksi.
  - Modal/Dropdown: tombol `Esc` untuk menutup tampilan.
  - Listbox/Select: tombol panah `Up` / `Down` untuk memilih opsi.

---

## 🗣️ 2. Elemen Semantik & Atribut ARIA

1. **HTML Semantik Murni**:
   - Selalu prioritaskan tag HTML semantik asli (`<main>`, `<header>`, `<footer>`, `<nav>`, `<aside>`, `<section>`, `<article>`) daripada membuat tumpukan `<div>`.
   - Gunakan `<button>` untuk aksi dan `<Link>` / `<a>` untuk navigasi halaman. Dilarang membuat `<div onClick={...}>` untuk tombol tanpa peran keyboard.

2. **Atribut ARIA**:
   - Berikan `aria-label` pada tombol icon yang tidak memiliki teks (misal: tombol tutup silang 'X' atau tombol hamburger menu).
     ```html
     <button aria-label="Tutup menu navigasi">
       <XIcon className="h-5 w-5" />
     </button>
     ```
   - Gunakan `aria-expanded="true/false"` pada dropdown dan menu accordion.
   - Gunakan `aria-live="polite"` atau `role="alert"` untuk pesan error dinamis agar dibacakan oleh screen reader.

---

## 👁️ 3. Kontras Warna & Aksesibilitas Visual

- **Rasio Kontras Minimum**: Teks biasa harus memenuhi rasio kontras minimal **4.5:1** terhadap latar belakangnya. Teks besar/tebal minimal **3:1**.
- **Jangan Mengandalkan Warna Semata**: Jangan hanya menggunakan warna merah untuk menunjukkan error. Sertakan juga icon peringatan atau teks penjelas.
- **Alt Text Gambar**: Selalu isi atribut `alt` pada komponen `next/image` (`<Image src="..." alt="Deskripsi gambar yang bermakna" />`). Jika gambar hanya bersifat dekoratif, gunakan `alt=""` agar dilewati oleh screen reader.