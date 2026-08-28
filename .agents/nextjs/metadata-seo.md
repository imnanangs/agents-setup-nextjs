# Metadata & Optimasi SEO

Pedoman pengelolaan metadata halaman, Open Graph, Twitter Cards, dynamic SEO, dan sitemap pada Next.js App Router.

---

## 🔍 1. Metadata Statis & Dinamis

Next.js menyediakan Metadata API berbasis tipe data TypeScript (`Metadata` dan `ResolvingMetadata`).

### A. Metadata Statis (Root Layout atau Halaman Statis)
```tsx
// src/app/layout.tsx atau src/app/about/page.tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: {
    template: "%s | Nama Aplikasi",
    default: "Nama Aplikasi - Solusi Modern",
  },
  description: "Deskripsi lengkap aplikasi untuk optimasi mesin pencari (SEO).",
  metadataBase: new URL(process.env.NEXT_PUBLIC_APP_URL || "https://example.com"),
  openGraph: {
    title: "Nama Aplikasi",
    description: "Deskripsi Open Graph untuk media sosial.",
    type: "website",
    locale: "id_ID",
    images: [
      {
        url: "/og-image.png",
        width: 1200,
        height: 630,
        alt: "Preview Nama Aplikasi",
      },
    ],
  },
};
```

### B. Metadata Dinamis (`generateMetadata`)
Gunakan fungsi `generateMetadata` untuk halaman dengan data dinamis (artikel blog, detail produk, profil user):

```tsx
// src/app/products/[id]/page.tsx
import type { Metadata, ResolvingMetadata } from "next";
import { productApi } from "@/modules/products/products.api";

interface Props {
  params: Promise<{ id: string }>;
}

export async function generateMetadata({ params }: Props): Promise<Metadata> {
  const { id } = await params;
  const { data: product } = await productApi.getProductById(id);

  if (!product) {
    return { title: "Produk Tidak Ditemukan" };
  }

  return {
    title: product.name,
    description: product.description.slice(0, 160),
    openGraph: {
      title: product.name,
      description: product.description,
      images: [{ url: product.imageUrl }],
    },
  };
}
```

---

## 🗺️ 2. Sitemap & Robots (`sitemap.ts` & `robots.ts`)

Letakkan file `sitemap.ts` dan `robots.ts` langsung di `src/app/`:

```typescript
// src/app/robots.ts
import { MetadataRoute } from "next";

export default function robots(): MetadataRoute.Robots {
  return {
    rules: {
      userAgent: "*",
      allow: "/",
      disallow: ["/api/", "/dashboard/admin/"],
    },
    sitemap: `${process.env.NEXT_PUBLIC_APP_URL}/sitemap.xml`,
  };
}
```

```typescript
// src/app/sitemap.ts
import { MetadataRoute } from "next";

export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const baseUrl = process.env.NEXT_PUBLIC_APP_URL || "https://example.com";

  return [
    {
      url: baseUrl,
      lastModified: new Date(),
      changeFrequency: "daily",
      priority: 1,
    },
    {
      url: `${baseUrl}/products`,
      lastModified: new Date(),
      changeFrequency: "weekly",
      priority: 0.8,
    },
  ];
}
```