# Tugas 2 — Market Prioritization untuk UMKM (Kelompok 2)

Study Club Data Science Basic, KSM Veterantech UPNVJ 2026. Studi kasus dari Skalar
Solutions: menentukan segmen dan wilayah target sourcing leads B2B SaaS untuk produk
Klay, yaitu platform kasir (POS) berbasis awan dengan dashboard analitik AI untuk UMKM.
Dokumen penugasan dan ICP brief: https://s.id/PengumpulanStudyClubxSkalar

**Notebook Colab (hasil tereksekusi penuh):**
https://colab.research.google.com/drive/1Do2Qb1VEwmLDYHRO-IlmMAeA82qnKunw?usp=sharing

## Struktur repo

```
├── README.md            <- file ini (ringkasan proyek)
├── RESULT.md            <- hasil notebook: 5 visualisasi EDA dan tabel bobot ICP
├── assets/              <- PNG hasil notebook (g1 sampai g5)
├── notebooks/           <- Kelompok2_DS_SC_Public_VeteranTech_FINAL.ipynb
├── data/                <- 5 CSV mentah + PDF BPS + README.md (detail sumber dan penggugur)
└── laporan/             <- laporan progres
```

## Ringkasan pengerjaan

| Tahap | Hasil |
|---|---|
| Data Collection | 5 dataset dari 4 sumber publik resmi, level kabupaten/kota, mencakup 27 wilayah Jawa Barat |
| Data Preparation | pembersihan (2 missing, penyamarataan 29 menjadi 27 kode wilayah e-Samsat), lalu merging dengan kunci `kode_kabupaten_kota` |
| Kriteria Penggugur | batas persentil ke-10 sebesar 76.475 unit, sehingga 3 kota tereliminasi dan 27 wilayah menjadi 24 |
| Normalisasi | Min-Max ke rentang 0 sampai 1 atas 5 indikator (seluruhnya berarah positif sesuai ICP) |
| EDA | 5 visualisasi (2 univariat, 2 bivariat, 1 multivariat) beserta interpretasi bisnisnya |
| Tabel Bobot ICP | 5 komponen dengan total 100% (30/25/20/15/10) |

Rincian lengkap berupa grafik, interpretasi, dan tabel disajikan pada [RESULT.md](RESULT.md).

## Link Sumber Data

1. **UMKM Proyeksi (Diskuk, `od_17371`)** — https://data.jabarprov.go.id/api-backend/bigdata/diskuk/od_17371_proyeksi_jml_ush_mikro_kecil_menengah_umkm__kabupa_v2
2. **UMKM Binaan per Jenis Usaha (Diskuk, `od_17367`)** — https://data.jabarprov.go.id/api-backend/bigdata/diskuk/od_17367_jml_ush_mikro_kecil_menengah_umkm_binaan__jenis_us_v2
3. **Internet (BPS Kesra Jabar 2023, Tabel 7.4)** — https://jabar.bps.go.id/id/publication/2023/12/28/8a2667bb40625076d8ee9c30/statistik-kesejahteraan-rakyat-provinsi-jawa-barat-2023.html
4. **e-Samsat (Bapenda, `od_15834`)** — https://data.jabarprov.go.id/api-backend/bigdata/bapenda/od_15834_jml_pembayaran_pajak_kndrn_brmtr_melalui_e_samsat__v6
5. **PDRB per Kapita ADHB (BPS, via portal Jabar)** — https://galura.jabarprov.go.id/api/bigdata/produk-domestik-regional-bruto-per-kapita-atas-dasar-harga-berlaku-berdasarkan-kabupatenkota-di-jawa-barat?download=csv

Portal pendukung: [Galura](https://galura.jabarprov.go.id), [data.jabarprov.go.id](https://data.jabarprov.go.id), dan [BPS Jawa Barat](https://jabar.bps.go.id).

## Proses pencarian data

Pencarian sumber dibantu dengan katalog terbuka [indonesia-gov-apis](https://github.com/suryast/indonesia-gov-apis),
yaitu indeks 59 sumber data pemerintah Indonesia yang dikelompokkan per tingkat verifikasi
(tier 1 sampai tier 8) dan dilengkapi catatan akses per sumber. Katalog ini dipakai sebagai
peta awal, dan kontribusinya terasa pada tiga hal.

Pertama, katalog mempersempit ruang cari. Alih-alih menelusuri puluhan portal satu per satu,
kita langsung diarahkan ke dua kandidat yang relevan untuk indikator ICP, yaitu BPS (tier 1,
dokumentasi primer sudah diperiksa) dan portal data daerah termasuk Open Data Jabar (tier 3).
Kedua, catatan aksesnya menyelamatkan waktu: katalog menegaskan bahwa portal yang menyala
belum tentu API-nya jalan. Peringatan itu terbukti, karena CKAN lama Open Data Jabar
(`opendata.jabarprov.go.id/api`) memang mengembalikan 403, sedangkan backend
`data.jabarprov.go.id/api-backend` masih aktif dan akhirnya dipakai untuk mengunduh tiga
dataset UMKM dan e-Samsat. Ketiga, katalog memberi contoh pola akses per sumber sehingga
verifikasi langsung menjadi cepat: setiap endpoint diuji sendiri sebelum data diambil, dan
dataset PDRB ditemukan melalui API pencarian portal baru Jabar (Galura), sedangkan indikator
internet, yang tidak tersedia sebagai dataset API, dilacak sampai ke publikasi PDF BPS.

Katalog ini sendiri tidak dikutip sebagai sumber data pada Tabel 1.1, sebab ia hanya
menghubungkan ke sumber aslinya; sumber yang dicantumkan tetap Open Data Jabar dan BPS.
Repo katalog juga ditempatkan pada folder `tools/` yang di-gitignore agar tidak ikut ke
dalam repo ini.

## Kriteria Penggugur (ICP Klay, Bagian 4) dan penerapannya

1. **Wilayah dengan jumlah UMKM sangat kecil** (batas ditentukan sendiri oleh peserta).
   Batas yang dipilih adalah persentil ke-10 dari total UMKM 2023, yaitu **76.475 unit**.
   Tiga wilayah terkena kriteria ini: Kota Sukabumi (60.866), Kota Cirebon (61.234), dan
   Kota Banjar (39.422).
2. **Wilayah dengan data kunci kosong.** Tidak ada wilayah yang terkena, karena seluruh
   nilai kosong telah ditangani pada tahap pembersihan tanpa imputasi tebakan.
3. **Sektor tanpa transaksi harian rutin.** Sejak tahap pemetaan sektor, hanya Ritel
   (FASHION, KONVEKSI), F&B (KULINER, MAKANAN, MINUMAN), dan Jasa yang dihitung ke dalam
   kepadatan sektor target.

Hasilnya, dari 27 wilayah tersisa **24 wilayah** yang lolos ke tahap normalisasi.
Rincian lengkapnya ada pada [data/README.md](data/README.md).

## Keterbatasan

- Tahun data antar sumber tidak seragam: UMKM 2023, internet 2023, e-Samsat 2024, dan
  PDRB 2024.
- Kepadatan sektor 2023 dihitung tanpa kontribusi F&B, karena data binaan F&B tercatat 0
  pada 2023 (anomali pencatatan).
- Indikator internet hanya tersedia dari publikasi PDF BPS, karena portal data tidak
  menyediakan API untuk indikator tersebut.
- e-Samsat dipakai sebagai proksi adopsi pembayaran digital, karena data transaksi QRIS
  UMKM tidak tersedia untuk publik.
- Indeks skor akhir, K-Means, uji sensitivitas, dan Top 5 segmen dikerjakan pada tahap
  Final Project, di luar cakupan Milestone 1.
