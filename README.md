# Proyek Akhir: Menyelesaikan Permasalahan Perusahaan Edutech

## Business Understanding
Proyek ini bertujuan membantu meningkatkan performa akademik siswa serta mengurangi angka putus kuliah (dropout). Dengan berkembangnya teknologi dan analisis data, proyrk ini ingin menggunakan data terkait mahasiswa yang mereka peroleh dari berbagai sumber (demografi, jalur akademik, dan faktor sosial ekonomi) untuk membangun model yang dapat memprediksi keberhasilan akademik dan dropout mahasiswa sejak awal perkuliahan. Dengan prediksi ini, perusahaan dapat membantu institusi pendidikan menawarkan solusi yang tepat sasaran kepada mahasiswa.

### Permasalahan Bisnis
1. Jumlah siswa yang dropout tinggi mengindikasikan ada suatu hal negatif memengaruhinya.
2. Tidak adanya sistem prediksi dini untuk mengidentifikasi siswa yang berpotensi dropout.
3. Institusi ingin memahami faktor-faktor penyebab dropout untuk menargetkan intervensi yang tepat.

### Cakupan Proyek
- **Pengumpulan Data:** Mengumpulkan data dari berbagai sumber yang berisi informasi terkait mahasiswa, termasuk jalur akademik, demografi, sosial ekonomi, dan performa akademik.
- **Data Understanding:** Melakukan eksplorasi data untuk memahami pola, tren, dan hubungan antar fitur dalam dataset, serta mengidentifikasi variabel-variabel yang berpotensi mempengaruhi keberhasilan akademik dan risiko dropout.
- Data Preparation: Melakukan pembersihan data, penanganan missing values, transformasi fitur, dan encoding untuk memastikan data siap digunakan dalam pengembangan model machine learning.
- **Pengembangan Model:** Membangun model machine learning menggunakan teknik yang sesuai untuk memprediksi risiko dropout dan keberhasilan akademik mahasiswa, serta melakukan tuning hyperparameter untuk meningkatkan performa model.
- **Evaluasi:** Mengukur kinerja model yang dikembangkan menggunakan metrik evaluasi yang relevan (seperti akurasi, presisi, dan recall), dan melakukan analisis lebih lanjut untuk memastikan model memenuhi kebutuhan bisnis dan akurasi yang diharapkan.

### Persiapan

Sumber data: Sumber data: Data berasal dari informasi masing-masing pegawai yang dikumpulkan oleh Jaya Maju Institut. [dataset](https://github.com/dicodingacademy/dicoding_dataset/tree/main/students_performance)

Setup environment:
```bash
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.preprocessing import LabelEncoder
from sklearn.model_selection import train_test_split

#model machine learning
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC
from xgboost import XGBClassifier

# evaluasi model
from sklearn.metrics import confusion_matrix, recall_score, f1_score, accuracy_score, precision_score

# deploy
from joblib import dump, load
```


## Business Dashboard
Business dashboard yang dibuat menggunakan Looker Studio. Dashboard ini memberikan informasi yang jelas mengenai performa akademik siswa dan indikator dropout. Salah satu faktor utama yang ditemukan adalah rasio performa akademik pada semester 1 dan 2. Siswa dengan rasio rendah, dihitung dari jumlah unit kurikulum yang diambil (enrolled) dibandingkan dengan yang lulus (approved), cenderung memiliki risiko dropout yang lebih tinggi.
![image](./images/oktaagnes_dashboard.png)
gambar 1 dashboard

## Menjalankan Sistem Machine Learning
#### Menjalankan Prototype Sistem Machine Learning 

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


## Conclusion
Analisis yang dilakukan menunjukkan bahwa siswa dengan performa akademik rendah di awal masa studi berpotensi tinggi untuk dropout. Dengan menggunakan model prediksi yang dibangun berdasarkan data akademik dan faktor lainnya, pihak Jaya Jaya Institut dapat memberikan intervensi dini kepada siswa yang berisiko. Dashboard yang dibuat memungkinkan pihak institusi untuk memonitor performa siswa secara real-time dan mengambil tindakan yang tepat sesuai data yang disajikan.

Laporan ini memberikan dasar untuk perbaikan strategi pembelajaran dan dukungan akademik di masa depan.

### Rekomendasi Action Items
Berdasarkan hasil analisis dan modeling, berikut adalah beberapa rekomendasi action items yang dapat dilakukan oleh Jaya Jaya Institut untuk mengurangi angka dropout dan meningkatkan performa akademik siswa:

- Program Bimbingan Akademik Khusus Mengimplementasikan program bimbingan akademik khusus bagi siswa yang teridentifikasi memiliki rasio performa akademik rendah di semester awal. Program ini dapat mencakup mentoring, bantuan pengajaran tambahan, dan pemantauan yang lebih dekat oleh tutor.
- Link app.py: https://s6kk3tckfbfcj2u6rofppd.streamlit.app/
