<!-- BEGIN:nextjs-agent-rules -->

## This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` (resolved from this file's directory; in monorepos the `next` package may not be visible from the repo root) before writing any code. Heed deprecation notices.

This block is written and re-added by `next dev` — verify at `node_modules/next/dist/server/lib/generate-agent-files.js`. Removing it from a diff only re-creates the uncommitted change; committing it with your work keeps the tree clean.

<!-- END:nextjs-agent-rules -->

## Architecture & Layering (Clean Architecture)

Pisahkan tanggung jawab secara tegas agar logika bisnis tidak tercampur dengan UI atau detail framework:

- `app/` — Routing, layout, dan entry page Next.js (jaga tetap tipis; jangan taruh bisnis logic berat atau query langsung di sini).
- `components/` — Komponen UI presentational & reusable (tidak boleh langsung memanggil API/database).
- `features/<nama-fitur>/` (atau `services/` & `repositories/`) — Logika bisnis / use-case dan akses data yang terpisah dari UI.
- `lib/` — Utilitas murni (helper, formatter, validator) yang bebas side-effect dan reusable.
- `types/` — Definisi tipe/interface TypeScript bersama.
- **Arah ketergantungan**: UI (`app/`, `components/`) -> Use-case/Service -> Repository/Data -> Types/Domain. Jangan membalik arah ketergantungan.

## Clean Code Standards

- **Single Responsibility**: Satu fungsi atau komponen hanya mengerjakan satu tugas yang jelas. Pecah jika sudah terlalu panjang atau menangani banyak concern sekaligus.
- **Penamaan Jelas**: Gunakan nama variabel, fungsi, dan komponen yang deskriptif sesuai domain (hindari nama ambigu seperti `tmp`, `data2`, `handleStuff`).
- **Early Return**: Gunakan guard clause / early return untuk menghindari percabangan `if/else` bersarang terlalu dalam.
- **Type Safety**: Hindari `any`; selalu definisikan tipe input dan output secara eksplisit.
- **Tanpa Kode Mati**: Hapus import, variabel, fungsi, tipe, atau kode komentar yang tidak terpakai.
- **Server vs Client Component**: Utamakan React Server Components; gunakan `"use client"` hanya jika benar-benar membutuhkan state, event handler, atau browser API.

## Anti-Redundancy (DRY)

- **Cek dulu sebelum membuat baru**: Sebelum membuat helper, hook, tipe, atau komponen baru, telusuri codebase terlebih dahulu apakah sudah ada yang serupa dan bisa dipakai ulang atau digeneralisasi.
- **Single Source of Truth**: Jangan duplikasi konstanta, schema validasi, tipe data, atau logika yang sama di banyak tempat; ekstrak ke modul bersama (`lib/`, `types/`, atau `components/`).
- **Refaktor saat duplikasi muncul**: Jika pola atau logika yang sama muncul lebih dari 2 kali, ekstrak menjadi fungsi/komponen reusable.
- **Hindari over-abstraction**: Jangan membuat wrapper atau abstraksi tipis yang hanya membungkus satu fungsi sederhana tanpa nilai tambah.

## No AI Slop — Kode & Fungsi

- **Komentar seperlunya**: Dilarang menulis komentar narasi yang hanya mengulang isi kode (misalnya `// Set loading state`, `// Render button`). Komentar hanya untuk menjelaskan *alasan* (trade-off, batasan, atau edge case).
- **Tanpa kode spekulatif**: Jangan membuat helper, hook, opsi konfigurasi, atau abstraksi "jaga-jaga untuk masa depan" yang tidak dipakai saat ini.
- **Tanpa placeholder / fallback diam-diam**: Dilarang meninggalkan `TODO`, dummy handler, mock data diam-diam, atau `try/catch` kosong yang menelan error. Tangani error secara eksplisit.
- **Validasi di boundary**: Lakukan validasi ketat di pintu masuk (input user, response API, env), bukan defensive check berlebihan yang berulang di setiap fungsi internal.
- **Fokus pada scope**: Jangan melakukan refaktor kosmetik atau mengubah kode di luar ruang lingkup tugas yang diminta.

## No AI Slop — UI & Desain

- **Hindari estetika template generik**: Jangan default ke palet gradasi ungu/biru-neon klise, efek glow/glassmorphism berlebihan, grid 3-kartu identik penuh ikon dekoratif, atau memakai emoji sebagai pengganti ikon UI.
- **Hierarki & karakter visual**: Gunakan tipografi yang tegas dan terukur, spacing yang konsisten, kontras yang kuat, serta variasi layout yang jelas (hindari semua section serba center-aligned dan monoton).
- **Palet terarah**: Batasi warna ke sistem yang konsisten (warna dasar netral, 1 warna utama, 1 aksen) melalui CSS variables/theme Tailwind v4 daripada warna acak per komponen.
- **Copywriting natural**: Tulis teks UI yang ringkas, spesifik, dan langsung ke poin. Hindari buzzword klise (*"Revolutionize"*, *"Elevate"*, *"Seamless"*, *"Cutting-edge"*, *"Unlock the power"*).
- **Responsif & aksesibel**: Terapkan pendekatan mobile-first, semantic HTML, focus state yang jelas, dan kontras teks yang mudah dibaca.

## Verification

- Setelah selesai melakukan perubahan kode, jalankan `npm run lint` dan pastikan tidak ada error TypeScript maupun lint.
- Jangan menginstal package baru tanpa persetujuan terlebih dahulu.
