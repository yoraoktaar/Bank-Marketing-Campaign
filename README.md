# Bank-Marketing-Campaign 🏦💱💳🏧

## Pemahaman Bisnis

<span style="font-size:20px; font-family: monospace;">`1. Latar Belakang`</span>

Industri perbankan saat ini semakin kompetitif dalam menawarkan produk keuangan, salah satunya adalah deposito berjangka. Deposito berjangka merupakan produk investasi dimana nasabah menyimpan dana untuk jangka waktu tertentu dengan imbalan bunga tetap. Untuk memenangkan persaingan dan menarik nasabah baru, bank perlu melakukan kampanye pemasaran yang efektif dan efisien.

<span style="font-size:20px; font-family: monospace;">`2. Masalah Bisnis`</span>

Bank menghadapi tantangan dalam mengoptimalkan kampanye pemasaran deposito berjangka. Pendekatan pemasaran tradisional yang menghubungi semua nasabah secara acak menghasilkan tingkat konversi yang rendah dan pemborosan sumber daya. Bank membutuhkan cara untuk mengidentifikasi nasabah yang paling berpotensi tertarik dengan produk deposito berjangka agar dapat memfokuskan upaya pemasaran dengan lebih tepat sasaran.

<span style="font-size:20px; font-family: monospace;">`3. Tujuan`</span>

Tujuan utama adalah membangun model prediktif yang dapat mengidentifikasi nasabah yang berpotensi tinggi untuk melakukan deposito berjangka. Dengan model ini, tim marketing dapat meningkatkan efektivitas kampanye, mengurangi biaya operasional, dan meningkatkan tingkat konversi. Selain itu, model ini juga diharapkan dapat meningkatkan kepuasan nasabah dengan mengurangi gangguan dari penawaran yang tidak relevan.

<span style="font-size:20px; font-family: monospace;">`4. Pendekatan Analitik`</span>

Untuk mencapai tujuan tersebut, pendekatan analitik yang digunakan meliputi:

- Analisis data historis dari kampanye pemasaran sebelumnya.
- Identifikasi pola karakteristik nasabah yang menerima atau menolak penawaran deposito.
- Objektif: Pembangunan model klasifikasi machine learning untuk memprediksi kemungkinan nasabah akan menerima penawaran sebelum dilakukan kontak

<span style="font-size:20px; font-family: monospace;">`2. Evaluasi Metrik`</span>

Karena tujuan utama adalah mengidentifikasi nasabah yang akan melakukan deposito berjangka, maka target adalah sebagai berikut:
- 0 (no) untuk tidak deposit 
- 1 (yes) untuk deposit
  
## Data Understanding

#### Profil Nasabah

| **Kolom**   | **Deskripsi**                                       |
| ----------- | --------------------------------------------------- |
| **age**     | Usia nasabah                                        |
| **job**     | Jenis pekerjaan nasabah                             |
| **balance** | Saldo nasabah                                       |
| **housing** | Status kepemilikan kredit ruma                h     |
| **loan**    | Status kepemilikan pinjaman p                ribadi |

#### Data Pemasaran

| **Kolom**    | **Deskripsi**                                                          |
| ------------ | ---------------------------------------------------------------------- |
| **contact**  | Jenis komunikasi yang digunakan untuk menghubungi nasabah              |
| **month**    | Bulan terakhir nasabah dihubungi dalam tahun tersebut                  |
| **campaign** | Jumlah kontak yang dilakukan selama kampanye ini terhadap nasabah      |
| **pdays**    | Jumlah hari sejak nasabah terakhir dihubungi dalam kampanye sebelumnya |
| **poutcome** | Hasil dari kampanye pemasaran sebelumnya                               |
| **deposit**  | Apakah nasabah melakukan deposito atau tidak (target: ya atau tidak)   |

## Business Understanding

<span style="font-size:20px; font-family: monospace;">`Counclusion`</span>

**Confusion Matrix**

Nasabah:

|                     | **Pred: No Deposit**     | **Pred: Deposit**        |
| ------------------- | ------------------------ | ------------------------ |
| **Actual: No**      | **553** (True Negative)  | **263** (False Positive) |
| **Actual: Deposit** | **180** (False Negative) | **567** (True Positive)  |


Presentase:

|                 | Pred: No Deposit | Pred: Deposit |
| --------------- | ---------------- | ------------- |
| **Actual: No**  | **68%** (TN)     | **32%** (FP)  |
| **Actual: Yes** | **24%** (FN)     | **76%** (TP)  |

Insight:

1. Threshold 0.4 membuat model lebih agresif dalam memprediksi deposit, meningkatkan recall namun menambah false positive. Cocok untuk menghindari kehilangan nasabah potensial.

2. Recall 76%: Model berhasil menangkap 76% nasabah (567 nasabah) yang benar-benar berminat — sangat penting untuk memaksimalkan potensi konversi. 

3. Precision 68%: Dari seluruh prediksi nasabah yang akan deposit, 68% benar-benar tertarik. Artinya, 32% kampanye bisa kurang tepat sasaran.

4. False Positive 32%: 263 nasabah diprediksi akan deposit, tetapi sebenarnya tidak. Bisa menyebabkan pemborosan biaya telemarketing.

5. False Negative 24%: Artinya, terdapat 24% nasabah (180 nasabah) yang sebenarnya berminat tapi tidak terdeteksi → potensi kehilangan revenue.

**Simulasi Sebelum dan Sesudah ada model:**

Final Model: LightGBM (Threshold = 0.4)
Precision = 68%
Recall = 76%

Asumsi:
- Total = 100 nasabah
- 20 orang benar-benar akan deposit
- Biaya menghubungi 1 nasabah = Rp10.000
- Keuntungan dari 1 nasabah yang deposit = Rp100.000

Sebelum Pakai Model (Tanpa Prediksi):
- Hubungi semua nasabah (100 nasabah)
- Biaya: 100 × Rp10.000 = Rp1.000.000
- Revenue: 20 × Rp100.000 = Rp2.000.000
- Profit = Rp2.000.000 – Rp1.000.000 = Rp1.000.000

Setelah Pakai Model LightGBM (Threshold = 0.4):
- Prediksi: Precision 68%, Recall 76%
- Nasabah diprediksi tertarik = 20 / 0.68 = 30 orang
- True Positives = 20 × 0.76 = 15 nasabah
- Biaya: 20/0.68 × Rp10.000 = Rp300.000
- Revenue: 0.76*20 × Rp100.000 = Rp1.500.000
- Profit = Rp1.500.000 – Rp300.000 = Rp1.200.000


Tabel Perbandingan Sebelum vs Sesudah Pakai Model
|                          | Sebelum Model   | Sesudah LightGBM |
| ------------------------ | --------------- | ---------------- |
| Nasabah Dihubungi        | 100 orang       | 30 orang         |
| Biaya Telemarketing      | Rp1.000.000     | Rp300.000        |
| Nasabah Terkonversi (TP) | 20 orang        | 15 orang         |
| Total Revenue            | Rp2.000.000     | Rp1.500.000      |
| **Profit Bersih**        | **Rp1.000.000** | **Rp1.200.000**  |*  |


<span style="font-size:20px; font-family: monospace;">`Recomendation`</span>
#### Rekomendasi Strategis Kampanye Deposito

##### 1. Berdasarkan Kelompok Usia
- **Elder (60–70 tahun)**: 82.9% melakukan deposit (yes: 82.9%)
- **Super_Senior (>70 tahun)**: 77.4% deposit
- **Very_Young (<25 tahun)**: 68.4% deposit
- Fokuskan kampanye pada kelompok ini karena tingkat konversinya 1.5–2x lebih tinggi dibandingkan kelompok Adult (41.7%)

##### 2. Berdasarkan Kategori Saldo (balance)
- **High balance**: 59.2% melakukan deposit
- **Medium balance**: 56%
- **Low balance**: 45.1%
- **Negative balance**: hanya 33.9% yang deposit
- Fokuskan ke nasabah dengan saldo medium–high untuk efisiensi kampanye

##### 3. Riwayat Kontak Sebelumnya (previously_contacted)
- **Sudah pernah dihubungi (1)**: 68.1% melakukan deposit
- **Belum pernah dihubungi (0)**: hanya 40.8%
- Retarget nasabah yang sudah pernah dihubungi → konversi lebih tinggi 1.67x

##### 4. Frekuensi Kontak (campaign)
- Kontak efektif berada di **3–6 kali** (non-outlier)
- Outlier: kontak >6 kali (5.49% data, hingga 63 kali)
- Rekomendasi: **maksimal 10 kali kontak** (cap ekstrem) → untuk efisiensi

##### 5. Berdasarkan Pekerjaan (job)
- **Student (247 orang)** dan **Retired (540 orang)** → konversi 2–4x lipat lebih tinggi dibandingkan pekerjaan lain
- Segmentasi lanjut: 
  - Student usia sekolah atau setelah lulus SD (education: basic.6y)
  - Retired usia ≥60 tahun

##### 6. Jenis Kontak (contact)
- **Cellular (5628 data)** → channel paling efektif
- **Telephone (546 data)** dan **Unknown (1639 data)** → lebih rendah efektivitas
- Gunakan cellular dan pastikan nasabah mencantumkan nomor seluler saat registrasi

##### 7. Waktu Kampanye (month)
- Bulan efektif (volume tinggi): **May (1976), Aug (1085), Jul (1050), Jun (857)**
- Hindari bulan dengan data rendah dan kemungkinan lebih kecil: **Dec (68), Mar (199), Sep (212)**

##### 8. Hasil Kampanye Sebelumnya (poutcome)
- **Success (761 data)** → 91.5% dari mereka kembali melakukan deposit
- Gunakan ini sebagai sinyal kuat untuk **retargeting** kampanye berikutnya
untuk **retargeting** kampanye berikutnya

### Contoh Prediksi dengan Data Baru Nasabah

| Index | age | balance | campaign | pdays | job\_encoded | housing\_encoded | loan\_encoded | contact\_encoded | month\_encoded | poutcome\_encoded | has\_negative\_balance | campaign\_capped | high\_contact\_customer | previously\_contacted |
| ----- | --- | ------- | -------- | ----- | ------------ | ---------------- | ------------- | ---------------- | -------------- | ----------------- | ---------------------- | ---------------- | ----------------------- | --------------------- |
| 0     | 55  | 3662    | 2        | -1    | 1            | 0                | 0             | 1                | 5              | 0                 | 0                      | 2                | 0                       | 0                     |
| 1     | 39  | -3058   | 60       | -1    | 6            | 1                | 1             | 1                | 3              | 0                 | 1                      | 3                | 0                       | 0                     |

Probabilitas: [[0.59664646 0.40335354]
 [0.80745446 0.19254554]]
Predict class : [1 0]

Data Pelanggan 1: Kemungkinan besar akan membuka deposit (yes)
Data Pelanggan 2: Kemungkinan besar tidak akan membuka deposit (no)

### Terima kasih!
