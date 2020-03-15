# Eksplorasi KNIME BigData
Pada tugas ni saya melakukan eksplorasi dari tugas yang diberikan, dengan menggunakan DB tradisional dan Hive, serta dilakukan pengecekan melalui DBClient.

## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [DB](#2-DB)
	- [01_DB_Connect](#21-01_db_connect)
	- [02_DB_InDB_Processing_Exercise](#22-02_db_indb_processing_exercise)
	- [03_DB_Modelling_Exercise](#23-03_db_modelling_exercise)
	- [04_DB_WritingToDB_Exercise](#24-04_db_writingtodb_exercise)
3. [Hadoop](#3-Hadoop)
	- [00_Setup_Hive_Table](#31-00_setup_hive_table)
	- [01_Hive_Modeling_Exercise](#32-01_Hive_Modeling_Exercise)
	- [02_Hive_WritingtoDB_Exercise](#33-02_Hive_WritingtoDB_Exercise)

## 1. Kebutuhan
- KNIME
- DBClient (Saya menggunakan DBeaver)

## 2. DB
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1_Rename_Table.PNG)

Sebelum melakukan eksplorasi, saya memeberikan prefix pada semua table yang ada, sehingga masing masing table diawali 05111640000157.


### 2.1 01_DB_Connect
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/01_Import_Connection.PNG)

Import koneksi SQLite yang telah disediakan pada file data kemudian jalankan koneksi tersebut.

![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/02_Add_Node_Table_Selector.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/02_Configure_Select_SS13PME.PNG)

Tambahkan node table selector untuk melakukan seleksi pada table yang diinginkan, kemudian pada konfigurasi saya melakukan select pada table 05111640000157.ss13pme.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/03_Add_DB_Reader.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/1/03_DB_Result.PNG)

Tambahkan node db reader agar data yang telah di select dapat di baca, kemudian lihat hasil result, data telah berhasil di tambahkan.

### 2.2 02_DB_InDB_Processing_Exercise
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

### 2.3 03_DB_Modelling_Exercise
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/01_Add_Decision_Tree.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/01_Add_Decision_Tree_Configure.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/01_Add_Decision_Tree_Result.PNG)

Tambahkan node decision tree learner untuk melakukan learn pada data, pada kasus ini saya menggunakan gini index dan akan melakukan train pada cow, jalankan dan lihat hasil dari train yang telah dilakukan.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/02_Add_Decision_Predictor.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/3/02_Add_Decision_Predictor_Result.PNG)

Tambah node decision predictor dan lakukan prediksi dari data yang telah di train dan data yang telah dibaca sebelumnya.

### 2.4 04_DB_WritingToDB_Exercise
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/4/01_Add_Table_Writer.PNG)

Tambahkan node table writer dan lakukan konfigurasi untuk membuat table baru dengan nama 05111640000157.ss13pme_original, ini adalah table yang dibuat untuk backup.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/4/02_Add_DB_Update.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/4/02_Add_DB_Update_Configure.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/DB/4/02_Add_DB_Update_Result.PNG)

Tambahkan node db update untuk melakukan update data ke database dimana untuk mengisi kolom ``serial no`` berdasarkan kolom ``cow`` yang telah di prediksi sebelumnya, jalankan kemudian kolom null akan terisi dengan hasil prediksi yang telah di lakukan sebelumnya.

## 3. Hadoop
### 3.1 00_Setup_Hive_Table
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/0/01_Setup_Hive.PNG

Jalankan semua node yang telah di sediakan oleh KNIME untuk membuat temp dir pada local yang nantinya digunakan sebagai DB ``hive``.

![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/0/01_Setup_Hive_1.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/0/01_Setup_Hive_2.PNG)

Pada kasus ini saya akan membuat 2 table 05111640000157_ss13pme dan 05111640000157_ss13hme, lanjutkan dengan menjalankan semua notenya hingga selesai, setelah itu uji coba dengan menggunakan DBClient untuk mengetest apakah data sudah masuk kedalam ``hive`` atau belum.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/0/02_Test_DBClient.PNG)
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/0/02_Test_DBClient_2.PNG)

Data telah berhasil dimasukan kedalam ``hive``, ``apache hive`` telah berhasil di jalankan, untuk menjalankan workflow selanjutnya, pastikan semua workflow pada setup hive ini telah berjalan semua.

### 3.2 01_Hive_Modeling_Exercise
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/1/01_Add_Env.PNG

Tambahkan node env, dan hapus sqlite connector yang sebelumnya, hubungkan env kedalam table selector, pastikan workflow sebelumnya telah berjalan yaitu workflow setup hive.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/1/02_Change_Table_Selector.PNG

Ganti table sesuai table yang telah dibuat sebelumnya.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/1/03_Success.PNG

Jalankan semua node, node berjalan dengan baik tanpa adanya error ditandai dengan semua node menyala warna hijau.

### 3.3 02_Hive_WritingtoDB_Exercise
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/2/01_Add_Table_Creator.PNG
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/2/01_Table_Configure.PNG

Tambahkan node baru table creator, buatlah sebuah table baru, pada kasus ini saya membuat table dengan nama 05111640000157_newTable.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/2/02_Add_Table_Loader_Temp_Dir.PNG

Tambahkan node baru table loader, dan temp dir, pada kasus ini saya hanya melakukan copy paste konfigurasi temp dir pada setup hive sebelumnya, yang kemudian diarahkan ke table loader.


![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/2/02_Loader_Configure.PNG

Atur lokasi hive dan table yang akan di load, kemudian jalankan.

![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/2/03_Success_Data_Knime.PNG
![](/tugas_2_eksplorasi-knime-big-data/screenshoot/Hadoop/2/03_Success_Data_DBeaver.PNG

Berikut adalah hasil yang telah didapatkan, serta uji coba melalui DBClient DBeaver, Eksplorasi telah berhasil dilakukan tanpa adanya error.

