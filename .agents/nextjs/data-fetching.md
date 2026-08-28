# Panduan Integrasi API & Data Fetching (Best Practices)

Pedoman arsitektur integrasi API dan pengambilan data di Next.js App Router. **DILARANG KERAS** menulis panggilan `fetch()` mentah secara inline langsung di dalam komponen halaman (`page.tsx`) atau komponen UI.

---

## 🚫 1. Larangan Keras (Anti-Patterns)

- ❌ **Dilarang Raw Fetch di Page/UI Component**: Jangan menulis `fetch('/api/users')` langsung di dalam file `page.tsx` atau komponen form/tabel.
- ❌ **Dilarang Hardcode Endpoint URL**: Jangan menyebarkan string endpoint URL (misal: `https://api.domain.com/v1/...`) di berbagai komponen.
- ❌ **Dilarang Duplikasi Logika Error & Header**: Jangan menulis ulang logic parsing token JWT, handling status code, atau parsing JSON berulang-ulang di setiap pemanggilan API.

---

## 🏗️ 2. Arsitektur Integrasi API Berlapis

Struktur pengambilan data harus dipisahkan menjadi layer-layer yang rapi:

```mermaid
flowchart TD
    A[Page / UI Component] -->|Memanggil hook atau function| B[Feature API Layer]
    B -->|src/modules/[feature]/[feature].api.ts| C[Centralized HTTP Client]
    C -->|src/lib/api-client.ts| D[Backend API / External Services]
```

### 1. Centralized HTTP Client (`src/lib/api-client.ts` / `src/common/utils/http.ts`)
Membungkus `fetch` dengan konfigurasi global:
- Otomatis menyertakan Header (Content-Type, Token Auth/Bearer).
- Otomatis mengekstrak standard response JSON (`status`, `message`, `data`, `meta`).
- Otomatis menangani error status HTTP (401 unauthorized redirect, 403 forbidden, 500 server error).

```typescript
// src/lib/api-client.ts
import { ApiResponse } from "@/types/api";

interface FetchOptions extends RequestInit {
  params?: Record<string, string | number | boolean | undefined>;
}

export async function apiClient<T>(
  endpoint: string,
  options: FetchOptions = {}
): Promise<ApiResponse<T>> {
  const { params, headers, ...customConfig } = options;

  // Build Query Params jika ada
  let url = endpoint;
  if (params) {
    const searchParams = new URLSearchParams();
    Object.entries(params).forEach(([key, value]) => {
      if (value !== undefined) searchParams.append(key, String(value));
    });
    const queryString = searchParams.toString();
    if (queryString) url += `?${queryString}`;
  }

  const config: RequestInit = {
    headers: {
      "Content-Type": "application/json",
      ...headers,
    },
    ...customConfig,
  };

  const response = await fetch(url, config);
  const data: ApiResponse<T> = await response.json();

  if (!response.ok || !data.status) {
    throw new Error(data.message || "Terjadi kesalahan pada permintaan data");
  }

  return data;
}
```

---

## 📦 3. Feature API Layer (`src/modules/[feature]/[feature].api.ts`)

Buat fungsi wrapper bertipe data ketat (*type-safe*) untuk setiap endpoint:

```typescript
// src/modules/users/users.api.ts
import { apiClient } from "@/lib/api-client";
import { User, CreateUserInput, GetUsersQueryInput } from "./users.interface";

export const userApi = {
  // Ambil daftar user dengan filter & pagination
  getUsers: (params?: GetUsersQueryInput) => {
    return apiClient<User[]>("/api/users", {
      method: "GET",
      params: params as Record<string, string | number | boolean>,
    });
  },

  // Ambil detail user by ID
  getUserById: (id: string) => {
    return apiClient<User>(`/api/users/${id}`, {
      method: "GET",
      // Contoh tag-based cache revalidation di Server Component
      next: { tags: [`user-${id}`] },
    });
  },

  // Tambah user baru
  createUser: (payload: CreateUserInput) => {
    return apiClient<User>("/api/users", {
      method: "POST",
      body: JSON.stringify(payload),
    });
  },

  // Hapus user
  deleteUser: (id: string) => {
    return apiClient<void>(`/api/users/${id}`, {
      method: "DELETE",
    });
  },
};
```

---

## 💻 4. Konsumsi di Komponen

### A. Di React Server Components (RSC - Server-side Data Fetching)
Panggil fungsi service secara asinkron di dalam Server Component tanpa `useEffect` / `useState`:

```tsx
// src/app/(dashboard)/users/page.tsx
import { userApi } from "@/modules/users/users.api";
import { UserListTable } from "./_components/user-list-table";

interface PageProps {
  searchParams: Promise<{ page?: string; search?: string }>;
}

export default async function UsersPage({ searchParams }: PageProps) {
  const { page = "1", search } = await searchParams;
  
  // Memanggil API Client terstandarisasi (bukan raw fetch)
  const response = await userApi.getUsers({
    page: Number(page),
    search,
  });

  return (
    <main className="p-6">
      <h1 className="text-2xl font-bold mb-4">Manajemen Pengguna</h1>
      <UserListTable users={response.data} meta={response.meta} />
    </main>
  );
}
```

### B. Di Client Components (Mutasi / Interaksi Dinamis)
Gunakan custom hooks di `[feature].hooks.ts` dengan library seperti **TanStack React Query** (atau SWR):

```tsx
// src/modules/users/users.hooks.ts
"use client";

import { useQuery, useMutation, useQueryClient } from "@tanstack/react-query";
import { userApi } from "./users.api";
import { CreateUserInput, GetUsersQueryInput } from "./users.interface";

export function useUsers(params?: GetUsersQueryInput) {
  return useQuery({
    queryKey: ["users", params],
    queryFn: () => userApi.getUsers(params),
  });
}

export function useCreateUser() {
  const queryClient = useQueryClient();

  return useMutation({
    mutationFn: (payload: CreateUserInput) => userApi.createUser(payload),
    onSuccess: () => {
      // Otomatis refresh cache query users
      queryClient.invalidateQueries({ queryKey: ["users"] });
    },
  });
}
```

---

## ⚡ 5. Keuntungan Pendekatan Ini

1. **Reusability**: Endpoint API yang sama bisa dipanggil dari berbagai halaman/komponen tanpa duplikasi logic.
2. **Strict Type-Safety**: Data return dan payload otomatis memiliki tipe TypeScript yang valid.
3. **Easy Maintenance**: Jika ada perubahan URL endpoint atau header autentikasi, hanya perlu diubah di 1 file service / HTTP client.
4. **Clean Code**: Komponen UI (`page.tsx` atau `component.tsx`) tetap bersih dan hanya fokus pada rendering UI.