# Proyek Akhir: Menyelesaikan Permasalahan Perusahaan Edutech

## Business Understanding

Jaya Jaya Institut adalah sebuah institusi pendidikan tinggi yang telah berdiri sejak tahun 2000 dan dikenal memiliki reputasi lulusan yang baik. Namun, masalah tingginya angka dropout menjadi tantangan besar bagi institusi tersebut, yang berpotensi mengurangi kepercayaan publik terhadap kualitas pendidikannya. Oleh karena itu, pihak institusi ingin mendeteksi siswa yang berpotensi dropout sedini mungkin, untuk memberikan bimbingan yang lebih personal guna meningkatkan peluang mereka menyelesaikan studi.

### Permasalahan Bisnis

1. Jumlah siswa yang dropout tinggi mengindikasikan ada suatu hal negatif memengaruhinya.
2. Tidak adanya sistem prediksi dini untuk mengidentifikasi siswa yang berpotensi dropout.
3. Institusi ingin memahami faktor-faktor penyebab dropout untuk menargetkan intervensi yang tepat.

### Cakupan Proyek

Proyek ini akan mencakup:

1. Mengidentifikasi variabel-variabel yang paling memengaruhi siswa yang dropout.
2. Menganalisis data yang dimiliki untuk memahami pola dan tren drop rate yang meningkat.
3. Membuat dashboard yang membantu manajemen dalam memahami informasi penting terkait drop rate.

### Persiapan

Sumber data: Sumber data: Data berasal dari informasi masing-masing pegawai yang dikumpulkan oleh Jaya Maju Institut.

Setup environment:
Penyiapan Environment di Google Cloud Platform (GCP) untuk Machine Learning

Untuk melakukan pengembangan model machine learning di Google Cloud, langkah-langkah berikut harus dilakukan:

#### 1. Membuat Akun Google Cloud dan Mengaktifkan API

- Langkah pertama adalah mendaftar dan membuat akun di [Google Cloud Console](https://console.cloud.google.com/). Setelah itu, buatlah proyek baru yang akan digunakan untuk pengembangan.
- Aktifkan API yang dibutuhkan, seperti **Vertex AI API** untuk keperluan model machine learning dan **Cloud Storage API** untuk penyimpanan data.

#### 2. Membuat Virtual Environment

- Google Cloud menyediakan layanan **Vertex AI Workbench**, yang memungkinkan pengguna untuk membuat environment pengembangan berbasis Jupyter Notebooks. Untuk membuat lingkungan ini, buka layanan **Vertex AI** di Google Cloud Console, pilih **Workbench**, lalu buat instance baru dengan memilih konfigurasi yang sesuai (seperti jenis mesin dan tipe runtime Python).
- Setelah workbench dibuat, pengguna dapat menginstal berbagai paket yang dibutuhkan menggunakan `pip` dalam notebook, seperti `numpy`, `pandas`, dan `scikit-learn` untuk analisis data dan pengembangan model.

#### 3. Penyimpanan Data dan Model Menggunakan Cloud Storage

- Data dan model yang digunakan dalam pengembangan dapat disimpan di **Cloud Storage**. Pengguna dapat membuat bucket baru untuk menyimpan file, seperti dataset atau model yang telah dilatih. Cloud Storage memungkinkan akses data secara cepat dan aman dari lingkungan pengembangan di GCP.
- Untuk mengakses file yang disimpan, dapat menggunakan pustaka **Google Cloud Storage** di Python, seperti berikut:

````python
from google.cloud import storage
client = storage.Client()
bucket = client.get_bucket('nama-bucket')
blob = bucket.blob('nama-file.pkl')
blob.download_to_filename('nama-file-lokal.pkl')


## Business Dashboard
Business dashboard yang dibuat menggunakan Looker Studio. Dashboard ini memberikan informasi yang jelas mengenai performa akademik siswa dan indikator dropout. Salah satu faktor utama yang ditemukan adalah rasio performa akademik pada semester 1 dan 2. Siswa dengan rasio rendah, dihitung dari jumlah unit kurikulum yang diambil (enrolled) dibandingkan dengan yang lulus (approved), cenderung memiliki risiko dropout yang lebih tinggi.

## Menjalankan Sistem Machine Learning
#### Menjalankan Prototype Sistem Machine Learning dengan K-Means Clustering

Prototipe sistem machine learning yang dibuat menggunakan algoritma K-Means clustering bertujuan untuk mengelompokkan data berdasarkan kesamaan fitur. Berikut adalah langkah-langkah yang dilakukan untuk menjalankan sistem ini.

### 1. Persiapan Data
Sebelum melakukan clustering, data yang digunakan perlu diproses dengan menghapus kolom target (seperti kolom "Status" atau "Attrition") agar hanya fitur yang digunakan dalam proses clustering.

```python
data_for_clustering = processed_df.drop(columns=['Status'], errors='ignore')

### 2. Menentukan Jumlah Cluster Optimal
Fungsi determine_optimal_clusters digunakan untuk menentukan jumlah cluster yang optimal menggunakan dua metode evaluasi:
- Elbow Method: Menghitung inertia (jumlah kuadrat jarak antara titik data dan centroid) untuk setiap jumlah cluster yang diuji. Titik di mana penurunan inertia mulai melambat menunjukkan jumlah cluster optimal.
- Silhouette Score: Mengukur kualitas clustering berdasarkan kedekatan data dengan cluster yang tepat dan seberapa jauh jaraknya ke cluster lainnya

#### 3. Melatih Model K-Means
Setelah menentukan jumlah cluster yang optimal, model K-Means diinisialisasi dengan jumlah cluster yang telah dipilih (misalnya 4 cluster). Model kemudian dilatih menggunakan data yang telah diproses.
```python
kmeans = KMeans(n_clusters=4, random_state=42)
cluster_labels = kmeans.fit_predict(data_for_clustering)

#### 4. Menambahkan Hasil Clustering ke DataFrame
Label cluster yang dihasilkan oleh K-Means kemudian ditambahkan ke dalam DataFrame untuk mempermudah analisis lebih lanjut

#### 5. Menampilkan Centroid dan Distribusi Cluster
Centroid dari setiap cluster (titik tengah setiap cluster) dan jumlah data dalam setiap cluster ditampilkan untuk memberikan wawasan lebih lanjut mengenai distribusi data dalam cluster.

#### 6. Visualisasi Cluster dengan PCA
Untuk mempermudah pemahaman tentang hasil clustering, visualisasi dilakukan dengan menggunakan PCA (Principal Component Analysis) untuk mereduksi data menjadi dua dimensi.

#### 7. Menghitung Silhouette Score
Silhouette Score dihitung untuk mengevaluasi seberapa baik clustering yang dilakukan. Skor ini mengukur seberapa baik data dalam setiap cluster dibandingkan dengan data di cluster lainnya.

#### 8. Distribusi Attrition di Setiap Cluster
Untuk menganalisis distribusi status "Attrition" (misalnya, apakah karyawan berhenti atau tidak) di setiap cluster, digunakan grafik countplot.

- Link ML: http://localhost:8503/

## Conclusion
Analisis yang dilakukan menunjukkan bahwa siswa dengan performa akademik rendah di awal masa studi berpotensi tinggi untuk dropout. Dengan menggunakan model prediksi yang dibangun berdasarkan data akademik dan faktor lainnya, pihak Jaya Jaya Institut dapat memberikan intervensi dini kepada siswa yang berisiko. Dashboard yang dibuat memungkinkan pihak institusi untuk memonitor performa siswa secara real-time dan mengambil tindakan yang tepat sesuai data yang disajikan.

Laporan ini memberikan dasar untuk perbaikan strategi pembelajaran dan dukungan akademik di masa depan.

### Rekomendasi Action Items
Berdasarkan hasil analisis dan modeling, berikut adalah beberapa rekomendasi action items yang dapat dilakukan oleh Jaya Jaya Institut untuk mengurangi angka dropout dan meningkatkan performa akademik siswa:

- Program Bimbingan Akademik Khusus Mengimplementasikan program bimbingan akademik khusus bagi siswa yang teridentifikasi memiliki rasio performa akademik rendah di semester awal. Program ini dapat mencakup mentoring, bantuan pengajaran tambahan, dan pemantauan yang lebih dekat oleh tutor.
````
