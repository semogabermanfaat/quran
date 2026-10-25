# Catatan Project — Al-Quran Immersive

## Struktur
- Aplikasi ini **satu file saja**: `index.html` (HTML + CSS + JS inline). Tidak ada build step, tidak ada dependency npm.
- File pendamping: `README.md`, `PANDUAN-GITHUB.md`.

## Cara Menjalankan
- Cukup buka `index.html` di browser (file:// sudah jalan). Tidak perlu dev server.
- Deploy produksi: GitHub Pages dari branch `main`, folder root.

## Arsitektur Kode Penting
- `SVG_SCENES` (objek) — definisi tiap latar: `sky`, `body`, `kind`.
- `renderScene(key)` — menghasilkan string SVG per `kind`: `night`, `sun` (fajar), `mosque`, `forest`, `sea`.
- `applyWallpaper(mode, keyOrUrl)` — mode `svg` (scene bawaan) atau `custom` (URL gambar).
- `findClosestInList(currentEl, list, dir)` — navigasi D-pad remote TV berbasis posisi geometris.
- Handler keyboard global menangani `ArrowUp/Down/Left/Right`, `Enter`, `Escape/Backspace`.

## Pitfall yang Sudah Ditemukan
- **Animasi bintang**: jangan pakai `transform: scale()` untuk kedip — `transform-origin` default bikin bintang terlihat berotasi/bergeser. Pakai `opacity` saja.
- **Animasi komet**: `stroke-dashoffset` + `translate` harus searah dengan kemiringan garis, kalau tidak komet terlihat naik.
- **Navigasi TV**: setiap daftar tombol (`topButtons`, dock, panel) butuh fallback fokus untuk arah Up DAN Down, kalau tidak D-pad mentok.
- **`lastTopBtn`**: harus di-reset saat `setImmersive()` karena daftar `topButtons` berubah.
- **Input teks**: handler global wajib cek `isTextInput` sebelum menangani Arrow/Space/Enter.
- **URL ke CSS**: `backgroundImage = url('...')` harus disanitasi — kutip tunggal dan `)` di URL merusak string CSS.
- **Pinterest**: proxy publik (allorigins/corsproxy/codetabs) sering mati. Selalu pakai multi-proxy + timeout + fallback tebak URL dari ID pin.

## Preferensi User
- User minta perubahan diterapkan lewat **branch baru + Pull Request**, tidak langsung menimpa `main`.
- Bahasa komunikasi: Indonesia.
