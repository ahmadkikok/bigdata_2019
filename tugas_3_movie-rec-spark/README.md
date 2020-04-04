# Movie Recommendation Spark
Tugas 3 untuk melakukan Implementasi Algoritma Rekomendasi dengan menggunakan dataset movie reccomendation dan apache spark.

## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [CRISP-DM Framework](#2-crisp-dm-framework)
	- [Business Understanding](#21-business-understanding)
	- [Data Understanding](#22-data-understanding)
	- [Data Preparation](#23-data-preparation)
	- [Modeling](#24-modeling)
	- [Evaluation](#25-evaluation)
	- [Deployment](#26-deployment)
3. [CSV to Spark VS File Reader](#3-csv-to-spark-vs-file-reader)
4. [Referensi](#4-referensi)

## 1. Kebutuhan
- KNIME
- MovieLens 20M Dataset (http://files.grouplens.org/datasets/movielens/ml-20m.zip)


## 2. CRISP-DM Framework
Pada bagian ini saya akan menjelaskan proses melakukan rekomandasi film menggunakan framework yang telah ada.

### 2.1 Business Understanding
Untuk kasus penggunaan ini, saya menggunakan dataset MovieLens (20M), dataset ini berisi kumpulan data film secara umum dengan peringkat film yang dinilai oleh pengguna,
Dari data-data yang ada didalam datasets ini, mampu melakukan rekomandasi film terbaik yang telah dinilai oleh pengguna sebelumnya.


### 2.2 Data Understanding
Dataset ini berisi sejumlah data berbeda terkait dengan judul dan peringkat film. Di sini saya menggunakan file ratings.csv dan movies.csv.

Dataset dalam file ratings.csv berisi 20 juta peringkat film oleh sekitar 130.000 pengguna, dan terdiri dari: movieID (number), userID(number), rating(number), timestamp sebagai kolom.
Setiap baris berisi peringkat untuk setiap film, diidentifikasi oleh movieID, oleh salah satu pengguna, diidentifikasi oleh userID.

Dataset dalam file movies.csv berisi sekitar 27.000 film, terdiri dari : movieID(number), judul(text), genre(text).
Setiap baris berisi judul film, dan genre, yang diidentifikasi oleh movieID.


### 2.3 Data Preparation
Pada persiapan data, terdapat dua persiapan data yang dilakukan, dimana yang pertama adalah persiapan data menggunakan file reader, dan data menggunakan spark.

#### 2.3.1 File Reader
Pada data preparation untuk file reader, berikut adalah workflow yang dijalankan.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-1_data_prepation_model.png)

Pada konfigurasi untuk file reader, saya menggunakan datasets yang telah didownload sebelumnya, dengan menggunakan files movies.csv, pada node ini hanya membaca file saja.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-1_file_config.png)

Pada konfigurasi timestamp terdapat model tambahan didalamnya, untuk menambahkan UserID = 99999 dan Timestamp = 123.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-1_tmp_model.png)

Konfigurasi untuk penambahan UserID dan Timestamp, pada node ini dilakukan proses penambahan UserID untuk meminta UserID 99999 untuk menilai 20 film secara acak.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-1_tmp_model_cnf_1.png)
![](/tugas_3_movie-rec-spark/screenshoot/2-3-1_tmp_model_cnf_2.png)

Selanjutnya adalah konfigurasi row splitter, dimana data akan di pecah menjadi dua, data pertama adalah data yang akan dilakukan training, yaitu 20 film yang telah di pilih secara acak.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-1_row_splitter.png)

Hasil setelah node untuk meminta user menilai 20 film secara acak, didapatkan semua film -1 yang artinya belum pernah ditonton.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-1_row_splitter_result.png)

Selanjutnya adalah menjalan kedua node table to spark, dimana data yang telah berisi rate akan dikirimkan untuk di training, dan data yang tidak ada rate akan langsung di deploy nantinya, semua node telah berhasil di jalankan.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-1_final.png)

#### 2.3.2 Spark
Pada data preparation untuk spark, berikut adalah workflow yang dijalankan.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-2_data_prepation_model.png)

Pertama adalah dilakukan konfigurasi untuk local environtment spark.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-2_config_env.png)

Selanjutnya adalah melakukan konfigurasi untuk input file ratings .csv yang telah di download sebelumnya, kemudian dimasukan kedalam spark.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-2_csv_spark.png)

Selanjutnya adalah dilakukan partisi 80% - 20% pada data csv, hasil partisi tersebut yang nantinya akan digunakan pada training model data set.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-2_csv_part.png)

Selanjutnya adalah menjalankan training set dengan 80% adalah original film dan 20% film yang telah dinilai oleh pengguna, dan jalankan node ALS Model untuk melakukan collaborative filtering.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-2_als.png)

Hasil dari collaborative filtering serta semua node pada data preparation spark telah berhasil di lakukan.

![](/tugas_3_movie-rec-spark/screenshoot/2-3-2_result.png)

![](/tugas_3_movie-rec-spark/screenshoot/2-3-2_finish.png)


### 2.4 Modeling
Untuk penyelesain kasus ini, saya menggunakan model workflow yang telah disediakan pada sumber tugas.

![](/tugas_3_movie-rec-spark/screenshoot/2-4_movie-recommendations_model.png)


### 2.5 Evaluation
Pada evaluation dijalan model seperti yang ada pada model workflow.

![](/tugas_3_movie-rec-spark/screenshoot/2-5_evaluate_model.png)

Jalankan node spark predictor untuk generate prediksi rating pada test set, dengan test set adalah 20% original film.

![](/tugas_3_movie-rec-spark/screenshoot/2-5_evaluate_test_set.png)

Selanjutnya adalah menjalankan node remova NaN untuk menghapus data NaN atau prediksi yang hilang, setelah itu jalankan 
spark numeric scorer untuk menghitung kesalahan numerik antara peringkat film original dan peringkat yang diprediksi, semua node telah berhasil dijalankan.

![](/tugas_3_movie-rec-spark/screenshoot/2-5_result.png)

![](/tugas_3_movie-rec-spark/screenshoot/2-5_finish.png)


### 2.6 Deployment
Selanjutnya adalah tahap terakhir deploy model untuk memberikan top 10 prediksi film rekomendasi, akan menjalankan model seperti pada workflow.

![](/tugas_3_movie-rec-spark/screenshoot/2-6_deploy_model.png)

Lakukan hal yang seperti tahap evaluate dengan menjalankan node spark predictor.

![](/tugas_3_movie-rec-spark/screenshoot/2-6_predictor.png)

Kemudian jalankan spark to table untuk memindahkan data dari spark ke dalam tabel, setelah itu buka pada konfigurasi top 20 movies reccomendation, terdapat node tambahan seperti berikut
pada node ini akan dilakukan join film yang berasal dari dataset original dengan data yang telah diprediksi dan difilter.

![](/tugas_3_movie-rec-spark/screenshoot/2-6_add_node.png)

Berikut adalah konfigurasi pada row filter dimana pada filter pertama ini akan menghilangkan hasil prediksi NaN, sort filter secara Ascending, dan row filter untuk mengambil 10 top film, kemudian jalankan ketiga node tersebut.

![](/tugas_3_movie-rec-spark/screenshoot/2-6_row_config.png)

![](/tugas_3_movie-rec-spark/screenshoot/2-6_sort_config.png)

![](/tugas_3_movie-rec-spark/screenshoot/2-6_row2_config.png)

Selanjutnya adalah memasukan kembali film yang ada pada movies.csv untuk dilakukan join, berikut adalah konfigurasi file reader dan joiner, setelah itu jalankan kedua node.

![](/tugas_3_movie-rec-spark/screenshoot/2-6_fileread_config.png)

![](/tugas_3_movie-rec-spark/screenshoot/2-6_join_config.png)

Semua node pada node tambahan telah berhasil dijalankan.

![](/tugas_3_movie-rec-spark/screenshoot/2-6_add_node_res.png)

Kemudian jalankan node terakhir display recommendation, dan hasil dari rekomandasi yang diberikan adalah seperti berikut, dan semua node telah berhasil dijalankan.

![](/tugas_3_movie-rec-spark/screenshoot/2-6_result.png)

![](/tugas_3_movie-rec-spark/screenshoot/2-6_finish.png)


## 3. CSV to Spark VS File Reader
Pada bagian ini akan membahas perbedaan CSV to Spark dan File Reader

Pertama tambahkan node timer info pada workflow knime, kemudian input file yang sama pada file reader yang telah di sediakan untuk melakukan ujicoba.

![](/tugas_3_movie-rec-spark/screenshoot/3_file_nodef.png)

![](/tugas_3_movie-rec-spark/screenshoot/3_file_cnf.png)

Kemudian jalankan file reader, node csv to spark, serta timer info.

![](/tugas_3_movie-rec-spark/screenshoot/3_result_timer.png)

Terdapat perbedaan waktu eksekusi yang sangat jauh dimana file reader 251812 sedangkan csv to spark hanya membutuhkan 82202, hal ini terjadi dikarenkan ketika melakukan
eksekusi dari file reader, komputer akan melakukan eksekusi dengan jumlah data yang sangat besar, sehingga sangat membebani komputer itu sendiri, beda halnya dengan csv to spark
dimana spark sendiri merupakan open-source cluster framework computing, yang dibangun untuk pemrosesan big data dengan cepat sehingga terdapat waktu eksekusi yang jauh berbeda antara file reader dan spark.


## 4. Referensi
https://grouplens.org/datasets/movielens/                                           
https://www.knime.com/learning-hub                                                    
https://github.com/ahmadkikok/bigdata_2019/tree/master/tugas_1_etl-menggunakan-knime
https://www.knime.com/blog/movie-recommendations-with-spark-collaborative-filtering