# Standarisasi API & Route Handlers

Pedoman pembuatan endpoint API menggunakan Next.js App Router Route Handlers (`app/api/.../route.ts`).

---

## 📌 1. Aturan Dasar Route Handler

- Gunakan Next.js Route Handlers standar (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`).
- Bungkus selalu eksekusi di dalam blok `try...catch`.
- Gunakan HTTP status code yang tepat:
  - `200 OK`: Permintaan berhasil dan mengembalikan data.
  - `201 Created`: Berhasil membuat data baru.
  - `400 Bad Request`: Payload/validasi gagal atau input tidak sesuai.
  - `401 Unauthorized`: Autentikasi gagal atau token tidak ditemukan/tidak valid.
  - `403 Forbidden`: Pengguna tidak memiliki hak akses/izin untuk sumber daya ini.
  - `404 Not Found`: Data atau endpoint yang dicari tidak ditemukan.
  - `409 Conflict`: Konflik data (misal: email sudah terdaftar).
  - `422 Unprocessable Entity`: Data valid secara tipe namun gagal validasi bisnis.
  - `500 Internal Server Error`: Kesalahan tak terduga pada server/database.

---

## 📦 2. Struktur Standar JSON Response

Semua endpoint API **WAJIB** mengembalikan format response berikut:

```typescript
export interface ApiResponse<T = any> {
  status: boolean;      // true jika sukses, false jika gagal
  message: string;      // Pesan deskriptif yang mudah dibaca pengguna/klien
  data: T[] | T;        // Data hasil query atau array kosong [] / detail error validasi
  meta?: {              // Opsional: Untuk metadata paginasi atau info tambahan
    page?: number;
    limit?: number;
    total?: number;
    total_pages?: number;
    [key: string]: any;
  };
}
```

---

## 💡 3. Contoh Format Response

### 1. Sukses Single / List Data (200 OK / 201 Created)
```json
{
  "status": true,
  "message": "Data berhasil diambil",
  "data": [
    {
      "id": "usr_123",
      "name": "Budi Pratama",
      "email": "budi@example.com"
    }
  ]
}
```

### 2. Sukses dengan Paginasi (200 OK)
```json
{
  "status": true,
  "message": "Daftar pengguna berhasil diambil",
  "data": [
    { "id": "usr_1", "name": "Budi" },
    { "id": "usr_2", "name": "Siti" }
  ],
  "meta": {
    "page": 1,
    "limit": 10,
    "total": 50,
    "total_pages": 5
  }
}
```

### 3. Error Validasi Input (400 Bad Request / 422 Unprocessable Entity)
```json
{
  "status": false,
  "message": "Validasi gagal. Silakan periksa kembali data yang dimasukkan.",
  "data": [
    {
      "field": "email",
      "message": "Format email tidak valid"
    },
    {
      "field": "password",
      "message": "Password minimal harus 8 karakter"
    }
  ]
}
```

### 4. Client Error (401 / 403 / 404)
```json
{
  "status": false,
  "message": "Sesi tidak valid atau telah berakhir. Silakan login kembali.",
  "data": []
}
```

### 5. Server Error (500 Internal Server Error)
```json
{
  "status": false,
  "message": "Terjadi kesalahan internal pada server",
  "data": []
}
```
