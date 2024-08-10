# Latar Belakang
Perkembangan penggunaan data pada industri sudah sangat berkembang, data berasal dari mana saja dan siapa saja. Data digunakan untuk memberikan informasi untuk suatu organisasi atau individu. Berbagai perusahaan telah memanfaatkan perkembangan data dengan membuat sistem rekomendasi yang dapat memberikan rekomendasi terhadap pengguna lebih personal.

Dalam industri sistem rekomendasi telah digunakan oleh banyak perusahaan agar dapat meningkatkan peluang pengguna untuk melakukan transaksi dengan memberikan rekomendasi yang sesuai dengan pengguna butuhkan. Dalam topik sistem rekomendasi, Content-Based Filtering menjadi sesuatu yang cukup sering dibahas [1].

Sebagai contoh Netflix menggunakan sistem rekomendasi untuk memberikan rekomendasi film yang sesuai dengan kesukaan pengguna. Hal tersebut dapat membantu pengguna agar tidak perlu mencari film yang ingin ditonton dan dapat meningkatkan transaksi *subscription* Netflix dikarenakan sistem rekomendasi telah menciptakan rekomendasi film yang *personalized* dengan kesukaan pengguna

# Business Understanding
Dengan latar belakang tersebut maka perlu dilakukan *busines understanding* dengan mengidentifikasi masalah yang dihadapi dan tujuan yang ingin dicapai dengan membuat sistem rekomendasi Netflix.
## Problem Statements
Berdasarkan *Business Understanding* tersebut, maka didapatkan *Problem Statement* sebagai berikut:
- Bagaimana cara membuat sebuah rekomendasi sistem menggunakan Content-Based Filtering dan Collaborative Filtering?
- Bagaimana Performa Content-Based Filtering dibandingkan Collaborative Filtering dalam memberikan rekomendasi kepada pengguna?
## Goals
Berdasarkan *Problem Statement* di atas, maka didapatkan *Goals* sebagai berikut:
- Membuat sebuah rekomendasi sistem dengan menggunakan metode Content-Based Filtering dan Collaborative Filtering
- Memberikan rekomendasi film kepada pengguna
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
## Langkah-langkah pemrosesan data:
- Mengecek missing value pada dataset
- Menghapus data yang memiliki missing value
- Mengecek duplikasi pada dataset
### Check missing Value
Sebelum menggunakan dataset kita perlu mengecek missing value yang ada pada dataset. Kita bisa menggunakan kode seperti berikut:
```Python
movies.isna().sum()
```
Output:
title             0
year            527
certificate    3453
duration       2036
genre            73
rating         1173
description       0
stars             0
votes          1173
dtype: int64

Dari dataset yang digunakan terdapat beberapa data yang memiliki missing value, agar hasil rekomendasi dapat memberikan hasil yang optimal maka kita perlu men-drop data yang mengandung missing value dengan kode berikut:
```Python
movies.dropna(inplace=True)
```
# Modelling
## Vektorisasi Menggunakan TF-IDF
Dalam membuat sistem rekomendasi flm, kolom description digunakan untuk menemukan kemiripan antara satu film dengan film lainnya, agar dapat menemukan nilai dari kemiripan tersebut maka perlu dilakukan vektorisasi nilai dari kolom description menggunakan TF-IDF. 

|                      | thriller | drama    | romance  |
| -------------------- | -------- | -------- | -------- |
| Puffin Rock          | 0.000000 | 0.000000 | 0.000000 |
| Soni                 | 0.000000 | 0.000000 | 0.000000 |
| Sijipeuseu: The Myth | 0.000000 | 0.000000 | 0.000000 |
| Shattered            | 0.000000 | 0.000000 | 0.000000 |
## Derajat kemiripan dengan Cosine Similarity
Setelah berhasil mengidentifikasi kemiripan antara judul dengan genre. Maka perlu dilakukan untuk menghitung derajat kemiripan dengan Cosine Similarity antar judul film.

| title                                | Republic of Doyle | The Playlist |
| ------------------------------------ | ----------------- | ------------ |
| Never Have I Ever                    | 0.702260          | 0.262653     |
| Dana Carvey: Straight White Male, 60 | 0.532369          | 0.000000     |
Pada tabel diatas kita bisa melihat bahwa nilai kesamaan antar judul film. Jika dilihat judul film `Never Have I Ever` memiliki nilai kemiripan dengan film `Republic of Doyle` sebesar 0.702260.

Setelah melakukan vektorisasi pada kolom description dan menghitung derajat kemiripan antar judul film menggunakan Cosine Similarity. Kita perlu mengecek hasil rekomendasi dengan membuat fungsi yang dapat menerima inputan berupa `judul_film, similarity_data, items, dan k` dengan definisi masing-masing sebagai berikut:
- judul_film: Merupakan judul film yang ingin dicari kesamaannya
- similarity_data: Dataframe yang berisikan nilai kemiripan antar judul film
- items: fitur yang digunakan untuk mendefinisikan kemiripan
- k: jumlah rekomendasi yang ditampilkan

Hasil rekomendasi film dengan inputan `Danur`

| title                                 | genre  |
| ------------------------------------- | ------ |
| Beast of Morocco                      | Horror |
| The Human Centipede 2 (Full Sequence) | Horror |
| The Axe Murders of Villisca           | Horror |
| Verónica                              | Horror |
| Rape Zombie: Lust of the Dead         | Horror |
| FirstBorn                             | Horror |
| The Precipice Game                    | Horror |
| The Amityville Haunting               | Horror |
| Nini Thowok                           | Horror |
| At the Devil's Door                   | Horror |
# Evaluasi
Untuk melakukan evaluase terhadap hasil yang diberikan oleh fungsi rekomendasi yang telah dibuat sebelumnya dapat digunakan Precision untuk mengukur seberapa akurat hasil rekomendasi yang diberikan. 

Rumus _Precision_: $\dfrac{n}{i}$ 
- n: Jumlah rekomendasi yang sesuai
- i: Jumlah rekomendasi yang diberikan
Dari hasil prediksi yang diberikan dengan fungsi yang telah dibuat sebelumnya dengan menampilkan film yang mirip dengan film `Danur` terdapat 10 dari 10 film yang sesuai. Maka dapat dihitung precision yang didapat adalah 100%.
# References
1. Tie-min, M., Xue, W., Fu-cai, Z. , Shuang, W., 2020. Research on diversity and accuracy of the recommendation system based on multi-objective optimization. Neural Computing and Applications.