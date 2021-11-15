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
Solusi dalam menyelesaikan masalah *Anime Recommendation* adalah dengan membuat sebuah sistem rekomendasi dengan metode *Content-Based Filtering*. Metode ini digunakan untuk merekomendasikan *item* yang pada masalah saat ini adalah *anime* berdasarkan kemiripin atribut yang dimiliki, yang pada masalah ini atribut tersebut adalah *genre*. Dalam membangun sistem rekomendasi tersebut, dilakukan dengan menggunakan beberapa teknik yaitu menghitung frekuensi kemunculan suatu *item* pada suatu dokumen atau yang sering disebut sebagai *Term Frequency-Inverse Document Frequency* dan juga menghitung perbandingan kemiripan suatu dokumen atau yang sering disebut sebagai *Cosine Similiarity*
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
Model yang akan dibuat adalah sebuah sistem rekomendasi berbasis konten (*content-based filtering*). Model ini menggunakan teknik *TF-IDF Vectorizer* dan juga *Cosine Simiiarity* dalam prosesnya.*TF-IDF Vectorizer* digunakan untuk melakukan perhitungan *idf*  dari setiap *genre* lalu mengkonversinya ke dalam bentuk matriks. Lalu selanjutnya adalah menghitung tingkat kesamaan atau *Cosine Similarity* dari setiap *anime*. Dalam melakukan kedua hal tersebut, kita menggunakan *class* *TfidfVectorize*r dan *cosine_similarity* dari library *scikit-learn*. \
Setelah mengimplementasikan kedua hal tersebut, untuk melakukan rekomendasi dan juga menampilkan hasilnya kita membuat sebuah fungsi yang memiliki parameter sebagai berikut.
- *df* : *dataframe* yang memuat data *anime*
- *animeName* : judul *anime* yang menjadi kata kunci dalam melakukan rekomendasi
- *k* : jumlah rekomendasi yang dihasilkan 

Hal yang dilakukan oleh fungsi tersebut adalah pertama kita mengambil index dari seluruh data, lalu kita ambil index dari *anime* yang dicari. Selanjutnya, kita ambil beberapa data yang memiliki *cosine similarity* terbesar dengan *anime* yang dicari, setelah berhasil mendapatkan data-data tersebut, selanjutnya data-data tersebut  di-*sorting* berdasarkan *cosine similarity* terbesar.\
Berikut adalah hasil dari sistem rekomendasi dengan kata kunci 'One Piece' : 
![hasil](https://github.com/aldysp34/Anime_Recommendation/blob/master/images/hasil.png?raw=true)
### F. Evaluation
Model ini menggunakan metrik *Precision*, dimana kita menentukan *anime* yang relevan dengan nama dan juga *genre* anime yang dicari. Berikut rumus dari *Precision* : 
![precision](data:image/png;base64,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)
Berdasarkan hasil rekomendasi pada model diatas, terlihat bahwa dari 14 jumlah rekomendasi terdapat 13 rekomendasi yang memiliki nama dan *genre* yang sama, dan sisanya memiliki kemiripan dengan *anime* 'One Piece'. Artinya bahwa model yang kita bangun memiliki nilai *precision* sebesar 92% (13 dari 14 *anime*)




