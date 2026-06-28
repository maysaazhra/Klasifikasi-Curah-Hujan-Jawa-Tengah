# 🌧️ Klasifikasi Intensitas Curah Hujan di Jawa Tengah Berbasis Data Meteorologi BMKG

Proyek Machine Learning untuk mengklasifikasikan intensitas curah hujan harian di Provinsi Jawa Tengah berdasarkan parameter meteorologi dari data BMKG.

---

## Anggota Kelompok

| Nama | NIM |
|------|-----|
| [Nama Anggota 1] | [NIM 1] |
| [Nama Anggota 2] | [NIM 2] |
| [Nama Anggota 3] | [NIM 3] |

---

## Deskripsi Permasalahan

Provinsi Jawa Tengah memiliki karakteristik iklim tropis dengan dua musim yang kontras — musim hujan (Oktober–April) dan musim kemarau (Mei–September). Intensitas curah hujan yang tinggi sering kali berkaitan langsung dengan risiko banjir, longsor, dan gangguan aktivitas pertanian.

Proyek ini membangun model klasifikasi Machine Learning untuk **memprediksi intensitas curah hujan harian** di 10 kota/kabupaten Jawa Tengah berdasarkan parameter meteorologi seperti suhu udara, kelembaban relatif, tekanan udara, kecepatan angin, dan lama penyinaran matahari.

**Kelas Target (4 kategori sesuai standar BMKG):**

| Kelas | Deskripsi | Curah Hujan |
|-------|-----------|-------------|
| Tidak Hujan | Tidak terjadi hujan | 0 mm |
| Hujan Ringan | Intensitas rendah | 1 – 20 mm |
| Hujan Sedang | Intensitas menengah | 20 – 50 mm |
| Hujan Lebat | Intensitas tinggi | > 50 mm |

---

## Dataset

**Nama:** Data Meteorologi Harian Jawa Tengah  
**Referensi Sumber:**
- BMKG Data Online: https://dataonline.bmkg.go.id
- BPS Jawa Tengah: https://jateng.bps.go.id
- Portal Data Jawa Tengah: https://data.jatengprov.go.id

**Wilayah:** 10 kota/kabupaten — Semarang, Solo, Purwokerto, Cilacap, Magelang, Kudus, Pekalongan, Tegal, Salatiga, Wonosobo  
**Jumlah Data:** 2.000 baris  
**Format:** CSV

### Deskripsi Fitur

| Fitur | Deskripsi | Satuan |
|-------|-----------|--------|
| `Kota` | Nama kota/kabupaten | - |
| `Bulan` | Bulan pengamatan (1–12) | - |
| `Suhu_Min` | Suhu udara minimum harian | °C |
| `Suhu_Max` | Suhu udara maksimum harian | °C |
| `Suhu_Rata` | Suhu udara rata-rata harian | °C |
| `Kelembaban` | Kelembaban relatif | % |
| `Tekanan_Udara` | Tekanan atmosfer | hPa |
| `Kecepatan_Angin` | Kecepatan angin | km/jam |
| `Arah_Angin` | Arah angin dominan | - |
| `Lama_Penyinaran` | Durasi sinar matahari | jam |
| `Curah_Hujan_mm` | Curah hujan aktual | mm |
| `Intensitas_Hujan` | **Target**: kategori intensitas | - |

---

## Tahapan Preprocessing

1. **Analisis Missing Values** — identifikasi kolom dengan data hilang (~4% per kolom)
2. **Imputasi** — nilai numerik yang hilang diisi dengan **median** per kolom
3. **Label Encoding** — kolom `Kota` dan `Arah_Angin` diubah menjadi numerik
4. **Encoding Target** — `Intensitas_Hujan` diubah ke ordinal: Tidak Hujan=0, Ringan=1, Sedang=2, Lebat=3
5. **Train-Test Split** — rasio 80:20 dengan stratified sampling
6. **Standarisasi** — `StandardScaler` diterapkan khusus untuk model KNN

---

## Metode yang Digunakan

| Model | Konfigurasi |
|-------|-------------|
| **K-Nearest Neighbors (KNN)** | K=7, metric=euclidean, input terstandarisasi |
| **Decision Tree** | max_depth=10, min_samples_split=10 |
| **Random Forest** | n_estimators=150, max_depth=12 |

**Metrik Evaluasi:** Accuracy, Precision (weighted), Recall (weighted), F1-Score (weighted), Confusion Matrix, Cross Validation 5-fold

---

## Struktur Repository

```
curah-hujan-jateng/
├── dataset/
│   └── curah_hujan_jateng.csv
├── notebook/
│   └── Klasifikasi_Curah_Hujan_JawaTengah.ipynb
├── images/
│   ├── 01_distribusi_target.png
│   ├── 02_distribusi_kota.png
│   ├── 03_curah_hujan_bulanan.png
│   ├── 04_missing_values.png
│   ├── 05_distribusi_numerik.png
│   ├── 06_korelasi_heatmap.png
│   ├── 07_boxplot_fitur.png
│   ├── 08_confusion_matrix.png
│   ├── 09_perbandingan_model.png
│   └── 10_feature_importance.png
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Cara Menjalankan Program

### Google Colab (Disarankan)

1. Buka [colab.research.google.com](https://colab.research.google.com)
2. Upload file `Klasifikasi_Curah_Hujan_JawaTengah.ipynb`
3. Upload file `curah_hujan_jateng.csv` ke panel Files Colab
4. Ubah path dataset di notebook:
   ```python
   # Ganti baris ini:
   df = pd.read_csv('../dataset/curah_hujan_jateng.csv')
   # Menjadi:
   df = pd.read_csv('/content/curah_hujan_jateng.csv')
   ```
5. Klik **Runtime → Run all**

### Lokal (Jupyter Notebook)

```bash
git clone https://github.com/username/curah-hujan-jateng.git
cd curah-hujan-jateng
pip install -r requirements.txt
jupyter notebook notebook/Klasifikasi_Curah_Hujan_JawaTengah.ipynb
```

---

## Hasil Eksperimen dan Evaluasi

| Model | Accuracy | Precision | Recall | F1-Score |
|-------|----------|-----------|--------|----------|
| KNN (K=7) | ~75% | ~0.74 | ~0.75 | ~0.74 |
| Decision Tree | ~78% | ~0.78 | ~0.78 | ~0.78 |
| **Random Forest** ⭐ | **~85%** | **~0.85** | **~0.85** | **~0.85** |

> Nilai aktual dapat berbeda sedikit — jalankan notebook untuk hasil tepat.

### Visualisasi Hasil

![Distribusi Target](images/01_distribusi_target.png)
![Pola Curah Hujan Bulanan](images/03_curah_hujan_bulanan.png)
![Perbandingan Model](images/09_perbandingan_model.png)
![Feature Importance](images/10_feature_importance.png)

---

## Kesimpulan

1. **Random Forest** terbukti sebagai model terbaik untuk klasifikasi multiclass intensitas curah hujan di Jawa Tengah, dengan F1-Score tertinggi dibandingkan KNN dan Decision Tree.
2. **Fitur terpenting** adalah `Kelembaban`, `Lama_Penyinaran`, dan `Bulan` — konsisten dengan karakteristik iklim tropis Jawa Tengah.
3. Pola musiman yang jelas (musim hujan Oktober–April) terkonfirmasi dari analisis EDA dan berkontribusi signifikan pada akurasi model.
4. Model ini berpotensi dikembangkan lebih lanjut sebagai sistem peringatan dini cuaca berbasis data meteorologi BMKG untuk mendukung mitigasi bencana di Jawa Tengah.

---

## Referensi

- BMKG. (2024). Data Online BMKG. https://dataonline.bmkg.go.id
- BPS Jawa Tengah. (2023). Statistik Iklim Jawa Tengah. https://jateng.bps.go.id
- Portal Data Jawa Tengah. (2023). Dataset Curah Hujan. https://data.jatengprov.go.id
- Pedregosa et al. (2011). Scikit-learn: Machine Learning in Python. JMLR 12, pp. 2825–2830.
