# Kafka Infrastruktur
Tugas 4 untuk melakukan Implementasi Kafka dengan Menggunakan Docker dan pengujian menggunakan Conduktor.

## Menu Cepat
1. [Kebutuhan](#1-kebutuhan)
2. [Kafka With Docker Compose](#2-kafka-with-docker-compose)
	- [Docker Compose Configuration](#21-docker-compose-configuration)
	- [Running](#22-running)
	- [Testing](#23-testing)
4. [Referensi](#4-referensi)

## 1. Kebutuhan
- Docker for Windows
- Conduktor
- Bitnami/Kafka
- Bitnami/Zookeeper


## 2. Kafka With Docker Compose
Pada tugas ini, saya menggunakan docker-compose dalam instalasi infrastruktur kafra, infrastruktur yang akan dibuat adalah seperti gambar dibawah, dengan menggunakan 3 Kafka Server Sebagai Broker dan 1 Zookeper sebagai cluster yang berisi dari 3 Broker.
![](/tugas_4_kafka/screenshoot/1.png)


### 2.1 Docker Compose Configuration
```
version: '2'

networks:
  kafka-net:
    driver: bridge

services:
  zookeeper-server:
    image: 'bitnami/zookeeper:latest'
    networks:
      - kafka-net
    ports:
      - '2181:2181'
    environment:
      - ALLOW_ANONYMOUS_LOGIN=yes
      
  kafka-server1:
    image: 'bitnami/kafka:latest'
    networks:
      - kafka-net    
    ports:
      - '9092:9092'
    environment:
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper-server:2181
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092
      - ALLOW_PLAINTEXT_LISTENER=yes
    depends_on:
      - zookeeper-server
      
  kafka-server2:
    image: 'bitnami/kafka:latest'
    networks:
      - kafka-net    
    ports:
      - '9093:9092'
    environment:
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper-server:2181
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9093
      - ALLOW_PLAINTEXT_LISTENER=yes
    depends_on:
      - zookeeper-server
      
  kafka-server3:
    image: 'bitnami/kafka:latest'
    networks:
      - kafka-net    
    ports:
      - '9094:9092'
    environment:
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper-server:2181
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9094
      - ALLOW_PLAINTEXT_LISTENER=yes
    depends_on:
      - zookeeper-server
```

Berikut adalah konfigurasi ``yaml`` pada ``docker-compose`` yang nanti akan dijalankan.

```
networks:
  kafka-net:
    driver: bridge
```

Pertama yang dilakukan adalah membangun docker network, pada network ini kita akan membangun network kafka dengan driver bridge

```
  zookeeper-server:
    image: 'bitnami/zookeeper:latest'
    networks:
      - kafka-net
    ports:
      - '2181:2181'
    environment:
      - ALLOW_ANONYMOUS_LOGIN=yes
```

Selanjutnya adalah membuat Zookeeper server sebagai Cluster, dimana Zookeeper ini dibutuhkan untuk menjalankan kafka, Pada kasus ini saya menggunakan zookeeper yang berasal dari bitnami,dengan port 2181.

```
  kafka-server1:
    image: 'bitnami/kafka:latest'
    networks:
      - kafka-net    
    ports:
      - '9092:9092'
    environment:
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper-server:2181
      - KAFKA_CFG_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092
      - ALLOW_PLAINTEXT_LISTENER=yes
    depends_on:
      - zookeeper-server
```

Selanjutnya adalah membangun 3 broker, pada kasus ini saya akan mengambil salah satu contoh broker, yaitu kafka-server1 akan diinstall pada container docker dengan menggunakan kafka(terbaru) dari bitnami, dengan port 9092 serta tcp port 9092.
``environtment`` berisi broker terhubung pada cluster zookeper-server yaitu zookeeper yang telah dibuat sebelumnya.


### 2.2 Running
Pada bagian ini kita akan mencoba menjalankan docker-compose, step yang dilakukan adalah

1. Jalankan docker, maka akan terlihat tidak ada container.
![](/tugas_4_kafka/screenshoot/1.png)

2. Masuk kedalam cmd.

3. Masuk kedalam direktori dimana docker-compose.yaml/yml berada.

4. Jalankan ``docker-compose up``, maka akan tampil seperti pada gambar, tunggu hingga proses selesai, setelah itu buka kembali kedalam docker, maka container akan bertambah.
![](/tugas_4_kafka/screenshoot/3.png)

![](/tugas_4_kafka/screenshoot/4.png)

3 Broker dan 1 Cluster telah berhasil dibuat dan berhasil dijalankan.


### 2.3 Testing
Selanjutnya adalah melakukan testing dengan menggunakan conduktor, untuk memastikan kafka telah terbuat dan dapat digunakan.

![](/tugas_4_kafka/screenshoot/5.png)

Buat konfigurasi cluster, kemudian tekan save, setelah hubungan dengan cluster yang telah dibuat.

![](/tugas_4_kafka/screenshoot/6.png)


## 4. Referensi
https://itnext.io/how-to-install-kafka-using-docker-a2b7c746cbdc                                                              
https://www.conduktor.io/                                                                                             
https://docs.docker.com/docker-for-windows/install                                                    
https://bitnami.com/stack/kafka                                                                                     