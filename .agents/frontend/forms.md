# Panduan Form & Validasi Frontend

Pedoman pembuatan form interaktif yang aman, memiliki validasi skema bertipe (*type-safe*), dan memberikan UX yang responsif.

---

## 🛠️ 1. Tooling & Standard Stack

- **Form State Management**: Gunakan **React Hook Form** (`react-hook-form`) untuk performa tinggi tanpa re-render berlebih.
- **Schema Validation**: Gunakan **Zod** (`zod`) bersama `@hookform/resolvers/zod`.
- **Integrasi API Layer**: **Dilarang keras memakai `fetch()` mentah secara inline**. Selalu integrasikan form dengan **Service API Layer** (`src/modules/[feature]/services/` atau `src/services/`) atau custom mutation hooks (misal TanStack Query / Server Actions).

---

## 📋 2. Pola Implementasi Form Standar

```tsx
"use client";

import { useForm } from "react-hook-form";
import { zodResolver } from "@hookform/resolvers/zod";
import { z } from "zod";
import { useState } from "react";
import { useRouter } from "next/navigation";
// Import dari API Layer (BUKAN raw fetch)
import { authApi } from "@/modules/auth/auth.api";

// 1. Skema Validasi Form
export const loginFormSchema = z.object({
  email: z.string().trim().email("Format email tidak valid"),
  password: z.string().min(8, "Password minimal 8 karakter"),
});

export type LoginFormValues = z.infer<typeof loginFormSchema>;

// 2. Komponen Form
export function LoginForm() {
  const router = useRouter();
  const [serverError, setServerError] = useState<string | null>(null);

  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<LoginFormValues>({
    resolver: zodResolver(loginFormSchema),
    defaultValues: {
      email: "",
      password: "",
    },
  });

  const onSubmit = async (values: LoginFormValues) => {
    setServerError(null);
    try {
      // Memanggil API Service yang sudah terstandarisasi
      await authApi.login(values);

      // Berhasil login, arahkan ke dashboard
      router.push("/dashboard");
      router.refresh();
    } catch (err: any) {
      setServerError(err.message || "Terjadi kesalahan saat masuk. Silakan coba lagi.");
    }
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      {serverError && (
        <div className="rounded-lg border border-red-500 bg-red-50 p-3 text-sm text-red-700 dark:bg-red-950/50 dark:text-red-300">
          {serverError}
        </div>
      )}

      <div>
        <label className="block text-sm font-medium mb-1">Email</label>
        <input
          type="email"
          autoComplete="email"
          {...register("email")}
          className="w-full rounded-lg border border-gray-300 px-3 py-2 text-sm focus:border-slate-900 focus:outline-none dark:border-gray-700 dark:bg-zinc-900"
          placeholder="nama@email.com"
        />
        {errors.email && (
          <p className="mt-1 text-xs text-red-600 dark:text-red-400">{errors.email.message}</p>
        )}
      </div>

      <div>
        <label className="block text-sm font-medium mb-1">Password</label>
        <input
          type="password"
          autoComplete="current-password"
          {...register("password")}
          className="w-full rounded-lg border border-gray-300 px-3 py-2 text-sm focus:border-slate-900 focus:outline-none dark:border-gray-700 dark:bg-zinc-900"
          placeholder="••••••••"
        />
        {errors.password && (
          <p className="mt-1 text-xs text-red-600 dark:text-red-400">{errors.password.message}</p>
        )}
      </div>

      <button
        type="submit"
        disabled={isSubmitting}
        className="w-full rounded-lg border border-slate-900 bg-slate-900 px-4 py-2 text-sm font-medium text-white hover:bg-slate-800 disabled:opacity-50 dark:border-slate-100 dark:bg-slate-100 dark:text-slate-900"
      >
        {isSubmitting ? "Memproses..." : "Masuk"}
      </button>
    </form>
  );
}
```

---

## 💡 3. Aturan UX & Aksesibilitas Form

1. **State Disable Saat Submit**: Selalu nonaktifkan tombol submit (`disabled={isSubmitting}`) saat mutasi berlangsung untuk mencegah *double submit*.
2. **Pesan Error yang Jelas**: Tampilkan error inline tepat di bawah field input yang bermasalah.
3. **Autofill & Tipe Input**: Selalu gunakan `type="email"`, `type="password"`, `autoComplete="email"`, `autoComplete="current-password"` untuk membantu password manager dan autokomplet peramban.
4. **Service Layer Integration**: Semua mutasi data harus melalui API Service atau Server Actions, bukan `fetch()` inline.