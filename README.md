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
