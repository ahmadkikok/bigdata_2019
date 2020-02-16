# ETL Menggunakan KNIME
Untuk melakukan ETL saya menggunakan datasets yang disediakan oleh www.kaggle.com serta menggunakan MySQL sebagai media penyimpanan database. Dimana datasets nantinya akan di split dan dimasukan kedalam database yang kemudian dilakukan join kembali.

## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [Business Understanding](#2-business-understanding)
3. [Data Understanding](#3-data-understanding)
4. [Data Preparation](#4-data-preparation)
5. [Modeling](#5-modeling)
6. [Evaluation](#6-evaluation)
7. [Deployment](#7-deployment)
8. [Referensi](#8-referensi)


## 1. Kebutuhan
- KNIME
- Datasets (https://www.kaggle.com/fmejia21/demographics-of-academy-awards-oscars-winners)
- MySQL


## 2. Business Understanding
Pada datasets ini adalah berisi kumpulan data tentang ras, agama, usia, dan detail demografis lainnya dari semua pemenang Oscar sejak 1928.

Dari data-data tersebut kemungkinan yang dapat dilakukan adalah :
- Proses untuk melihat Aktor Terbaik.
- Proses untuk melihat Aktris Terbaik.
- Proses untuk melihat Aktor Pendukung Terbaik.
- Proses untuk melihat Artis Pendukung Terbaik
- Proses untuk melihat Direktur Terbaik.
	
	
## 3. Data Understanding
- Datasets ini berisi tentang data ras, agama, usia, dan detail demografis lainnya dari semua pemenang Oscar sejak 1928.
- Datasets ini memiliki 27 Kolom dan 441 Row.
- Kolom Datasets lainnya berisi tentang informasi ras, agama, usia, biografi, dan detail mengenai demografis lainnya.

	
## 4. Data Preparation
![](/tugas_1_etl-menggunakan-knime/screenshoot/Split_Diagram_1.PNG)

Pada data preparation untuk melakukan splitting data, saya menggunakan KNIME untuk memisahkan data tersebut, dengan skema workflow seperti diatas. Dimana file **.csv** dari Datasets harus di baca terlebih dahulu kedalam KNIME, kemudian dilakukan **column filtering** untuk melakukan filter berdasarkan kolom, yang kemudian di **write** kembali dalam bentuk **.csv** dan **database/sql**, untuk detail konfigurasi dijelaskan lebih lanjut.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Split_FileReader_2.PNG)

Pada **Node 2** File Reader membaca file **.csv** dengan konfigurasi seperti digambar.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Split_FilterColumn_3.PNG)

Pada **Node 3 dan 4** Dilakukan Column Filter untuk memisahkan kolum mana saja yang ingin di pisahkan. **Node 3** adalah node yang nantinya akan di write kembali dalam bentuk **.csv** sedangkan **Node 4** akan di write kedalam database langsung.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Split_CSVWriter_4.PNG)

Pada **Node 5** adalah konfigurasi untuk melakukan write dan output dari file **.csv** setelah dilakukan column filter.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Split_SQLWriter_5.PNG)

Pada **Node 6** adalah konfigurasi untuk melakukan write kedalam **database** setelah dilakukan column filter, untuk melakukan write kedalam **database** diperlukan koneksi untuk menghubungkan **KNIME** ke **database**, akan dijelaskan dibawah.

Proses data preparation telah selesai, dan file output terdapat pada lokasi output di **Node 5**, dan **database** sudah secara otomatis terbuat table **split_awards** sesuai konfigurasi yang di lakukan di **Node 6**.


## 5. Modeling
![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_Diagram_1.PNG)

Pada modelling berikut workflow yang saya buat dan saya pakai dalam ETL, dimana file dimasukan dan dibuat node untuk menghubungkan database dengan KNIME, kemudian dilakukan join keduanya, dan dilakukan write kembali kedalam file dan database.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_MySQLConn_2.PNG)

Pada **Node 9** berisi konfigurasi untuk menghubungkan **database** dengan **KNIME** 

![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_MySQLTableSelect_3.PNG)
![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_MySQLTableReader_4.PNG)

Setelah **database** dan **KNIME** sudah terhubung, selanjutnya adalah dilakukan select dan read agar table yang beradara di **database** serta isinya dapat terbaca, diatas adalah hasil select table dan membaca data yang ada didalam table, pada data yang berada di **database** terdapat **441 Row** dan **13 Kolom**.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_CSVReader_5.PNG)
![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_CSVResult_11.PNG)

Selanjutnya adalah membaca file **.csv** yang sudah di split, sehingga menghasilkan data seperti diatas, pada data **.csv** terdapat **441 Row** dan **15 Kolom**.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_Joiner_6.PNG)

Ini adalah konfigurasi dan proses **Join** data yang berasal dari **database** digabungkan dengan data yang berasal dari **.csv**, dimana **Node 12** ini akan membaca data dari kedua file, yang kemudian proses **Join** yang digunakan adalah inner join, dengan menyamakan data pada file **.csv** dan **database**, dijoin berdasarkan **row_id** dan **unit_id** sebagai unique dari tiap file.


## 6. Evaluation
![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_JoinResult_10.PNG)

**Join** ini menghasilkan **441 Row** dan **27 Kolom**, **Join** berhasil dilakukan tanpa adanya error ditandai dengan lampu pada **Node** yang berwarna **Hijau**.
 
 
## 7. Deployment
Setelah join berhasil dilakukan, selanjutnya adalah deployement, yaitu dimana file yang telah di satukan akan di lakukan write kembali kedalam **database** dan **.csv**.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_CSVWriter_7.PNG)

Pada **Node 13** akan melakukan write data ke file berbentuk **.csv** dengan lokasi output sesuai pada gambar.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_ColumnFilter_8.PNG)
![](/tugas_1_etl-menggunakan-knime/screenshoot/Workflow_DBWriter_9.PNG)

Pada **Node 14 dan 15** dilakukan column filter untuk menyeleksi kolom yang akan di write kedalam **database**, pada kasus ini saya memilih semua kolom, kemudian dilakukan write data kedalam **database**, untuk melakukan write data ke dalam **database** diperlukan column filtering terlebih dahulu.

![](/tugas_1_etl-menggunakan-knime/screenshoot/Result_1.PNG)
![](/tugas_1_etl-menggunakan-knime/screenshoot/Result_2.PNG)

Berikut adalah hasil dari output file **.csv** dan **database**.


## 8. Referensi
https://www.kaggle.com/fmejia21/demographics-of-academy-awards-oscars-winners
https://www.youtube.com/user/KNIMETV
https://www.knime.com/learning-hub