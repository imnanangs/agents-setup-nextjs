# Konvensi App Router Next.js

Pedoman penggunaan fitur-fitur dan konvensi file khusus pada **Next.js App Router**.

---

## 📑 1. Hirarki File Spesial (Special Files Hierarchy)

Di dalam direktori rute manapun, Next.js mengevaluasi file-file berikut dalam urutan bersarang (*nested*):

```text
<Layout>
  <Template>
    <ErrorBoundary fallback={<Error />}>
      <Suspense fallback={<Loading />}>
        <NotFoundBoundary fallback={<NotFound />}>
          <Page />
        </NotFoundBoundary>
      </Suspense>
    </ErrorBoundary>
  </Template>
</Layout>
```

---

## 🛠️ 2. Aturan Implementasi File Spesial

1. **`page.tsx`**:
   - Titik masuk antarmuka halaman rute.
   - Wajib berupa **Default Export** (`export default function Page() {}`).
   - Default berupa Server Component yang memanggil API Service layer.

2. **`layout.tsx`**:
   - Membungkus seluruh sub-halaman di jalurnya tanpa melakukan re-render layout saat navigasi antar sub-rute.
   - Wajib menerima dan merender prop `{ children }: { children: React.ReactNode }`.

3. **`loading.tsx`**:
   - Otomatis membungkus `page.tsx` dengan `<Suspense fallback={<Loading />}>`.
   - Gunakan komponen Skeleton berbasis border tegas dan `animate-pulse` untuk transisi loading yang mulus.

4. **`error.tsx`**:
   - **Wajib Client Component** (`'use client'`).
   - Menerima props `{ error: Error & { digest?: string }, reset: () => void }`.
   - Sediakan tombol coba lagi yang memicu fungsi `reset()`.

5. **`not-found.tsx`**:
   - Menampilkan UI kustom saat fungsi `notFound()` dari `next/navigation` dipanggil.

---

## ⚡ 3. Dynamic Routing & Parallel / Intercepting Routes

- **Dynamic Segments**: `[id]`, `[slug]`. Ekstrak parameter menggunakan `params: Promise<{ id: string }>` di Page Server Component.
- **Catch-all Segments**: `[...slug]` atau `[[...slug]]` untuk rute dinamis bertingkat.
- **Parallel Routes (`@slot`)** & **Intercepting Routes (`(.)photo`)**: Gunakan saat membangun modal routing atau dashboard dengan panel independen.