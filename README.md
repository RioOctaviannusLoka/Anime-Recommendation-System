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
    <br><img src="https://github.com/user-attachments/assets/6c86f2ab-9389-4804-8d67-84ada15c42f1" align="center" width=500>
    <br>Insight: Dari grafik diatas, terlihat bahwa user kebanyakan memberikan rating dari rentang 7 hingga 10 dengan rating 8 merupakan rating terbanyak yang diberikan oleh user kepada anime-anime yang ada.
   
2. Analisis Genre
   <br>Ditahap ini, dilakukan pengecekan jumlah genre yang ada pada setiap anime dengan memisahkan setiap list genre pada anime yang memiliki genre lebih dari satu. Lalu kita juga mengecek apa saja genre-genre yang ada pada **anime_df**. Diketahui bahwa terdapat 43 jumlah genre. Selain itu, ditampilkan visualisasi distribusi frekuensi anime dari 43 genre yang ada.
   <br><img src="https://github.com/user-attachments/assets/e8283bd5-a5fe-4b04-a201-6a7cf5049728" align="center" width=500>
   <br>Insight: Dari grafik diatas, dapat dilihat bahwa anime dengan genre Comedy merupakan anime yang paling banyak jumlahnya yaitu lebih dari 4000, kemudian diikuti dengan genre Action dan Adventure. Genre yang paling sedikit jumlah animenya yaitu Yaoi, Yuri, dan Josei.

3. Analisis Distribusi Tipe Anime
    <br><img src="https://github.com/user-attachments/assets/9d20d15a-e3e3-4dcf-9ec9-8038e850361d" align="center" width=500>
    <br>Insight: Terlihat dari grafik diatas, bahwa tipe anime yang paling dominan adalah TV, dengan jumlah lebih dari 3700 judul. Ini menandakan bahwa format serial televisi merupakan bentuk yang paling umum digunakan dalam industri anime, kemungkinan karena daya tariknya yang tinggi dalam membangun cerita panjang dan basis penggemar yang stabil. Di posisi kedua dan ketiga terdapat tipe OVA (Original Video Animation) dan Movie, masing-masing dengan jumlah sekitar 3300 dan 2300 judul. Secara keseluruhan, grafik ini memberikan gambaran bahwa produksi anime masih sangat didominasi oleh format TV, sementara tipe-tipe lain berperan sebagai pelengkap atau variasi distribusi konten.

## Data Preparation
Pada bagian ini Anda menerapkan dan menyebutkan teknik data preparation yang dilakukan. Teknik yang digunakan pada notebook dan laporan harus berurutan.

**Rubrik/Kriteria Tambahan (Opsional)**: 
- Menjelaskan proses data preparation yang dilakukan
- Menjelaskan alasan mengapa diperlukan tahapan data preparation tersebut.

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
