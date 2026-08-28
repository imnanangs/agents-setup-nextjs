# AI Communication Guidelines

Pedoman format output, gaya komunikasi, dan interaksi agent dengan developer.

---

## 💬 1. Gaya Komunikasi

- **Ringkas, Tepat, dan Berbobot**: Langsung ke inti solusi tanpa basa-basi (*no conversational fluff*), hindari pengantar berulang atau penjelasan konsep pemrograman dasar kecuali diminta.
- **Non-Apologetic**: Hindari kalimat meminta maaf seperti *"Maaf, Anda benar..."* atau *"Mohon maaf atas kesalahan..."*. Cukup sampaikan koreksi, akar masalah secara objektif, dan solusi/kode yang diperbaiki.
- **Transparan & Akuntabel**: Jika terjadi kendala atau trade-off desain, jelaskan opsi yang tersedia beserta konsekuensinya.

---

## 💻 2. Format Kode & Output

1. **Syntax Highlighting & File Path**
   - Selalu sertakan bahasa pada fenced code block (contoh: ```typescript, ```tsx, ```bash, ```json).
   - Berikan konteks file berupa path lengkap atau relatif yang jelas (misal: `src/components/ui/button.tsx`).

2. **Diff & Partial Changes**
   - Saat memodifikasi file besar yang sudah ada, prioritaskan diff terfokus atau snippet bagian yang berubah alih-alih mencetak ulang ratusan baris file yang tidak terkait.
   - Cantumkan komentar penjelas lokasi edit bila diperlukan (misal: `// ... baris sebelumnya`).

3. **Clickable File Links**
   - Gunakan format link markdown github-style dengan skema `file://` saat mereferensikan file lokal dan baris kode agar mudah ditinjau oleh developer.
