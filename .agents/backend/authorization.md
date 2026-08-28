# Otorisasi (Authorization)

Otorisasi (*AuthZ*) mengatur hak akses sumber daya bagi pengguna yang telah terautentikasi melalui **Role-Based Access Control (RBAC)** dan **Resource Ownership (Row-Level Security)**.

---

## 👥 1. Role-Based Access Control (RBAC)

1. **Payload Role**:
   - Sertakan identitas role pengguna (contoh: `admin`, `merchant`, `customer`) di dalam payload JWT saat proses login.

2. **Pengecekan Tingkat Route**:
   - Middleware mengekstrak role dari token dan menyuntikkannya ke header request (`x-user-role`).
   - Pada Route Handler (`route.ts`) atau middleware khusus, periksa kecocokan role sebelum memproses logika:
   ```typescript
   const userRole = req.headers.get("x-user-role");
   if (userRole !== "admin") {
     return NextResponse.json(
       {
         status: false,
         message: "Akses ditolak. Tindakan ini memerlukan hak akses Administrator.",
         data: []
       },
       { status: 403 }
     );
   }
   ```

---

## 🔒 2. Kepemilikan Sumber Daya (Resource Ownership)

1. **Prinsip Dasar**:
   - Pengguna hanya boleh membaca, mengubah, atau menghapus data miliknya sendiri, kecuali jika memiliki hak override (seperti role `admin`).

2. **Validasi di Service Layer**:
   - Validasi kepemilikan data **WAJIB** dilakukan di dalam **Service Layer** (`[feature].service.ts`), bukan hanya di Route Handler.
   - Jangan pernah mempercayai `userId` yang dikirim dari body client. Selalu gunakan `userId` yang diekstrak secara aman dari token JWT/header terverifikasi.

3. **Prinsip Fail-Closed**:
   - Jika kondisi otorisasi atau kepemilikan tidak terpenuhi secara eksplisit, tolak akses secara *default*.

---

## 🚫 3. Standar Format Error Otorisasi (403 Forbidden)

Jika otorisasi gagal, kembalikan HTTP status `403 Forbidden`:

```json
{
  "status": false,
  "message": "Akses ditolak. Anda tidak memiliki izin untuk mengakses atau memodifikasi data ini.",
  "data": []
}
```
