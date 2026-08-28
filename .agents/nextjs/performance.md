# Performa & Optimasi Next.js

Pedoman optimasi Core Web Vitals (LCP, FID/INP, CLS), pemecahan bundle JavaScript, dan efisiensi runtime di Next.js.

---

## ⚡ 1. Optimasi Core Web Vitals

1. **LCP (Largest Contentful Paint)**:
   - Tambahkan prop `priority` pada gambar utama di atas lipatan layar (*Above the Fold* / Hero Image).
   - Hindari render blocking scripts pada initial load.

2. **INP (Interaction to Next Paint)**:
   - Pecah komponen interaktif besar menjadi komponen kecil terisolasi.
   - Hindari blocking computational task di main thread. Gunakan `useTransition` atau web workers untuk pemrosesan berat.

3. **CLS (Cumulative Layout Shift)**:
   - Selalu tentukan aspek rasio atau `width` & `height` eksplisit pada gambar dan video.
   - Gunakan Skeleton loading dengan dimensi yang identik dengan konten akhir saat data sedang dimuat via Suspense.

---

## 📦 2. Code Splitting & Dynamic Imports

Gunakan `next/dynamic` untuk komponen berat yang hanya muncul saat kondisi tertentu (seperti Modal dialog besar, Chart canvas, Rich Text Editor, Maps):

```tsx
import dynamic from "next/dynamic";

// Memuat Chart hanya saat dibutuhkan di browser (mengurangi initial JS bundle)
const SalesChart = dynamic(
  () => import("./_components/sales-chart").then((mod) => mod.SalesChart),
  {
    ssr: false,
    loading: () => <div className="h-64 animate-pulse rounded-lg border border-border bg-muted" />,
  }
);
```

---

## 🗜️ 3. Bundle Analyzer & Tree Shaking

- Pastikan import library dilakukan secara spesifik (contoh: `import { Check, X } from "lucide-react"` alih-alih import all).
- Gunakan `@next/bundle-analyzer` secara berkala untuk memantau dependensi pihak ketiga yang terlalu berat.
- Aktifkan fitur kompresi output standalone di `next.config.ts` untuk meminimalkan ukuran Docker image production:
  ```typescript
  const nextConfig = {
    output: "standalone",
    // ...
  };
  ```