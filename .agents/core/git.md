# Panduan Konvensi Git & Standar Commit (Git Guidelines)

Pedoman pengelolaan versi kode, konvensi pesan commit (*Conventional Commits*), strategi branching, dan alur Pull Request pada repositori ini.

---

## 📝 1. Standar Pesan Commit (Conventional Commits)

Format pesan commit **WAJIB** mengikuti format standar:

```text
<type>(<scope>): <deskripsi singkat imperative>

[opsional body: penjelasan detail jika ada breaking changes atau konteks tambahan]

[opsional footer: referensi issue/ticket, misal: Closes #123, Refs #456]
```

### A. Tipe Commit (`type`):
| Type | Keterangan | Contoh |
| :--- | :--- | :--- |
| **`feat`** | Penambahan fitur baru untuk user | `feat(auth): implement login with google oauth` |
| **`fix`** | Perbaikan bug atau error sistem | `fix(users): resolve n+1 query issue on user list api` |
| **`refactor`** | Perubahan struktur kode tanpa mengubah fungsi | `refactor(api-client): standardize error response parsing` |
| **`perf`** | Perubahan kode yang meningkatkan performa | `perf(images): enable avif format and priority loading` |
| **`style`** | Perubahan format, spasi, CSS tanpa efek logika | `style(button): adjust padding and border radius` |
| **`docs`** | Pembaruan atau penambahan dokumentasi | `docs(agents): add git commit convention guideline` |
| **`test`** | Penambahan atau perbaikan unit/integration tests | `test(auth): add unit test for jwt verification` |
| **`chore`** | Pembaruan tooling, konfigurasi, dependensi | `chore(deps): update nextjs to latest version` |
| **`ci`** | Perubahan konfigurasi CI/CD workflow | `ci(github): add automated type-check action` |

### B. Lingkup (`scope`) (Opsional tapi Direkomendasikan):
Gunakan nama modul fitur atau layer:
- `(auth)`, `(users)`, `(products)`, `(ui)`, `(middleware)`, `(db)`, `(api-client)`, `(deps)`

### C. Aturan Penulisan Deskripsi:
1. Gunakan kalimat imperatif (*present tense*), contoh: `add feature`, `fix bug`, `update schema` (bukan `added`, `fixes`, `fixing`).
2. Huruf kecil di awal deskripsi dan **tanpa tanda titik** di akhir kalimat.
3. Maksimal panjang baris pertama adalah 72 karakter.

---

## 🌿 2. Strategi Penamaan Branch (Branching Strategy)

Gunakan pola penamaan branch yang konsisten:

```text
<type>/<keterangan-singkat-fitur>
```

Contoh:
- `feat/user-profile-management`
- `fix/login-redirect-loop`
- `refactor/api-service-layer`
- `chore/update-tailwind-config`

---

## 🚀 3. Best Practices Sebelum Commit & Push

1. **Atomic Commits**: Satu commit sebaiknya hanya mencakup satu perubahan logika terkait. Jangan mencampur `fix bug` dan `fitur baru yang tidak terkait` dalam satu commit besar.
2. **Cek Perubahan Terlebih Dahulu**:
   ```bash
   git status
   git diff
   ```
3. **Verifikasi Kesiapan Kode**:
   - Pastikan tidak ada error kompilasi TypeScript (`npm run type-check`).
   - Pastikan kode lolos linting (`npm run lint`).
   - Hapus semua `console.log` debug sementara dan file `.env` rahasia.
4. **Hindari Force Push ke Branch Utama**: Dilarang menggunakan `git push --force` pada branch `main` / `master` / `staging`.
