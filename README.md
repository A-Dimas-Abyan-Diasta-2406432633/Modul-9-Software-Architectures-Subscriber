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

## Sending and processing event

![Subscriber Run](images/Console_Subscriber_Run.png)
![Publisher Run](images/Console_Publisher_Run.png)

Setelah subscriber jalan, saya menjalankan publisher. Setiap eksekusi publisher mengirim 5 event `user_created` dengan isi data user yang berbeda. Event itu diterima subscriber dan tercetak di console. Ini menunjukkan flow publish subscribe sudah bekerja, yaitu publisher kirim ke broker lalu subscriber consume dari queue.

## Monitoring chart based on publisher

![Publisher Chart](images/Publisher_run.png)

Ketika publisher dijalankan berulang, grafik message rate di RabbitMQ muncul spike. Spike ini muncul karena message dikirim secara burst dalam waktu singkat. Semakin sering `cargo run` di publisher, semakin sering spike terlihat. Setelah message selesai diproses, grafik kembali turun. Jadi pola spike sangat berkaitan dengan frekuensi publish event.

## Simulating slow subscriber

Pada file `subscriber/src/main.rs`, baris delay saya aktifkan:

```rust
thread::sleep(_ten_millis);
```

![Slow Subscriber Run](images/Subscriber_run.png)

Dengan delay 1 detik per message, proses consume jadi lebih lambat. Efeknya queue bisa menumpuk lebih dulu saat publisher kirim cepat. Ini menggambarkan kasus real ketika consumer lebih lambat dari producer.

## Reflection and running at least three subscribers

![Publisher with 3 Subscribers](images/Console_Publisher_run_3_Subscriber.png)
![Graph with 3 Subscribers](images/Graph_3_Subscriber.png)

Saat saya jalankan minimal 3 subscriber, beban konsumsi event terbagi ke beberapa worker. Dari console terlihat event tidak selalu diproses oleh satu proses saja, tapi tersebar ke beberapa subscriber. Dampaknya grafik queue lebih cepat turun dibanding saat hanya satu subscriber lambat. Ini membuktikan scaling horizontal di sisi consumer bisa bantu mengurangi bottleneck. Perbaikan yang bisa dilakukan dari kode sekarang adalah menambahkan retry strategy, logging yang lebih terstruktur, dan graceful shutdown agar service lebih siap dipakai di kondisi produksi.
