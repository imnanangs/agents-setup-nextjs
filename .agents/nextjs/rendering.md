# Strategi Rendering Next.js

Pedoman pemilihan strategi rendering: **Static Site Generation (SSG)**, **Dynamic Server-Side Rendering (SSR)**, dan **Incremental Static Regeneration (ISR)** di Next.js App Router.

---

## 🧭 1. Spektrum Rendering di App Router

| Strategi | Kapan Digunakan? | Implementasi Next.js |
| :--- | :--- | :--- |
| **Static Rendering (SSG)** | Konten jarang berubah (Landing Page, Docs, FAQ, Kebijakan Privasi). | Default pada route tanpa request dinamis / cookies. |
| **Incremental Static Regeneration (ISR)** | Halaman statis yang perlu refresh otomatis berkala (Katalog produk, Artikel blog). | Menggunakan `revalidate = 3600` atau `revalidateTag()`. |
| **Dynamic Rendering (SSR)** | Halaman dengan data pengguna personal, data real-time, atau membaca cookies/headers. | Otomatis saat mengakses `cookies()`, `headers()`, atau `searchParams`. |
| **Streaming SSR & Suspense** | Halaman dengan bagian data cepat dan bagian lambat secara bersamaan. | Komposisi `<Suspense fallback={<Skeleton />}>`. |

---

## ⚡ 2. Konfigurasi Route Segment

Gunakan Route Segment Options di `page.tsx` atau `layout.tsx` untuk mengontrol sifat rendering:

```typescript
// src/app/products/page.tsx

// 1. Force Dynamic Rendering (tidak dicache statis)
export const dynamic = "force-dynamic";

// 2. ISR: Revalidasi halaman setiap 60 detik
export const revalidate = 60;

// 3. Static Export Params untuk dynamic route
export async function generateStaticParams() {
  const products = await productApi.getAllProductIds();
  return products.map((id) => ({ id }));
}
```

---

## 🌊 3. Streaming SSR dengan React Suspense

Hindari memblokir seluruh halaman hanya karena satu query yang lambat. Pisahkan komponen data-heavy ke dalam blok `<Suspense>` terpisah:

```tsx
// src/app/(dashboard)/analytics/page.tsx
import { Suspense } from "react";
import { QuickStats } from "./_components/quick-stats";
import { HeavyChart } from "./_components/heavy-chart";
import { Skeleton } from "@/components/ui/skeleton";

export default function AnalyticsPage() {
  return (
    <div className="space-y-6">
      <h1 className="text-2xl font-bold">Laporan Analitik</h1>

      {/* Komponen cepat dirender langsung */}
      <QuickStats />

      {/* Komponen lambat di-stream secara independen */}
      <Suspense fallback={<Skeleton className="h-72 w-full rounded-lg" />}>
        <HeavyChart />
      </Suspense>
    </div>
  );
}
```