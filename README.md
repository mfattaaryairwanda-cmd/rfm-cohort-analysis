# Analisis Retensi Pelanggan dengan Cohort Analysis

Analisis retensi pelanggan berbasis cohort menggunakan Python pada data transaksi ritel online tahun 2010. Proyek ini mengukur seberapa banyak pelanggan yang kembali bertransaksi setelah pembelian pertamanya, lalu menyajikannya dalam heatmap.

## Tujuan

- Mengukur tingkat retensi pelanggan dari bulan ke bulan.
- Membandingkan kualitas pelanggan antar cohort (kelompok bulan pertama order).
- Menemukan pola musiman dan titik kebocoran pelanggan untuk dasar rekomendasi bisnis.

## Dataset

- File: `Online Retail Data.csv` (transaksi ritel online, Januari sampai Desember 2010).
- Ukuran data mentah: 461.773 baris, 7 kolom.
- Kolom: `order_id`, `product_code`, `product_name`, `quantity`, `order_date`, `price`, `customer_id`.
- - Sumber data: dataset pembelajaran dari e-learning MySkill (myskill.id), berisi data transaksi ritel online tahun 2010.

## Metodologi

**1. Data cleaning**
- Mengubah `order_date` ke format datetime dan membuat kolom `year_month`.
- Menghapus baris tanpa `customer_id` dan tanpa `product_name`.
- Membuang produk uji coba (kata "test" pada kode atau nama produk).
- Membuat kolom `order_status` dari awalan `order_id`, serta kolom `amount` (quantity × price).
- Mengubah `quantity` negatif menjadi positif dan menghapus `price` negatif.
- Menyeragamkan `product_name` untuk setiap `product_code` dengan nama yang paling sering muncul.
- Menghapus outlier pada `quantity` dan `amount` dengan z-score (|z| < 3).
- Hasil akhir: 358.482 baris.

**2. Pembentukan retention cohort**
- Mengagregasi jumlah order per pelanggan per bulan.
- Menentukan cohort, yaitu bulan pertama pelanggan bertransaksi.
- Menghitung `period_num`, yaitu jarak bulan dari order pertama (bulan pertama = 1).
- Membuat tabel pivot jumlah pelanggan unik per cohort dan periode.
- Membagi tiap nilai dengan ukuran cohort untuk mendapat retention rate.
- Menampilkan hasilnya dalam heatmap (seaborn).

## Hasil

![User Retention Cohort](images/retention_heatmap.png)

*Simpan gambar heatmap dari notebook ke folder `images/` dengan nama di atas.*

### Temuan utama

- **Retensi rendah.** Pada bulan ke-2 hanya 20% hingga 39% pelanggan yang kembali bertransaksi, sehingga mayoritas pelanggan hanya membeli sekali.
- **Kualitas cohort menurun sepanjang tahun.** Cohort Januari paling loyal (retensi berkisar 38% hingga 47%), sedangkan cohort pertengahan hingga akhir tahun sekitar 20% pada bulan ke-2.
- **Efek musiman November.** Retensi naik di banyak cohort pada November 2010, kemungkinan karena musim belanja akhir tahun.
- **Penurunan Desember.** Kemungkinan besar karena data Desember belum lengkap (transaksi terakhir pada data bersih tercatat 23 Desember 2010), bukan karena pelanggan berhenti.

### Rekomendasi

- Fokuskan program retensi (voucher pembelian kedua, email tindak lanjut) pada 30 hari pertama setelah pembelian pertama.
- Telusuri faktor yang membuat cohort Januari lebih loyal, lalu replikasi.
- Siapkan kampanye menjelang November untuk memanfaatkan momentum musiman.
- Pastikan kelengkapan data Desember sebelum mengambil keputusan.

## Keterbatasan

- Analisis bersifat deskriptif, sehingga penyebab pasti belum dapat disimpulkan. Data sumber akuisisi dan riwayat promosi tidak tersedia.
- Cohort kecil (misalnya Desember, 66 pelanggan) mudah berfluktuasi.
- Sel kosong pada heatmap berarti data belum tersedia, bukan retensi 0%.
- Retensi dihitung dari pelanggan yang memiliki `customer_id`, sehingga transaksi tanpa ID tidak ikut dianalisis.

## Cara Menjalankan

1. Clone repository ini.
```
   git clone https://github.com/mfattaaryairwanda-cmd/retention-cohort-analysis.git
cd retention-cohort-analysis
```
2. Pasang library yang dibutuhkan.
```
   pip install pandas numpy scipy matplotlib seaborn jupyter
```
3. Letakkan file data di folder `data/`, lalu ubah path pada sel pertama notebook, misalnya:
```python
   df = pd.read_csv("data/Online Retail Data.csv")
```
4. Jalankan `Retention_Analysis.ipynb` dengan Jupyter Notebook.

## Struktur Repository

```
.
├── Retention_Analysis.ipynb
├── README.md
├── data/
│   └── Online Retail Data.csv
└── images/
    └── retention_heatmap.png
```

## Tools

Python, pandas, NumPy, SciPy, Matplotlib, Seaborn, Jupyter Notebook.

## Penulis

Fatta, Program Studi Statistika, Universitas Negeri Padang.