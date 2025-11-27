# 5220411078 | Haryo Prastiko

# Analisis Sentimen Twitter tentang Menkeu Purbaya

## Deskripsi Proyek

Proyek ini bertujuan untuk melakukan analisis sentimen pada cuitan Twitter terkait **Menkeu Purbaya**. Metode analisis yang digunakan bukan lagi pendekatan lexicon seperti InSet, tetapi menggunakan **pendekatan machine learning semi-supervised**, yaitu **Self-Training Classifier berbasis Logistic Regression**.

Pendekatan ini memungkinkan model belajar dari sedikit data berlabel, kemudian secara otomatis memberi label pada data yang belum berlabel.

---

## Tahapan Proses

### 1. Pengumpulan Data

* Data dikumpulkan dari Twitter menggunakan kata kunci yang berkaitan dengan **Menkeu Purbaya**.
* Dataset berisi teks tweet + metadata.
* Link scraping Colab:
  (https://colab.research.google.com/drive/1zXTAFSdnScbMxokrhiIYe2r50X0TPwii)

---

### 2. Pembersihan Data (Preprocessing)

Langkah-langkah preprocessing mencakup:

* Menghapus URL, mention, hashtag, angka, dan simbol.
* Lowercasing.
* Normalisasi kata tidak baku.
* Tokenisasi.
* Hasil akhir disimpan dalam kolom `preprocessing`.

---

### 3. Pembagian Data Berlabel dan Tidak Berlabel

Dataset dibagi menjadi:

**Data Berlabel Manual:**

| Label    | Jumlah |
| -------- | ------ |
| Positive | 100    |
| Negative | 100    |
| Neutral  | 50     |

**Data Tidak Berlabel:**

± 1000 tweet lainnya

Data berlabel digunakan sebagai *seed* untuk memulai proses self-training.

---

## 4. Ekstraksi Fitur (TF-IDF)

Sebelum masuk ke Logistic Regression, semua teks dikonversi menggunakan:

```
TF-IDF Vectorizer
ngram_range = (1,2)
max_features = 5000
```
---
### 5. Semi-Supervised Learning — Self-Training

Metode utama yang digunakan adalah:

```
SelfTrainingClassifier(base_estimator=LogisticRegression)
```

#### **5.1 Pelatihan Awal (Supervised)**

Model Logistic Regression dilatih hanya dengan 250 tweet berlabel.

#### **5.2 Pseudo-Labeling**

Model memprediksi data yang belum berlabel.
Jika confidence model cukup tinggi (misal > 0.8), data tersebut diberikan pseudo-label.

#### **5.3 Pelatihan Ulang (Iterasi Self-Training)**

Dataset final = data berlabel + pseudo-label.
Model dilatih ulang sehingga:

* Mampu memahami data jauh lebih besar
* Label otomatis semakin akurat
* Performa meningkat meskipun data manual sedikit

---

### 6. Pelabelan Sentimen

Setelah proses self-training selesai, model digunakan untuk memberi label pada seluruh dataset.

Label yang digunakan:

* **Positive**
* **Neutral**
* **Negative**

Label akhir disimpan dalam kolom `sentiment`.

---

### 7. Analisis Distribusi

Jumlah tweet per kategori dihitung, lalu divisualisasikan menggunakan grafik batang untuk melihat persepsi masyarakat di Twitter terhadap Menkeu Purbaya.

---

## Referensi

* Scikit-Learn Self-Training Classifier
* Scikit-Learn Logistic Regression
* TF-IDF Vectorizer Documentation

---

