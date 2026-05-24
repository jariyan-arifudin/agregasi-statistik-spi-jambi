# Agregasi Statistik dan Klasifikasi Indeks Kekeringan (SPI)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Research-orange)

## Deskripsi Proyek

Repositori ini memuat skrip Python untuk melakukan agregasi temporal dan klasifikasi spasial terhadap data **Standardized Precipitation Index (SPI)** skala 1, 3, 6, dan 12 bulan. 

Fokus utama dari skrip ini adalah untuk mengonsolidasikan ratusan file *raster* bulanan (rentang waktu Januari 2010 – Desember 2020) menjadi representasi statistik tunggal (Mean dan Median) per piksel, lalu mereklasifikasikannya ke dalam tingkat keparahan kekeringan berstandar.

## Alur Kerja Geokomputasi

Skrip ini dibagi menjadi dua tahapan pemrosesan utama:

### 1. Komputasi Statistik Berbasis Blok (Block-based Processing)
Menghitung nilai *Mean* dan *Median* dari tumpukan (*stack*) matriks *raster* selama 11 tahun. 
* **Efisiensi Memori:** Untuk mencegah *OverFlow* pada RAM saat membaca 132 file *raster* sekaligus, proses pembacaan dan penulisan data dilakukan secara bertahap menggunakan metode `block_windows` dari pustaka `rasterio`.
* **Penanganan Data Kosong:** Menggunakan `np.nanmean` dan `np.nanmedian` agar piksel laut atau area di luar batas administrasi (*NoData*) tidak merusak kalkulasi statistik. Output disimpan dalam format `Float32`.

### 2. Klasifikasi Tingkat Kekeringan
Melakukan *reclassify* pada *raster* statistik yang telah dihasilkan ke dalam 5 kelas kekeringan. Output dikonversi menjadi format `Unsigned Integer 8-bit (uint8)` untuk optimasi ukuran file.

| Rentang Nilai SPI | Deskripsi Kelas | Nilai Raster Output |
| :--- | :--- | :---: |
| > 0.99 | Tidak Ada Kekeringan / Normal | 1 |
| -1.00 s.d 0.99 | Mendekati Normal | 2 |
| -1.50 s.d -1.00 | Agak Kering | 3 |
| -2.00 s.d -1.50 | Kering | 4 |
| <= -2.00 | Sangat Kering | 5 |

## Struktur Direktori Lingkungan Kerja

Skrip dikonfigurasi untuk dieksekusi di **Google Colab**. Pastikan direktori Google Drive Anda memiliki struktur hierarki sebagai berikut:

```text
/Colab Notebooks - [email]/Skripsi/
├── SPI-Output/            <-- (Input SPI-1)
├── SPI3-Output/           <-- (Input SPI-3)
├── SPI6-Output/           <-- (Input SPI-6)
├── SPI12-Output/          <-- (Input SPI-12)
├── Statistik_SPI_Output_2010_2020/   <-- (Output Tahap 1: Mean & Median)
└── Klasifikasi_SPI_2010_2020/        <-- (Output Tahap 2: Raster Kelas)

```

## Prasyarat Instalasi

Pastikan pustaka pemrosesan matriks dan data spasial telah terinstal:

```bash
pip install numpy rasterio

```

## Panduan Eksekusi

1. Buka skrip `.ipynb` di Google Colab.
2. Berikan izin akses integrasi ke Google Drive.
3. Jalankan *Cell* secara berurutan. Skrip dilengkapi dengan penanda proses (*print statement*) untuk memudahkan pemantauan (*monitoring*) progres iterasi pada setiap indeks SPI.

## Penulis

**Jariyan Arifudin** Mahasiswa Geografi Lingkungan

Universitas Gadjah Mada (UGM)

## Lisensi & Sitasi

Kode ini didistribusikan di bawah **MIT License**.
Jika Anda menggunakan metode ini untuk penelitian, silakan sitasi repositori ini:

> Arifudin, J. (2026). *Agregasi Statistik dan Klasifikasi Indeks Kekeringan (SPI)*. GitHub Repository.
