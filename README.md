# Module 9 - Event Driven Architecture

## Subscriber

### 1. What is amqp?
AMQP adalah singkatan dari Advanced Message Queuing Protocol. Protokol ini dipakai untuk kirim pesan antar service lewat message broker, contohnya RabbitMQ. Di tugas ini, publisher tidak kirim data langsung ke subscriber, tapi kirim dulu ke broker melalui AMQP. Lalu subscriber mengambil pesan dari queue yang sama. Cara ini bikin publisher dan subscriber lebih loosely coupled.

### 2. What does `guest:guest@localhost:5672` mean?
`guest` pertama adalah username default RabbitMQ. `guest` kedua adalah password untuk user tersebut. `localhost` artinya RabbitMQ dijalankan di mesin yang sama dengan aplikasi. `5672` adalah port default AMQP untuk koneksi dari publisher atau subscriber. Jadi string `amqp://guest:guest@localhost:5672` adalah URL koneksi aplikasi ke broker RabbitMQ lokal.

## Publisher

### 1. How much data your publisher program will send to the message broker in one run?
Dalam satu kali `cargo run`, publisher mengirim 5 event `user_created`. Masing masing event berisi `user_id` dan `user_name`. Jadi total payload yang terkirim adalah 5 message per eksekusi program.

### 2. The url of: `amqp://guest:guest@localhost:5672` is the same as in the subscriber program, what does it mean?
Artinya sama seperti di subscriber, yaitu alamat koneksi ke RabbitMQ lokal. Publisher memakai kredensial `guest:guest` ke host `localhost` port `5672`. Ini adalah endpoint AMQP yang dipakai buat publish event ke broker.

## Running RabbitMQ as message broker

![RabbitMQ Running](images/RabbitMQ_Running.png)

Dari screenshot ini RabbitMQ sudah berjalan normal di `localhost:15672`. Kondisi awal masih `Connections: 0`, `Queues: 0`, dan `Consumers: 0` karena belum ada subscriber yang aktif. Ini normal dan memang expected sebelum proses `cargo run` dari subscriber dijalankan.
