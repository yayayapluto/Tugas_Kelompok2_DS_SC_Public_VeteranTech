# Paket Data Mentah — Market Prioritization UMKM (Kelompok 2)

Kelompok 2 — Study Club Data Science Basic, KSM Veterantech UPNVJ 2026.
Kunci penggabungan antar file: `kode_kabupaten_kota` (kode BPS: 3201–3218 kabupaten,
3271–3279 kota). Cakupan: **27 wilayah Jawa Barat**.
Dokumen penugasan & ICP brief: https://s.id/PengumpulanStudyClubxSkalar

## Daftar file & sumber

| File | Indikator ICP | Sumber resmi & link |
|---|---|---|
| `01_umkm_proyeksi_2017-2023_raw.csv` | 1. Ukuran Pasar | Open Data Jabar – Diskuk, "Proyeksi Jumlah UMKM Berdasarkan Kabupaten/Kota" (`od_17371`): `https://data.jabarprov.go.id/api-backend/bigdata/diskuk/od_17371_proyeksi_jml_ush_mikro_kecil_menengah_umkm__kabupa_v2` |
| `02_umkm_binaan_2019-2023_raw.csv` | 2. Kepadatan Sektor Target | Open Data Jabar – Diskuk, "UMKM Binaan Berdasarkan Jenis Usaha" (`od_17367`): `https://data.jabarprov.go.id/api-backend/bigdata/diskuk/od_17367_jml_ush_mikro_kecil_menengah_umkm_binaan__jenis_us_v2` |
| `03_internet_bps_2023_raw.csv` | 3. Kesiapan Digital | BPS, "Statistik Kesejahteraan Rakyat Provinsi Jawa Barat 2023", Tabel 7.4 (hal. PDF 164): https://jabar.bps.go.id/id/publication/2023/12/28/8a2667bb40625076d8ee9c30/statistik-kesejahteraan-rakyat-provinsi-jawa-barat-2023.html — % penduduk 5+ pengguna internet 2023, kolom Laki-laki+Perempuan; diekstrak dari PDF (`BPS_Statistik_Kesejahteraan_Rakyat_Jabar_2023.pdf` di folder ini) |
| `04_esamsat_2014-2025_raw.csv` | 4. Adopsi Pembayaran Digital (proksi) | Open Data Jabar – Bapenda, "Pembayaran Pajak Kendaraan Bermotor via e-Samsat" (`od_15834`): `https://data.jabarprov.go.id/api-backend/bigdata/bapenda/od_15834_jml_pembayaran_pajak_kndrn_brmtr_melalui_e_samsat__v6` |
| `05_pdrb_perkapita_2017-2025_raw.csv` | 5. Aktivitas Ekonomi | Open Data Jabar – dataset BPS, "PDRB per Kapita ADHB Berdasarkan Kabupaten/Kota" (ribu Rp, harga berlaku): `https://galura.jabarprov.go.id/api/bigdata/produk-domestik-regional-bruto-per-kapita-atas-dasar-harga-berlaku-berdasarkan-kabupatenkota-di-jawa-barat?download=csv` |

## Link Sumber Data

1. **UMKM Proyeksi (Diskuk, `od_17371`)** — https://data.jabarprov.go.id/api-backend/bigdata/diskuk/od_17371_proyeksi_jml_ush_mikro_kecil_menengah_umkm__kabupa_v2
2. **UMKM Binaan per Jenis Usaha (Diskuk, `od_17367`)** — https://data.jabarprov.go.id/api-backend/bigdata/diskuk/od_17367_jml_ush_mikro_kecil_menengah_umkm_binaan__jenis_us_v2
3. **Internet (BPS Kesra Jabar 2023, Tabel 7.4)** — https://jabar.bps.go.id/id/publication/2023/12/28/8a2667bb40625076d8ee9c30/statistik-kesejahteraan-rakyat-provinsi-jawa-barat-2023.html
4. **e-Samsat (Bapenda, `od_15834`)** — https://data.jabarprov.go.id/api-backend/bigdata/bapenda/od_15834_jml_pembayaran_pajak_kndrn_brmtr_melalui_e_samsat__v6
5. **PDRB per Kapita ADHB (BPS, via portal Jabar)** — https://galura.jabarprov.go.id/api/bigdata/produk-domestik-regional-bruto-per-kapita-atas-dasar-harga-berlaku-berdasarkan-kabupatenkota-di-jawa-barat?download=csv

Portal & dokumen pendukung:
- Portal Open Data Jabar (baru): https://galura.jabarprov.go.id
- Portal tabel statistik Jabar (lama): https://data.jabarprov.go.id
- BPS Jawa Barat: https://jabar.bps.go.id
- Dokumen Case Study + ICP Brief Skalar: https://s.id/PengumpulanStudyClubxSkalar

## Kriteria Penggugur (ICP Klay — Bagian 4) & penerapan

Kriteria dari ICP Brief: segmen/wilayah yang terkena kriteria berikut tidak diikutkan
dalam ranking, dan dicatat dalam daftar tersendiri beserta alasannya:

1. **Wilayah dengan jumlah UMKM sangat kecil** — peserta menentukan batas sendiri dan
   menjelaskannya.
   *Penerapan:* batas = persentil ke-10 total UMKM 2023 = **76.475 unit**.
   Kena: Kota Sukabumi (60.866), Kota Cirebon (61.234), Kota Banjar (39.422) —
   alasannya jumlah UMKM di bawah batas, kontribusi marjinal bagi pipeline SDR.
2. **Wilayah dengan data kunci kosong** — dinyatakan eksplisit, tanpa imputasi tebakan.
   *Penerapan:* 0 wilayah kena (missing yang ada sudah ditangani di tahap cleaning).
3. **Sektor tanpa transaksi harian rutin.**
   *Penerapan:* sejak pemetaan sektor (Bagian 2 — Poin 2a), hanya Ritel (FASHION,
   KONVEKSI), F&B (KULINER, MAKANAN, MINUMAN), dan Jasa (JASA) yang dihitung ke
   kepadatan sektor target; jenis usaha lain (mebel, batik, agribisnis, dll.) di luar ICP.

Hasil: **27 → 24 wilayah** lolos ke tahap normalisasi.

## Cara mengunduh ulang data

API backend Open Data Jabar (User-Agent harus browser lengkap; paginasi `?limit=1000&skip=0`).
Portal baru: https://galura.jabarprov.go.id (pencarian: `GET /api/search?q=`;
unduh CSV per dataset: `GET /api/bigdata/{slug}?download=csv`).

## Anomali yang diketahui (ditangani & didokumentasikan di notebook)

- 1 missing `kode_provinsi` di file 01 dan 04 → diisi 32 (seluruh data berasal dari Jawa Barat).
- File 04 memuat 29 kode wilayah (kode bergeser di sebagian tahun) → diseragamkan ke kode
  BPS berdasarkan nama wilayah; hasil 27 kode, 0 baris tanpa kode.
- F&B binaan = 0 pada 2023 (2019–2022: 1.619–3.632 unit) → anomali pencatatan; kepadatan
  sektor 2023 dihitung tanpa F&B sehingga cenderung underestimate untuk wilayah kuliner.
- Tahun antar sumber tidak seragam: UMKM 2023, internet 2023, e-Samsat 2024, PDRB 2024 —
  dicatat sebagai keterbatasan di laporan.
