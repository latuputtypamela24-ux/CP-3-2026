 # Blue Bird: Menutup Kebocoran Revenue dan Menyeimbangkan Armada

Capstone Project Module 3, Business & Data Analyst.

Analisis 300.000 perjalanan taksi Blue Bird (Okt 2023 – Mar 2024) untuk menjawab enam pertanyaan bisnis seputar penyalahgunaan eVoucher korporat, anomali argometer IoT, dan idle time pengemudi.

## Latar belakang

Blue Bird mengoperasikan armada fisik, sehingga produktivitas tiap unit menentukan profitabilitas. Tiga masalah yang diselidiki:

1. Idle time (ngetem) yang menurunkan ritase.
2. eVoucher B2B yang diduga dipakai untuk keperluan pribadi di luar jam kerja.
3. Data argometer IoT dan aplikasi yang tidak sinkron (label kotor, jarak tidak masuk akal, argo menyala sebelum pesan).

## Dataset

| File | Isi | Baris |
|---|---|---|
| `bb_trips.csv` | Fakta perjalanan argometer | 300.000 |
| `bb_drivers.csv` | Pengemudi dan pool asal | 12.000 |
| `bb_corporate_clients.csv` | Klien B2B dan industri | 500 |

## Pertanyaan bisnis dan temuan utama

| # | Pertanyaan | Temuan |
|---|---|---|
| Q1 | Klien dengan eVoucher off-hours terbanyak | 31,7% trip eVoucher terjadi di weekend atau ≥ 22.00 (vs 28,3% pembayaran lain). Penyebab utama: lonjakan Sabtu ≥ 21.00 (±3.065 trip, ±Rp 0,38 miliar). Perbedaan antar-klien tidak signifikan (chi-square p ≈ 0,28), jadi masalahnya sistemik |
| Q2 | Pool dengan idle time terburuk | Puri tertinggi (±116 menit), BSD terendah (±112), tetapi tidak signifikan (Kruskal-Wallis p ≈ 0,79). Data tidak memuat lokasi |
| Q3 | Kekotoran `service_type` | 7 variasi penulisan untuk 4 kelas; 40,0% trip memakai tulisan non-standar |
| Q4 | eVoucher tanpa `company_id` | 9.002 dari 60.279 transaksi (14,93%), ±Rp 1,2 miliar |
| Q5 | Argo menyala sebelum booking | 9.000 trip (3,0%), tersebar acak, mengarah ke bug sinkronisasi |
| Q6 | Outlier jarak < 1 km bertarif tinggi | 1.956 trip, ditambah 1.044 trip berjarak 9.999 km. Total 3.000 trip = 7,0% revenue tercatat (Rp 2,76 miliar) |

## Metodologi

- **Data cleaning:** konversi datetime, cek duplikat dan relasi, standardisasi `service_type`, flag `company_id` kosong pada eVoucher, flag argo sebelum booking, dan pengecualian outlier jarak. Setiap langkah dicatat alasannya di notebook.
- **Analisis:** heatmap hari × jam, funnel plot antar-klien, uji chi-square, Kruskal-Wallis, ANOVA, dan perbandingan dengan distribusi Poisson.
- **Visualisasi:** matplotlib dan seaborn.

## Rekomendasi

1. Aturan jam eVoucher: Sabtu ≥ 21.00 wajib persetujuan atasan klien.
2. Wajibkan `company_id` pada eVoucher dan telusuri transaksi lama yang belum tertagih.
3. Audit sinkronisasi jam IoT dan aplikasi, dengan alert otomatis jika `meter_start < booking`.
4. Validasi sensor jarak di pipeline (tolak ≤ 0 atau ≥ 1.000 km).
5. Master data layanan dengan 4 kelas baku.
6. Tambahkan data lokasi (GPS/geofence) sebelum mengubah kuota ngetem.

## Keterbatasan

- Dataset tidak memuat lokasi, sehingga dugaan ngetem di bandara tidak bisa diuji.
- Log hanya mencatat trip yang terlayani.
- Idle time adalah proksi (jeda 0–240 menit antar-trip pengemudi yang sama).
- Pemetaan `service_type` bersifat asumsi dan perlu konfirmasi Data Engineering.
- Tarif hampir identik antar-kelas layanan, tidak lazim dan perlu dicek ke sumber.

## Struktur repository

```
.
├── Capstone_Project_3_Blue_Bird.ipynb
├── README.md
├── data/
│   ├── bb_trips.csv
│   ├── bb_drivers.csv
│   └── bb_corporate_clients.csv
└── images/        # dibuat otomatis saat notebook dijalankan
```
