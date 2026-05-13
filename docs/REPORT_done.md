# Laporan Kelompok — ANN Bake-Off

## Identitas Kelompok

* **Nama Kelompok:** Sawit Supremacy
* **Anggota:**
1. Andreanus Joe Reynald — 32230010 — Koordinator GitHub, Merge PR & Review Akhir
2. Randy Frederick Leonardy — 32230016 — Varian 04 (MLP ReLU)
3. Stevan Wiyandi — 32230023 — Varian 01 (Single Layer)
4. Steven Wiyandi — 32230024 — Varian 02 (MLP Sigmoid) 
5. Dustin Darmawan Isya Widjaja — 32230026 — Kompilasi & Plotting Grafik
6. Dantacitto Jonatan — 32230027 — Varian 03 (MLP Tanh)
7. Wendy Tjung — 32230036 — Comparisson (5)

---

## 1. Ringkasan Hasil Eksperimen

| Varian | Arsitektur | Aktivasi | Test Accuracy | Test Loss | Jumlah Parameter |
| --- | --- | --- | --- | --- | --- |
| 01 | Single layer | — | 0.7500 | 0.5437 | 15 |
| 02 | 1 hidden (16) | Sigmoid | 0.9333 | 0.3025 | 131 |
| 03 | 1 hidden (16) | Tanh | 0.9583 | 0.1265 | 131 |
| 04 | 2 hidden (32→16) | ReLU | 0.9583 | 0.0807 | 739 |

*(Catatan: Test Accuracy & Loss untuk varian 01, 03, dan 04 diestimasi berdasarkan metrik validasi akhir dari grafik eksperimen perbandingan, sementara varian 02 diambil dari log eksperimen).*

---

## 2. Analisis & Diskusi

### 2.1 Apakah single-layer mampu mencapai akurasi yang sebanding dengan multi-layer? Mengapa?

**Tidak sebanding.** Berdasarkan grafik *Validation Accuracy*, model `01_single_layer` tertahan di akurasi sekitar 75% (0.75). Sebaliknya, semua varian *multi-layer* (MLP dengan Sigmoid, Tanh, maupun ReLU) mampu mencapai akurasi akhir yang jauh lebih tinggi, yaitu sekitar 95.8% (0.958). Pada grafik *Validation Loss*, varian *single-layer* juga memiliki *loss* tertinggi (sekitar 0.54) di akhir epoch.

**Hal ini terjadi karena** model *single-layer* hanya mampu membentuk batas keputusan (*decision boundary*) yang linier. Karena dataset ini tidak sepenuhnya *linearly separable*, model *single-layer* kesulitan memisahkan kelas-kelas data dengan akurat. Varian *multi-layer* (MLP) menangani masalah ini dengan memanfaatkan *hidden layer* yang memberikan kapasitas ekstra bagi model untuk mengekstraksi dan mempelajari representasi data non-linear yang jauh lebih kompleks.

### 2.2 Pada dataset ini, apakah ReLU benar-benar konvergen lebih cepat dibanding Sigmoid? Buktikan dengan grafik loss.

**Benar, ReLU konvergen secara signifikan lebih cepat.** Pada grafik "Perbandingan Validation Loss Antar Varian", garis merah (`04_mlp_relu`) terlihat menukik tajam ke bawah (*steep drop*) pada 20 epoch pertama dan dengan cepat menstabilkan *loss* di angka yang sangat rendah (0.08). Sebaliknya, garis oranye (`02_mlp_sigmoid`) turun dengan sangat lambat dan melandai, mengakhiri epoch 100 dengan *loss* yang masih di atas 0.25. Hal ini membuktikan bahwa ReLU meminimalisir masalah *vanishing gradient* yang sering dialami oleh fungsi Sigmoid, sehingga proses pembaruan bobot (*weight update*) berjalan lebih optimal dan cepat.

### 2.3 Bandingkan klaim slide 2.7 ("Tanh mempercepat pembelajaran karena zero-centered") dengan hasil empiris Anda.

**Hasil empiris sejalan dan membuktikan klaim tersebut.** Jika kita membandingkan performa Tanh dengan Sigmoid pada kedua grafik, Tanh secara konsisten unggul. Pada grafik *Validation Accuracy*, model Tanh menyentuh akurasi 90% jauh lebih awal (sekitar epoch 25) dibandingkan Sigmoid yang baru mencapai angka tersebut setelah epoch 30-an. Pada grafik *Loss*, Tanh juga turun lebih cepat dan berakhir di angka yang lebih rendah (0.12) dibandingkan Sigmoid. Sifat *zero-centered* (berpusat di nol dengan rentang -1 hingga 1) pada Tanh membuat rata-rata aktivasi mendekati nol, yang memperlancar arus gradien saat *backpropagation* dan mencegah pembaruan bobot bergerak zig-zag.

---

## 3. Refleksi Proses Kerja Kelompok

Proses pengerjaan proyek *ANN Bake-Off* ini berjalan dengan cukup baik meskipun dengan ukuran kelompok yang cukup besar (7 orang). Untuk memastikan semua anggota berkontribusi secara merata, kami membagi tugas ke dalam dua fase utama: fase implementasi kode dan fase penyusunan analisis. Andreanus, Randy, Stevan, dan Steven masing-masing mengambil tanggung jawab untuk membangun, melatih, dan menyimpan *history* untuk satu dari empat varian arsitektur neural network. Sementara itu, Dustin dan Dantacitto berfokus pada pengolahan log eksperimen, visualisasi komparatif, dan perumusan draf narasi. Wendy bertugas sebagai koordinator repositori untuk memastikan *version control* berjalan lancar.

Salah satu kesulitan teknis terbesar yang kami temui adalah mengatur alur kerja Git di GitHub. Dengan tujuh orang yang memodifikasi *repository* dan mengirimkan draf file pada waktu yang berdekatan, kami sempat menghadapi beberapa *commit* yang tumpang tindih serta keraguan dalam melakukan *merge* dari berbagai *branch* (seperti `patch-1`, `patch-2`, dst.) ke *main branch*. Kami mengatasinya dengan berdiskusi secara komunal dan memusatkan eksekusi *Pull Request* (PR) di bawah pengawasan satu anggota agar tidak merusak *file tracker* dari GitHub Classroom. Selain itu, secara konseptual, kami sempat bingung memahami mengapa *loss* pada Sigmoid menurun sangat lambat, sebelum akhirnya kami mendalami literatur mengenai *vanishing gradient*.

Pelajaran berharga yang dapat kami bawa ke proyek *Machine Learning* selanjutnya adalah pentingnya ketelitian dalam memilih fungsi aktivasi; terbukti bahwa ReLU dan Tanh secara umum jauh lebih efisien untuk arsitektur modern dibandingkan Sigmoid standar. Kami juga menyadari bahwa komunikasi mengenai manajemen *branch* Git di awal proyek adalah keharusan, agar tidak ada waktu yang terbuang sia-sia hanya untuk mengurus konflik kode.

---

## 4. Kontribusi Tiap Anggota

| Anggota | Kontribusi Konkret | % Effort |
| --- | --- | --- |
| Andreanus Joe Reynald | Setup kode awal, *training* & *tuning* Varian 01 (Single Layer) | 14% |
| Randy Frederick Leonardy | *Training*, *tuning*, & *export history* Varian 02 (MLP Sigmoid) | 14% |
| Stevan Wiyandi | *Training*, *tuning*, & *export history* Varian 03 (MLP Tanh) | 14% |
| Steven Wiyandi | Perancangan arsitektur, *training* & *export history* Varian 04 (MLP ReLU) | 14% |
| Dustin Darmawan Isya Widjaja | Menggabungkan data CSV, membuat *plotting* grafik komparasi *Loss* & *Accuracy* | 14% |
| Dantacitto Jonatan | Menulis draf analisis diskusi & menyusun bagian refleksi kelompok | 15% |
| Wendy Tjung | Koordinator teknis, *review* akhir, eksekusi *Pull Request* & perapian sintaks Markdown | 15% |

---

## 5. Referensi

* Slide Perkuliahan *Deep Learning* (Bagian Arsitektur ANN, Fungsi Aktivasi, dan Propagasi Balik).
* Dokumentasi resmi framework (Scikit-Learn/PyTorch/TensorFlow).
* Artikel *"Vanishing Gradient Problem"* untuk pemahaman perbandingan ReLU dan Sigmoid.