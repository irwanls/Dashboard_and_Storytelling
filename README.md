# Superstore Regional Performance Analysis

## Project Overview

Project ini menganalisis performa penjualan dan profitabilitas **Superstore** berdasarkan wilayah, kategori produk, dan tingkat diskon. Tujuan utama analisis adalah mengidentifikasi region dengan performa terbaik dan terlemah, mencari faktor yang berkaitan dengan rendahnya profit, serta menyusun rekomendasi bisnis yang dapat digunakan untuk memperbaiki profitabilitas.

Analisis dilakukan menggunakan **Python (Pandas & Jupyter Notebook)** untuk proses data understanding dan data quality checking, kemudian **Power BI** digunakan untuk visualisasi dan data storytelling.

---

## Business Questions

Analisis difokuskan untuk menjawab beberapa pertanyaan berikut:

1. Bagaimana performa penjualan pada setiap region?
2. Region mana yang memiliki profit margin tertinggi dan terendah?
3. Apa yang menyebabkan profit Region Central relatif rendah?
4. Bagaimana hubungan antara discount dan profit?
5. Apakah diskon yang lebih besar benar-benar diikuti oleh volume penjualan yang lebih tinggi?
6. Bagaimana tingkat urgentsi jika dilihat dari trend waktu?

---

## Data Source

Dataset didapat dari [**Superstore Dataset**](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final/data)

Karakteristik data yang digunakan dalam project:

| Informasi | Nilai |
|---|---:|
| Jumlah baris | 9,994 |
| Jumlah kolom | 21 |
| Periode transaksi | 3 Januari 2014 - 30 Desember 2017 |
| Negara | United States |
| Region | Central, East, South, West |
| Kategori produk | Furniture, Office Supplies, Technology |
| Jumlah customer | 793 |
| Jumlah order unik | 5,009 |
| Jumlah produk unik | 1,862 |

Dataset mencakup informasi transaksi seperti `Order Date`, `Ship Date`, `Customer`, `Region`, `Category`, `Sales`, `Quantity`, `Discount`, dan `Profit`.

---

## Data Quality

Sebelum analisis dilakukan, dataset diperiksa untuk memastikan kualitas dan konsistensi data.

### Pemeriksaan yang dilakukan

- Pemeriksaan struktur dan tipe data.
- Konversi `Order Date` dan `Ship Date` menjadi format tanggal.
- Penyesuaian identifier seperti `Row ID` dan `Postal Code`.
- Pemeriksaan missing value.
- Pemeriksaan duplicated rows.
- Pemeriksaan outlier menggunakan metode **Interquartile Range (IQR)** pada `Quantity`, `Sales`, `Discount`, dan `Profit`.
- Pemeriksaan konsistensi tanggal pengiriman terhadap tanggal order.

### Hasil pemeriksaan

| Data Quality Check | Hasil |
|---|---:|
| Missing value | 0 |
| Duplicate rows | 0 |
| Ship Date sebelum Order Date | 0 |
| Sales <= 0 | 0 |
| Quantity <= 0 | 0 |
| Discount di luar 0-100% | 0 |
| Negative-profit transactions | 1,871 |

Outlier yang terdeteksi dengan metode IQR:

| Variable | Jumlah Outlier |
|---|---:|
| Quantity | 170 |
| Sales | 1,167 |
| Discount | 856 |
| Profit | 1,881 |

Outlier tidak langsung dihapus karena nilai ekstrem pada transaksi retail dapat merepresentasikan transaksi besar, diskon tinggi, atau kerugian yang benar-benar terjadi. Menghapus seluruh outlier berpotensi menghilangkan informasi bisnis yang justru penting dalam analisis profitabilitas.

Secara struktural, dataset memiliki kualitas yang baik karena tidak ditemukan missing value maupun duplikasi. Tantangan utama bukan pada kelengkapan data, tetapi pada variasi nilai transaksi dan keberadaan transaksi dengan profit negatif.

---

## Analisis 


### 1. Bagaimana performa penjualan pada setiap region?

Total penjualan seluruh dataset mencapai sekitar **$2.30 juta**.

Kontribusi sales berdasarkan region:

| Region | Sales | Kontribusi Sales |
|---|---:|---:|
| West | $725.5K | 31.58% |
| East | $678.8K | 29.55% |
| Central | $501.2K | 21.82% |
| South | $391.7K | 17.05% |

**West** menjadi region dengan penjualan terbesar, diikuti oleh **East**. Sementara itu, South memiliki total sales paling rendah.

Namun, sales yang tinggi belum tentu berarti region tersebut memiliki profitabilitas terbaik. Karena itu, performa juga perlu dievaluasi menggunakan profit margin.

---

### 2. Region mana yang memiliki profit margin tertinggi dan terendah?

| Region | Profit | Profit Margin |
|---|---:|---:|
| West | $108.4K | 14.94% |
| East | $91.5K | 13.48% |
| South | $46.7K | 11.93% |
| Central | $39.7K | 7.92% |

**West** dan **East** tidak hanya memiliki sales tinggi tetapi juga profit margin yang relatif kuat.

Masalah utama terdapat pada **Central**. Meskipun region ini menghasilkan sales sekitar **$501.2K**, margin profitnya hanya **7.92%**, terendah dibandingkan seluruh region.

Artinya, persoalan Central bukan sekadar kemampuan menghasilkan penjualan, tetapi kemampuan mengubah penjualan tersebut menjadi profit.

---

### 3. Apa yang menyebabkan profit Region Central relatif rendah?

Analisis profit berdasarkan kategori menunjukkan:

| Category | Central Profit |
|---|---:|
| Furniture | **-$2.87K** |
| Office Supplies | $8.88K |
| Technology | $33.70K |

Kategori **Furniture** menjadi satu-satunya kategori di Central yang menghasilkan kerugian secara agregat.

Sebaliknya, Technology menghasilkan profit yang tinggi. Hal ini menunjukkan bahwa profit dari kategori yang sehat harus menutupi kerugian yang berasal dari Furniture.

Dengan demikian, masalah Central lebih tepat dipandang sebagai **masalah profitabilitas kategori**, bukan sekadar masalah rendahnya permintaan pada seluruh region.

---

### 4. Bagaimana hubungan antara discount dan profit?

Pola pada dataset menunjukkan bahwa profit memburuk ketika discount semakin tinggi.

| Discount | Total Profit |
|---:|---:|
| 0% | $320.99K |
| 10% | $9.03K |
| 15% | $1.42K |
| 20% | $90.34K |
| 30% | **-$10.37K** |
| 32% | **-$2.39K** |
| 40% | **-$23.06K** |
| 50% | **-$20.51K** |
| 60% | **-$5.94K** |
| 70% | **-$40.08K** |
| 80% | **-$30.54K** |

Pada dataset ini, tingkat diskon **30% dan lebih tinggi seluruhnya menghasilkan profit agregat negatif**.

Kondisi tersebut terlihat lebih jelas pada **Furniture di Region Central**:

- 0% discount: profit sekitar **+$16.64K**
- 30% discount: profit sekitar **-$6.87K**
- 32% discount: profit sekitar **-$2.39K**
- 50% discount: profit sekitar **-$4.31K**
- 60% discount: profit sekitar **-$5.94K**

Temuan ini menunjukkan hubungan yang kuat antara diskon tinggi dan kerugian pada kelompok transaksi tersebut.

Namun, hasil ini harus dibaca sebagai **hubungan/asosiasi**, bukan bukti kausalitas mutlak. Profit juga dapat dipengaruhi oleh harga pokok, product mix, lokasi, biaya operasional, dan karakteristik transaksi lainnya.

---

### 5. Apakah diskon yang lebih besar benar-benar diikuti oleh volume penjualan yang lebih tinggi?

Data tidak menunjukkan bahwa penjualan hanya dapat didorong dengan diskon besar.

Sebagian besar transaksi justru terjadi pada tingkat diskon yang relatif rendah, terutama pada **0% dan 20%**. Dengan kata lain, tingginya diskon tidak otomatis menjadi syarat untuk menarik pelanggan.

Ini menjadi sinyal bahwa perusahaan berpotensi mengurangi ketergantungan pada aggressive discounting tanpa harus langsung mengorbankan seluruh volume penjualan.

---

### 6. Bagaimana tingkat urgentsi jika dilihat dari trend waktu?

Performa tahunan menunjukkan perbedaan tren yang cukup jelas antarregion.

| Year | Central | East | South | West |
|---|---:|---:|---:|---:|
| 2014 | $0.54K | $17.06K | $11.88K | $20.07K |
| 2015 | $11.72K | $21.09K | $8.32K | $20.49K |
| 2016 | $19.90K | $20.14K | $17.70K | $24.05K |
| 2017 | $7.55K | $33.23K | $8.85K | $43.81K |

Pada 2017, **West dan East mengalami peningkatan profit yang kuat**, sedangkan **Central dan South turun dibanding tahun sebelumnya**.

Hal ini menunjukkan bahwa strategi tidak sebaiknya disamaratakan untuk semua region. Central dan South membutuhkan evaluasi yang lebih spesifik, sedangkan praktik yang berhasil di West dan East dapat dijadikan benchmark untuk dianalisis lebih lanjut.

---

## Kesimpulan


Analisis menunjukkan bahwa tingginya penjualan tidak selalu menghasilkan profitabilitas yang tinggi. West dan East memiliki performa yang relatif kuat, sedangkan Central menghadapi masalah profitabilitas yang terutama terlihat pada kategori Furniture.

Diskon menjadi salah satu variabel penting yang perlu diawasi. Pada data ini, tingkat discount 30% atau lebih berkaitan dengan profit agregat negatif. Oleh karena itu, perusahaan sebaiknya tidak hanya mengejar peningkatan volume penjualan, tetapi memastikan setiap strategi promosi tetap menghasilkan **profitable growth**.

---

## Recommendasi


### 1. Review Discount Policy

- menjadikan **discount di bawah 30% sebagai default policy**;
- mengevaluasi profit setelah discount, bukan hanya peningkatan sales.

### 2. Mengganti penggunaan discount dengan strategi marketing yang lain

Daripada memberikan diskon besar secara luas, perusahaan dapat menggunakan promosi yang lebih terarah seperti:

- bundling;
- voucher untuk pembelian berikutnya;
- loyalty reward;
- minimum-purchase promotion;
- hadiah atau doorprize;
- discount terbatas hanya untuk SKU tertentu.

Tujuannya adalah menjaga daya tarik promosi tanpa mengorbankan margin secara berlebihan.

---

## Tools

- **Jupyter Notebook (Python)**
- **Power BI**
- **Microsoft PowerPoint**

---

## Repository Contents

```text
.
├── Data_Storytelling_and_Analysis.ipynb
├── Superstore_Clean.csv
├── Data Storytelling & Analysis.pbix
└── Data Storytelling.pdf
```

- `Data_Storytelling_and_Analysis.ipynb` - proses data understanding, quality checking, dan outlier detection.
- `Superstore_Clean.csv` - dataset yang digunakan untuk analisis.
- `Data Storytelling & Analysis.pbix` - dashboard dan visualisasi Power BI.
- `Data Storytelling.pdf` - presentasi hasil analisis dan storytelling.

## Kontak
[**Linkedin**](https://www.linkedin.com/in/irwanls/)
[WhatsApp](https://wa.me/6285363679097)
