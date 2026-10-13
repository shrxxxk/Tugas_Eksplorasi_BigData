# Tugas_Eksplorasi_BigData: Deteksi Aritmia EKG dengan Apache Spark Structured Streaming

Tugas Eksplorasi Mandiri, Mahadata / Big Data (IF25-40402), Institut Teknologi Sumatera.

## Tujuan
Mereplikasi sebagian alur kerja artikel berikut menggunakan PySpark mode lokal:

> Ilbeigipour, S., Albadvi, A., & Akhondzadeh Noughabi, E. (2021). Real-time heart arrhythmia
> detection using Apache Spark Structured Streaming. *Journal of Healthcare Engineering, 2021*,
> Article 6624829. https://doi.org/10.1155/2021/6624829

Jenis eksperimen: **replikasi parsial dengan perluasan** (5 kelas AAMI, pemisahan DS1/DS2 per rekaman,
fitur berupa 200 sampel sinyal mentah, bukan 25 fitur DWT seperti artikel). Hasil tidak boleh
dibandingkan langsung dengan angka artikel; lihat Bagian C laporan.

## Struktur folder
```
.
├── big-data.ipynb       # notebook eksperimen (simpan dengan output setelah Run All)
├── requirements.txt     # versi pustaka
└── README.md
```
Setelah notebook dijalankan, folder `results/` berisi:
`01_distribusi_kelas.csv`, `02_perbandingan_model.csv`, `03_streaming_microbatch.csv`, `04_run_summary.json`.

## Data
Dataset: **mondejar/mitbih-database** (MIT-BIH Arrhythmia Database dalam format CSV + anotasi TXT)
https://www.kaggle.com/datasets/mondejar/mitbih-database
Data tidak disertakan pada repositori ini; tambahkan melalui **Add Data** pada Kaggle Notebook.

## Menyiapkan lingkungan dan menjalankan
**Kaggle (lingkungan yang dipakai pada eksperimen):**
1. Unggah `big-data.ipynb` ke Kaggle Notebook.
2. Klik **Add Data**, cari dan tambahkan `mondejar/mitbih-database`.
3. Jalankan seluruh sel (**Run All**). Hasil tersimpan di `/kaggle/working/results`.

**Lokal:** pasang Java (JDK 17 atau lebih baru), lalu `pip install -r requirements.txt`.
Notebook mencari data di `/kaggle/input`; ubah pola pencarian `glob` pada sel 2 agar menunjuk ke
folder dataset di komputer Anda, lalu jalankan seluruh sel.

## Hasil utama (satu kali pelaksanaan, data uji DS2, 49.697 beat)
| Model | Akurasi | F1 makro | Latih (s) | Prediksi (s) |
|---|---|---|---|---|
| Random Forest | 0,9070 | 0,3260 | 16,40 | 1,98 |
| Logistic Regression | 0,8993 | 0,3067 | 23,79 | 1,57 |
| Decision Tree | 0,8776 | 0,3053 | 13,44 | 2,18 |

Akurasi *baseline* kelas mayoritas pada data uji adalah 0,8903, sehingga akurasi saja menyesatkan.
Simulasi Structured Streaming: 4 micro-batch, 49.697 beat, total 4,434 detik.

## Keterbatasan
Tanpa penyaringan FIR dan DWT; satu kali pelaksanaan tanpa validasi silang; model terbaik dipilih
pada data uji yang sama; streaming hanya simulasi file micro-batch pada satu mesin.

## Sumber dan atribusi
Artikel acuan di atas; MIT-BIH Arrhythmia Database (Moody & Mark, 2001; PhysioNet);
dataset Kaggle `mondejar/mitbih-database`. Naskah laporan disusun dengan bantuan generative AI
(Claude, Anthropic); lihat pernyataan pada laporan.
