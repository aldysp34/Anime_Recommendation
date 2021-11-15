# Laporan Proyek Machine Learning - Muhammad Aldi Surya Putra
## Anime Recommendation 

### A. Project Overview
Animasi dari Jepang atau yang sering kita sebut sebagai *anime* saat ini menjadi salah satu tontonan yang paling banyak diminati oleh masyarakat khususnya para remaja. Pada awalnya *anime* kurang terkenal di masyarakat, namun seiring berkembangnya kualitas cerita dan juga visualnya, *anime* jadi semakin diminati oleh masyarakat. Karena menjadi salah satu tontonan yang paling diminati, produksi *anime* pun berkembang pesat hingga berjumlah ribuan dengan berbagai cerita yang menarik untuk diikuti. Dengan banyaknya *anime* yang ada sekarang, tentu akan sulit untuk mencari *anime* - *anime* yang mungkin dapat membuat kita tertarik untuk menontonnya apalagi jika kita hanya menyukai suatu / beberapa *genre* tertentu, atau jika kita hanya ingin menonton *anime* yang mirip seperti *anime* yang sudah ditonton. Maka perlu dibuat sebuah solusi untuk mendapatkan *list* atau daftar *anime* yang sesuai dengan apa yang kita inginkan.
> Referensi : 
> [Towards Data Science](https://towardsdatascience.com/building-a-recommendation-system-for-anime-566f864acea8)
### B. Bussiness Understanding
#### Problem Statements
*Problem Statements* pada proyek ini adalah bagaimana cara untuk mencari sebuah / beberapa *anime* yang *genre* mirip seperti *anime* yang pernah kita tonton tanpa mencarinya secara manual.
#### Goals
Berdasarkan *Problem Statements* diatas, *goals* yang ingin dicapai adalah membuat sebuah sistem rekomendasi yang memberikan suatu *list* atau daftar anime yang memiliki *genre* mirip dengan anime yang pernah ditonton sebelumnya secara otomatis.
#### Solution Statements
Solusi dalam menyelesaikan masalah *Anime Recommendation* adalah dengan membuat sebuah sistem rekomendasi dengan metode *Content-Based Filtering*. Metode ini digunakan untuk merekomendasikan *item* yang pada masalah saat ini adalah *anime* berdasarkan kemiripin atribut yang dimiliki, yang pada masalah ini atribut tersebut adalah *genre*. Dalam membangun sistem rekomendasi tersebut, dilakukan dengan menggunakan beberapa teknik yaitu menghitung frekuensi kemunculan suatu *item* pada suatu dokumen atau yang sering disebut sebagai *Term Frequency-Inverse Document Frequency* dan juga menghitung perbandingan kemiripan suatu dokumen atau yang sering disebut sebagai *Cosine Similarity*
### C. Data Understanding
Dataset yang digunakan yaitu berasal dari [kaggle](https://www.kaggle.com/CooperUnion/anime-recommendations-database) tentang database yang diambil dari website [*myanimelist*](https://myanimelist.net/). Pada dataset ini terdapat 7 atribut yaitu, *anime_id*, *name*, *genre*, *type*, *episodes*, *rating*, *members*.
#### Atribut pada dataset
- *anime_id* : id dari *list* atau daftar *anime* pada dataset
- *name* : judul *anime*
- *genre* : *genre* atau ragam kategori *anime*
- *type* : tipe *anime* yaitu TV atau *movie*
- *episodes* : jumlah episode *anime*
- *members* : jumlah pengikut yang mengikuti *anime* tersebut
#### Dataset Information
##### Jumlah index dan kolom pada dataset
- Index : 12294
- Kolom : 7

##### Info pada tiap kolom dataset
*Dataset Information*
| --- | Kolom | Jumlah Non - Null | Tipe Data |
| --- | ----- | ----------------- | --------- | 
| 1 | *anime_id* | 12294 Non - Null | int64 | 
| 2 | *name* | 12294 Non - Null | object |
| 3 | *genre* | 12232 Non - NUll | object |
| 4 | *type* | 12269 Non - Null | object |
| 5 | *episodes* | 12294 Non - Null | object | 
| 6 | *rating* | 12064 Non - Null | float64 | 
| 7 | *members* | 12294 Non - Null | int64 | 

### D. Data Preparation 
#### Mengatasi Missing Value
Jumlah *missing value* pada setiap kolom
| Atribut | jumlah missing value | 
| ------- | -------------------- |
| anime_id | 0 |
| name | 0 |
| genre | 62 | 
| type | 25 |
| episodes | 0 |
| rating | 230 |
| members | 0 |
##### Menghapus missing value
Dalam mengatasi jumlah missing value pada setiap kolom, kita akan menghapus setiap baris yang memiliki *missing value* pada tiap kolomnya atau melakukan *dropna*. 

#### Menghapus kolom yang tidak dibutuhkan
Karena sistem rekomendasi yang akan dibuat berdasarkan *genre* maka, ada beberapa kolom yang tidak dibutuhkan yaitu *type*, *episode*, dan *members*. Kolom - kolom tersebut akan dihapus
#### Sorting data
Karena dataset ini terurut berdasarkan *rating*, maka akan diurutkan berdasarkan *anime_id*
#### Konversi Data Series menjadi List
Agar data dapat diproses oleh model, maka *data series* harus diubah dulu menjadi *list*. Atribut yang akan dikonversi antara lain, *anime_id*, *name*, dan juga *genre*
### E. Modelling
Model yang akan dibuat adalah sebuah sistem rekomendasi berbasis konten (*content-based filtering*). Model ini menggunakan teknik *TF-IDF Vectorizer* dan juga *Cosine Similarity* dalam prosesnya.*TF-IDF Vectorizer* digunakan untuk melakukan perhitungan *idf* dari setiap *genre* lalu mengkonversinya ke dalam bentuk matriks. Lalu selanjutnya adalah menghitung tingkat kesamaan atau *Cosine Similarity* dari setiap *anime*. Dalam melakukan kedua hal tersebut, kita menggunakan *class* *TfidfVectorizer* dan *cosine_similarity* dari library *scikit-learn*. \
Setelah mengimplementasikan kedua hal tersebut, untuk melakukan rekomendasi dan juga menampilkan hasilnya kita membuat sebuah fungsi yang memiliki parameter sebagai berikut.
- *df* : *dataframe* yang memuat data *anime*
- *animeName* : judul *anime* yang menjadi kata kunci dalam melakukan rekomendasi
- *k* : jumlah rekomendasi yang dihasilkan 

Hal yang dilakukan oleh fungsi tersebut adalah pertama kita mengambil index dari seluruh data, lalu kita ambil index dari *anime* yang dicari. Selanjutnya, kita ambil beberapa data yang memiliki *cosine similarity* terbesar dengan *anime* yang dicari, setelah berhasil mendapatkan data-data tersebut, selanjutnya data-data tersebut  di-*sorting* berdasarkan *cosine similarity* terbesar.\
Berikut adalah hasil dari sistem rekomendasi dengan kata kunci 'One Piece' : 
![hasil](https://github.com/aldysp34/Anime_Recommendation/blob/master/images/hasil.png?raw=true)
### F. Evaluation
Model ini menggunakan metrik *Precision*, dimana kita menentukan *anime* yang relevan dengan nama dan juga *genre* anime yang dicari. Berikut rumus dari *Precision* : 
![precision](https://github.com/aldysp34/Anime_Recommendation/blob/master/images/precision.png?raw=true)
Berdasarkan hasil rekomendasi pada model diatas, terlihat bahwa dari 14 jumlah rekomendasi terdapat 13 rekomendasi yang memiliki nama dan *genre* yang sama, dan sisanya memiliki kemiripan dengan *anime* 'One Piece'. Artinya bahwa model yang kita bangun memiliki nilai *precision* sebesar 92% (13 dari 14 *anime*)




