# Keuangan Pribadi

Aplikasi pencatatan keuangan pribadi berbasis **satu file HTML mandiri** (single-file web app). Tidak butuh instalasi, server, atau koneksi internet — semua logika (JavaScript) dan tampilan (CSS) sudah menyatu dalam satu file `.html`, dan seluruh data tersimpan di **localStorage browser** milik perangkat kamu sendiri.

File terbaru: `keuangan_pribadi-v1_1_006.html` · versi di footer app: **v1.1.006**

Riwayat perubahan ada di [CHANGELOG.md](CHANGELOG.md).

---

## Daftar Isi

- [Setup / Cara Menjalankan](#setup--cara-menjalankan)
- [Penyimpanan Data](#penyimpanan-data)
- [Tab & Fitur](#tab--fitur)
- [Tipe Akun](#tipe-akun)
- [Pinjaman Online (pinjol)](#pinjaman-online-pinjol)
- [Akun Aset — termasuk Forex & Crypto Futures](#akun-aset--termasuk-forex--crypto-futures)
- [Titipan / Piutang](#titipan--piutang)
- [Mencatat Bulan yang Sudah Lewat (Backdate)](#mencatat-bulan-yang-sudah-lewat-backdate)
- [Tab Laporan: Utang, Proyeksi & Simulasi](#tab-laporan-utang-proyeksi--simulasi)
- [Export, Import & Backup](#export-import--backup)
- [Keterbatasan yang Perlu Diketahui](#keterbatasan-yang-perlu-diketahui)
- [Ide yang Belum Dibuat](#ide-yang-belum-dibuat)
- [Struktur Kode (untuk yang mau modifikasi)](#struktur-kode-untuk-yang-mau-modifikasi)
- [Penomoran Versi](#penomoran-versi)

---

## Setup / Cara Menjalankan

Tidak ada proses instalasi. Tiga cara pakai:

1. **Buka langsung di browser** — klik dua kali file `keuangan_pribadi-v1_1_006.html`, atau seret ke jendela browser (Chrome/Edge/Firefox/Safari versi modern). Selesai, app langsung jalan.
2. **Simpan sebagai app di HP** — buka file di browser HP, lalu pakai menu "Tambahkan ke Layar Utama" (Android) / "Add to Home Screen" (iOS) supaya bisa dibuka seperti aplikasi biasa.
3. **Host sendiri (opsional)** — kalau mau diakses dari beberapa perangkat lewat jaringan lokal, taruh file ini di static file server apa pun (Nginx, `python3 -m http.server`, GitHub Pages, dll). Karena semua data ada di localStorage **per-browser**, tiap perangkat/browser akan punya datanya sendiri-sendiri (tidak otomatis sinkron).

**Syarat:** browser modern dengan JavaScript aktif dan localStorage tidak diblokir (jangan mode Incognito/Private kalau mau data bertahan lama — mode privat biasanya menghapus localStorage begitu jendela ditutup).

**Pindah ke versi baru:** data ada di browser, bukan di file, dan sebagian browser memperlakukan tiap file HTML lokal sebagai penyimpanan terpisah. Cara paling aman: **Export JSON** dari versi lama, buka file versi baru, lalu **Import JSON** (atau "Reset lalu isi dari file").

---

## Penyimpanan Data

- Semua akun & transaksi disimpan di `localStorage` dengan key `keuangan-app-data-v2` (dalam format JSON).
- **Tidak ada server/cloud** — data 100% lokal di browser tempat kamu membuka file ini. Kalau clear cache/data browser, uninstall browser, atau ganti perangkat, data akan hilang kecuali sudah di-**export** lebih dulu.
- Preferensi tampilan (tema terang/gelap, kartu ringkasan yang disembunyikan, status grup akun yang di-collapse) disimpan terpisah di localStorage dan **tidak ikut** ter-export ke JSON.
- **Wajib backup rutin** lewat menu Export JSON (lihat bagian [Export, Import & Backup](#export-import--backup)) — app akan mengingatkan otomatis kalau sudah lama tidak export.

---

## Tab & Fitur

Header menampilkan sapaan sesuai jam + nama pemilik, tanggal singkat, dan ikon pengaturan (Akun, Data & Export, tema tampilan). Footer di bawah semua tab menampilkan nama app, nomor versi, dan tanggal build.

| Tab | Isi |
|---|---|
| **Ringkasan** | Kartu total kekayaan bersih, saldo per kategori, pengingat jatuh tempo (7/30/90 hari/1 tahun), transaksi terbaru, insight bulanan, grafik tren, kurva kekayaan bersih, cashflow, dan breakdown kategori pemasukan/pengeluaran (pie chart). Kartu yang ditampilkan bisa diatur lewat tab Data. |
| **Akun** | Daftar semua akun dikelompokkan per tipe, dengan progress bar untuk akun berlimit (kartu kredit/pinjaman). Tambah/edit/hapus akun, buka detail akun (saldo, riwayat, jadwal cicilan, dll). |
| **Transaksi** | Catat pemasukan/pengeluaran/transfer. Filter per akun, tipe, bulan, dan pencarian teks. Edit & hapus transaksi individual. |
| **Titipan** | Catat uang yang dititipkan/dibelanjakan untuk orang lain, atau uang yang diterima dari orang lain — tanggalnya bisa diubah untuk catat titipan bulan yang sudah lewat. Ringkasan siapa berutang & siapa dititipi lebih. Tombol **Lunasi sekarang** untuk melunasi penuh dalam satu klik. |
| **Laporan** | Ringkasan per periode (harian s.d. alltime/custom): total masuk-keluar, breakdown kategori, tren, perbandingan periode lalu, plus kartu utang: **Rincian utang, Proyeksi kas, Total biaya utang, Simulasi pelunasan, dan Saran**. Export ke PDF (print). Lihat [Tab Laporan](#tab-laporan-utang-proyeksi--simulasi). |
| **Data** | Muat data contoh (dummy), export JSON/CSV, import JSON, reset semua data, reset & ganti dengan file lain — semuanya dengan backup otomatis sebelum aksi yang merusak data. |

---

## Tipe Akun

| Tipe | Perilaku |
|---|---|
| **Kas / Dompet, Bank, E-wallet** | Akun biasa: saldo naik-turun lewat transaksi masuk/keluar/transfer. |
| **Aset** *(emas, kendaraan, properti, investasi, forex, kripto, dll.)* | **Tidak** dicatat lewat masuk/keluar harian — nilainya berdasarkan **valuasi periodik** yang kamu update manual. Beli/jual tambahan dicatat lewat Transfer. Lihat detail di bagian [Akun Aset](#akun-aset--termasuk-forex--crypto-futures). |
| **Kartu Kredit** | Pengeluaran otomatis nambah tagihan; ada limit, tanggal cetak tagihan, bunga, dan skema pembayaran minimum. |
| **PayLater** | Mirip kartu kredit, mendukung cicilan flat dengan tenor & bunga per rencana. |
| **Pinjaman Bank** | Melacak sisa pokok, bunga (flat/menurun), jadwal jatuh tempo, biaya admin & materai. Panel "Catat pembayaran" di detail akun punya field tanggal sendiri — bisa dipakai untuk mencatat angsuran bulan-bulan yang sudah lewat, bukan cuma hari ini. |
| **Pinjaman Online** | Bunga selalu flat, angsuran dihitung otomatis dari pokok, tenor, dan bunga. Mendukung biaya admin (dipotong saat cair atau dicicil) dan asuransi/proteksi opsional. Lihat [Pinjaman Online](#pinjaman-online-pinjol). |
| **Titipan / Piutang** | Satu akun per orang. Saldo positif = orang itu berutang ke kamu; saldo negatif = kamu yang "berutang" (memegang titipan lebih dari mereka). Lihat [Titipan / Piutang](#titipan--piutang). |

Total **kekayaan bersih** otomatis menjumlahkan semua tipe di atas dengan tanda yang benar (piutang dihitung sebagai aset, sisa hutang & titipan lebih dihitung sebagai kewajiban).

---

## Pinjaman Online (pinjol)

Form akun tipe **Pinjaman Online** punya field berikut:

| Field | Keterangan |
|---|---|
| **Bunga flat** | Persen per bulan (atau per tahun) dari **pokok awal**, sama tiap bulan sepanjang tenor |
| **Biaya admin (% dari pokok)** | Dihitung ke Rupiah otomatis |
| **Cara bayar biaya admin** | *Dipotong dari pencairan* (default) atau *Dicicil bersama angsuran* (pencairan Rp0 biaya, gaya aplikasi pinjol seperti ShopeePay) |
| **Asuransi / proteksi (% per bulan dari pokok)** | **Opsional, boleh kosong** (kosong = tanpa asuransi) |
| **Biaya materai (sekali)** | Nominal Rupiah, opsional, dipotong saat cair |
| **Tenor, tanggal pencairan, tgl jatuh tempo** | Dasar jadwal angsuran & pengingat |

**Rumus angsuran (otomatis, bisa diubah manual):**

```
angsuran = pokok / tenor
         + pokok x bunga per bulan
         + pokok x asuransi per bulan
         + (pokok x admin% / tenor)    <- hanya kalau admin dicicil
```

Contoh: pokok Rp4.200.000, tenor 9 bulan, bunga 2,95%/bln, asuransi 0,15%/bln, admin 5% dicicil → **Rp620.200/bulan** (aplikasi aslinya Rp620.036; selisih kecil karena bunga aslinya sekitar 2,946%).

Bunga, asuransi, dan admin yang dicicil diperlakukan sebagai satu komponen "**bunga & biaya**" di tiap angsuran (kategori transaksi `Bunga & biaya bank`). Artinya sisa hutang, jadwal angsuran, dan rincian "Catat pembayaran" otomatis ikut menghitungnya.

**Pinjol yang sudah berjalan:** isi *Sisa pokok sekarang* dengan **sisa pokok yang sebenarnya**, bukan total angsuran terakhir. Kalau angka ini salah, jumlah angsuran yang dianggap lunas ikut salah (jadwal dihitung dari selisih pokok awal dan sisa pokok). Sisa hutang untuk pinjaman yang sudah berjalan memakai sisa pokok saja, karena bunga yang sudah terbayar sebelum app dipakai tidak diketahui.

---

## Akun Aset — termasuk Forex & Crypto Futures

Field pada akun tipe **Aset**:

- **Jenis aset**: Emas, Kendaraan, Properti, Investasi, **Forex / trading (akun cent)**, **Kripto / crypto futures**, atau Lainnya
- **Jumlah** & **Satuan** *(opsional)* — alat bantu hitung, bukan penentu nilai utama
- **Valuasi**: riwayat `{tanggal, nilai (Rp), harga per satuan (opsional)}` — inilah sumber kebenaran nilai akun

> ⚠️ App ini **satu mata uang (Rupiah)** — tidak ada konversi kurs otomatis (tidak ada `GOOGLEFINANCE` atau API kurs live). Semua nilai akhirnya harus dalam Rupiah, dikonversi manual oleh kamu.

**Cara pakai untuk akun forex cent:**
1. Jenis aset → "Forex / trading (akun cent)"
2. Jumlah = **equity dalam USD** (bagi saldo sen ÷ 100 dulu sebelum diinput), Satuan = `USD`
3. Setiap kali review: edit akun → update Jumlah ke equity terbaru → buka detail akun → tambah valuasi baru → isi "Harga per satuan" dengan **kurs USD/IDR hari itu** → nilai total Rp terhitung otomatis

**Cara pakai untuk akun crypto futures (margin USDT):**
1. Jenis aset → "Kripto / crypto futures"
2. Jumlah = equity dalam **USDT**, Satuan = `USDT`
3. Update berkala sama seperti di atas, tapi harga per satuan = **kurs USDT/IDR** dari exchange yang kamu pakai (bisa beda dari kurs USD resmi karena premium lokal)

Satuan `USD`/`USDT` (dan alias umum lain: IDR, EUR, GBP, SGD, JPY, BTC, ETH) otomatis dinormalisasi ke huruf besar baku, baik saat diketik manual maupun saat import dari file JSON — jadi `"usd"`, `"Usd"`, `"USD"` semuanya jadi `"USD"`.

Kalau tidak mau ribet dengan Jumlah/Satuan/Harga, kamu bisa langsung isi **nilai total dalam Rupiah** yang sudah kamu hitung sendiri di kolom valuasi, kosongkan Jumlah.

---

## Titipan / Piutang

- **"Belanjakan"** = kamu keluar uang untuk orang itu → piutang ke dia bertambah
- **"Terima bayar"** = orang itu bayar/kembalikan uang ke kamu → piutang berkurang (atau kalau tidak ada piutang sebelumnya, berarti dia menitipkan uang ke kamu)
- Tombol **"Lunasi sekarang"** di detail orang otomatis mengisi mode & nominal penuh sesuai sisa saldo — tinggal pilih akun dana dan simpan
- Hapus orang di tab ini otomatis menghapus seluruh riwayat transaksinya (beda dari akun biasa yang harus dikosongkan transaksinya dulu)

---

## Mencatat Bulan yang Sudah Lewat (Backdate)

Kalau kamu baru mulai memakai app ini padahal pinjaman/titipan sudah berjalan beberapa bulan, dua form berikut mendukung tanggal mundur (bukan cuma hari ini):

- **Pinjaman (bank/online) → detail akun → "Catat pembayaran"** — field **Tanggal** di form ini bisa diubah ke tanggal angsuran bulan lalu, bulan sebelumnya, dst. Catat satu per satu dari bulan paling lama ke paling baru, supaya perhitungan bunga terbayar & sisa pokok tetap akurat.
- **Titipan → "+ Catat titipan"** — field **Tanggal** di form ini juga bisa diubah, untuk mencatat titipan/pembayaran yang sebetulnya terjadi di bulan lalu.

**Untuk pinjaman yang memang sudah berjalan sejak sebelum pakai app ini** (bukan sekadar telat dicatat beberapa bulan), gunakan alur khusus saat *menambah akun baru*:
1. Status pinjaman → **"Sudah berjalan (sudah dipakai / dicicil sebagian)"**
2. Isi **Pokok awal saat pertama cair** (jumlah pinjaman asli) — beda dari **Sisa pokok sekarang** (saldo yang diisi di kolom saldo akun)
3. Isi **Tanggal pencairan** (tanggal asli pinjaman cair, boleh jauh di masa lalu) dan **Tenor**

Dengan kombinasi ini, jadwal angsuran otomatis menghitung mundur berapa bulan yang sudah "lunas" berdasarkan selisih pokok awal vs sisa sekarang — kamu tidak perlu mengetik ulang riwayat cicilan bulan demi bulan dari nol. Field "Catat pembayaran" di atas baru dipakai untuk angsuran-angsuran berikutnya (termasuk kalau ada yang telat dicatat).

> Catatan: field ini mengubah tanggal *transaksi* yang dicatat (pembayaran/titipan), bukan menambah field "tanggal akun dibuat" baru — akun pinjaman sudah punya field tanggal historisnya sendiri (Tanggal pencairan), dan itulah yang dipakai form-form di atas sebagai rujukan.

---

## Tab Laporan: Utang, Proyeksi & Simulasi

Selain ringkasan periode (pemasukan, pengeluaran, kategori, tren, perbandingan periode lalu), tab Laporan punya kartu-kartu berikut. Semuanya menggambarkan **kondisi saat ini**, tidak terpengaruh filter periode.

| Kartu | Isi |
|---|---|
| **Posisi saat ini** | Total aset, total utang (sisa pokok), sisa bunga kontrak, kekayaan bersih |
| **Rincian utang** | Per jenis: **Kartu kredit** (pemakaian vs limit, tagihan cetak, minimum, jatuh tempo, estimasi bunga), **PayLater** (limit terpakai, cicilan aktif, sisa bunga cicilan, tagihan periode ini), **Pinjol & Pinjaman bank** (sisa pokok + bunga & biaya, angsuran ke-n dari tenor, angsuran berikutnya, biaya efektif per bulan) |
| **Proyeksi kas 30/60 hari** | Saldo kas + bank + e-wallet setelah tagihan & angsuran terjadwal dibayar, urut per tanggal dengan saldo sesudahnya. Kalau saldo diperkirakan minus, tanggal dan tagihan penyebabnya ditandai |
| **Total biaya utang** | Bunga + admin + asuransi + materai per akun: sudah dibayar vs sisa, dan persen dari pokok |
| **Simulasi pelunasan** | Kolom dana ekstra per bulan, lalu bandingkan 3 skenario: tanpa ekstra, ekstra dengan bunga tertinggi dulu, ekstra dengan saldo terkecil dulu (cicilan yang lunas digulung ke utang lain). Hasil: bulan bebas utang dan bunga tambahan. Hasil langsung berubah saat angka diketik |
| **Saran** | Saran otomatis satu baris per poin (ketuk untuk detail), 3 tampil awal. Isinya: tagihan lewat jatuh tempo, dana 30 hari kurang, beban cicilan vs pemasukan, limit tinggi, pinjol berbiaya tinggi, pengeluaran melebihi pemasukan, beban bunga, prioritas dana ekstra |

**Cara kerja singkat:**
- **Beban cicilan** dibandingkan dengan rata-rata pemasukan bulanan 90 hari terakhir (tanpa pencairan pinjaman). Batas sehat yang dipakai: 30–35%.
- **Bunga pinjaman flat** (pinjol, dll.) dianggap sudah terkunci di saldo, jadi bayar lebih awal tidak menghemat bunga di simulasi. Manfaatnya hanya cicilan yang lunas bisa digulung ke utang lain.
- **Bunga kartu kredit** yang kosong dianggap 0% di simulasi. Isi bunganya di akun kartu supaya hasil akurat.
- Saran hanya berbasis data yang kamu catat dan bukan nasihat keuangan profesional.

---

## Export, Import & Backup

| Aksi | Fungsi |
|---|---|
| **Export JSON** | Unduh seluruh akun & transaksi sebagai `keuangan-YYYY-MM-DD.json` — ini file backup utama kamu |
| **Export CSV** | Unduh daftar transaksi (bukan akun) sebagai CSV, untuk dibuka di Excel/Sheets |
| **Import JSON** | **Gabungkan** isi file JSON ke data yang sudah ada — akun dengan nama+tipe yang sama tidak diduplikasi, transaksi baru ditambahkan |
| **Reset semua data** | Mengosongkan semua akun & transaksi (ada backup otomatis dulu sebelum dihapus) |
| **Reset lalu isi dari file** | Menghapus **semua** data lama lalu menggantinya total dengan isi file JSON yang dipilih (ada backup otomatis dulu) |

Semua field aset (Jenis, Jumlah, Satuan, Valuasi) dan field pinjol (termasuk cara bayar admin dan asuransi) ikut divalidasi & dinormalisasi konsisten di jalur Import maupun Reset-dari-file.

App juga akan menampilkan pengingat kalau kamu sudah lama tidak export (backup reminder), supaya tidak kehilangan data kalau tiba-tiba clear cache browser.

---

## Keterbatasan yang Perlu Diketahui

- **Satu mata uang (Rupiah)** — tidak ada dukungan multi-currency asli; forex/kripto ditangani lewat trik "harga per satuan = kurs", bukan konversi otomatis
- **Tidak ada kurs/harga live** — semua update valuasi aset dilakukan manual
- **Data lokal per-browser** — tidak sinkron otomatis antar perangkat; kalau ganti HP/laptop, export dari yang lama lalu import di yang baru
- **Mode privat/incognito** bisa menghapus data begitu jendela ditutup — hindari untuk pemakaian jangka panjang
- **Asuransi pinjol** dihitung sebagai persen per bulan dari pokok awal. Kalau aplikasi pinjolmu memakai skema lain (misalnya sekali di awal), pakai kolom angsuran manual
- **Denda keterlambatan** pinjol belum dihitung otomatis
- **Proyeksi kas** hanya menghitung tagihan & angsuran yang sudah terjadwal. Pemasukan (gaji dll.), belanja harian, dan tagihan kartu yang belum dicetak belum ikut
- **Simulasi pelunasan** memakai bunga bulanan sederhana dan tidak memasukkan belanja baru di kartu/PayLater
- Fitur unduh file (`downloadsCap`) memakai capability opsional (`claude.use('downloads')`) kalau tersedia di lingkungan tempat file ini dijalankan; kalau tidak tersedia, otomatis jatuh ke unduhan file browser biasa — tidak ada langkah tambahan yang perlu dilakukan

---

## Ide yang Belum Dibuat

- Tren total utang per bulan (grafik naik/turun)
- Dana darurat: saldo likuid setara berapa bulan pengeluaran
- Pencatatan denda keterlambatan terpisah
- Anggaran per kategori dengan target bulanan dan daftar langganan berulang
- Pengecekan otomatis akun pinjaman yang sisa pokoknya tidak cocok dengan jadwal, serta rekonsiliasi saldo
- Akun forex dalam USD/cent dengan kurs

---

## Struktur Kode (untuk yang mau modifikasi)

Semua JavaScript ada dalam satu blok `<script>` di akhir file, dengan penanda komentar `====` di tiap bagian:

1. Konfigurasi & state terpusat
2. Lapisan data (load/save, kalkulasi saldo)
3. Navigasi, header & aksi cepat
4. Form transaksi
5. Akun (rekening/kas/kartu)
6. Titipan / piutang
7. Util UI (modal, toast, format tanggal)
8. Render: ringkasan akun & daftar transaksi
9. Render: titipan/piutang
10. Grafik (SVG): tren, net worth, cashflow, kategori
11. Render: beranda
12. Tab laporan
13. Render utama (entry point: `render()`)
14. Import / export / reset data

Cari komentar `STRUKTUR FILE INI` di baris paling atas blok `<script>` untuk peta lengkapnya.

Titik masuk yang sering dipakai saat memodifikasi:

| Yang mau diubah | Cari |
|---|---|
| Rumus bunga/angsuran pinjaman | `computeLoanMonthlyInterest`, `computeOnlineInstallment`, `loanMonthlyFees`, `computeLoanSchedule`, `computeLoanRemaining` |
| Form akun pinjol | `updateAccFormFields`, `updateOnlineLoanEstimate`, `saveAccount` |
| Kartu-kartu di Laporan | `renderLaporanExtra`, `laporanDebtDetailHtml`, `laporanCashProjectionHtml`, `laporanDebtCostHtml`, `laporanSimulate`, `laporanAdviceHtml` |
| Nomor versi, nama pemilik di header | konstanta `APP_VERSION`, `APP_BUILD`, `OWNER_NAME` |

---

## Penomoran Versi

Nomor versi mengikuti nama file: `keuangan_pribadi-v1_1_NNN.html` ↔ footer `v1.1.NNN`. Nomor `NNN` naik tiap ada revisi. Detail per versi ada di [CHANGELOG.md](CHANGELOG.md).
