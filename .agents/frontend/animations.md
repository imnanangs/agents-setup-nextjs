# Panduan Animasi & Transisi (Animations)

Pedoman penambahan animasi antarmuka yang halus, fungsional, hemat performa, dan mendukung preferensi aksesibilitas.

---

## 🎯 1. Filosofi Animasi: Subtil & Fungsional

- **Tujuan Animasi**: Animasi bertujuan memberikan feedback status kepada pengguna (misal: hover, active, loading, buka/tutup modal), bukan sebagai hiasan visual berlebihan yang memperlambat alur kerja.
- **Durasi Cepat & Alami**: Gunakan durasi transisi yang singkat dan responsif:
  - Micro-interactions (tombol, hover, switch): `150ms` – `200ms` (`duration-150` / `duration-200`).
  - Modal, drawer, dropdown, accordion: `200ms` – `300ms` (`duration-200` / `duration-300`).
  - Easing kurva: `ease-out` atau `ease-in-out`.

---

## 🎨 2. Standar Animasi Tailwind CSS

Gunakan class transisi bawaan Tailwind untuk interaksi dasar:

```html
<!-- Contoh Transisi Hover Tombol -->
<button class="border border-slate-900 bg-white px-4 py-2 text-slate-900 transition-colors duration-150 ease-in-out hover:bg-slate-900 hover:text-white">
  Simpan Perubahan
</button>

<!-- Contoh Animasi Skeleton Loading -->
<div class="h-6 w-32 animate-pulse rounded-lg bg-gray-200 dark:bg-gray-800"></div>

<!-- Contoh Spinner Loading -->
<svg class="h-4 w-4 animate-spin text-current" ...></svg>
```

---

## 🎭 3. Animasi Kompleks (Framer Motion / Motion)

Jika proyek memerlukan animasi layout atau transisi enter/exit:
- Gunakan **Framer Motion** (`motion/react` atau `framer-motion`) secara selektif di Client Component.
- Manfaatkan `AnimatePresence` untuk transisi buka/tutup modal, toast notification, atau collapsible menu.

```tsx
"use client";

import { motion, AnimatePresence } from "framer-motion";

export function Modal({ isOpen, children }: { isOpen: boolean; children: React.ReactNode }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          initial={{ opacity: 0, y: 8 }}
          animate={{ opacity: 1, y: 0 }}
          exit={{ opacity: 0, y: 8 }}
          transition={{ duration: 0.2, ease: "easeOut" }}
          className="rounded-lg border border-slate-900 bg-white p-6 dark:border-slate-100 dark:bg-zinc-900"
        >
          {children}
        </motion.div>
      )}
    </AnimatePresence>
  );
}
```

---

## ♿ 4. Aksesibilitas (Reduced Motion)

- Hormati preferensi pengguna yang mengaktifkan *Reduced Motion* pada sistem operasinya (mencegah pusing/motion sickness).
- Tailwind utility: Gunakan `motion-safe:` atau `motion-reduce:` bila perlu.
  ```html
  <div class="transition-transform motion-reduce:transition-none">...</div>
  ```