# Eksplorasi KNIME BigData
Pada tugas ni saya melakukan eksplorasi dari tugas yang diberikan, dengan menggunakan DB tradisional dan Hive, serta dilakukan pengecekan melalui DBClient.

## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [DB](#2-DB)
	- [01_DB_Connect](#01_DB_Connect)
	- [02_DB_InDB_Processing_Exercise](#02_DB_InDB_Processing_Exercise)
	- [03_DB_Modelling_Exercise](#03_DB_Modelling_Exercise)
	- [04_DB_WritingToDB_Exercise](#04_DB_WritingToDB_Exercise)
3. [Data Understanding](#3-data-understanding)
4. [Data Preparation](#4-data-preparation)
5. [Modeling](#5-modeling)
6. [Evaluation](#6-evaluation)
7. [Deployment](#7-deployment)
8. [Referensi](#8-referensi)


## 1. Kebutuhan
- KNIME
- DBClient (Saya menggunakan DBeaver)

## 2. DB
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1_Rename_Table.PNG)

Sebelum melakukan eksplorasi, saya memeberikan prefix pada semua table yang ada, sehingga masing masing table diawali 05111640000157.


## 2.1 01_DB_Connect
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/01_Import_Connection.PNG)

Import koneksi SQLite yang telah disediakan pada file data kemudian jalankan koneksi tersebut.

![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/02_Add_Node_Table_Selector.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/02_Configure_Select_SS13PME.PNG)

Tambahkan node table selector untuk melakukan seleksi pada table yang diinginkan, kemudian pada konfigurasi saya melakukan select pada table 05111640000157.ss13pme.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/03_Add_DB_Reader.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/03_DB_Result.PNG)

Tambahkan node db reader agar data yang telah di select dapat di baca, kemudian lihat hasil result, data telah berhasil di tambahkan.

## 2.2 02_DB_InDB_Processing_Exercise
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/01_Select_Ss13pme_Ss13hme.PNG)

Buat 2 node table selector untuk memilih table ss13pme dan table ss13hme.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/02_Add_DB_Join.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/02_Configure_Join_On_SerialNo.PNG)

Tambahkan node join untuk melakukan join kedua table, kedua table tersebut dijoin dengan menggunakan inner join pada var ``serialno``.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/03_Add_DB_Reader.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/03_DB_Result.PNG)

Tambahkan node DB reader untuk membaca hasil yang telah dijoin tadi, dan lihatlah hasil dari join tersebut, data telah berhasil di join.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/04_Add_RowFilter_CowNullisNull.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/04_Configuration_Cow_Not_Null.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/04_Configuration_Cow_Null.PNG)

Tambahkan 2 node row filter untuk melakukan filter pada table tersebut, dimana row filter sebut dibuat menjadi 2, yaitu ``cow`` null dan ``cow`` not null, kemudian dilakukan konfigurasi kepada keduanya, dimana node 1 akan dilakukan setup cow is null, dan satunya setup cow is not null.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/05_Create_Reader.PNG)

Pada masing masing hasil row filter buat table reader untuk membaca table tersebut, kemudian jalankan dan lihat hasil dari table yang telah di filter.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/06_Add_GroupBy.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/06_GroupBy_Configure.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/06_GroupBy_Configure_2.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/2/06_Result.PNG)

Selanjutnya adalah menambahkan node group by untuk melakukan group pada kolom ``sex`` dan dilakukan penghitungan rata-rata(avg), setelah itu jalankan dan lihat hasil dari data ``sex`` yang telah di rata-ratakan.

## 2.3 03_DB_Modelling_Exercise
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/01_Add_Decision_Tree.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/01_Add_Decision_Tree_Configure.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/01_Add_Decision_Tree_Result.PNG)

Tambahkan node decision tree learner untuk melakukan learn pada data, pada kasus ini saya menggunakan gini index dan akan melakukan train pada cow, jalankan dan lihat hasil dari train yang telah dilakukan.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/02_Add_Decision_Predictor.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/02_Add_Decision_Predictor_Result.PNG)

Tambah node decision predictor dan lakukan prediksi dari data yang telah di train dan data yang telah dibaca sebelumnya.

## 2.4 04_DB_WritingToDB_Exercise
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/4/01_Add_Table_Writer.PNG)

Tambahkan node table writer dan lakukan konfigurasi untuk membuat table baru dengan nama 05111640000157.ss13pme_original, ini adalah table yang dibuat untuk backup.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/4/02_Add_DB_Update.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/4/02_Add_DB_Update_Configure.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/4/02_Add_DB_Update_Result.PNG)

Tambahkan node db update untuk melakukan update data ke database dimana untuk mengisi kolom ``serial no`` berdasarkan kolom ``cow`` yang telah di prediksi sebelumnya, jalankan kemudian kolom null akan terisi dengan hasil prediksi yang telah di lakukan sebelumnya.


