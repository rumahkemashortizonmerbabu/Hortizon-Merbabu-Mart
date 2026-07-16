# Panduan Edit Produk HortizonMart (Tanpa Coding)

Mulai sekarang, data produk (nama, harga, stok, deskripsi) diambil
otomatis dari Google Sheets. Untuk mengubah katalog, cukup edit sel
di Google Sheets — **tidak perlu buka kode atau GitHub sama sekali**,
kecuali kalau mau menambah foto produk baru.

---

## 🧩 SETUP AWAL (dilakukan sekali oleh kamu)

### 1. Buat Google Sheet baru
1. Buka [sheets.google.com](https://sheets.google.com) → **Blank spreadsheet**.
2. Beri nama, misal "Data Produk HortizonMart".

### 2. Import data 25 produk yang sudah ada
1. Di Google Sheets: **File → Import**.
2. Tab **Upload** → pilih file `data-produk-hortizonmart.csv` (sudah saya siapkan).
3. Pilih **"Replace current sheet"** → klik **Import data**.
4. Sekarang sheet-mu sudah berisi 25 produk dengan 6 kolom:
   `id | name | price | stock | image | description`

### 3. Publish sheet supaya bisa diakses website
1. Klik **File → Share → Publish to web**.
2. Di dropdown pertama pilih **sheet yang aktif** (bukan "Entire Document").
3. Di dropdown kedua pilih format **Comma-separated values (.csv)**.
4. Klik **Publish** → konfirmasi.
5. Copy link yang muncul (bentuknya seperti ini):
   ```
   https://docs.google.com/spreadsheets/d/e/2PACX-xxxxxxx/pub?output=csv
   ```

   ⚠️ **Penting:** setelah ini, sheet-nya bisa dibaca siapa saja yang
   punya link tersebut (read-only, orang lain tidak bisa mengedit).
   Karena isinya cuma data katalog (bukan data pribadi/rahasia),
   ini aman.

### 4. Tempel link ke kode
1. Buka file `js/script.js`.
2. Cari baris ini (dekat bagian atas):
   ```js
   const SHEET_CSV_URL = "TEMPEL_URL_CSV_GOOGLE_SHEETS_DI_SINI";
   ```
3. Ganti jadi link yang kamu copy tadi, contoh:
   ```js
   const SHEET_CSV_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-xxxxxxx/pub?output=csv";
   ```
4. Simpan file, upload ulang ke GitHub seperti biasa.

Selesai! Sekarang website ambil data langsung dari Google Sheets.

---

## ✏️ CARA ORANG TUA EDIT PRODUK SEHARI-HARI

Cukup buka Google Sheets yang sudah dibuat tadi (bisa di-bookmark /
pin di HP juga, ada aplikasi Google Sheets di Play Store/App Store).

| Ingin apa? | Caranya |
|---|---|
| **Ubah harga/stok** | Klik sel harga atau stok di baris produk itu, ketik angka baru, Enter |
| **Ubah deskripsi** | Klik sel `description`, edit teksnya, Enter |
| **Ubah nama produk** | Klik sel `name`, edit, Enter |
| **Hapus produk** | Klik kanan nomor baris di kiri → **Delete row** |
| **Tambah produk baru** | Isi baris kosong di bawah: `id` (angka unik, urut dari terakhir), `name`, `price`, `stock`, `image` (link ImgBB atau `images/nama-file.jpg`, lihat bagian foto di bawah), `description` |

**Perubahan otomatis muncul di website** dalam waktu sekitar 1-5
menit (Google butuh sedikit waktu untuk update cache publish).
Tinggal refresh browser.

---

## 🖼️ CARA TAMBAH / GANTI FOTO PRODUK (Tanpa GitHub, Tanpa Akun)

Orang tua bisa lakukan ini sendiri sepenuhnya, tidak perlu bantuan
teknis kamu lagi.

1. Buka **[imgbb.com](https://imgbb.com)** di browser HP atau komputer
   (tidak perlu daftar/login).
2. Klik **"Start uploading"** → pilih foto sayuran dari galeri/file.
3. Setelah ke-upload, cari kolom **"Direct link"** → klik untuk copy
   link-nya. Bentuknya seperti:
   ```
   https://i.ibb.co/xxxxxxx/foto-wortel.jpg
   ```
4. Buka Google Sheets → tempel link itu ke kolom `image` pada baris
   produk yang sesuai.
5. Simpan (otomatis tersimpan di Google Sheets). Tunggu 1-5 menit,
   refresh website → foto baru sudah tampil.

**Kelebihan cara ini:** orang tua 100% bisa mandiri, tidak perlu buka
kode atau GitHub untuk urusan foto maupun data produk sama sekali.

---

### Alternatif (opsional, buat kamu si admin teknis)

Kalau suatu saat kamu (bukan orang tua) mau tetap simpan foto di
dalam folder project sendiri (misal supaya loading sedikit lebih
cepat karena satu domain), caranya:

1. Upload foto ke folder `images/` di GitHub lewat **Add file →
   Upload files** di website GitHub.
2. Isi kolom `image` di Sheets dengan `images/nama-file.jpg`
   (path relatif, bukan link penuh).

Dua cara ini bisa dicampur bebas — sebagian produk pakai link ImgBB,
sebagian pakai file lokal di `images/`, keduanya akan tetap tampil
normal karena kode website menerima kedua jenis path tersebut.

---

## 🛟 TROUBLESHOOTING

**Produk tidak muncul / masih data lama?**
- Cek lagi apakah sheet sudah di-**Publish to web** (bukan cuma di-share biasa)
- Tunggu 5 menit, lalu hard refresh browser (`Ctrl+Shift+R`)
- Cek `SHEET_CSV_URL` di `script.js` sudah benar link-nya (buka Console
  browser, cek ada warning "SHEET_CSV_URL belum di-setup" atau tidak)

**Gambar tidak muncul?**
- Pastikan nama file di kolom `image` di Sheets **persis sama** dengan
  nama file di folder `images/` (huruf besar/kecil berpengaruh)

**Situs offline / Sheets tidak bisa diakses?**
- Website otomatis pakai data cadangan dari `js/products.js` (fallback),
  jadi katalog tetap tampil walau tidak yang paling baru
