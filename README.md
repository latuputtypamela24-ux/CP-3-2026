 Blue Bird: Menutup Kebocoran Revenue dan Menyeimbangkan Armada

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
