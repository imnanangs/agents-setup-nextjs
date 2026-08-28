# Penanganan Error (Error Handling)

Pedoman penanganan error yang konsisten, aman, dan dapat diprediksi di seluruh layer backend.

---

## ⚡ 1. Melempar Error (Service Layer)

- **Dilarang melempar HTTP Response dari Service**: Service Layer (`src/modules/[feature]/[feature].service.ts`) hanya bertugas memproses data. Jika terjadi kegagalan logika bisnis, lemparkan custom error (*throw error*).
- **Custom Error Class (`AppError` / `ApiError`)**:
  Gunakan class error kustom yang memuat HTTP status code dan pesan yang jelas:
  ```typescript
  // src/common/errors/app-error.ts
  export class AppError extends Error {
    public readonly statusCode: number;
    public readonly data: any[];

    constructor(message: string, statusCode: number = 400, data: any[] = []) {
      super(message);
      this.statusCode = statusCode;
      this.data = data;
      Object.setPrototypeOf(this, new.target.prototype);
    }
  }
  ```
  Contoh penggunaan di Service:
  ```typescript
  throw new AppError("Pengguna dengan email ini sudah terdaftar", 409);
  throw new AppError("Data produk tidak ditemukan", 404);
  ```

---

## 🎣 2. Menangkap Error (Route Handlers & Centralized Handler)

Setiap Route Handler wajib menangkap error menggunakan helper terpusat (`handleApiError`):

```typescript
// src/common/utils/error-handler.util.ts
import { NextResponse } from "next/server";
import { ZodError } from "zod";
import { AppError } from "../errors/app-error";

export function handleApiError(error: unknown) {
  // 1. Zod Validation Error
  if (error instanceof ZodError) {
    const errorDetails = error.issues.map((issue) => ({
      field: issue.path.join("."),
      message: issue.message,
    }));

    return NextResponse.json(
      {
        status: false,
        message: "Validasi input gagal",
        data: errorDetails,
      },
      { status: 400 }
    );
  }

  // 2. Custom App/Api Error
  if (error instanceof AppError) {
    return NextResponse.json(
      {
        status: false,
        message: error.message,
        data: error.data,
      },
      { status: error.statusCode }
    );
  }

  // 3. Unhandled Server Error (500)
  console.error("Unhandled Backend Error:", error);
  return NextResponse.json(
    {
      status: false,
      message: "Terjadi kesalahan internal pada server",
      data: [],
    },
    { status: 500 }
  );
}
```

---

## 🛡️ 3. Keamanan & Pencegahan Kebocoran Data (Data Leakage)

- **Sanitasi Error di Production**: Jangan pernah menampilkan pesan raw SQL, stack trace, error ORM prisma/drizzle ke klien pada respon 500.
- **Server Logging**: Catat full stack trace ke log server/monitoring tool (seperti Sentry, CloudWatch, Datadog) untuk keperluan debugging tim internal.
