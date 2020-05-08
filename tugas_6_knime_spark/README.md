# MLlib model to PMML dan Spark Compiled Model Predictor menggunakan KNIME
Tugas 6 adalah mencoba materi yang telah diberikan untuk menjalankan MLlib model to PMML dan Spark Compiled Model Predictor pada KNIME

## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [MLlib model to PMML](#2-mllib-model-to-pmml)
	- [Business Understanding](#21-business-understanding)
	- [Data Understanding](#22-data-understanding)
	- [Data Preparation](#23-data-preparation)
	- [Modeling](#24-modeling)
	- [Evaluation](#25-evaluation)
	- [Deployment](#26-deployment)
3. [Spark Compiled Model Predictor](#3-crisp-dm-framework)
	- [Business Understanding](#31-business-understanding)
	- [Data Understanding](#32-data-understanding)
	- [Data Preparation](#33-data-preparation)
	- [Modeling](#34-modeling)
	- [Evaluation](#35-evaluation)
	- [Deployment](#36-deployment)
4. [Referensi](#4-referensi)

## 1. Kebutuhan
- KNIME

## 2. MLlib model to PMML
Berikut adalah workflow yang akan sayang jalankan dan jelaskan pada MLlib model to PMML

![](/tugas_6_knime_spark/screenshoot/1/1.PNG)


### 2.1 Business Understanding
Data test yang digunakan pada workflow ini adalah data Iris, sehingga kemungkinan proses yang dapat dilakukan pada data ini adalah melakukan klasifikasi terhadap bunga iris.


### 2.2 Data Understanding
Datasets ini berisi sejumlah data yang berisi spesifikasi dari sebuah bunga, baik dari besar kelopak, atau bunganya, serta class dari bunga tersebut.

Terdapat 5 kolom data dengan :
- sepal length sebuah angka float yang menjelaskan panjang sepal
- sepal width sebuah angka float yang menjelaskan lebar sepal
- petal length sebuah angka float yang menjelaskan panjang petal
- petal width sebuah angka float yang menjelaskan lebar petal
- class sebuah karakter yang menjelaskan kelas dari bunga tersebut


### 2.3 Data Preparation

![](/tugas_6_knime_spark/screenshoot/1/2.PNG)

Pada data preparation ini adalah mempersiapkan data, yaitu dengan cara membuat sebuah local environtment serta data yang ingin di persiapkan, yang kemudian ditampilkan dalam bentuk table yang nantinya akan digunakan dalam modeling, hasil dari table tersebut adalah

![](/tugas_6_knime_spark/screenshoot/1/3.PNG)


### 2.4 Modeling
Selanjutnya adalah melakukan modeling untuk melakukan train pada table sebelumnya dalam spark k-means, workflow yang akan dijalankan adalah

![](/tugas_6_knime_spark/screenshoot/1/4.PNG)

Berikut adalah konfigurasi dari spark k-mean, yaitu dengan melakukan train pada 4 kolom antara lain

![](/tugas_6_knime_spark/screenshoot/1/5.PNG)

Hasil dari train tersebut nantinya akan di rubah dalam bentuk PMML yang kemudian dilakukan convert dalam bentuk Java agar data bisa digunakan nantinya.


### 2.5 Evaluation
Pada evaluation akan dijalankan workflow

![](/tugas_6_knime_spark/screenshoot/1/6.PNG)

Akan dilakukan evaluasi dengan mencoba melakukan prediksi dengan menambahkan sebuah data test baru. 

![](/tugas_6_knime_spark/screenshoot/1/7.PNG)

Hasil dari prediksi, serta hasil dari entropynya adalah

![](/tugas_6_knime_spark/screenshoot/1/8.PNG)

![](/tugas_6_knime_spark/screenshoot/1/9.PNG)


### 2.6 Deployment
Selanjutnya pada tahap deployment kita akan menjalankan workflow

![](/tugas_6_knime_spark/screenshoot/1/10.PNG)

Dari data yang sebelumnya telah di prediksi, kita akan mencoba menambakan sebuah inputan berbentuk json, yang nantinya dari inputan tersebut akan diketahui bunga tersebut berada pada klasifikasi yang mana.

![](/tugas_6_knime_spark/screenshoot/1/11.PNG)

Yang kemudian data dari inputan tersebut akan dirubah dalam bentu table terlebih dahulu agar bisa dibaca pada node selanjutnya.

![](/tugas_6_knime_spark/screenshoot/1/12.PNG)

Selanjutnya adalah dilakuakn compiled model predictor untuk mengetahui data yang diinput sebelumnya berada pada klasifikasi mana, yang kemudian didapatkan hasilnya

![](/tugas_6_knime_spark/screenshoot/1/13.PNG)

Kemudian dari hasil tersebut data akan dikembalikan dalam bentuk json, sehingga dilakukan convert dari table ke json, yang kemudian dilakukan output json yang menghasilkan

![](/tugas_6_knime_spark/screenshoot/1/14.PNG)

Didapatkan bahwa inputan JSON diawal berada pada cluster 0.


## 3. Spark Compiled Model Predictor
Berikut adalah workflow yang akan sayang jalankan dan jelaskan pada Spark Compiled Model Predictor

![](/tugas_6_knime_spark/screenshoot/2/1.PNG)


### 3.1 Business Understanding
Data test yang digunakan pada workflow ini adalah data Iris, sehingga kemungkinan proses yang dapat dilakukan pada data ini adalah melakukan klasifikasi terhadap bunga iris.


### 3.2 Data Understanding
Datasets ini berisi sejumlah data yang berisi spesifikasi dari sebuah bunga, baik dari besar kelopak, atau bunganya, serta class dari bunga tersebut.

Terdapat 5 kolom data dengan :
- sepal length sebuah angka float yang menjelaskan panjang sepal
- sepal width sebuah angka float yang menjelaskan lebar sepal
- petal length sebuah angka float yang menjelaskan panjang petal
- petal width sebuah angka float yang menjelaskan lebar petal
- class sebuah karakter yang menjelaskan kelas dari bunga tersebut


### 3.3 Data Preparation
Pada tahap data preparation ini kita akan mempersiapkan data yang dilakukan learner kedalam PMML Cell, workflow yang akan dijalankan adalah

![](/tugas_6_knime_spark/screenshoot/2/2.PNG)

Dilakukan decision tree learner dengan konfigurasi dan hasil sebagai berikut

![](/tugas_6_knime_spark/screenshoot/2/3.PNG)

![](/tugas_6_knime_spark/screenshoot/2/4.PNG)

Kemudian dilakuakn MLP learner dengan class sebagai table yang dilakukan learner dan didapatkan hasil errorprop nya adalah sebagai berikut

![](/tugas_6_knime_spark/screenshoot/2/5.PNG)

![](/tugas_6_knime_spark/screenshoot/2/6.PNG)


### 3.4 Modeling
Selanjutnya kita akan menjalankan model dengan workflow

![](/tugas_6_knime_spark/screenshoot/2/7.PNG)

Pada tahap ini dilakukan penggabungan(append) dari 2 table cell sebelumnya, yang selanjutnya dari hasil penggabungan tersebut dirubah dalam bentuk PMML Ensemble untuk dilakukan compiler.


### 3.5 Evaluation
Pada tahap ini kita akan menjalankan model dengan workflow

![](/tugas_6_knime_spark/screenshoot/2/8.PNG)

Tahap ini dilakukan penambahan data untk dilakukan test yang kemudian data tersebut buat dalam bentuk table yang berisi

![](/tugas_6_knime_spark/screenshoot/2/9.PNG)

Setelah itu dilakukan prediksi secara massal didalam hadoop dan memiliki hasil

![](/tugas_6_knime_spark/screenshoot/2/10.PNG)

Data yang diprediksi persis seperti klasifikasi yang sudah ada, yang artinya prediksi cukup akurat


### 3.6 Deployment
Selanjutnya adalah menjalankan workflow

![](/tugas_6_knime_spark/screenshoot/2/11.PNG)

Pada tahap ini kita merubah data pada spark menjadi dalam bentuk table yang kemudian dilakukan scorer yang menghasilkan

![](/tugas_6_knime_spark/screenshoot/2/12.PNG)

Didapatkan terdapat 25 iris dan versi-color tanpa adanya klasifikasi yang salah, sedangkan pada virginica terdapat kesalhaan prediksi sebesar 4, yaitu dimana 4 data dibaca sebagai versi-color. Akurasi dari hasil prediksi adalah sebesar 94,667% dengan error 5.333%.


## 4. Referensi
https://archive.ics.uci.edu/ml/datasets/iris                                                                      
https://www.knime.com/learning-hub                                                                    
https://github.com/ahmadkikok/bigdata_2019/tree/master/tugas_1_etl-menggunakan-knime                      
https://hub.knime.com/knime/spaces/Examples/latest/10_Big_Data/02_Spark_Executor/02_Mass_Learning_Event_Prediction_MLlib_to_PMML                                                   
https://hub.knime.com/knime/spaces/Examples/latest/10_Big_Data/02_Spark_Executor/03_PMML_to_Spark_Comprehensive_Mode_Learning_Mass_Prediction