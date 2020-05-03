# Spark With Docker
Tugas 5 ini akan melakukan uji coba apache spark dengan docker, dimana akan dibuat 2 docker dengan spesifikasi yang berbeda

Spesifikasi 1 :\
Jumlah worker: 2 Dimana ini adalah worker yang akan dibuat pada apache spark\
Jumlah CPU: 2 Dengan total CPU nya adalah 2 cores\
Memory: 1G Dengan memory 1G dimasing masing workernya\
Partisi: 100 Uji coba yang dilakukan adalah melakukan partisi 100 untuk menghitung pi


Spesifikasi 2 :\
Jumlah worker: 5 Dimana ini adalah worker yang akan dibuat pada apache spark\
Jumlah CPU: 5 Dengan total CPU nya adalah 5 cores\
Memory: 1G Dengan memory 1G dimasing masing workernya\
Partisi: 1000 Uji coba yang dilakukan adalah melakukan partisi 1000 untuk menghitung pi\
Dari hasil uji coba nanti akan ditarik kesimpulan spark mana yang lebih cepat dan proses spark bekerja.


## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [Uji Coba Spesifikasi 1](#2-uji-coba-spesifikasi-1)
2. [Uji Coba Spesifikasi 2](#3-uji-coba-spesifikasi-2)
4. [Kesimpulan](#4-kesimpulan)
5. [Referensi](#5-referensi)

## 1. Kebutuhan
- Docker for Windows
- Bitnami/Spark
- CLI


## 2. Uji Coba Spesifikasi 1
Pertama kita akan melakukan uji coba pada spesifikasi 1

1. Membuat docker-compose.yaml dengan konfigurasi seperti berikut :
```
version: '2'

services:
  spark:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=master
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
    ports:
      - '8080:8080'
  spark-worker-1:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-2:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no

```

Konfigurasi diatas menunjukan bahwa kita akan membuat 1 Master Spark sebagai master, dan 2 Spark sebagai worker
dimana masing masing worker akan mendapatkan memory sebesar 1G dan 1 cores dan total coresnya adalah 2.

2. Nyalakan docker, kemudian masuk kedalam folder ``docker-compose.yaml`` berada lalu jalankan docker-compose dengan menggunakan command
``docker-compose up``

3. Jika install berhasil, lakukan check dengan menggunakan command
``docker ps``

![](/tugas_5_docker_spark/screenshoot/1.png)

command ini berfungsi untuk melihat status docker

4. Check melalui UI Website dengan cara membuka web browser dan masukan url ``127.0.0.1/8080`` maka akan tampak seperti ini

![](/tugas_5_docker_spark/screenshoot/2.png)

Dibrowser menunjukan terdapat 2 worker dengan masing masing worker memiliki 1 core, yang artinya docker telah berhasil dengan total 2 core.

5. Selanjutnya adalah menjalankan script python yang ada didalam apache spark
Untuk menjalankan script didalam apache spark, pertama perlu masuk kedalam cmd didalam apache spark dengan cara menjalankan

``docker exec -it <container_id> /bin/bash``

Saya akan memasuki container master sehingga saya melakukan eksekusi ``docker exec -it 937f8df7a8de /bin/bash``

![](/tugas_5_docker_spark/screenshoot/3.png)

telah berhasil masuk kedalam container master, selanjutnya untuk menjalankan script diperlukan beberapa parameter diantara lain

```
Alamat master node
Script atau kelas yang akan dijalankan
Data/file (apabila ada)
```

untuk mengetahui alamat node dapat menjanlankan command ``hostname -i`` pada cli, sehingga akan mendapatkan alamat 

![](/tugas_5_docker_spark/screenshoot/4.png)

Selanjutnya adalah menjalankan script python, kita akan mencoba dengan partisi 100 terlebih dahulu

``spark-submit --master spark://172.19.0.4:7077 examples/src/main/python/pi.py 100``

Didapatkan hasil pi, dan proses job dijalankan selama 27s untuk 100 partisi

![](/tugas_5_docker_spark/screenshoot/5.png)

![](/tugas_5_docker_spark/screenshoot/6.png)

Selanjutnya adalah lakukan hal yang sama dengan partisinya adalah 1000, maka didapatkan hasil execution job

![](/tugas_5_docker_spark/screenshoot/7.png)

Untuk menjalakan 1000 dibutuhkan waktu 2,5 min

Uji coba pada spesifikasi 1 telah berhasil dilakukan.

## 3. Uji Coba Spesifikasi 2
Selanjutnya kita akan melakukan uji coba pada spesifikasi 2

1. Membuat docker-compose.yaml dengan konfigurasi seperti berikut :
```
version: '2'

services:
  spark:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=master
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
    ports:
      - '8080:8080'
  spark-worker-1:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-2:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-3:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-4:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no
  spark-worker-5:
    image: bitnami/spark:2
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark:7077
      - SPARK_WORKER_MEMORY=1G
      - SPARK_WORKER_CORES=1
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
      - SPARK_LOCAL_STORAGE_ENCRYPTION_ENABLED=no
      - SPARK_SSL_ENABLED=no

```

Konfigurasi diatas menunjukan bahwa kita akan membuat 1 Master Spark sebagai master, dan 5 Spark sebagai worker
dimana masing masing worker akan mendapatkan memory sebesar 1G dan 1 cores dengan total 5 core.

2. Lakukan langkah yang sama seperti spesifikasi 1

3. Lakukan langkah yang sama seperti spesifikasi 1

4. Check melalui UI Website dengan cara membuka web browser dan masukan url ``127.0.0.1/8080`` maka akan tampak seperti ini

![](/tugas_5_docker_spark/screenshoot/8.png)

Dibrowser menunjukan terdapat 5 worker dengan masing masing worker memiliki 1 cores, yang artinya docker telah berhasil dengan total 5 cores.

5. Selanjutnya adalah menjalankan script python yang ada didalam apache spark
Untuk menjalankan script didalam apache spark, pertama perlu masuk kedalam cmd didalam apache spark dengan cara menjalankan

``docker exec -it <container_id> /bin/bash``

Saya akan memasuki container master sehingga saya melakukan eksekusi ``docker exec -it bead463cc2dc /bin/bash``

untuk mengetahui alamat node dapat menjanlankan command ``hostname -i`` pada cli, sehingga akan mendapatkan alamat node

Selanjutnya adalah menjalankan script python, kita akan mencoba dengan partisi 100 terlebih dahulu

``spark-submit --master spark://172.20.0.3:7077 examples/src/main/python/pi.py 100``

proses job dijalankan selama 43s untuk 100 partisi dan menghasilkan nilai pi

![](/tugas_5_docker_spark/screenshoot/9.png)

![](/tugas_5_docker_spark/screenshoot/10.png)

Selanjutnya adalah lakukan hal yang sama dengan partisinya adalah 1000, maka didapatkan hasil

![](/tugas_5_docker_spark/screenshoot/11.png)

Untuk menjalakan 1000 dibutuhkan waktu 52s

Uji coba pada spesifikasi 2 telah berhasil dilakukan.

## 4. Kesimpulan
Pada worker dengan total core 2 yaitu 2 worker, untuk 100 partisi terdapat waktu penyelesain 27 detik, sedangkan pada 1000 partisi diperlukan waktu 2,5 min, pada 1000 partisi terdapat overhead proses map-reduce dikarenakan hanya 2 worker sehingga banyak waktu yang terbuang diproses map-reduce.

Pada worker dengan total core 5 yaitu 5 worker, untuk 100 partisi terdapat waktu penyelesaian 43 detik, sedangkan pada 1000 partisi diperlukan waktu 52 detik.

Kesimpulan yang dapat diambil adalah, proses 100 partisi pada 2 worker lebih cepat dibanding dengan 5 worker hal ini dikarenakan dibutuhkan waktu lebih untuk mempersiapkan worker dalam pembagian kerja, sedangkan 1000 partisi lebih pada 5 worker yang artinya proses akan lebih efektif ketika worker semakin banyak.

Dibutuhkan waktu untuk mempersiapkan pembagian kerja pada setiap worker, tetapi apabila worker lebih banyak akan lebih efektif ketika proses partisi setelah worker disiapkan.

## 5. Referensi
https://hub.docker.com/r/bitnami/spark                                                              
https://github.com/bitnami/bitnami-docker-spark                                                                                                                                                                               