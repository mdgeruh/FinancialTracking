# Changelog

Riwayat perubahan **Keuangan Pribadi**. Format: yang terbaru di atas. Nomor versi mengikuti nama file (`keuangan_pribadi-v1_1_NNN.html`) dan footer app.

## v1.1.006 — 21 Sep 2026

**Diubah**
- Kartu **Saran** di tab Laporan dipadatkan: satu baris judul per saran dengan titik warna (merah/kuning/abu/hijau), ketuk untuk detail, maksimal 3 tampil awal, sisanya di tombol "Tampilkan N lagi"
- Ringkasan jumlah "segera / waspada" ditampilkan di judul kartu
- Saran serupa digabung (limit kartu + PayLater jadi satu; pinjol berbiaya tinggi jadi satu). Catatan bunga flat pindah ke dalam detail pinjol
- Saran "tagihan terdekat" dihapus karena sudah ada di Proyeksi kas
- Disclaimer dikecilkan jadi satu baris

## v1.1.005 — 21 Sep 2026

**Diubah**
- Header dirapikan: judul "Keuangan harian" jadi **"Keuangan pribadi"** (judul tab browser ikut), sapaan sesuai jam + nama pemilik (`OWNER_NAME`), tanggal singkat dengan hari ("Sen, 21 Sep 2026")
- Ikon pengaturan ⚙ diganti ikon SVG yang gayanya sama dengan ikon mata di kartu saldo, supaya tampil konsisten di semua perangkat
- Perataan eyebrow (sapaan, tanggal, ikon) dan jarak ke judul dirapikan

## v1.1.004 — 21 Sep 2026

**Ditambah**
- **Footer versi** di bawah semua tab: nama app, nomor versi, tanggal build (konstanta `APP_NAME`, `APP_VERSION`, `APP_BUILD`). Tidak ikut tercetak saat Export PDF

## v1.1.003 — 21 Sep 2026

**Ditambah** (tab Laporan)
- **Proyeksi kas 30/60 hari**: saldo kas + bank + e-wallet setelah tagihan & angsuran terjadwal, urut per tanggal, dengan peringatan tanggal saldo diperkirakan minus. Angsuran pinjaman diambil dari jadwal semua bulan, cicilan PayLater bulan berikutnya ikut dihitung
- **Total biaya utang**: bunga, admin, asuransi, materai per akun (sudah dibayar vs sisa, persen dari pokok), plus total gabungan
- **Simulasi pelunasan**: input dana ekstra per bulan, membandingkan 3 skenario (tanpa ekstra, ekstra bunga tertinggi dulu, ekstra saldo terkecil dulu) dengan cicilan yang lunas digulung ke utang lain. Hasil terbarui langsung saat mengetik

## v1.1.002 — 21 Sep 2026

**Ditambah** (tab Laporan)
- **Rincian utang** per jenis: kartu kredit (pemakaian vs limit, tagihan cetak, minimum, jatuh tempo, estimasi bunga), PayLater (limit terpakai, cicilan aktif, sisa bunga cicilan, tagihan periode ini), pinjol & pinjaman bank (sisa pokok + bunga & biaya, angsuran ke-n, angsuran berikutnya, biaya efektif per bulan)
- Kartu **Saran** otomatis: tagihan lewat jatuh tempo, pemakaian limit, beban cicilan vs rata-rata pemasukan 90 hari, dana likuid vs tagihan 30 hari, pinjol berbiaya tinggi, prioritas dana ekstra, arus kas & beban bunga

## v1.1.001 — 21 Sep 2026

**Ditambah** (akun Pinjaman Online)
- Field **Asuransi / proteksi pinjaman** (% per bulan dari pokok awal), opsional, boleh kosong
- Pilihan **Cara bayar biaya admin**: dipotong dari pencairan (default, perilaku lama) atau dicicil bersama angsuran (pencairan Rp0 biaya)
- Angsuran otomatis kini = pokok/tenor + bunga flat + asuransi + (admin/tenor kalau dicicil), dengan rincian di bawah field angsuran
- Meta akun menampilkan "Admin ... dicicil" dan "Asuransi ...%/bln (Rp.../bln)"
- Field baru ikut Export/Import JSON dan Reset-dari-file (`loanAdminMode`, `loanInsurancePercent`)

**Diubah**
- Asuransi dan admin yang dicicil diperlakukan sebagai bagian "bunga & biaya" tiap angsuran, jadi otomatis ikut sisa hutang, jadwal angsuran (baris jadi "bunga & biaya"), dan rincian "Catat pembayaran"
- Kalau admin dicicil, pencairan tidak lagi memotong biaya admin

**Catatan**
- Akun pinjol lama tidak terpengaruh (admin tetap dipotong saat cair, tanpa asuransi)

## v1.1.000 — basis revisi

Titik awal penomoran file. Berisi seluruh fitur dari riwayat sebelum penomoran di bawah.

---

## Riwayat sebelum penomoran file

**v1.2**
- Tambah field **Tanggal** di form "Catat pembayaran" (pinjaman/pinjaman online) dan form "Catat titipan" — sebelumnya keduanya selalu memakai tanggal hari ini tanpa bisa diubah, jadi tidak bisa dipakai untuk mencatat bulan-bulan yang sudah lewat. Sekarang tanggalnya bebas dipilih (maksimal hari ini), dengan validasi supaya tidak bisa diisi tanggal masa depan
- Pesan konfirmasi kedua form otomatis menampilkan tanggal kalau tanggal yang dipilih bukan hari ini, supaya tidak salah catat
- Lihat bagian "Mencatat Bulan yang Sudah Lewat" di [README.md](README.md) untuk panduan lengkapnya

**v1.1**
- Tambah jenis aset **"Forex / trading (akun cent)"** dan **"Kripto / crypto futures"** di dropdown, lengkap dengan hint cara pakai (Jumlah = equity USD/USDT, Harga per satuan = kurs)
- Tambah tombol **"Lunasi sekarang"** di detail Titipan — mengisi mode & nominal otomatis untuk pelunasan penuh
- **Perbaikan bug**: sheet detail Titipan sekarang ikut disegarkan otomatis saat ada transaksi yang dihapus/diedit dari riwayatnya (sebelumnya cuma detail Akun biasa yang disegarkan)
- Tambah normalisasi `assetKind`/`assetUnit` (mis. `"usd"` → `"USD"`, `"forex"` → `"Forex"`) yang berlaku konsisten di input manual **dan** saat Import JSON / Reset-dari-file

**v1.0**
- Rilis awal: pencatatan kas/bank/e-wallet, aset dengan valuasi, kartu kredit, PayLater, pinjaman bank/online, titipan/piutang, grafik, laporan, export/import JSON & CSV
