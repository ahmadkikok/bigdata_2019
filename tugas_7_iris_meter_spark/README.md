# Melakukan Analisa Iris Meter Spark Menggunakan KNIME
Tugas 7 mencoba melakukan analisa iris meter spark mengugnakan KNIME.

## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [Iris Meter Spark](#2-iris-meter-spark)
	- [Business Understanding](#21-business-understanding)
	- [Data Understanding](#22-data-understanding)
	- [Data Preparation](#23-data-preparation)
	- [Modeling](#24-modeling)
	- [Evaluation](#25-evaluation)
	- [Deployment](#26-deployment)
3. [Referensi](#4-referensi)

## 1. Kebutuhan
- KNIME

## 2. Iris Meter Spark
Workflow yang akan dijalankan pada tugas ini adalah sebagai berikut

![](/tugas_7_iris_meter_spark/screenshoot/1.PNG)

Workflow ini berisi 3 meta node diantara lain ``Load Data Node``, ``Extract date-time attributes``, ``Aggregation and time series``.

### 2.1 Business Understanding
Data test yang digunakan pada workflow ini adalah data Irish Energy Meter, sehingga kemungkinan proses yang dapat dilakukan pada data ini adalah melakukan analisa terhadap penggunaan meteran energi di irlandia.


### 2.2 Data Understanding
Datasets ini berisi sejumlah data yang berisi penggunaan meter energi irlandia.

Terdapat 3 kolom data dengan :
- meterID sebuah integer yang menjadi id utama pada data ini.
- enc_datetime sebuah integer yang berisi waktu yang telah di konversi.
- reading sebuah float yang berisi data meter energi.


### 2.3 Data Preparation

![](/tugas_7_iris_meter_spark/screenshoot/2.PNG)

Pada data preparation kita akan mempersiapkan data sets yang telah disediakan pada KNIME, yang akan disiapkan dalam bentuk spark nantinya.
Node yang dijalankan pertama kali adalah file manger, yaitu akan dilakukan load data yang berasa dari knime, kemudian membuat local env yang kemudian akan jalankan Meta Node ``Load Data``.

![](/tugas_7_iris_meter_spark/screenshoot/5.PNG)

![](/tugas_7_iris_meter_spark/screenshoot/3.PNG)

Meta node ``Load Data`` berisi 2 node, node ini akan melakukan pembuatan table pada hive serta melakukan load table yang telah dibuat, hasil table yang telah di buat adalah sebagai berikut.

![](/tugas_7_iris_meter_spark/screenshoot/4.PNG)

Selanjutnya yang dilakukan adalah merubah table hive tadi menjadi spark, dengan menjalankan node ``Hive to Spark``, hasil dari table spark tersebut adalah

![](/tugas_7_iris_meter_spark/screenshoot/6.PNG)


### 2.4 Modeling
Selanjutnya adalah melakukan modeling untuk merubah isi table yang ada, yang nantinya akan dilakukan pemecahan data untuk dilakukan analisa, workflow yang dijalankan adalah

![](/tugas_7_iris_meter_spark/screenshoot/7.PNG)

Pada workflow ini kita akan menjalankan meta node ``Extract time`` node ini akan melakukan pemisahan data yang nantinya akan di lakukan analisa, isi dari node ``Extract time`` ini adalah sebagai berikut

![](/tugas_7_iris_meter_spark/screenshoot/8.PNG)

Pada node ini, pertama kita akan melakukan konversi waktu int kedalam bentuk Date, node yang dijalankan adalah node ``Spark SQL Query``

![](/tugas_7_iris_meter_spark/screenshoot/9.PNG)

Pada query ini melakukan select data yang berada pada table, yang kemudian data enc_datetime dilakukan konversi, sehingga menghasilkan

![](/tugas_7_iris_meter_spark/screenshoot/10.PNG)

Selanjutnya adalah melakukan ekstrasi waktu baru untuk mendapatkan tahun, bulan, minggu, hari, dengan menjalankan ``Spark SQL Query``, dengan query yang berisi

```
SELECT 

meterid,
kw30,
eventDate,
year(eventDate) as year,
month(eventDate) as month,
weekofyear(eventDate) as week,
date_format(eventDate, 'EEEE') as dayOfWeek,
hour(my_time) as hour

FROM #table# t1
```

Pada query ini melakukan select year, month, week, dayofweek yang diambil dari eventDate (date yang telah diekstrasi pada node sebelumnya), sehingga menghasilkan

![](/tugas_7_iris_meter_spark/screenshoot/11.PNG)

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

![](/tugas_7_iris_meter_spark/screenshoot/12.PNG)

Selanjutnya adalah melakukan pemisahan jam dengan menjalankan node query yang berisi

```
SELECT meterID, kw30, eventDate, year, month, week, dayOfWeek, dayClassifier, hour,
CASE 
WHEN hour >=7 AND hour <9 THEN '7-9'
WHEN hour >=9 AND hour <13 THEN '9-13' 
WHEN hour >=13 AND hour <17 THEN '13-17' 
WHEN hour >=17 AND hour <21 THEN '17-21' 
WHEN hour >=21 OR hour <7 THEN '21-7'  
								 
END as daySegment

from #table#
```

Pada query ini akan melakukan klasifikasi berdasarkan jam, dimana harinya akan di buat menjadi beberapa segment seperti pada query, sehingga menghasilkan 

![](/tugas_7_iris_meter_spark/screenshoot/13.PNG)

Semua node telah dijalankan, hasil column nantinya akan dilakukan analisa pada meta node ``Aggregation and time series``, meta node ini berisi sejumlah node seperti berikut

![](/tugas_7_iris_meter_spark/screenshoot/14.PNG)
![](/tugas_7_iris_meter_spark/screenshoot/15.PNG)

Pada node ini, akan dilakukan analisa dengan menghitung rata rata dari data yang dianalisa, saya akan menjelaskan beberapa node, dikarenakan node yang lain melakukan hal yang sama, hanya berbeda pada column yang di analisa.

![](/tugas_7_iris_meter_spark/screenshoot/16.PNG)

Pada node diatas dilakukan group berdasarkan ``total usage`` dan ``usage by year``, akan melakukan penghitungan berdasarkan kw30 seperti pada gambar

![](/tugas_7_iris_meter_spark/screenshoot/17.PNG)

![](/tugas_7_iris_meter_spark/screenshoot/18.PNG)

![](/tugas_7_iris_meter_spark/screenshoot/20.PNG)

Sedangkan ``usage by year``

![](/tugas_7_iris_meter_spark/screenshoot/19.PNG)

![](/tugas_7_iris_meter_spark/screenshoot/21.PNG)

Selanjutnya pada ``avg by year`` dilakukan penghitungan rata-rata berdasarkan tahun

![](/tugas_7_iris_meter_spark/screenshoot/22.PNG)

Hasil dari rata-rata adalah sebagai berikut

![](/tugas_7_iris_meter_spark/screenshoot/23.PNG)

Selanjutnya adalah melakukan column rename, pada column rata=rata, sesuai nama yang kita mau

![](/tugas_7_iris_meter_spark/screenshoot/24.PNG)

Selanjutnya dilakukan join pada data sebelumnya, sehingga menghasilkan

![](/tugas_7_iris_meter_spark/screenshoot/25.PNG)

Data yang di join adalah data yang berasal dari ``usage by year`` dan ``total usage``, selanjutnya adalah lakukan hal yang sama pada node lainnya, setelah melakukan analisa rata-rata lakukan join pada table tersebut, sehingga hasil akhir dari table yang telah selesai di proses adalah sebagai berikut

![](/tugas_7_iris_meter_spark/screenshoot/26.PNG)

Data hasil akhir didapat dari data yang telah dipisah sebelumnya yang kemudian dilakukan analisa berdasarkan kebut

### 2.5 Evaluation
Pada evaluation akan dijalankan workflow

![](/tugas_7_iris_meter_spark/screenshoot/27.PNG)

Pada tahap ini dilakukan penghitungan dari hasil yang didapatkan dengan menjalankan 

![](/tugas_7_iris_meter_spark/screenshoot/28.PNG)

Pada tahap ini dilakukan komputasi penghitungan percent berdasarkan hari atau segmen hari, hasil dari komputasi adalah sebagai berikut

![](/tugas_7_iris_meter_spark/screenshoot/29.PNG)

Selanjutnya adalah melakukan Plot K-Mean, PCA, dan memiliki hasil sebagai berikut

![](/tugas_7_iris_meter_spark/screenshoot/30.PNG)
![](/tugas_7_iris_meter_spark/screenshoot/31.PNG)


### 2.6 Deployment
Selanjutnya pada tahap deployment kita akan menjalankan workflow

![](/tugas_7_iris_meter_spark/screenshoot/32.PNG)

Pada tahap ini dilakukan perubahan data dari spark kembali mejadi hive serta menympan spark kedalam HDFS dalam bentuk parquet, hasil dari data tersebut adalah sebagai berikut

![](/tugas_7_iris_meter_spark/screenshoot/33.PNG)


## 3. Referensi                                                                
https://www.knime.com/learning-hub                                                                    
https://github.com/ahmadkikok/bigdata_2019/tree/master/tugas_1_etl-menggunakan-knime                      
https://hub.knime.com/knime/spaces/Examples/latest/10_Big_Data/02_Spark_Executor/09_Big_Data_Irish_Meter_on_Spark_only