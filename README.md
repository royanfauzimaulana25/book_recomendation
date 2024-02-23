# Laporan Proyek Machine Learning - Muh Royan Fauzi Maulana

## *Project Overview*
---
Film menjadi salah satu media hiburan bagi manusia untuk mendapatkan kesenangan, edukasi dan lain sebagainya. Tahun ke tahun perkembangan film semakin pesat yang juga dipengaruhi perkembangan teknologi informasi yang semakin maju. Teknologi informasi yang berkembang menjadikan peluang bagi produser film untuk membuat karya yang beranekaragam, lebih kreatif dan unik. Perkembangan tersebut akhirnya melahirkan berbagai macam film dengan *genre* yang berbeda beda. 

Tiap orang memiliki selera atau minat yang berbeda tiap tiap *genre*, sebagai contoh mereka yang menyukai film teka teki seperti Sherlock Holmes mungkin saja juga menyukai film Detektif Conan. Bahkan ada yang menyukai film tertentu atas rekomendasi rekan atau orang lain yang mempengaruhi selera film mereka [^1]. Hal ini menjadi sebuah peluang dan masalah bagi distributor atau penyedia film untuk merekomendasikan film mana yang cocok bagi penonton mereka. 

Rekomendasi film yang sesuai akan meningkatkan retensi penonton untuk selalu menonton film dari distributor tersebut, sebaliknya bila rekomendasi tidak sesuai ada kemungkinan penonton akan merasa bosan karena tidak menemukan film yang sesuai dengan selera mereka. Oleh karena itu perusahaan distributor film membutuhkan suatu metode untuk memberikan rekomendasi film yang sesuai dengan minat dan selera mereka agar penonton terus menggunakan *platform* perusahaan distributor film tersebut terus menerus yang berdampak pada keuntungan perusahaan. 
  

## *Business Understanding*
---
### *Problem Statement*
Berdasarkan uraian latar belakang diatas, permasalahan yang dapat diselesaikan pada proyek ini ialah : 

* Berdasarkan data pengguna, bagaimana membuat sistem rekomendasi yang terpersonalisasi dengan teknik *collaborative filltering* agar penonton mendapatkan pengalaman menonton film yang sesuai selera terus menerus sehingga meningkatkan penggunaan *platform* distributor film tersebut dari pembelian langganan yang menjadi keuntungan bagi perusahaan ? 

### *Goals*
* Menghasilkan sejumlah rekomendasi film yang sesuai dengan preferensi penonton dengan teknik *collaboratif filltering* agar penonton mendapat film sesuai selera terus menerus dan meningkatkan keuntungan langganan perusahaan dan memiliki metrik skor *Root Mean Squared Error* < 30 %.

### *Solutions Statement*
* Menganalisis Data dengan melakukan *univariate analysis*.
* Mempersiapkan Data untuk digunakan dalam model.
* Mengembangkan Model dengan pendekatan *collaborative filtering* yang bekerja berdasarkan **rating** penonton.

## *Data Understanding*
**dataset** yang digunakan merupakan **dataset** film **rating** pada *platform* Netflix yang dapat diunduh pada *platform* Kaggle berikut : [Netflix Movie *dataset*](https://www.kaggle.com/*dataset*s/rishitjavia/netflix-movie-*rating*-*dataset*/)

Terdapat dua **dataset**, yaitu *`movie`* yang berisi data terkait judul film dan tahun film dan *dataset* *`**rating*s*`* berisi **rating** yang diberikan penonton pada tiap film.

Variabel-variabel pada *Movie *dataset** adalah sebagai berikut :
1. *Movies*
  * *`Movie_ID`* = *Identifier* *Movie*
  * *`Year`* = Tahun Penayangan
  * *`Name`* = Nama *Movie*

2. ***rating*s**
  * *`User_ID`* : *ID* pengguna
  * *`*rating*`* : **rating** yang diberikan oleh penonton (1-5)
  * *`Movie_ID`* : *Identifier* *Movie*

### *Univariate Analysis*
Pada tahapan ini tiap fitur dianalisis secara terpisah untuk mengetahui sebaran datanya pada **dataset**. 

#### Analisis fitur pada *dataset* *Movie*

tabel 1. Informasi sebaran **dataset* movie*
|         | **_Movie_ID_** |  **_Year_**  |
|--------:|:--------------:|:------------:|
| _count_ |   17770.000000 | 17770.000000 |
|  _mean_ |    8885.500000 |  1990.235566 |
|  _std_  |    5129.901477 |    16.564501 |
|  _min_  |       1.000000 |  1915.000000 |
|  _25%_  |    4443.250000 |  1985.000000 |
|  _50%_  |    8885.500000 |  1997.000000 |
|  _75%_  |   13327.750000 |  2002.000000 |
|  _max_  |   17770.000000 |  2005.000000 |

![Histogram Fitur Year](https://github.com/royanfauzimaulana25/movie_recomendation/assets/99244948/31c6fff7-d048-479b-8eb0-37ae93a24730)

Gambar 1. Histogram fitur *Year* pada **dataset* movie*

Berdasarkan hasil analisis pada fitur **dataset* movie* didapatkan *insight* sebagai berikut :
* Pada tabel diatas jumlah data yang terdapat pada *dataset* tersebut berjumlah 17.770 Film.
* Tahun rilis film berada pada rentang tahun 1915 - 2005.
* Tidak terdapat nilai *null* pada tiap kolom.
* Sebagian besar film rilis ditahun 2000-an.

#### Analisis fitur pada *dataset* **ratings*
tabel 2. Informasi sebaran *dataset* *rating*
|       | **_User_ID_** | **_*Rating*_** | **_Movie_ID_** |
|------:|:-------------:|:------------:|:--------------:|
| _count_ |     100000.00 |    100000.00 |      100000.00 |
|  _mean_ |    1320551.54 |         3.59 |        2303.55 |
|  _std_  |     764802.55 |         1.07 |        1304.48 |
|  _min_  |          6.00 |         1.00 |           3.00 |
|  25%  |     661203.25 |         3.00 |        1175.00 |
|  50%  |    1316604.00 |         4.00 |        2342.00 |
|  75%  |    1983701.00 |         4.00 |        3433.00 |
|  _max_  |    2649335.00 |         5.00 |        4496.00 |

![Histogram Fitur *rating* *dataset* *rating*](https://github.com/royanfauzimaulana25/movie_recomendation/assets/99244948/1d08b042-047d-4ed0-a239-cbbc7082a44c)

Gambar 2. Histogram fitur *rating* pada *dataset* *ratings* 

Berdasarkan hasil analisis pada fitur *dataset movie* didapatkan *insight* sebagai berikut :
* Pada tabel diatas jumlah data yang terdapat pada *dataset* tersebut berjumlah 100.000 Film.
* *rating* film berada pada rentang tahun 1 - 5.
* Tidak terdapat nilai *null* pada tiap kolom.
* Sebagian besar *rating* yang diberikan ialah 3 dan 4.

Berdasarkan analisis univariate dapat disimpulkan bahwa *rating* 3 dan 4 dan tahun film 2000 akan menjadi film yang paling banyak direkomendasikan

## *Data Preparation*
Sebelum data dapat digunakan kedalam model, data perlu melalui beberapa persiapan yaitu sebagai berikut :

* Menyandikan (*encoding*) fitur *`user`* dan *`Movie_ID`* ke dalam *indeks integer* yang bertujuan untuk mewakili tiap fitur dalam nilai *integer* berurut agar mudah dipetakan dengan *dataset* transaksis yaitu *rating*.
* Memetakan *`User_ID`* dan *`Movie_ID`* ke dataframe yang berkaitan menyesuaikan encoding fitur sesuai data *rating*. Bertujuan untuk memudahkan model untuk mengkomputasi nilai yang berdekatan pada *layer embeding*.
* Membagi data menjadi data latih dan uji untuk mengukur performa pelatihan model dengan data fitur kolom *user* dan *movie* dan fitur target adalah kolom *rating* dengan rasio 80% : 20%.
* Mendapatkan beberapa hal terkait data seperti jumlah *user* pada *dataset*, jumlah *movie* dan mengubah *rating* menajdi *float* agar dapat dikomputasi oleh *model*.

## *Modeling*
Sistem yang dibangun oleh proyek ini adalah sistem rekomendasi sederhana berdasarkan *rating* yang diberikan oleh penonton untuk tiap film berbasis *collaborative filtering*. 

*Collaborative filtering* merupakan algoritma sistem rekomendasi yang memberikan *Top-N* rekomendasi berdasarkan *rating* yang telah diberikaan oleh penonton lainnya. Algoritma akan menentukan kemiripan suatu *movie* bila memiliki *rating* yang sesuai/mirip dengan penonton sebelumnya.

Misalkan, suatu *movie* A,B,C,D diberikan *rating* 4,3,3,5 dan jika seorang penonton memberikan *rating* 4 pada movie D maka algoritma akan merekomendasikan *movie* yang disesuaikan dengan *rating* dari penonton lainnya pada film tersebut. 

*Model* dibangun dengan arsitektur *keras model* sebagai berikut : 
![Recomennder Net](https://github.com/royanfauzimaulana25/movie_recomendation/assets/99244948/0252fb01-f9ea-4349-b1dd-daff119ad669)

Gambar 3. *RecommenderNet* Arsitektur

Data akan masuk kedalam *layer emmbeding* untuk dihitung kedekatannya yang kemudian masing masing *vector user* dan *movie* dikalikan pada operasi *dot product* yang kemudian ditambahkan bias *user* dan *movie*. Setelah itu nilai bobot tersebut dihitung ke dalam *layer neural network* dengan aktivasi fungsi *sigmoid*.

Adapaun *hyperparameter* dan alasan penggunaannya sebagai berikut : 
tabel 3. *Hyperparameter* yang digunakan 
| **_Hyperparameter_** |          **_Value_**          |                                      **_Details_**                                     |
|:--------------------:|:-----------------------------:|:--------------------------------------------------------------------------------------:|
| _Loss Function_      |      _BinaryCrossentropy_     | Penggunaan pada kasus *binary*, dimana *movie* direkomendasikan atau tidak             |
| _Optimizer_          | _Adam(Learning_rate = 0.001)_ | *Optimizer* yang cocok untuk arsitektur *neural network*                               |
| _Metrics_            |   _Root Mean Squared Error_   | Metrik untuk mengukur kemiripan hasil prediksi dan data aktual karena sensitif *outlier* |
| _Batch Size_         |               16              |                                                                                        |
| _Epoch_              |               20              |                                                                                        |

Hasil prediksi *Top*-5 rekomendasi yang diberikan sebagai berikut : 

Tabel 4.  Hasil *top* 5 rekomendasi dari *user* 709532
| **_Movie with Higt *rating* from user_** |    **_Top 10 Movie recommendation_**   |
|:--------------------------------------:|:--------------------------------------:|
| *Lilo and Stitch*                        | *The Autobiography of Miss Jane Pittman* |
| *Ray*                                    | *Stargate SG-1: Season 7*               |
| *The Mummy*                              | *Pee-Wee's Playhouse: Vol. 1*            |
| *Beverly Hills Cop*                      | *The Simpsons: Season 6*                 |
| *Elizabeth*                              | *The Twilight Zone: Vol. 33*             |


Kelebihan Algoritma :
* Kemampuan generalisir baik.
* Dapat mengatasi *Cold-Start problem*, yaitu kesulitan dalam meberikan rekomendasi dengan tidak ada data sebelumnya.

Kekurangan Algoritma : 
* Kompleksitas : membutuhkan banyak data.
* *Overfitting* : rawan terjadi *overfiiting*.

## *Evaluation*
Evaluasi hasil sistem rekomendasi menggunakan metrik *`Root Mean Squared Error`* untuk menentukan kemiripan *movie* yang diberikan.

Hasil pelatihan model mencapai konvergen yang dapat dilihat pada gambar 4.

![Trainin Test RMSE](https://github.com/royanfauzimaulana25/movie_recomendation/assets/99244948/973dcc7b-064a-46b3-9080-c4dfd7d52d6c)

Gambar 4. Hasil pelatihan pada metrik *RMSE* di data latih dan uji. 

Berdasarkan gambar 4, diketahui bahwa model sudah mencapai konvergen, namun dapat dikatakan bahwa model *overfiting* pada data latih karena nilai *RMSE* data latih cenderung lebih kecil ketimbang data uji dengan *gap* yang relatif jauh. Untuk mengatasi permasalahan tersebut, arsitektur model perlu mempertimbangkan menggunakan layer regularisasi lebih besar dan mengurangi kompleksitas arsitektur model. 

adapun formulanya sebagai berikut : 
$$\text{RMSE} = \sqrt{\frac{\sum_{i=1}^{n} (y_i - \hat{y}_i)^2}{n}}$$

Keterangan : 
* y_i adalah nilai sebenarnya dari titik data ke-i
* ŷ_i adalah nilai prediksi yang dibuat oleh algoritma untuk titik data ke-i
* n adalah jumlah titik data
* 

*`Root Mean Squared Error`* mengukur kemiripan hasil prediksi dengan data aktual nya dengan mengkuadratkan selisih nilai prediksi dan aktual yang dijumlahkan kemudian diakarkan dan dibagi n data untuk mendapatkan rata rata *error* nya. `Semakin kecil nilai *RMSE*, maka hasil yang diberikan semakin mirip`.

Nilai *RMSE* yang didapatkan dari hasil pelatihan pada data uji ialah sebesar `0.254`. Dimana nilai tersebut sudah mencapai target yaitu < 30% dan hasil rekomendasi sesuai dengan selera / minat penonton serta menjadi pemicu bagi penonton untuk mendapatkan pengalaman yang baik dan memperpanjang langganan pada *platform* distributor film tersebut yang menjadi keuntungan perusahaan.


## Kesimpulan
Berdasarkan hasil penelitian dan evaluasi yang telah dilakukan algoritma *collaborative filtering* yang dibangun menggunakan *model keras neural network* mampu memberikan rekomendasi *top-n movie* dengan capaian metrik `RMSE` sebesar `0.254` dan model dapat memberikan hasil rekomendasi sesuai dengan minat atau selera dari penonton sehingga berdampak pada langganan penggunaan *platform* distributor film yang menjadi keuntungan perusahaan. Namun model dikatakan lebih baik bila mendapatkan nilai *RMSE* sekecil mungkin. dan batasan pada model ini ialah kompleksitas model dan dibutuhkan data yang besar agar model tergeneralisir.

## Referensi 

[^1]: N. K. Ayyiyah, R. Kusumaningrum, dan R. Rismiyati, “Film Recommender System Menggunakan Metode *Neural Collaborative Filtering*,” Jurnal Teknologi Informasi dan Ilmu Komputer, vol. 10, no. 3. Fakultas Ilmu Komputer Universitas Brawijaya, hlm. 699–708, Jul 01, 2023. doi: 10.25126/jtiik.20231036616.
