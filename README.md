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



Paragraf awal bagian ini menjelaskan informasi mengenai jumlah data, kondisi data, dan informasi mengenai data yang digunakan. Sertakan juga sumber atau tautan untuk mengunduh dataset. Contoh: [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Restaurant+%26+consumer+data).

Selanjutnya, uraikanlah seluruh variabel atau fitur pada data. Sebagai contoh:  

Variabel-variabel pada Restaurant UCI dataset adalah sebagai berikut:
- accepts : merupakan jenis pembayaran yang diterima pada restoran tertentu.
- cuisine : merupakan jenis masakan yang disajikan pada restoran.
- dst

**Rubrik/Kriteria Tambahan (Opsional)**:
- Melakukan beberapa tahapan yang diperlukan untuk memahami data, contohnya teknik visualisasi data beserta insight atau exploratory data analysis.

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