# Panduan Desain Responsif (Responsive Design)

Pedoman membangun tampilan antarmuka yang adaptif di berbagai ukuran layar, mulai dari perangkat mobile terkecil hingga monitor desktop lebar.

---

## 📱 1. Prinsip Mobile-First

- **Mobile-First Approach**: Tulis class default untuk perangkat mobile (layar kecil), kemudian tambahkan breakpoint Tailwind secara progresif untuk layar yang lebih besar (`sm:`, `md:`, `lg:`, `xl:`, `2xl:`).
  ```html
  <!-- Contoh: 1 kolom di mobile, 2 di tablet, 3 di desktop -->
  <div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-3">
    ...
  </div>
  ```

---

## 📐 2. Breakpoints Standar Tailwind CSS

| Prefix | Resolusi Minimal | Target Perangkat |
| :--- | :--- | :--- |
| `default` | `< 640px` | Smartphone (Portrait) |
| `sm` | `640px` | Smartphone Besar / Phablet |
| `md` | `768px` | Tablet (iPad / Android Tablet) |
| `lg` | `1024px` | Laptop / Desktop Kecil |
| `xl` | `1280px` | Desktop Standar |
| `2xl` | `1536px` | Monitor Ultra-Wide / Layar Besar |

---

## 🧭 3. Pola Layout Responsif

1. **Navigasi & Sidebar**:
   - Tampilkan hamburger menu / sheet drawer pada layar mobile (`< md`).
   - Tampilkan sidebar statis atau header navigasi penuh pada desktop (`md:` ke atas).

2. **Tabel Data vs Card List**:
   - Pada desktop: gunakan tabel HTML (`table`, `thead`, `tbody`) dengan border solid.
   - Pada mobile: ubah baris tabel menjadi kartu individual (`flex-col` / `grid-cols-1`) atau gunakan wrapper dengan overflow horizontal (`overflow-x-auto`).

3. **Touch Targets pada Mobile**:
   - Pastikan tombol dan elemen interaktif di layar sentuh memiliki area klik minimal **44x44 pixel** untuk kenyamanan pengguna mobile.

4. **Ukuran Font & Spacing Dinamis**:
   - Gunakan tipografi responsif (contoh: `text-xl sm:text-2xl lg:text-3xl`).
   - Gunakan padding kontainer dinamis: `px-4 sm:px-6 lg:px-8`.