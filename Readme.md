
#  OCR Plat Nomor Kendaraan dengan VLM & LMStudio

Proyek ini bertujuan untuk melakukan *Optical Character Recognition (OCR)* pada gambar plat nomor kendaraan menggunakan *Visual Language Model (VLM)* yang dijalankan melalui **LMStudio API** secara lokal. Evaluasi dilakukan dengan menghitung *Character Error Rate (CER)* untuk menilai seberapa akurat hasil pembacaan model.

---

## Tujuan

- Menerapkan **VLM (seperti llava-phi-3-mini)** untuk mengenali karakter dari gambar plat nomor.
- Membandingkan hasil prediksi dengan label asli (*ground truth*) menggunakan metrik **CER**.
- Menghasilkan file CSV berisi ringkasan prediksi dan evaluasi.

---

## Struktur Folder

```
.
├── UAS.py                # Script utama untuk menjalankan OCR
├── test/                 # Folder berisi gambar plat nomor
├── ground_truth.csv      # File CSV dengan label ground truth
├── ocr_results.csv       # File output hasil prediksi (otomatis dibuat)
└── README.md             # Dokumentasi proyek
```

---

## Format File ground_truth.csv

```csv
image,ground_truth
plat1.jpg,B1234XYZ
plat2.jpg,D5678ABC
```

- Kolom `image` harus sesuai nama file di folder `test/`.
- Kolom `ground_truth` berisi teks plat nomor asli.

---

## Prasyarat

- Python 3.8 atau lebih tinggi
- LMStudio terinstal dan berjalan di `http://localhost:1234`
- Model VLM (misalnya `llava-phi-3-mini`) aktif di LMStudio

### Instalasi Dependensi

```bash
pip install requests pillow
```

---

## Cara Menjalankan

1. Letakkan gambar dalam folder `test/`
2. Siapkan file `ground_truth.csv`
3. Jalankan script:

```bash
python UAS.py
```

---

## Apa yang Terjadi Saat Program Berjalan?

- Gambar dikonversi ke base64 dan dikirim ke LMStudio
- Model VLM memproses dan memprediksi teks plat nomor
- Prediksi dibandingkan dengan `ground_truth` → dihitung nilai **CER**
- Hasil dicetak ke terminal dan disimpan ke `ocr_results.csv`

---

##  Contoh Hasil Output

###  Di Terminal:

```
Processing: plat1.jpg
Ground Truth: B1234XYZ
Prediction: B1234XYZ
CER Score: 0.0000
--------------------------------------------------
SUMMARY RESULTS
Total Images Processed: 10
Accuracy (Exact Match): 80.00%
Average CER: 0.0625
```

### Di File `ocr_results.csv`:

```csv
image,ground_truth,prediction,CER_score
plat1.jpg,B1234XYZ,B1234XYZ,0.0000
plat2.jpg,D5678ABC,D5678AC,0.1429
```

---

## Metrik Evaluasi

| Metrik              | Deskripsi                                                |
|---------------------|----------------------------------------------------------|
| **CER**             | Rasio kesalahan karakter terhadap ground truth           |
| **Akurasi**         | Persentase prediksi yang 100% cocok                      |
| **Substitusi**      | Jumlah karakter yang diganti                             |
| **Insertions**      | Jumlah karakter tambahan yang tidak seharusnya ada       |
| **Deletions**       | Karakter dari ground truth yang hilang dalam prediksi    |

---

##  Teknologi Digunakan

| Komponen        | Deskripsi                              |
|-----------------|------------------------------------------|
| Python          | Bahasa pemrograman utama                 |
| LMStudio        | Menjalankan model VLM secara lokal       |
| LLaVA / Phi     | Model multimodal untuk gambar dan teks   |
| `requests`      | Kirim permintaan HTTP ke LMStudio        |
| `difflib`       | Hitung perbedaan karakter (untuk CER)    |
| `Pillow`        | Baca gambar dan ubah ke base64           |

---

##  Troubleshooting

| Masalah                          | Solusi                                                                 |
|----------------------------------|------------------------------------------------------------------------|
| `ConnectionError`                | Pastikan LMStudio aktif di `localhost:1234` dan model dimuat          |
| Gambar tidak terbaca             | Format gambar salah atau rusak → gunakan `.jpg`, `.png`, dll           |
| Prediksi kosong atau error       | Gambar tidak jelas / buram / format tidak tepat                        |
| ground_truth.csv tidak terbaca   | Pastikan file disimpan sebagai UTF-8 dan ada kolom `image,ground_truth`|


