# Prosedur Pembuatan Komponen UI Baru (New Component Workflow)

Standar pembuatan komponen antarmuka React/Next.js yang konsisten dengan *design system*, modular, dan accessible.

---

## 🎨 Alur Pembuatan Komponen Baru

```mermaid
flowchart TD
    A[1. Tentukan Kategori Komponen] -->|Global Atomic UI| B[src/components/ui/]
    A -->|Feature-Specific UI| C[src/modules/*/components/]
    B & C --> D[2. Definisikan TypeScript Props Interface]
    D --> E[3. Terapkan Flat Minimalist Styling & cn()]
    E --> F[4. Tambahkan Aksesibilitas & Focus States]
    F --> G[5. Export Named Component]
```

---

### Langkah 1: Tentukan Lokasi Komponen
- **Komponen Atomik Global**: Letakkan di `src/components/ui/` (misal: `badge.tsx`, `dialog.tsx`, `table.tsx`, `dropdown.tsx`).
- **Komponen Spesifik Fitur**: Letakkan di `src/modules/[feature]/components/` atau `src/app/(dashboard)/[feature]/_components/`.

### Langkah 2: Tulis Komponen dengan Strict Types & Flat Design
Contoh pembuatan komponen `Card`:

```tsx
import * as React from "react";
import { cn } from "@/lib/utils";

export interface CardProps extends React.HTMLAttributes<HTMLDivElement> {
  children: React.ReactNode;
}

export function Card({ className, children, ...props }: CardProps) {
  return (
    <div
      className={cn(
        "rounded-lg border border-border bg-card p-6 text-card-foreground",
        className
      )}
      {...props}
    >
      {children}
    </div>
  );
}

export function CardHeader({ className, children, ...props }: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div className={cn("mb-4 flex flex-col space-y-1.5", className)} {...props}>
      {children}
    </div>
  );
}

export function CardTitle({ className, children, ...props }: React.HTMLAttributes<HTMLHeadingElement>) {
  return (
    <h3 className={cn("text-lg font-semibold tracking-tight", className)} {...props}>
      {children}
    </h3>
  );
}
```

### Langkah 3: Verifikasi Kepatuhan
1. **Banned Styles Check**: Pastikan tidak ada box shadow (`shadow-*`) atau backdrop blur (`backdrop-blur-*`).
2. **Accessible**: Pastikan interaksi hover dan keyboard focus states jelas.
3. **Named Export**: Pastikan di-export menggunakan `export function NamaKomponen() {}`.
