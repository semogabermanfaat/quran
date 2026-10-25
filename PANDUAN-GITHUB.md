# Cara Menerapkan Perubahan ke GitHub

Semua perubahan sesi ini ada di **satu file saja**: `index.html`.
File di folder ini (`/workspace/index.html`) sudah versi final dan siap dipakai.

Repo tujuan: `https://github.com/semogabermanfaat/quran`
Branch: `main` (jangan ditimpa langsung tanpa konfirmasi — lihat Cara C).

---

## Cara A — Lewat GitHub Web (paling cepat, tanpa install apa pun)

Cocok kalau kamu cuma mau update 1 file dan tidak mau pakai terminal.

1. Buka `https://github.com/semogabermanfaat/quran`.
2. Klik file **`index.html`**.
3. Klik ikon **pensil (Edit this file)** di kanan atas.
4. **Select all** isi editor (`Ctrl+A` / `Cmd+A`), lalu **hapus**.
5. Buka file `index.html` dari folder project ini, **copy seluruh isinya**, lalu **paste** ke editor GitHub.
6. Scroll ke bawah → isi kolom commit:
   - **Commit message:** `feat: perbaiki latar animasi SVG, navigasi remote TV, dan wallpaper Pinterest`
   - Pilih **"Create a new branch for this commit and start a pull request"** (jangan commit langsung ke `main`).
   - Nama branch, misal: `perbaikan-latar-dan-navigasi`
7. Klik **Propose changes** → **Create pull request**.
8. Cek preview di branch itu, kalau sudah bagus baru klik **Merge pull request**.

> ⚠️ Jangan langsung commit ke `main` sebelum kamu lihat hasilnya di branch. Kalau ada yang salah, `main` tetap aman.

---

## Cara B — Lewat Git di komputer kamu (disarankan untuk perubahan rutin)

Butuh Git terpasang di komputer/HP kamu.

```bash
# 1. Clone repo (kalau belum punya)
git clone https://github.com/semogabermanfaat/quran.git
cd quran

# 2. Buat branch baru — JANGAN kerja di main
git checkout -b perbaikan-latar-dan-navigasi

# 3. Timpa index.html dengan file dari project ini
#    (copy manual file index.html yang baru ke folder ini)

# 4. Lihat apa yang berubah
git status
git diff --stat

# 5. Commit
git add index.html
git commit -m "feat: perbaiki latar animasi SVG, navigasi remote TV, dan wallpaper Pinterest"

# 6. Push branch ke GitHub
git push -u origin perbaikan-latar-dan-navigasi
```

Setelah push, GitHub akan menampilkan tombol **Compare & pull request**.
Buat PR → cek preview → merge kalau sudah oke.

---

## Cara C — Kalau tetap mau langsung ke `main`

Hanya kalau kamu sudah yakin dan sudah punya backup.

```bash
git checkout main
git pull origin main          # pastikan main terbaru
# copy index.html yang baru
git add index.html
git commit -m "feat: perbaiki latar animasi SVG, navigasi remote TV, dan wallpaper Pinterest"
git push origin main
```

**Backup dulu sebelum ini:**
```bash
git tag backup-sebelum-perbaikan
git push origin backup-sebelum-perbaikan
```
Kalau ada masalah, kamu bisa balik: `git checkout backup-sebelum-perbaikan -- index.html`

---

## Cara D — Upload lewat HP (Android, tanpa terminal)

1. Install **Termux** dari F-Droid.
2. Di Termux:
   ```bash
   pkg install git -y
   git clone https://github.com/semogabermanfaat/quran.git
   cd quran
   git checkout -b perbaikan-latar-dan-navigasi
   ```
3. Copy file `index.html` baru ke folder `~/quran/` (pakai aplikasi File Manager).
4. Lanjutkan seperti Cara B langkah 4–6.

---

## Setelah Merge — Cek Hasilnya

Repo ini pakai **GitHub Pages** (live demo: `https://semogabermanfaat.github.io/quran`).

1. Buka **Settings → Pages** di repo.
2. Pastikan **Source** = `Deploy from a branch`, **Branch** = `main`, folder = `/ (root)`.
3. Setelah merge ke `main`, tunggu 1–3 menit.
4. Buka `https://semogabermanfaat.github.io/quran` → tekan **Ctrl+Shift+R** (hard refresh) supaya tidak kena cache.
5. Kalau masih versi lama, tunggu lagi atau cek tab **Actions** untuk status deploy.

---

## Ringkasan Perubahan Sesi Ini

| Bagian | Yang diperbaiki |
|---|---|
| Latar **Masjid** | Kubah bawang + drum, 2 menara berundak, kubah pendamping, mihrab, jendela kisi keemasan, bukit berlapis |
| Latar **Hutan** | Pohon pinus berundak, 3 lapisan kedalaman, kabut, bulan purnama berkawah, kunang-kunang |
| Latar **Pantai** | Matahari senja + pantulan air, 3 lapis ombak berbusa, pasir, 2 pohon kelapa, camar |
| Latar **Fajar** | Matahari terbit di cakrawala, pita cahaya, awan senja, bukit, danau berpantulan |
| Animasi bintang | Diubah dari `scale` (terlihat berotasi) jadi murni `opacity` (berkedip) |
| Animasi komet | Arah diperbaiki — sekarang melesat turun ke kiri, bukan naik |
| Navigasi **Remote TV** | 6 bug diperbaiki: tombol Layar Penuh tak terjangkau, D-pad mentok di picker & panel, `lastTopBtn` invalid, input teks tak bisa diketik, Enter salah aksi |
| **Wallpaper Pinterest** | Multi-proxy + timeout, regex diperluas, tebak URL dari ID pin, validasi gambar, paksa resolusi `originals`, sanitasi URL CSS |

---

## Catatan Penting

- **Jangan ubah `main` tanpa konfirmasi** — selalu lewat branch + Pull Request dulu.
- Kalau semua proxy Pinterest mati, cara paling andal: buka pin di browser → klik kanan gambar → **Copy image address** → tempel URL `i.pinimg.com` langsung ke kolom input.
- File `index.html` di project ini sudah lolos pemeriksaan sintaks JavaScript dan preview tanpa error.
