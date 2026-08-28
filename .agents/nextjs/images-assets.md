# Optimasi Aset & Gambar (Images & Assets)

Pedoman penggunaan komponen gambar dan pengelolaan aset statis di Next.js untuk performa web maksimal.

---

## 🖼️ 1. Standar Penggunaan `next/image`

- **Wajib `next/image`**: Jangan gunakan tag `<img>` HTML biasa tanpa optimasi. Selalu gunakan komponen `Image` dari `next/image`.
- **Keuntungan Otomatis**:
  - Konversi format otomatis ke format modern yang ringan (**WebP / AVIF**).
  - Mencegah Cumulative Layout Shift (CLS) secara otomatis.
  - Pemuatan malas (*Lazy Loading*) otomatis untuk gambar di luar viewport.

---

## 📐 2. Pola Penulisan Komponen `Image`

### A. Gambar Lokal (Statis)
Impor file gambar secara langsung agar dimensi (width/height) dan blur placeholder terdeteksi otomatis:
```tsx
import Image from "next/image";
import heroImg from "@/public/images/hero.png";

export function HeroBanner() {
  return (
    <Image
      src={heroImg}
      alt="Hero banner aplikasi"
      placeholder="blur"
      priority // Gunakan priority jika gambar berada di atas lipatan layar (Above the Fold)
    />
  );
}
```

### B. Gambar Remote / Dinamis (URL Eksternal)
Wajib mendefinisikan `width` & `height`, atau menggunakan prop `fill`:
```tsx
// Menggunakan Dimensi Eksplisit
<Image
  src={user.avatarUrl}
  alt={user.name}
  width={48}
  height={48}
  className="rounded-full border border-border"
/>

// Menggunakan Prop 'fill' untuk Kontainer Responsif
<div className="relative h-64 w-full overflow-hidden rounded-lg border border-border">
  <Image
    src={product.imageUrl}
    alt={product.title}
    fill
    sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
    className="object-cover"
  />
</div>
```

---

## 🔒 3. Konfigurasi Remote Images (`next.config.ts`)

Setiap domain eksternal gambar remote **WAJIB** didaftarkan pada `images.remotePatterns`:

```typescript
// next.config.ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  images: {
    formats: ["image/avif", "image/webp"],
    remotePatterns: [
      {
        protocol: "https",
        hostname: "images.unsplash.com",
      },
      {
        protocol: "https",
        hostname: "res.cloudinary.com",
      },
    ],
  },
};

export default nextConfig;
```

---

## 🔤 4. Optimasi Font & Icon

- **Next Font (`next/font`)**: Gunakan `next/font/google` atau `next/font/local` untuk zero-layout-shift font loading tanpa request ke server pihak ketiga saat runtime.
- **Icons**: Gunakan icon library berbasis SVG modern (seperti `lucide-react`) yang mendukung *tree-shaking*. Hindari mengimpor icon secara massal yang dapat membengkakkan bundle size.