# Resto-Recommendation-System

**Latar Belakang:** Restoran menjadi salah satu sektor industri yang terus berkembang seiring dengan meningkatnya gaya hidup konsumtif masyarakat. Dengan banyaknya pilihan restoran yang tersedia, konsumen sering kali mengalami kesulitan dalam menentukan tempat makan yang sesuai dengan preferensi mereka. Oleh karena itu, sistem rekomendasi restoran berbasis machine learning menjadi solusi yang dapat membantu pengguna dalam memilih restoran yang paling sesuai dengan selera mereka.

**Mengapa dan Bagaimana Masalah Harus Diselesaikan:** Sistem rekomendasi dapat meningkatkan pengalaman pengguna dengan memberikan saran restoran berdasarkan ulasan, rating, dan preferensi individu. Dengan pendekatan berbasis machine learning, sistem ini dapat secara otomatis menyesuaikan rekomendasi seiring dengan perubahan preferensi pengguna, sehingga meningkatkan kepuasan pelanggan dan potensi peningkatan pendapatan bagi restoran yang direkomendasikan.

**Referensi**:
- [Restaurant & consumer data](https://archive.ics.uci.edu/dataset/232/restaurant+consumer+data) 

- [Restaurant Recommendation System Based on User Ratings with Collaborative Filtering](https://www.researchgate.net/publication/350084914_Restaurant_Recommendation_System_Based_on_User_Ratings_with_Collaborative_Filtering)

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
Sumber Data: Dataset yang digunakan berasal dari UCI Machine Learning Repository dengan nama "Restaurant & Consumer Data". Data terdiri dari informasi tentang restoran, pengguna, dan rating yang diberikan pengguna terhadap restoran.

![Data Review](https://github.com/dream2hvn/Resto-Recommendation-System/blob/main/Review%20Data)

### Variabel dalam dataset:
- userID: ID unik untuk setiap pengguna.
- placeID: ID unik untuk setiap restoran.
- rating: Rating keseluruhan yang diberikan pengguna terhadap restoran (skala 1-3).
- food_rating: Rating makanan yang diberikan pengguna terhadap restoran (skala 1-3).
- service_rating: Rating layanan yang diberikan pengguna terhadap restoran (skala 1-3).
- user: ID pengguna yang di-encode ke dalam indeks integer.
- resto: ID restoran yang di-encode ke dalam indeks integer.

## Data Preparation

- Menggabungkan Data Restoran: Untuk membentuk dataset restoran yang komprehensif agar sistem dapat mempertimbangkan berbagai fitur restoran.
- Menggabungkan Data Pengguna: Untuk membentuk dataset pengguna yang komprehensif agar sistem dapat memahami preferensi dan karakteristik pengguna.
- Menggabungkan Data Rating: Untuk menghubungkan rating pengguna dengan restoran dan pengguna yang sesuai, sehingga sistem dapat mempelajari pola preferensi.
- Membersihkan Data:
  Missing Values: Untuk mencegah kesalahan dalam pemodelan dan mengurangi akurasi rekomendasi.
  Data Duplikat: Untuk mencegah bias dalam pemodelan dan menghasilkan rekomendasi yang akurat.
  Penyamaan Jenis Masakan: Untuk memastikan konsistensi data dan mencegah sistem menganggap       jenis masakan yang sama sebagai entitas yang berbeda.
- Encoding Data: Untuk mengubah data kategorikal menjadi numerik agar dapat diproses oleh model   machine learning (khusus Collaborative Filtering).
- Normalisasi Data: Untuk meningkatkan kinerja model dan mencegah bias akibat perbedaan skala     antar fitur (khusus Collaborative Filtering).
  
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
- Berdasarkan matriks kesamaan, sistem merekomendasikan restoran yang paling mirip dengan preferensi pengguna.
- Restoran dengan kesamaan tertinggi akan direkomendasikan terlebih dahulu.

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
- Berdasarkan pola preferensi yang dipelajari, model memprediksi rating pengguna terhadap restoran yang belum pernah dikunjungi.
- Restoran dengan prediksi rating tertinggi akan direkomendasikan.
  
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

| Restoran Input | Rekomendasi | Relevansi | 
|---|---|---|
| KFC | Wendy's, McDonald's, Burger King, Sanborns Cafe, Vips | Relevan (Kategori Masakan: American) |
| El Rincon de San Francisco | Restaurant Los Equipales, Gorditas Doña Tota, Gorditas Doa Gloria,  Mariscos Tia Licha,  Paniroles | Relevan (Kategori Masakan: Mexican) |
| ... | ... | ... |


### 2. Collaborative Filtering

| Dataset | RMSE |
|---|---|
| Training | 0.23 |
| Validasi | 0.34 |

Setelah melakukan evaluasi performa kedua model menggunakan metrik Precision dan RMSE, didapatkan hasil yaitu Model Content-Based Filtering menghasilkan rekomendasi yang relevan dengan preferensi pengguna berdasarkan kategori masakan. Model Collaborative Filtering menghasilkan RMSE sebesar 0.23 pada data training dan 0.34 pada data validasi. Dari hasil ini, Collaborative Filtering lebih unggul dalam hal akurasi prediksi rating berdasarkan nilai RMSE yang relatif rendah, sedangkan Content-Based Filtering lebih unggul dalam hal memberikan rekomendasi yang relevan berdasarkan kesamaan fitur.

**Kesimpulan:** 

Meskipun Content-Based Filtering dirancang untuk memberikan rekomendasi yang relevan, pada dataset ini Collaborative Filtering memberikan hasil yang lebih akurat dalam memprediksi rating restoran jika ditinjau dari RMSE. Namun, Content-Based Filtering menunjukkan performa yang lebih baik dalam memberikan rekomendasi yang relevan dengan preferensi pengguna jika ditinjau dari kesamaan fitur. Oleh karena itu, pemilihan model terbaik bergantung pada prioritas Anda, apakah ingin meminimalkan rata-rata kesalahan (RMSE) atau memaksimalkan relevansi rekomendasi (kesamaan fitur). Jika ingin meminimalkan rata-rata kesalahan, Collaborative Filtering adalah pilihan yang lebih baik. Jika ingin memaksimalkan relevansi rekomendasi, Content-Based Filtering adalah pilihan yang lebih baik.
