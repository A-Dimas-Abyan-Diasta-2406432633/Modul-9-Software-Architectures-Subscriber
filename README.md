# Module 9 - Event-Driven Architecture

## Subscriber

### 1. What is AMQP?
AMQP adalah singkatan dari Advanced Message Queuing Protocol. Ini adalah protokol standar untuk komunikasi antar aplikasi lewat message broker seperti RabbitMQ. Jadi publisher bisa kirim event ke broker, lalu subscriber menerima event itu tanpa harus terhubung langsung satu sama lain.

### 2. Apa arti `amqp://guest:guest@localhost:5672`?
`amqp://` adalah skema protokol yang dipakai, yaitu AMQP.  
`guest` pertama adalah username RabbitMQ.  
`guest` kedua adalah password RabbitMQ.  
`localhost` adalah host server RabbitMQ yang dipakai, artinya dijalankan di mesin lokal.  
`5672` adalah port default AMQP untuk koneksi aplikasi ke RabbitMQ.

## Publisher

### 1. How much data your publisher program will send to the message broker in one run?
Dalam satu kali `cargo run`, program publisher mengirim 5 event dengan topik `user_created`. Masing masing event berisi `user_id` dan `user_name`.

### 2. Apa arti URL `amqp://guest:guest@localhost:5672` pada publisher?
Artinya sama seperti di subscriber. URL itu adalah alamat koneksi publisher ke RabbitMQ lokal, pakai username `guest`, password `guest`, dan port `5672`.
