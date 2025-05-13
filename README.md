# Laporan Proyek Machine Learning - Nama Anda

## Project Overview
Seiring dengan meningkatnya popularitas anime secara global, pengguna platform seperti MyAnimeList (MAL) dihadapkan pada tantangan untuk memilih tontonan yang sesuai dari ribuan judul anime yang tersedia. Dengan begitu banyak pilihan, pengguna kerap merasa kewalahan, sehingga pengalaman eksplorasi menjadi kurang efisien dan tidak menyenangkan. Dalam konteks ini, sistem rekomendasi menjadi solusi yang sangat relevan dan dibutuhkan. Sistem ini bertujuan membantu pengguna menemukan anime yang sesuai dengan minat dan preferensi mereka tanpa harus melalui proses pencarian manual yang panjang. Proyek ini dirancang untuk membangun dua jenis sistem rekomendasi berbasis machine learning, yaitu content-based filtering dan collaborative filtering, dengan memanfaatkan data anime peringkat teratas di MyAnimeList.

Content-based filtering bekerja dengan menganalisis karakteristik dari konten anime itu sendiri, seperti genre, sinopsis, dan informasi lainnya, untuk merekomendasikan anime yang memiliki kemiripan dengan yang pernah ditonton atau disukai oleh pengguna. Di sisi lain, collaborative filtering memanfaatkan preferensi pengguna lain yang memiliki kesamaan minat, untuk merekomendasikan anime yang belum pernah dilihat tetapi kemungkinan besar akan disukai. Kedua pendekatan ini saling melengkapi dan telah terbukti efektif di berbagai platform hiburan digital seperti Netflix dan Spotify. Penelitian sebelumnya oleh [Ricci et al. (2015)](https://link.springer.com/book/10.1007/978-1-4899-7637-6) menyatakan bahwa sistem rekomendasi dapat meningkatkan kepuasan dan loyalitas pengguna secara signifikan.

Dengan mengembangkan sistem rekomendasi ini, proyek ini bertujuan tidak hanya untuk menyelesaikan masalah overload informasi, tetapi juga untuk meningkatkan kenyamanan, personalisasi, dan keterlibatan pengguna terhadap konten anime. Solusi ini menjadi semakin penting dalam lanskap hiburan digital saat ini yang kompetitif, di mana rekomendasi yang akurat dan relevan dapat menjadi faktor penentu utama dalam mempertahankan pengguna.

## Business Understanding

### Problem Statements
Berdasarkan latar belakang masalah yang sudah dijelaskan, kita dapat merumuskan pernyataan masalah sebagai berikut:
- Bagaimana memberikan rekomendasi anime yang serupa berdasarkan genre yang disukai pengguna?
- Bagaimana memberikan rekomendasi anime berdasarkan preferensi pengguna lain yang memiliki kesamaan selera?

### Goals
Berdasarkan pernyataan masalah tersebut, tujuan dari proyek ini adalah:
- Mengembangkan sistem rekomendasi yang merekomendasikan anime serupa dengan yang telah disukai pengguna.
- Mengembangkan sistem rekomendasi berdasarkan preferensi pengguna lain yang memiliki kesamaan selera.

### Solution statements
- Menggunakan sistem rekomendasi berbasis konten (content-based filtering) untuk merekomendasikan anime berdasarkan genre yang disukai pengguna
- Menggunakan sistem rekomendasi berbasis kolaboratif (collaborative filtering) untuk memberikan rekomendasi berdasarkan preferensi pengguna lain yang memiliki kesamaan selera.

## Data Understanding
Dataset yang digunakan pada proyek ini diambil dari platform kaggle dengan sumber sebagai berikut: [AnimeList Dataset](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database). Dataset ini terdiri dari 2 file csv, yaitu `anime.csv` yang berisi metadata dari anime dan `rating.csv` yang berisi penilaian yang diberikan pengguna terhadap anime. File **anime.csv** terdiri dari 12.294 baris dan 7 kolom. File **rating.csv** terdiri dari 7.813.737 baris dan 3 kolom.

### Variabel pada anime.csv
Informasi terkait anime.csv dapat dilihat pada gambar berikut:<br>
<img src="https://github.com/user-attachments/assets/3f758fa6-df5a-40d7-9be6-8a95e754de1b" align="center" width=300>
<br>Terlihat bahwa ada 3 variabel numerik dan 4 variabel kategorikal bertipe data object. Terlihat juga bahwa ada beberapa variabel yang memiliki null type seperti pada variabel `genre`, `type`, dan `rating`.

Deskripsi variabel:
Variabel | Keterangan
---------|-----------
anime_id | ID unik myanimelist.net yang mengidentifikasi sebuah anime
name     | Nama lengkap anime
genre    | Daftar genre untuk anime ini yang dipisahkan dengan koma
type     | Tipe anime, seperti movie, TV, OVA, dll
episodes | Berapa banyak episode dalam acara ini (1 jika film)
rating   | Rata-rata penilaian dari 10 untuk anime ini
members  | Jumlah anggota komunitas yang berada dalam grup anime ini.

Variabel yang akan digunakan dalam proyek ini adalah `anime_id`, `name`, dan `genre`.

### Variabel pada rating.csv
Informasi terkait rating.csv dapat dilihat pada gambar berikut:<br>
<img src="https://github.com/user-attachments/assets/9c74ce68-15ca-425c-9b62-388de45a9b5f" align="center" width=300>
<br> Dari hasil kode diatas, terlihat bahwa ada 3 variabel numerik yaitu `user_id`, `anime_id`, `rating`. 

Deskripsi Variabel:
Variabel | Keterangan
---------|-----------
user_id  | ID pengguna
anime_id | Anime yang telah dinilai oleh pengguna ini
rating   | Penilaian dari 10 yang diberikan pengguna ini (-1 jika pengguna menontonnya tetapi tidak memberikan penilaian)

Ketiga variabel tersebut akan digunakan dalam proyek ini.

### Pengecekan Missing Value
Pada tahap ini, dilakukan pengecekan terhadap missing value dari anime_df dan rating_df dengan fungsi isna().sum(). Pada anime_df, terdapat 62 null value pada `genre`, 25 null value pada `type`, dan 230 null value pada `rating`. Sedangkan pada rating_df, tidak terdapat missing value sama sekali. Selanjutnya dilakukan pengecekan jumlah user yang memberikan rating -1. Rating -1 dianggap sebagai null value karena rating tersebut menunjukkan bahwa pengguna menonton anime tersebut tetapi tidak memberikan penilaian/rating. Jadi seluruh rating -1 akan ditangani selayaknya null value. Setelah ditelusuri, terdapat 1476496 baris yang ratingnya -1.

### Univariate Exploratory Data Analysis
1. Analisis Distribusi Rating
    <br><img src="https://github.com/user-attachments/assets/6c86f2ab-9389-4804-8d67-84ada15c42f1" align="center" width=800>
    <br>Insight: Dari grafik diatas, terlihat bahwa user kebanyakan memberikan rating dari rentang 7 hingga 10 dengan rating 8 merupakan rating terbanyak yang diberikan oleh user kepada anime-anime yang ada.
   
2. Analisis Genre
   <br>Ditahap ini, dilakukan pengecekan jumlah genre yang ada pada setiap anime dengan memisahkan setiap list genre pada anime yang memiliki genre lebih dari satu. Lalu dilakukan juga mengecek apa saja genre-genre yang ada pada **anime_df**. Diketahui bahwa terdapat 43 jumlah genre. Selain itu, ditampilkan visualisasi distribusi frekuensi anime dari 43 genre yang ada.
   <br><img src="https://github.com/user-attachments/assets/e8283bd5-a5fe-4b04-a201-6a7cf5049728" align="center" width=800>
   <br>Insight: Dari grafik diatas, dapat dilihat bahwa anime dengan genre Comedy merupakan anime yang paling banyak jumlahnya yaitu lebih dari 4000, kemudian diikuti dengan genre Action dan Adventure. Genre yang paling sedikit jumlah animenya yaitu Yaoi, Yuri, dan Josei.

3. Analisis Distribusi Tipe Anime
    <br><img src="https://github.com/user-attachments/assets/9d20d15a-e3e3-4dcf-9ec9-8038e850361d" align="center" width=800>
    <br>Insight: Terlihat dari grafik diatas, bahwa tipe anime yang paling dominan adalah TV, dengan jumlah lebih dari 3700 judul. Ini menandakan bahwa format serial televisi merupakan bentuk yang paling umum digunakan dalam industri anime, kemungkinan karena daya tariknya yang tinggi dalam membangun cerita panjang dan basis penggemar yang stabil. Di posisi kedua dan ketiga terdapat tipe OVA (Original Video Animation) dan Movie, masing-masing dengan jumlah sekitar 3300 dan 2300 judul. Secara keseluruhan, grafik ini memberikan gambaran bahwa produksi anime masih sangat didominasi oleh format TV, sementara tipe-tipe lain berperan sebagai pelengkap atau variasi distribusi konten.

## Data Preparation
Tahapan ini mencakup langkah-langkah untuk mempersiapkan data sebelum digunakan dalam proses modeling. Tujuannya adalah untuk memastikan bahwa data dalam kondisi bersih, konsisten, dan relevan untuk sistem rekomendasi. Ada beberapa tahap persiapan data yang dilakukan:
1. Menangani Missing Value (Nilai Kosong)
2. Menangani Duplikasi Data
3. Melakukan Penggabungan Data
4. Menghapus Fitur yang tidak dibutuhkan
5. Mengganti Nama Kolom

### Menangani Missing Value (Nilai Kosong)
Penanganan terhadap missing value yang terdapat pada **anime_df** dan **rating_df** dilakukan dengan mepenghapusan terhadap missing value tersebut. Penghapusan null value pada anime_df menggunakan kode: `anime_df.dropna(inplace=True)`. Beberapa fitur penting seperti genre, rating, dan episodes mengandung nilai null. Mengingat sistem rekomendasi membutuhkan informasi yang lengkap untuk menghasilkan prediksi yang akurat (terutama untuk content-based filtering), maka baris yang mengandung nilai kosong lebih baik dihapus untuk menjaga kualitas data. Untuk rating_df, tidak didapati adanya null value setelah melakukan pengecekan null value menggunakan kode: `rating_df.isna().sum()`. Kemudian untuk rating yang bernilai -1 pada dataset **rating_df** akan dihapus karena dianggap sebagai missing value. Rating -1 dianggap sebagai null value karena rating tersebut menunjukkan bahwa pengguna menonton anime tersebut tetapi tidak memberikan penilaian/rating. Nilai -1 akan merusak distribusi rating dan berpotensi menurunkan performa model rekomendasi. Menghapus nilai ini memastikan hanya data pengguna yang benar-benar memberikan rating yang diproses. Penghapusan rating tersebut menggunakan kode berikut: `rating_df = rating_df[rating_df['rating'] != -1]`. 

### Menangani Duplikasi Data
Data duplikat dapat menyebabkan bias dalam proses training model, terutama untuk collaborative filtering yang bergantung pada rating individu. Menghapus duplikasi memastikan bahwa setiap interaksi pengguna-anime hanya dihitung sekali. Pertama-tama dilakukan pengecekan jumlah data duplikat pada dataset **anime_df** menggunakan kode berikut: `anime_df.duplicated().sum()`. Hasil kode tersebut menunjukkan bahwa tidak terdapat data duplikat. Pengecekan jumlah data duplikat pada **rating_df** juga menggunakan kode yang sama dan didapati hasil bahwa ada 1 data duplikat. Karena jumlah data duplikat hanya bernilai 1, maka data duplikat tersebut akan dihapus dengan kode: `rating_df.drop_duplicates(inplace=True)`.

### Melakukan Penggabungan Data
Ditahap ini, dilakukan penggabungan dataset antara **rating_df** dan **anime_df** berdasarkan `anime_id`. Penggabungan dataset ini dilakukan secara inner, yaitu hanya anime_id yang beririsan pada **rating_df** dan **anime_df**. Penggabungan ini penting untuk menghubungkan interaksi pengguna dengan informasi konten dari anime (judul, genre, rating, dll). Inner join memastikan bahwa hanya data yang valid dan tersedia di kedua dataset yang diproses oleh model.
<br>Kode untuk melakukan penggabungan data adalah sebagai berikut: `df_ratings_anime = pd.merge(rating_df, anime_df, on='anime_id', how='inner')`

### Menghapus Fitur yang tidak dibutuhkan
Selanjutnya, dilakukan penghapusan fitur yang tidak dipakai untuk pelatihan model kita yaitu `type`, `episodes`, `rating_y`, `members`. Penghapusan fitur ini dilakukan dengan menggunakan method drop. Tahapan ini dibutuhkan karena fitur-fitur tersebut tidak relevan atau redundant untuk proses sistem rekomendasi. Selain itu, menghapus fitur ini juga membantu mengurangi noise dan mempercepat proses komputasi.
<br> Kode untuk menghapus fitur redundan adalah sebagai berikut: `df_ratings_anime.drop(['type', 'episodes', 'rating_y', 'members'], inplace=True, axis=1)`

### Mengganti Nama Kolom
Penggantian nama kolom dilakukan pada kolom `rating_x` menjadi `rating`. Alasan penggantian nama kolom ini dilakukan adalah karena setelah proses penggabungan, kolom rating dari dataset rating diberi nama rating_x. Agar lebih mudah dipahami dan digunakan, kolom ini diubah kembali menjadi rating yang lebih deskriptif.
<br> Kode untuk mengganti nama kolom adalah sebagai berikut: `df_ratings_anime.rename(columns={'rating_x': 'rating'}, inplace=True)`

### Content-Based Filtering
Pada Content-Based Filtering, terdapat beberapa persiapan data yang dilakukan, yaitu menghapus data anime_id yang duplikat dan pembuatan matriks TF-IDF yang digunakan untuk menentukan bobot fitur dan menghitung kesamaan antara item dalam hal ini adalah genre.

Pertama-tama dataframe yang dibuat sebelumnya dicopy terlebih dahulu ke sebuah variabel baru bernama **anime**. Tujuannya adalah untuk mencegah dataframe sebelumnya berubah karena akan dipakai juga untuk model Collaborative Filtering. Selanjutnya, untuk model Content-Based Filtering, hanya `anime_id` unik yang akan digunakan pada proses pemodelan. Oleh karena itu, dilakukan penghapusan data `anime_id` yang duplikat dengan fungsi drop_duplicates(). Alasan dilakukan penghapusan data duplikat ini adalah karena Model Content-Based tidak memerlukan interaksi pengguna, melainkan hanya informasi dari masing-masing anime. Oleh karena itu, setiap anime cukup direpresentasikan satu kali. Duplikasi akan menyebabkan redundansi dan mempengaruhi representasi TF-IDF genre secara tidak akurat.

<br>Hasilnya adalah sebagai berikut:
<br><img src="https://github.com/user-attachments/assets/392b61cd-ebc2-440e-b4bd-83f617147e0c" align="center" width=800>

Setelah itu, dilakukan konversi data series menjadi list menggunakan fungsi tolist() dari library numpy. Data-data ini adalah data yang akan digunakan untuk pemodelan Content Based Filtering yaitu data `anime_id`, `name`, dan `genre`. Lalu, dibuat sebuah dictionary untuk menentukan pasangan key-value pada data anime_id, anime_name, dan anime_genres yang telah disiapkan sebelumnya. Dictionary tersebut kemudian diubah menjadi sebuah dataframe bernama **anime_new**. Transformasi ini bertujuan menyederhanakan struktur data dan hanya menyimpan kolom penting yang akan digunakan untuk proses ekstraksi fitur genre. Ini memperkecil kompleksitas data dan mempermudah pipeline selanjutnya.

<img src="https://github.com/user-attachments/assets/2151bd7c-218f-4553-a6da-8ccb0e72536d" align="center" width=800>

Selanjutnya dilakukan penghapusan spasi setelah koma pada kolom genre sehingga dapat mempermudah proses ekstraksi fitur menggunakan tf-idf. Spasi yang tidak dibersihkan akan menyebabkan TF-IDF menganggap " Action" dan "Action" sebagai dua fitur yang berbeda, padahal keduanya identik. Ini akan menurunkan akurasi dalam menghitung kesamaan antar anime. Kodenya adalah sebagai berikut: `anime_new['genre'] = anime_new['genre'].apply(lambda x: ','.join(g.strip() for g in x.split(',')) if pd.notna(x) else '')`. Kemudian, akan digunakan TF-IDF Vectorizer untuk menemukan representasi fitur penting dari setiap genre anime. TF-IDF sangat cocok untuk mengekstrak dan merepresentasikan genre karena mampu membedakan fitur yang sering dan jarang muncul secara proporsional. Genre seperti "Action" mungkin sering muncul, sementara "Josei" lebih jarang—TF-IDF memberikan bobot yang mencerminkan ini. Penerapannya dilakukan dengan menggunakan fungsi tfidfvectorizer() dari library sklearn. Selanjutnya tfidf di fit menggunakan data kolom genre dan ditampilkan semua nama fitur pada tfidf tersebut. Selanjutnya, dilakukan fit dan transformasi tfidf ke dalam bentuk matriks dengan kode berikut: `tfidf_matrix = tfidf.fit_transform(anime_new['genre'])`. 

Untuk menghasilkan vektor tf-idf dalam bentuk matriks, digunakan fungsi todense(). Kodenya sebagai berikut: `tfidf_matrix.todense()`. Alasan konversi ke dense matrix diperlukan ketika kita ingin melihat representasi numerik dari fitur secara eksplisit atau ketika ingin menghitung kesamaan (misalnya dengan cosine similarity).

### Collaborative Filtering
Pada Collaborative Filtering, terdapat beberapa persiapan data yang dilakukan, yaitu:
- Menghapus Kolom yang Tidak Dibutuhkan
- Encoding user_id dan anime_id
- Membagi Data untuk Training dan Validasi

Jadi pertama-tama, hanya data kolom `user_id`, `anime_id`, dan `rating` saja yang dibutuhkan dalam tahap pemodelan, sehingga kolom `name` dan `genre` dihapus dengan menggunakan fungsi drop(). Alasan dilakukan hal ini adalah karena Collaborative Filtering murni berbasis interaksi antara pengguna dan item. Informasi seperti name dan genre tidak digunakan, sehingga menghapus kolom ini menyederhanakan data dan mempercepat proses training.

Hasilnya sebagai berikut:
<br><img src="https://github.com/user-attachments/assets/80dc49bb-c10d-4310-9a04-d14e064298cd" align="center" width=300>

Selanjutnya dilakukan encoding terhadap user_id dan anime_id menggunakan kode berikut:
```python
user_ids = anime_ratings['user_id'].unique().tolist()
user_to_user_encoded = {x: i for i, x in enumerate(user_ids)}
user_encoded_to_user = {i: x for i, x in enumerate(user_ids)}

anime_ids = anime_ratings['anime_id'].unique().tolist()
anime_to_anime_encoded = {x: i for i, x in enumerate(anime_ids)}
anime_encoded_to_anime = {i: x for i, x in enumerate(anime_ids)}
```
Encoding ini dilakukan karena user_id dan anime_id tidak berurutan dari nol, sehingga dilakukan encoding ke dalam indeks integer berurutan mulai dari nol agar data tersebut dapat digunakan sebagai input dalam layer embedding pada model rekomendasi berbasis Collaborative Filtering. Dengan cara ini, setiap user dan anime akan memiliki representasi vektor yang dapat dipelajari oleh model, sehingga memungkinkan identifikasi pola preferensi pengguna terhadap item secara lebih efektif.

Setelah itu, user_id dan anime_id dipetakan ke dataframe yang berkaitan. Dan terakhir, dilakukan pengecekan beberapa hal dalam data seperti jumlah user, jumlah anime, dan kemudian mengubah nilai rating menjadi float. Dilakukan juga pengecekan terhadap rating minimum dan rating maksimum. Pengecekan ini penting untuk mengetahui skala data serta distribusinya. Mengetahui jumlah user dan item sangat berguna untuk menentukan arsitektur input dan output model, sedangkan pengecekan rating memastikan tidak ada anomali atau nilai ekstrem yang salah. Hasil dari proses pengecekan yaitu jumlah user: 69600, jumlah anime: 9892, rating minimum: 1.0, dan rating maksimum: 10.0.

Setelah kedua tahapan diatas telah dilakukan, selanjutnya dilakukan pembagian data training dan data validasi untuk proses pelatihan model. Pertama-tama, dilakukan pengacakan data supaya distribusinya menjadi acak. Pengacakan data memastikan bahwa distribusi user dan anime tersebar merata di training dan validation, sehingga model tidak overfitting pada pola tertentu.

<img src="https://github.com/user-attachments/assets/85fe88c2-b1a6-41ca-addd-d828eba02f8f" align="center" width=500>

Selanjutnya, data train dan validasi dibagi dengan komposisi 80:20. Namun sebelumnya, dilakukan pemetaan (mapping) data user dan anime menjadi satu value terlebih dahulu yaitu x. Lalu, dibuat rating dalam skala 0 sampai 1 agar mudah dalam melakukan proses training dan disimpan pada variabel y. Normalisasi rating ini bertujuan untuk membuat training model lebih stabil dan konvergen lebih cepat.

## Modeling
Tahapan ini membahas mengenai model sisten rekomendasi yang Anda buat untuk menyelesaikan permasalahan. Sajikan top-N recommendation sebagai output.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menyajikan dua solusi rekomendasi dengan algoritma yang berbeda.
- Menjelaskan kelebihan dan kekurangan dari solusi/pendekatan yang dipilih.

## Evaluation
Pada bagian ini Anda perlu menyebutkan metrik evaluasi yang digunakan. Kemudian, jelaskan hasil proyek berdasarkan metrik evaluasi tersebut.

Ingatlah, metrik evaluasi yang digunakan harus sesuai dengan konteks data, problem statement, dan solusi yang diinginkan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan formula metrik dan bagaimana metrik tersebut bekerja.

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
