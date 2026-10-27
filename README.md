# Al-Quran · Immersive

Aplikasi web Al-Quran interaktif untuk TV Android / browser, dengan latar animasi bergerak (SVG), suara alam ambien, dukungan remote TV (D-pad), dan mode layar penuh (immersive). Aplikasi ini adalah **satu file** `index.html` (tanpa framework, tanpa build).

## Struktur File

| File | Peran |
|------|-------|
| `index.html` | Seluruh aplikasi (HTML + CSS + JS dalam satu file) |
| `README.md` | Dokumen ini: peta lokasi kode |
| `tasks.md` | Pelacak tugas (wajib diisi agen) |
| `AGENTS.md` | Aturan kerja agen |

## Cara Menjalankan

1. Buka `index.html` langsung di browser, atau
2. Jalankan server statis (mis. `python3 -m http.server`) lalu buka `localhost:8000`.

Tidak ada langkah build/install — murni HTML/CSS/JS.

---

## Peta Lokasi Kode — `index.html`

> Semua nomor baris merujuk ke `index.html` **saat ini**. Gunakan peta ini untuk langsung menuju lokasi, JANGAN mencari-cari dengan grep/scroll seluruh file. Jika nomor baris berubah karena edit, perbarui peta ini (lihat `AGENTS.md`).

### 1. CSS — `<style>` (baris 10–356)

| Baris | Bagian | Isi |
|-------|--------|-----|
| 11–25 | `:root` | Variabel warna & font (`--ink`, `--gold`, `--font-app`, `--ar-scale`, dst.) |
| 27–33 | Reset dasar | `*`, `html, body`, font global |
| 35–45 | Navigasi Remote TV | Gaya `:focus` tombol (outline emas), `.play`, `.ctl`, `.pill` |
| 47–77 | Latar Animasi | Keyframes: `twinkle`, `firefly`, `drift`, `sway`, `sun-pulse`, `fly-across`, `wave-*`, `shooting-star` |
| 79–80 | `#app` | Layout utama flex kolom |
| 82–111 | Bar atas | `.top`, `.brand`, `.pill` (tombol surah/ayat/qori/setelan), `.fs-float` (fullscreen) |
| 113–147 | Area Ayat | `.stage`, `.verse`, `.surah-title`, `.bismillah`, `.ar-text`, `.id-text`, `.ayah-footer` |
| 149–188 | Dock Pemutar | `.dock`, `.row`, `.play`, `.ctl`, `.meta`, `.progress-bar`/`.progress-fill` |
| 190–215 | Modal Picker TV | `.modal-backdrop`, `.picker-box`, `.picker-grid` (`.ayah-mode`/`.qori-mode`), `.picker-item` |
| 217–321 | Panel Pengaturan | `.panel`, `.panel-grid` (2 kolom), `.settings-card`, `.chip`, `.stepper-row`, `.input-text`, `.tv-shortcut-hints` |
| 323–343 | Mode Immersive | `body.immersive` (sembunyikan elemen, ubah ukuran dock/play); **auto-hide** kontrol `body.immersive.ui-hidden` (opacity/pointer-events) + transisi |
| 344–356 | Responsif Mobile | `@media (max-width: 720px)` — sembunyikan `.brand`, atur ulang pill/dock |

### 2. HTML — `<body>` (baris 359–553)

| Baris | ID / Elemen | Isi |
|-------|-------------|-----|
| 361–365 | `<audio id="audio-rain/thunder/river/forest/fire">` | Sumber suara alam ambien (CDN) |
| 367–369 | `#scene`, `#scene-custom`, `.overlay` | Kontainer latar animasi & wallpaper custom |
| 371 | `#app` | Kontainer utama |
| 372–378 | `#topBar` | Tombol `#btnSurah` (374), `#btnAyah` (375), `#btnReciter` (376), `#btnPanel` (377) |
| 380–382 | `#btnFs` | Tombol fullscreen mengambang (`.fs-float`) |
| 384–392 | `#pickerModal` | Modal picker (`#pickerTitle`, `#btnClosePicker`, `#pickerGrid`) |
| 394–522 | `#settingsBackdrop` → `#panel` | Panel setelan 4 kartu |
| 405–426 | Kartu 1 | Tampilan & Wallpaper: `#sceneChips`, `#wallInput`, `#btnWallSearch` |
| 428–447 | Kartu 2 | Suara Alam Ambien: `#soundChips`, `#volDown/val/Up` |
| 449–497 | Kartu 3 | Tipografi: `#fontArChips`, `#fontIdChips`, `#arDown/val/Up`, `#idDown/val/Up`, `#infoDown/val/Up` |
| 499–519 | Kartu 4 | Database & Petunjuk TV: `#apiChips`, `.tv-shortcut-hints` |
| 524–526 | `#stage` → `#verse` | Area tampil ayat |
| 528–550 | `#playerDock` | `#prevAyah` (530), `#play` (532, +`#playIcon`), `#nextAyah` (536), `#meta` (538), `#btnRepeat` (540), `#btnFx` (541), `.progress-wrap` (543–549: `#seekBar`/`#seekFill`, `#curTime`, `#durTime`) |
| 552 | `<audio id="audio">` | Elemen audio utama untuk bacaan ayat |

### 3. JavaScript — `<script>` (baris 555–1521)

| Baris | Bagian | Fungsi / Isi |
|-------|--------|--------------|
| 556–586 | PWA Setup | `setupPWA()` — manifest inline + service worker + `beforeinstallprompt` |
| 588–632 | Variabel & Data | `KEY` (591), `$` (592), `esc` (593), `QORIS` (595–603), `ICONS` (605–613), `safeParse` (615), `surahsList` (617), `state` (618–624), `saveState`/`fmtTime`/`pad3`/`clamp` (629–632) |
| 634–826 | Generator Latar | `SVG_SCENES` (635–641), `renderScene(key)` (643–826) — membangun SVG latar (night/dawn/mosque/forest/sea) |
| 828–847 | Wallpaper | `applyWallpaper(mode, keyOrUrl)` (828–845), handler chip `#sceneChips` (847) |
| 849–982 | Pinterest / Wallpaper Custom | `pinterestIdFromUrl` (854), `normalizeImageUrl` (860), `extractImageFromHtml` (871), `CORS_PROXIES` (888), `fetchViaProxy` (894), `setWallBtn` (913), handler `#btnWallSearch` (926), `imageLoads` (970), `finishWallSearch` (980) |
| 984–1023 | Audio Alam | `SOUND_LABELS`/`activeAudios` (985–986), `toggleSound(key, on, savePref)` (988), handler `#btnFx` (1019) |
| 1025–1058 | Kontrol Pengaturan | `applyUI()` (1026), handler stepper `vol/ar/id/info` (1047–1054), chip font & API (1056–1058) |
| 1060–1134 | Modal Picker | `openPicker(type, triggerBtn)` (1061), `closeAllModals()` (1110), handler `#btnSurah/#btnAyah/#btnReciter/#btnClosePicker/#btnPanel/#btnClosePanel` (1119–1134) |
| 1136–1191 | Fetcher API | `fetchDaftarSurah()` (1137), `loadSurah(n, startIdx)` (1158) — 3 sumber: equran / quranjson / alqurancloud |
| 1193–1266 | Tampil & Putar Ayat | `showAyah(i)` (1193), `setPlaying(p)` (1222), handler `#play` (1228), `#audio.onended` (1234), `#audio.ontimeupdate` (1240), `#seekBar` (1249), `nextAyah`/`prevAyah` (1257–1258), `updateRepeatUI` (1262), `#btnRepeat` (1266) |
| 1268–1326 | Fullscreen / Immersive | `uiHideTimer`/`UI_HIDE_MS` (1271–1272), `showImmersiveUI()` (1275), `setImmersive(on)` (1285), handler `#btnFs` (1308), `fullscreenchange` (1318), wake via `mousemove`/`touchstart` (1324–1326) |
| 1328–1501 | Navigasi Remote TV | `lastTopBtn` (1330), `findClosestInList(currentEl, list, dir)` (1331–1369), listener `keydown` global (1371–1501) |
| 1503–1521 | Inisialisasi | `init()` (1504–1519) — terapkan wallpaper, applyUI, isi `#soundChips`, panggil `fetchDaftarSurah()`; panggilan `init()` (1521) |

---

## Fitur Mode Layar Penuh (Immersive)

- Masuk via `#btnFs` atau tombol remote → otomatis **Play**; keluar → otomatis **Pause**.
- **Auto-hide kontrol**: di layar penuh, kontrol player (play/prev/next) dan tombol perkecil (`#btnFs`) **sembunyi otomatis setelah 5 detik** tanpa aktivitas (kelas `ui-hidden` pada `<body>`).
- **Muncul kembali** saat ada input remote (tombol/D-pad apa pun) atau sentuhan/gerakan mouse (`showImmersiveUI()`).
- Logika ada di: CSS `body.immersive.ui-hidden` (baris 337–342), JS `showImmersiveUI()` (1275), `setImmersive()` (1285), wake di `keydown` (1373) & `mousemove`/`touchstart` (1324–1326).

---

## Konvensi Kode

- **Mobile-first / TV-first**: gunakan Flexbox/Grid, `viewport-fit=cover`, kontrol sentuh + D-pad.
- **Animasi**: hanya `transform`/`opacity` (GPU), jangan width/height/top/left.
- **State**: semua preferensi disimpan di `state` lalu `saveState()` → localStorage (kunci `KEY`).
- **Bahasa**: komentar & teks UI berbahasa Indonesia.
- **Satu file**: semua kode di `index.html`; jika menambah file baru, perbarui peta di README ini.
