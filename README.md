# Laporan Proyek Akhir Membuat Model Sistem Rekomendasi - Aswin Setiawan

## Domain Proyek

**Latar Belakang:** Restoran menjadi salah satu sektor industri yang terus berkembang seiring dengan meningkatnya gaya hidup konsumtif masyarakat. Dengan banyaknya pilihan restoran yang tersedia, konsumen sering kali mengalami kesulitan dalam menentukan tempat makan yang sesuai dengan preferensi mereka. Oleh karena itu, sistem rekomendasi restoran berbasis machine learning menjadi solusi yang dapat membantu pengguna dalam memilih restoran yang paling sesuai dengan selera mereka.

**Mengapa dan Bagaimana Masalah Harus Diselesaikan:** Sistem rekomendasi dapat meningkatkan pengalaman pengguna dengan memberikan saran restoran berdasarkan ulasan, rating, dan preferensi individu. Dengan pendekatan berbasis machine learning, sistem ini dapat secara otomatis menyesuaikan rekomendasi seiring dengan perubahan preferensi pengguna, sehingga meningkatkan kepuasan pelanggan dan potensi peningkatan pendapatan bagi restoran yang direkomendasikan.

**Referensi**:

[Restaurant Recommendation System Based on User Ratings with Collaborative Filtering](https://www.researchgate.net/publication/350084914_Restaurant_Recommendation_System_Based_on_User_Ratings_with_Collaborative_Filtering)

## Business Understanding

### Problem Statements

Menjelaskan pernyataan masalah latar belakang:
- Bagaimana cara merekomendasikan restoran yang relevan kepada pengguna berdasarkan preferensi mereka?
- Bagaimana model dapat secara akurat dalam memberikan rekomendasi?
  
### Goals

Menjelaskan tujuan dari pernyataan masalah:
- Membangun sistem rekomendasi restoran yang dapat memberikan rekomendasi yang personal dan akurat kepada pengguna.
- Mengevaluasi model dan membandingkan performa model dengan metode yang berbeda.
  
### Solution statements
Dalam proyek ini, digunakan dua metode pendekatan:
- Collaborative Filtering: Menggunakan interaksi pengguna untuk memberikan rekomendasi.
- Content-Based Filtering: Menggunakan informasi restoran untuk menentukan rekomendasi.

## Data Understanding
Sumber Data: Dataset yang digunakan berasal dari UCI Machine Learning Repository dengan nama "Restaurant & Consumer Data". Data terdiri dari informasi tentang restoran, pengguna, dan rating yang diberikan pengguna terhadap restoran. Dataset dapat diakses [disini](https://archive.ics.uci.edu/dataset/232/restaurant+consumer+data)

### Data Preview

![Data Review](https://github.com/dream2hvn/Resto-Recommendation-System/blob/main/Review%20Data)

### Feature On Modelling Process:
- userID: ID unik yang diberikan kepada setiap pengguna dalam sistem. Digunakan untuk mengidentifikasi dan melacak preferensi pengguna.

- placeID: ID unik yang diberikan kepada setiap restoran dalam sistem. Digunakan untuk mengidentifikasi dan menghubungkan restoran dengan rating pengguna.

- rating: Rating keseluruhan yang diberikan oleh pengguna untuk sebuah restoran, dalam skala 1 hingga 2. Merupakan indikator kepuasan pengguna secara umum.

- food_rating: Rating khusus untuk makanan yang disajikan di restoran, dalam skala 1 hingga 2. Mencerminkan kualitas dan rasa makanan.

- service_rating: Rating khusus untuk layanan yang diberikan di restoran, dalam skala 1 hingga 2. Mencerminkan kualitas pelayanan dan pengalaman pelanggan.

- name: Nama restoran. Digunakan untuk mengidentifikasi dan menampilkan restoran kepada pengguna dalam hasil rekomendasi.

- Rcuisine: Jenis masakan yang ditawarkan oleh restoran (misalnya, Italia, Jepang, dll.). Digunakan untuk mengelompokkan restoran berdasarkan kesamaan konten dan merekomendasikan restoran serupa kepada pengguna.

### Data Preprocessing/ Checking

- Missing Value: Terdapat banyak missing value pada sebagian besar fitur. Hanya fitur userID, placeID, rating, food_rating, dan service_rating saja yang memiliki 0 missing value. Fitur-fitur lain, seperti Rcuisine, memiliki missing value dalam jumlah yang signifikan.
- Data Duplikat: Terdapat data duplikat pada beberapa fitur, seperti placeID. Data duplikat ini perlu dihapus untuk menghindari bias dalam data.
- Inkonsistensi Data: Terdapat inkonsistensi data pada beberapa fitur, seperti Rcuisine. Misalnya, restoran KFC memiliki dua kategori masakan yang berbeda, yaitu Game dan American. Inkonsistensi data ini perlu diperbaiki agar data menjadi lebih konsisten.

## Data Preparation

1. Data Preparation
- Handling Missing Value
  
Penjelasan:
Data yang telah diintegrasi, mungkin saja terdapat missing value. Missing value dapat disebabkan oleh berbagai faktor, seperti kesalahan input data atau data yang tidak tersedia. Pada tahap ini, missing value akan diatasi dengan cara:
  1.	Melakukan pengecekan missing value dengan fungsi isnull().
  2.	Membersihkan missing value dengan fungsi dropna().
  3.	Melihat kembali missing value pada variabel all_resto_clean.
- Handling Duplicates
  
Penjelasan:
Data yang telah diintegrasi, mungkin saja terdapat data duplikat. Data duplikat dapat menyebabkan bias dalam data. Pada tahap ini, data duplikat diatasi dengan cara:
  1.	Mengurutkan data restoran berdasarkan 'placeID' untuk memudahkan identifikasi duplikat.
  2.	Membuang data duplikat pada kolom 'placeID' dengan fungsi drop_duplicates(). Hal ini dilakukan karena dalam sistem rekomendasi, satu restoran idealnya memiliki satu kategori masakan.
- Handling Outliers

Penjelasan:
Outliers merupakan data yang memiliki nilai yang jauh berbeda dari data lainnya. Outliers dapat memengaruhi hasil pemodelan. Pada proyek ini, outliers tidak diatasi karena data yang digunakan tidak terlalu banyak dan fokus utamanya adalah pada kategori masakan dan rating. Penanganan outlier bisa dipertimbangkan jika dataset lebih besar dan kompleks.

2. Content Based Filtering Preparation
- Ekstraksi Fitur TF-IDF

Penjelasan:
TF-IDF digunakan untuk mengidentifikasi representasi fitur penting dari setiap kategori masakan. Proses ini mengubah teks kategori masakan menjadi representasi numerik yang dapat dipahami oleh model machine learning.
  1.	Fungsi tfidfvectorizer() dari library sklearn digunakan untuk menghitung nilai TF-IDF.
  2.	Data 'cuisine' digunakan sebagai input untuk TF-IDF Vectorizer.
  3.	Matrix TF-IDF yang dihasilkan digunakan untuk menghitung cosine similarity antar restoran berdasarkan kategori masakan mereka.
- Data Cleaning
  
Penjelasan:
  1.	Menyamakan kategori masakan, misal kategori 'Game' pada restoran KFC diubah menjadi 'American' agar konsisten.
  2.	Membuang data duplikat pada kolom 'PlaceID' agar setiap restoran hanya memiliki satu representasi dalam data.
- Membuat Dictianory resto_new

Penjelasan:
  1.	Data 'resto_id', 'resto_name', dan 'cuisine' dikonversi menjadi list.
  2.	List tersebut digunakan untuk membuat dictionary resto_new dengan 'id', 'resto_name', dan 'cuisine' sebagai key.
  3.	Dictionary resto_new digunakan sebagai input untuk pemodelan content-based filtering.

  
## Modeling
### Model 1: Content-Based Filtering
Content-Based Filtering digunakan untuk merekomendasikan restoran berdasarkan kesamaan fitur dengan preferensi pengguna, seperti jenis masakan. Metode ini memanfaatkan representasi fitur dari setiap restoran dan preferensi pengguna untuk menghitung kesamaan dan menghasilkan rekomendasi. Dalam proyek ini, Content-Based Filtering dipilih karena kesederhanaannya dan kemampuannya untuk merekomendasikan item baru yang belum pernah dinilai pengguna.


Langkah-langkah Content-Based Filtering:


1. TF-IDF Vectorizer:
Digunakan untuk mengidentifikasi representasi fitur penting dari setiap kategori masakan.
Mengubah data teks (kategori masakan) menjadi representasi numerik yang dapat diproses oleh model.

2. Cosine Similarity:
Digunakan untuk menghitung derajat kesamaan (similarity degree) antar restoran berdasarkan representasi fitur mereka.
Menghasilkan matriks kesamaan yang menunjukkan seberapa mirip setiap restoran dengan restoran lainnya.

3. Rekomendasi:
Berdasarkan matriks kesamaan, sistem merekomendasikan restoran yang paling mirip dengan preferensi pengguna. Restoran dengan kesamaan tertinggi akan direkomendasikan terlebih dahulu.


### Model 2: Collaborative Filtering
Collaborative Filtering digunakan untuk merekomendasikan restoran berdasarkan rating pengguna lain yang memiliki preferensi serupa. Metode ini memanfaatkan data rating pengguna untuk mengidentifikasi pola preferensi dan menghasilkan rekomendasi. Dalam proyek ini, Collaborative Filtering dipilih karena kemampuannya untuk memberikan rekomendasi yang personal dan akurat.


Langkah-langkah Collaborative Filtering:


1. Data Encoding:
Mengubah userID dan placeID menjadi indeks integer menggunakan teknik encoding.
Memudahkan pemrosesan data oleh model machine learning.

2. Normalisasi Data:
Menskalakan nilai rating ke dalam rentang 0 hingga 1 menggunakan min-max scaling.
Meningkatkan kinerja model dan mencegah bias akibat perbedaan skala antar fitur.

3. Model Training:
Menggunakan model RecommenderNet yang dibangun dengan Keras.
Model dilatih menggunakan data rating pengguna untuk mempelajari pola preferensi.

4. Rekomendasi:
Berdasarkan pola preferensi yang dipelajari, model memprediksi rating pengguna terhadap restoran yang belum pernah dikunjungi.Restoran dengan prediksi rating tertinggi akan direkomendasikan.

  
**Kelebihan dan Kekurangan:**


Model 1: Content-Based Filtering
- Kelebihan: Efektif untuk merekomendasikan restoran baru berdasarkan preferensi saat ini, memberikan rekomendasi relevan dengan cepat.
- Kekurangan: Kurang beragam, rentan terhadap cold start problem untuk item baru, terbatas pada item dengan fitur mirip.

Model 2: Collaborative Filtering

- Kelebihan: Rekomendasi personal dan akurat, mengatasi cold start problem untuk pengguna baru, mampu merekomendasikan item populer.
- Kekurangan: Membutuhkan data rating yang banyak, sulit untuk merekomendasikan item baru (cold start problem untuk item), rentan terhadap bias popularitas.


## Evaluation


### Evaluasi:
1. Metode Evaluasi Content-Based Filtering dilakukan secara kualitatif:
- Melalui pengamatan hasil rekomendasi: Dengan memberikan input nama restoran, sistem akan memberikan rekomendasi restoran lain yang memiliki kesamaan fitur (jenis masakan).
- Membandingkan rekomendasi dengan preferensi pengguna: Pengguna dapat menilai relevansi rekomendasi yang dihasilkan berdasarkan pengetahuan mereka tentang preferensi pengguna yang diwakili oleh restoran input.

2. Metode Evaluasi Content-Based Filtering dilakukan menggunakan metrik RMSE:
- RMSE mengukur rata-rata perbedaan antara rating yang diprediksi oleh model dan rating aktual yang diberikan oleh pengguna. Semakin rendah nilai RMSE, semakin akurat model dalam memprediksi rating.
- Rumus RMSE = akar kuadrat dari [(Σ(prediksi - aktual)^2) / n]

## Visualisasi Prediksi

### Content Based Filtering

![Recommendation Content Based Filtering](https://github.com/dream2hvn/Resto-Recommendation-System/blob/main/Recommendation%20Content%20Based%20Filtering)

### Collaborative Filtering

![RMSE Collaborative Filtering](https://github.com/dream2hvn/Resto-Recommendation-System/blob/main/RMSE)


### Hasil Evaluasi:


### 1. Content-Based Filtering

![Hasil Content-Based Filtering](https://github.com/dream2hvn/Resto-Recommendation-System/blob/main/Hasil%20Content-Based%20Filtering)

### 2. Collaborative Filtering

| Dataset | RMSE |
|---|---|
| Training | 0.23 |
| Validasi | 0.34 |


Setelah melakukan evaluasi performa kedua model menggunakan metrik Precision dan RMSE, didapatkan hasil yaitu Model Content-Based Filtering menghasilkan rekomendasi yang relevan dengan preferensi pengguna berdasarkan kategori masakan. Model Collaborative Filtering menghasilkan RMSE sebesar 0.23 pada data training dan 0.34 pada data validasi. Dari hasil ini, Collaborative Filtering lebih unggul dalam hal akurasi prediksi rating berdasarkan nilai RMSE yang relatif rendah, sedangkan Content-Based Filtering lebih unggul dalam hal memberikan rekomendasi yang relevan berdasarkan kesamaan fitur.


**Kesimpulan:** 


Meskipun Content-Based Filtering dirancang untuk memberikan rekomendasi yang relevan, pada dataset ini Collaborative Filtering memberikan hasil yang lebih akurat dalam memprediksi rating restoran jika ditinjau dari RMSE. Namun, Content-Based Filtering menunjukkan performa yang lebih baik dalam memberikan rekomendasi yang relevan dengan preferensi pengguna jika ditinjau dari kesamaan fitur. Oleh karena itu, pemilihan model terbaik bergantung pada prioritas Anda, apakah ingin meminimalkan rata-rata kesalahan (RMSE) atau memaksimalkan relevansi rekomendasi (kesamaan fitur). Jika ingin meminimalkan rata-rata kesalahan, Collaborative Filtering adalah pilihan yang lebih baik. Jika ingin memaksimalkan relevansi rekomendasi, Content-Based Filtering adalah pilihan yang lebih baik.
