# Tugas EAS dengan Menggunakan 4 Datasets
Tugas EAS ini berisi analisa mengenai 4 datasets time series yang berbeda.

## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [Pendahuluan](#2-pendahuluan)
3. [Daily Minimum Temperature](#3-daily-minimum-temperature)
	- [Business Understanding](#31-business-understanding)
	- [Data Understanding](#32-data-understanding)
	- [Data Preparation](#33-data-preparation)
	- [Modeling](#34-modeling)
	- [Evaluation](#35-evaluation)
	- [Deployment](#36-deployment)
4. [Electric Production](#4-electric-production)
	- [Business Understanding](#41-business-understanding)
	- [Data Understanding](#42-data-understanding)
	- [Data Preparation](#43-data-preparation)
	- [Modeling](#44-modeling)
	- [Evaluation](#45-evaluation)
	- [Deployment](#46-deployment)
5. [Monthly Beer](#5-monthly-beer)
	- [Business Understanding](#51-business-understanding)
	- [Data Understanding](#52-data-understanding)
	- [Data Preparation](#53-data-preparation)
	- [Modeling](#54-modeling)
	- [Evaluation](#55-evaluation)
	- [Deployment](#56-deployment)
6. [Sales Shampoo](#6-sales-shampoo)
	- [Business Understanding](#61-business-understanding)
	- [Data Understanding](#62-data-understanding)
	- [Data Preparation](#63-data-preparation)
	- [Modeling](#64-modeling)
	- [Evaluation](#65-evaluation)
	- [Deployment](#66-deployment)
7. [Referensi](#7-referensi)

## 1. Kebutuhan
- KNIME
- Time Series Datasets (https://www.kaggle.com/shenba/time-series-datasets)


## 2. Pendahuluan
Pada tugas eas ini, akan dilakukan analisa pada 4 datasets time series, antara lain :
- Daily minimum temperature
- Electric Production
- Monthly Beer
- Sales Shampoo

Dimana workflow yang dibuat adalah sebagai berikut :

![](/tugas_8_eas/screenshoot/1.PNG)

Pada workflow ini, create env local akan dibuat sebagai spark, dimana dihubungkan ke tiap metanode, tiap metanode berisi workflow yang sama, dengan konfigurasi yang berbeda, workflow dari tiap node adalah sebagai berikut :

![](/tugas_8_eas/screenshoot/2.PNG)

workflow diatas berisi pada setiap node, yang akan dijelaskan pada tahapan berikutnya, penjelasan disesuaikan berdasarkan data yang dianalisa, dikarenakan data yang di analisa berbeda.


## 2. Daily Minimum Temperature
Workflow yang akan dijalankan pada tugas ini adalah sebagai berikut

![](/tugas_8_eas/screenshoot/1/1.PNG)

Workflow ini berisi 3 meta node diantara lain ``Load Data Node``, ``Extract date-time attributes``, ``Aggregation and time series``.


### 2.1 Business Understanding
Data test yang digunakan pada workflow ini adalah data Time Series Daily Minimum Temperature, sehingga kemungkinan proses yang dapat dilakukan pada data ini adalah melakukan analisa terhadap minimal temperature, analisa yang dilakukan adalah sebagai berikut :
- Analisa berdasarkan Tahun (Total Tahun)
- Analisa berdasarkan Bulan (Rata-rata tiap Bulan)
- Analisa berdasarkan Minggu (Rata-rata tiap Minggu)
- Analisa berdasarkan Hari dalam Seminggu (Rata-rata tiap Hari Senin-Minggu, Rata-rata tiap hari)
- Analisa berdasarkan Hari Libur dan Hari Kerja (Rata-rata tiap Hari Libur dan Kerja)


### 2.2 Data Understanding
Datasets ini berisi sejumlah data yang berisi penggunaan minimum temp.

Terdapat 3 kolom data dengan :
- id_daily sebuah integer yang menjadi id utama pada data ini.
- tempdate sebuah integer yang berisi waktu dalam bentuk string.
- dailymintemp sebuah double yang berisi data mintemp.


### 2.3 Data Preparation
![](/tugas_8_eas/screenshoot/1/2.PNG)

Pada data preparation kita akan mempersiapkan data sets yang telah ditambahkan id dengan menggunakan database, data sudah berada pada folder /files/ yang disiapkan dalam bentuk spark nantinya.
Node yang dijalankan pertama kali adalah file manger, yaitu akan dilakukan load data yang berasa dari knime, kemudian membuat local env yang kemudian akan jalankan Meta Node ``Load Data``.

![](/tugas_8_eas/screenshoot/1/3.PNG)

![](/tugas_8_eas/screenshoot/1/4.PNG)

Meta node ``Load Data`` berisi 2 node, node ini akan melakukan pembuatan table pada hive serta melakukan load table yang telah dibuat, hasil table yang telah di buat adalah sebagai berikut.

![](/tugas_8_eas/screenshoot/1/5.PNG)

Daily Temp Minimum akan disimpan pada hive dengan nama table daily_temperature.

![](/tugas_8_eas/screenshoot/1/6.PNG)

Selanjutnya yang dilakukan adalah merubah table hive tadi menjadi spark, dengan menjalankan node ``Hive to Spark``, hasil dari table spark tersebut adalah

![](/tugas_8_eas/screenshoot/1/7.PNG)


### 2.4 Modeling
Selanjutnya adalah melakukan modeling untuk merubah isi table yang ada, yang nantinya akan dilakukan pemecahan data untuk dilakukan analisa, workflow yang dijalankan adalah

![](/tugas_8_eas/screenshoot/1/10.PNG)

Pada workflow ini kita akan menjalankan meta node ``Extract time`` node ini akan melakukan pemisahan data yang nantinya akan di lakukan analisa, isi dari node ``Extract time`` ini adalah sebagai berikut

![](/tugas_8_eas/screenshoot/1/8.PNG)

Pada node ini, pertama kita akan merubah date yang berbentuk string kedalam bentuk Date, node yang dijalankan adalah node ``Spark SQL Query``

![](/tugas_8_eas/screenshoot/1/9.PNG)

Pada query ini melakukan select data yang berada pada table, yang kemudian data date dilakukan perubahan dari string menjadi bentuk date, sehingga menghasilkan

![](/tugas_8_eas/screenshoot/1/11.PNG)

Pada kolom newdate, hasil perubahan -1 hari dari hari aslinya, hal ini terjadi karena sewaktu perubahan dilakukan konversi dalam bentuk timestamp, tapi tidak akan terpengaruh dengan hasil yang bulan, tahun, dan harinya. Selanjutnya adalah melakukan ekstrasi waktu baru untuk mendapatkan tahun, bulan, minggu, hari, dengan menjalankan ``Spark SQL Query``, dengan query yang berisi

```
SELECT 
`id_dailymintemp`,
`temp`,
`tempdate`,
year(`newdate`) as year,
month(`newdate`) as month,
weekofyear(`newdate`) as week,
date_format(`newdate`, 'EEEE') as dayOfWeek

FROM #table# t1
```

Pada query ini melakukan select year, month, week, dayofweek yang diambil dari newdate (date yang telah diekstrasi pada node sebelumnya), sehingga menghasilkan

![](/tugas_8_eas/screenshoot/1/12.PNG)

Selanjutnya adalah melakukan select baru dengan menjalankan query

```
SELECT *, 
CASE 
WHEN dayOfWeek in ('Saturday','Sunday') 	THEN 'WE' 
									        ELSE 'BD' 
END as dayClassifier

from #table#
```

Query ini akan melakukan pembuatan column baru, dengan value nya diambil dari dayOfWeek sebelumnya dengan kondisi dimana Saturday dan Sunday akan di isi dengan WE sedangkan hari lain di isi dengan BD, sehingga menghasilkan

![](/tugas_8_eas/screenshoot/1/13.PNG)

Semua node telah dijalankan, hasil column nantinya akan dilakukan analisa pada meta node ``Aggregation and time series``, meta node ini berisi sejumlah node seperti berikut

![](/tugas_8_eas/screenshoot/1/13.PNG)

Pada node ini, akan dilakukan analisa dengan menghitung rata rata dari data yang dianalisa, saya akan menjelaskan beberapa node, dikarenakan node yang lain melakukan hal yang sama, hanya berbeda pada column yang di analisa.

![](/tugas_8_eas/screenshoot/1/14.PNG)

Pada node diatas dilakukan group berdasarkan ``total usage`` dan ``usage by year``, akan melakukan penghitungan berdasarkan temp seperti pada gambar

![](/tugas_8_eas/screenshoot/1/15.PNG)

![](/tugas_8_eas/screenshoot/1/16.PNG)

![](/tugas_8_eas/screenshoot/1/17.PNG)

Sedangkan ``usage by month``

![](/tugas_8_eas/screenshoot/1/18.PNG)

![](/tugas_8_eas/screenshoot/1/19.PNG)

Selanjutnya pada ``avg by month`` dilakukan penghitungan rata-rata berdasarkan bulan, dengan group by tahun

![](/tugas_8_eas/screenshoot/1/20.PNG)

Hasil dari rata-rata adalah sebagai berikut

![](/tugas_8_eas/screenshoot/1/21.PNG)

Selanjutnya adalah melakukan column rename, pada column rata-rata, sesuai nama yang kita mau

![](/tugas_8_eas/screenshoot/1/22.PNG)

Selanjutnya dilakukan join pada data sebelumnya, sehingga menghasilkan

![](/tugas_8_eas/screenshoot/1/23.PNG)

Data yang di join adalah data yang berasal dari ``usage by month`` dan ``total usage``, selanjutnya adalah lakukan hal yang sama pada node lainnya, setelah melakukan analisa rata-rata lakukan join pada table tersebut, sehingga hasil akhir dari table yang telah selesai di proses adalah sebagai berikut

![](/tugas_8_eas/screenshoot/1/24.PNG)

Data hasil akhir didapat dari data yang telah dipisah sebelumnya yang kemudian dilakukan analisa berdasarkan kebutuhan, inti dari metanode ini adalah untuk mendapatkan analisa berdasarkan data yang telah didapatkan pada metanode sebelumnya.


### 2.5 Evaluation
Pada evaluation akan dijalankan workflow

![](/tugas_8_eas/screenshoot/1/25.PNG)

Pada tahap ini dilakukan penghitungan dari hasil yang didapatkan dengan menjalankan 

![](/tugas_8_eas/screenshoot/1/26.PNG)

Pada tahap ini dilakukan select semua data dari node sebelumnya.

![](/tugas_8_eas/screenshoot/1/27.PNG)

Selanjutnya adalah melakukan Plot K-Mean, PCA, dan memiliki hasil sebagai berikut

![](/tugas_8_eas/screenshoot/1/28.PNG)
![](/tugas_8_eas/screenshoot/1/29.PNG)


### 2.6 Deployment
Selanjutnya pada tahap deployment kita akan menjalankan workflow

![](/tugas_8_eas/screenshoot/1/30.PNG)

Pada tahap ini dilakukan perubahan data dari spark kembali mejadi hive serta menympan spark kedalam HDFS dalam bentuk parquet, hasil dari data tersebut adalah sebagai berikut

![](/tugas_8_eas/screenshoot/1/31.PNG)


## 7. Referensi                                                                
https://www.knime.com/learning-hub                                                                    
https://github.com/ahmadkikok/bigdata_2019/tree/master/tugas_1_etl-menggunakan-knime                      
https://github.com/ahmadkikok/bigdata_2019/tree/master/tugas_7_iris_meter_spark                      
https://hub.knime.com/knime/spaces/Examples/latest/10_Big_Data/02_Spark_Executor/09_Big_Data_Irish_Meter_on_Spark_only