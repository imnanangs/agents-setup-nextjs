# Navigasi & Routing Next.js

Pedoman navigasi klien, prefetching, redirect, dan manipulasi query parameters di Next.js App Router.

---

## 🧭 1. Komponen `<Link>` & Prefetching

- **Wajib Komponen `<Link>`**: Selalu gunakan `Link` dari `next/link` untuk navigasi internal aplikasi guna mendukung *Client-Side Navigation* tanpa full page reload.
  ```tsx
  import Link from "next/link";

  <Link
    href="/dashboard/users"
    className="rounded-lg border border-slate-900 px-4 py-2 text-sm font-medium"
  >
    Lihat Pengguna
  </Link>
  ```
- **Prefetching**: Next.js otomatis melakukan prefetch pada rute yang ada di dalam viewport pengguna.

---

## 🔀 2. Programmatic Navigation & Redirects

1. **Di Client Components**:
   - Gunakan `useRouter` dari `next/navigation`:
   ```tsx
   "use client";
   import { useRouter } from "next/navigation";

   export function NavigationButton() {
     const router = useRouter();

     const handleNavigate = () => {
       router.push("/checkout");
       router.refresh(); // Refresh Server Component tree jika diperlukan
     };

     return <button onClick={handleNavigate}>Beli Sekarang</button>;
   }
   ```

2. **Di Server Components / Server Actions**:
   - Gunakan fungsi `redirect()` atau `permanentRedirect()` dari `next/navigation`:
   ```tsx
   import { redirect } from "next/navigation";

   export default async function ProtectedPage() {
     const session = await getSession();
     if (!session) {
       redirect("/login");
     }
     return <div>Halaman Rahasia</div>;
   }
   ```

---

## 🔍 3. Membaca & Memanipulasi URL Parameters

1. **Pathname & Search Params di Client**:
   - Gunakan `usePathname()` dan `useSearchParams()` dari `next/navigation`.
   - Gunakan `URLSearchParams` untuk memperbarui query string tanpa me-reload halaman:
   ```tsx
   "use client";
   import { usePathname, useRouter, useSearchParams } from "next/navigation";

   export function SearchInput() {
     const searchParams = useSearchParams();
     const pathname = usePathname();
     const { replace } = useRouter();

     const handleSearch = (term: string) => {
       const params = new URLSearchParams(searchParams);
       if (term) {
         params.set("search", term);
       } else {
         params.delete("search");
       }
       replace(`${pathname}?${params.toString()}`);
     };

     return <input onChange={(e) => handleSearch(e.target.value)} defaultValue={searchParams.get("search") || ""} />;
   }
   ```

2. **Search Params di Server Component**:
   - Terima prop `searchParams` sebagai Promise di Server Component Page:
   ```tsx
   interface Props {
     searchParams: Promise<{ [key: string]: string | string[] | undefined }>;
   }

   export default async function Page({ searchParams }: Props) {
     const params = await searchParams;
     const query = params.search;
     return <div>Pencarian: {query}</div>;
   }
   ```