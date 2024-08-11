# Latar Belakang
Dalam industri teknologi, sistem rekomendasi telah digunakan oleh banyak perusahaan agar dapat meningkatkan peluang pengguna untuk melakukan transaksi dengan memberikan rekomendasi yang sesuai dengan pengguna butuhkan. Dalam topik sistem rekomendasi, Content-Based Filtering menjadi sesuatu yang cukup sering dibahas [1].

Sejak beberapa tahun terakhir kebiasaan orang untuk menonton film melalui TV atau Bioskop telah berpindah ke aplikasi seperti Netflix, Amazon Prime Videos, dan Disney+. Banyak orang memilih untuk menonton film favorit mereka melalui platform tersebut dikarenakan platform tersebut menyediakan layanan bebas iklan, dapat ditonton di mana saja, dan yang paling terpenting adalah platform tersebut memiliki sistem rekomendasi yang dapat meningkatkan engagement terhadap pengguna yang menggunakan platform tersebut [2]. 

Dengan persaingan yang semakin ketat antar platform tersebut, dibutuhkan sebuah sistem rekomendasi yang kokoh untuk menunjang kesuksesan platform. Pada era di mana pengguna bisa memilih banyak film dan series yang tersedia dalam platform maka dibutuhkan sebuah sistem yang dapat merekomendasikan tontonan kepada pengguna agar pengguna tidak perlu menghabiskan waktu untuk mencari tontonan.
# Business Understanding
Dengan latar belakang tersebut maka perlu dilakukan *busines understanding* dengan mengidentifikasi masalah yang dihadapi dan tujuan yang ingin dicapai dengan membuat sistem rekomendasi Netflix.
## Problem Statements
Berdasarkan *Business Understanding* tersebut, maka didapatkan *Problem Statement* sebagai berikut:
- Bagaimana cara membuat sebuah rekomendasi sistem menggunakan Content-Based Filtering yang dapat sesuai dengan prefensi pengguna?
- Bagaimana hasil evaluasi sistem rekomendasi menggunakan Content-Based Filtering?
## Goals
Berdasarkan *Problem Statement* di atas, maka didapatkan *Goals* sebagai berikut:
- Membuat sebuah rekomendasi sistem dengan menggunakan metode Content-Based Filtering.
- Mengevaluasi hasil sistem rekomendasi menggunakan metode Content-Based Filtering.
# Data Understanding
Data yang digunakan merupakan data yang didapatkan dari [Kaggle](https://www.kaggle.com/datasets/narayan63/netflix-popular-movies-dataset). Data tersebut berisikan 9957 baris dengan 9 kolom yang terdiri dari:
## Dataset Sample

| title     | year | certificate | duration | genre                 | rating | description                                       | stars                                             | votes   |
| --------- | ---- | ----------- | -------- | --------------------- | ------ | ------------------------------------------------- | ------------------------------------------------- | ------- |
| Cobra Kai | 2018 | TV-14       | 30 min   | Action, Comedy, Drama | 3.8    | Decades after their 1984 All Valley Karate Tou... | ['Ralph Macchio, ', 'William Zabka, ', 'Courtn... | 177,031 |

Kolom yang ada dari dataset adalah sebagai berikut:
- title: judul film
- year: tahun rilis film
- certificate rating kategori film
- duration: durasi film:
- genre: genre dari film
- rating: nilai rating film
- description: deskripsi singkat film
- stars: pemain yang ada pada film
- votes: jumlah suara dari pengguna yang telah memberikan rating
# Data Preperation
Dalam tahap Data Prepertation teknik yang digunakan adalah sebagai berikut:
## Drop Missing Value
Pada dataset `movies` terdapat beberapa missing value seperti berikut:
title             0
year            527
certificate    3453
duration       2036
genre            73
rating         1173
description       0
stars             0
votes          1173
Karena missing value dapat mempengaruhi performa dari sistem rekomendasi, maka salah satu cara untuk mengatasi missing value adalah dengan menghapus data yang memiliki missing value dengan menggunakan `dropna()`.

```Python
movies.dropna(inplace=True)
```
## Vektorisasi Menggunakan TF-IDF
Dalam membuat sistem rekomendasi flm, kolom description digunakan untuk menemukan kemiripan antara satu film dengan film lainnya, agar dapat menemukan nilai dari kemiripan tersebut maka perlu dilakukan vektorisasi nilai dari kolom description menggunakan TF-IDF. 

TF-IDF merupakan suatu algortima yang digunakan untuk menemukan kata-kata yang penting pada corpus. TF merepresentasikan frekuensi dari kata yang muncul, frekuensi tiap kata didapatkan dengan menghitung jumlah kemunculan suatu kata dari total jumlah kata yang ada pada corpus. Sedankgan IDF mengukur seberapa penting suatu kata di dalam corpus.

Projek ini menggunakan `TfidfVectorizer()` dari library `Scikit-learn` untuk mengubah fitur `genre` pada dataset menjadi sebuah vektor dan hasil vectorizer ditampilkan dalam gambar berikut.

![Vektorisasi dengan tfidf](https://github.com/user-attachments/assets/b3cb8613-d815-4aa1-a702-bd4d4c6d4423)
# Modelling
## Derajat kemiripan dengan Cosine Similarity
Cosine Similarity adalah sebuah algoritma yang digunakan untuk menemukan kemiripan menggunakan derajat cos dari dua buah vektor A dan B, yang di mana dapat direpresentasikan sebagai kata, kalimat, hingga paragraf [3]. 

$\cos \theta = \dfrac{A.B}{||A||||B||}$

Berikut adalah hasil Cosine Similarity antar judul film pada dataset:

![Table Cosine Similarity](https://github.com/user-attachments/assets/acd03acc-45db-4708-a6cf-7b5c5c2af2bf)

Pada tabel diatas kita bisa melihat bahwa nilai kesamaan antar judul film. Jika dilihat judul film `Never Have I Ever` memiliki nilai kemiripan dengan film `Republic of Doyle` sebesar 0.702260.
## Fungsi Rekomendasi
Setelah melakukan vektorisasi dengan fitur `genre` dan menghitung derajat kemiripan antar judul film menggunakan Cosine Similarity. Kita perlu mengecek hasil rekomendasi dengan membuat fungsi yang dapat menerima inputan berupa `judul_film, similarity_data, items, dan k` dengan definisi masing-masing sebagai berikut:
- judul_film: Merupakan judul film yang ingin dicari kesamaannya
- similarity_data: Dataframe yang berisikan nilai kemiripan antar judul film
- items: fitur yang digunakan untuk mendefinisikan kemiripan
- k: jumlah rekomendasi yang ditampilkan

Hasil rekomendasi film dengan inputan `Danur`
![Table rekomendasi film](https://github.com/user-attachments/assets/09837b4d-be7c-4869-8249-63c6547c4453)
# Evaluasi
Projek ini telah berhasil membuat sebuah sistem rekomendasi menggunaka metode Content-Based Filtering dengan menggunakan TF-IDF untuk melakukan vektorisasi dan Cosine Similarity untuk menemukan derajat kemiripan antar judul film dan membuat sebuah fungsi yang dapat menampilkan beberapa rekomendasi film sesuai dengan input yang diberikan.

Untuk mengukur hasil yang diberikan oleh fungsi rekomendasi yang telah dibuat sebelumnya dapat digunakan Precision untuk mengukur seberapa akurat hasil rekomendasi yang diberikan.

$Precision = \dfrac{i}{n}$
 - i: Film yang relevan dengan inputan pada fungsi rekomendasi
 - n: total film yang diberikan

Dari hasil prediksi yang diberikan dengan fungsi yang telah dibuat sebelumnya dengan menampilkan film yang mirip dengan film `Danur` terdapat 10 dari 10 film yang sesuai. Maka dapat dihitung precision yang didapat adalah 100%.
# References
1. Tie-min, M., Xue, W., Fu-cai, Z. , Shuang, W., 2020. Research on diversity and accuracy of the recommendation system based on multi-objective optimization. Neural Computing and Applications.
2. Kaur, H., & Ashfaq, R. (2023). The impact of Netflix on viewer behaviour and media consumption: An exploration of the effects of streaming services on audience engagement and entertainment preferences. _Journal of Media, Culture and Communication_, 3(4), 9-10.
3. Yunxiang, L., Qi, X., Zhang, T., 2020. Research on Text Classification Method based on PTF-IDF and Cosine Similarity. Journal of Information and Communication Engineering: Volume 6 pp. 335-338 (Issue 1).
