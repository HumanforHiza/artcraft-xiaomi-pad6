# ArtCraft untuk Xiaomi Pad 6

Fork pribadi dari [MostafaXS1/artcraft-android](https://github.com/MostafaXS1/artcraft-android) yang
difokuskan untuk satu perangkat, **Xiaomi Pad 6**, dan satu kebutuhan: mengolah dokumen,
spreadsheet, dan presentasi di tablet.

Fork ini tidak berafiliasi dengan, tidak didukung oleh, dan tidak mewakili tim ArtCraft, pembuat
port aslinya, maupun Xiaomi. Nama produk dan merek yang disebut di sini adalah milik pemiliknya
masing-masing dan dipakai hanya untuk menjelaskan perangkat dan format file yang didukung.

## Apa yang dibangun di sini

Hanya tiga aplikasi, bukan seluruh suite:

| Aplikasi | Jenis | Format utama |
| --- | --- | --- |
| WordCraft | Pengolah kata | `.docx`, `.odt`, `.rtf`, `.md`; membuka `.doc` lama |
| GridCraft | Spreadsheet | `.xlsx`, `.csv`, `.tsv`; `.xls`, `.xlsb`, `.ods` hanya sebagian |
| DeckCraft | Presentasi | `.pptx`, `.ppsx`, `.potx` |

Dukungan format di atas mengikuti klaim proyek upstream. Aplikasinya masih sangat muda, jadi
selalu kerjakan salinan file dan periksa hasil simpanannya sebelum mengandalkannya.

## Bedanya dengan repo asli

- **Hanya tiga aplikasi.** Matriks build dipangkas menjadi WordCraft, GridCraft, dan DeckCraft.
- **Kunci tanda tangan tetap.** Repo asli membuat kunci baru di setiap build, sehingga versi baru
  tidak bisa menimpa versi lama. Di sini semua APK ditandatangani dengan satu kunci yang disimpan
  sebagai repository secret, jadi pembaruan terpasang di atas versi lama tanpa uninstall.
- **Cek pembaruan mengarah ke fork ini.** Aplikasi tidak lagi menawarkan rilis repo asli, yang
  ditandatangani dengan kunci berbeda dan tidak bisa dipasang di atas build dari fork ini.

Belum ada perubahan kode yang khusus untuk Xiaomi Pad 6. "Difokuskan" di sini berarti build
dipilih, dipasang, dan diuji untuk perangkat itu; penyesuaian khusus akan dicatat di sini kalau
sudah ada.

## Kecocokan dengan Xiaomi Pad 6

- APK dibangun untuk **ARM64** dan butuh **Android 9 ke atas**; Xiaomi Pad 6 memenuhi keduanya.
- Antarmukanya adalah ribbon versi desktop, bukan antarmuka sentuh. Paling nyaman dipakai dengan
  keyboard dan mouse atau touchpad.
- Setiap kali dibuka, aplikasi meminta izin **akses semua file** sampai izin itu diberikan. Izin
  itu dipakai untuk membuka dan menyimpan file di luar pemilih dokumen sistem serta untuk membaca
  font tambahan di folder `Fonts` pada penyimpanan internal.

### Hasil uji 14 November 2026

Diuji di Xiaomi Pad 6 (HyperOS 2, Android 14, layar 2880×1800 mendatar) dengan build pertama
fork ini: WordCraft 0.18.1, GridCraft 0.19.1, DeckCraft 0.18.1. Bahan uji berupa file buatan
sendiri yang berisi teks berformat, daftar, tabel, rumus, dua lembar kerja, dan tiga slide.

| | WordCraft | GridCraft | DeckCraft |
|---|---|---|---|
| Terpasang dan terbuka | ya, tanpa crash | ya, tanpa crash | ya, tanpa crash |
| Membuka file | `.docx` utuh: format, daftar, tabel, karakter khusus | `.xlsx` utuh: rumus terhitung, rumus antarlembar, freeze pane | `.pptx` utuh: poin bertingkat, tabel, kotak teks |
| Menyimpan | menimpa file asli (dibuka lewat "Browse in WordCraft") | Ctrl+S membuka dialog simpan sebagai file baru | lihat catatan di bawah |
| Isi setelah disimpan | utuh | utuh, termasuk rumus baru | utuh, termasuk catatan slide |

Catatan:

- **DeckCraft: "Save" tidak menimpa file asli** kalau file dibuka lewat pemilih dokumen Android.
  Status bar menulis "Saved", tetapi yang tersimpan hanya salinan kerja di folder pribadi
  aplikasi (`Android/data/com.artcraft.deckcraft/files/media/`). Salinan itu ikut terhapus saat
  aplikasi di-uninstall. Untuk menyimpan ke lokasi yang dipilih, pakai **File → Save As**.
- Antarmuka pas untuk layar tablet tanpa perlu mengubah skala.
- Font Calibri dan Arial tidak ada di Android, jadi diganti font yang mirip. Aplikasi memberi
  tahu hal ini. Untuk tampilan yang sama persis, salin font ke folder `Fonts` di penyimpanan
  internal.
- File yang dikirim lewat `adb push` baru muncul di pemilih dokumen setelah dipindai media
  scanner Android.
- Uji di atas memakai input keyboard yang dikirim lewat ADB. Keyboard dan mouse fisik belum
  dicoba.

## Cara mengunduh dan memasang

1. Buka tab [Actions](../../actions) dan pilih run **Build APKs** terbaru yang berhasil.
2. Unduh artifact `artcraft-all-apks` di bagian bawah halaman run (perlu login GitHub), lalu
   ekstrak file zip-nya.
3. Salin APK ke tablet dan pasang. Android akan meminta izin memasang dari sumber tidak dikenal.

Kalau di tablet sudah terpasang versi dari repo asli, uninstall dulu. Tanda tangannya berbeda,
jadi Android menolak menimpanya. Setelah itu, build berikutnya dari fork ini bisa langsung
dipasang di atas yang lama.

## Kalau Anda mem-fork repo ini

Build akan gagal sampai dua repository secret diisi di
**Settings → Secrets and variables → Actions**:

| Secret | Isi |
| --- | --- |
| `ARTCRAFT_KEYSTORE_B64` | Keystore PKCS12 dengan alias `artcraftbuilder`, dalam base64 satu baris |
| `ARTCRAFT_KEYSTORE_PASSWORD` | Kata sandi keystore tersebut |

Buat keystore Anda sendiri dan simpan cadangannya. Kalau kuncinya hilang, build berikutnya tidak
bisa menimpa aplikasi yang sudah terpasang.

## Dokumentasi repo asli

Penjelasan lengkap tentang ArtCraft Android (fitur integrasi Android, daftar semua aplikasi, dan
cara build lokal dengan `build.sh`) ada di
[README repo asli](https://github.com/MostafaXS1/artcraft-android#readme). Rilis di sana
ditandatangani dengan kunci berbeda, jadi tidak bisa dipasang di atas build dari fork ini.

Dokumen teknis yang ikut di repo ini: [`architecture.md`](architecture.md),
[`agents.md`](agents.md), [`adapters/README.md`](adapters/README.md), dan
[`CHANGELOG.md`](CHANGELOG.md).
