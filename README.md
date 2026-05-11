# Subscriber - Event Driven Architecture

## Understanding Subscriber and Message Broker

### a. What is AMQP?

AMQP (Advanced Message Queuing Protocol) adalah protokol komunikasi jaringan 
terbuka yang dirancang untuk message-oriented middleware. AMQP memungkinkan 
aplikasi yang berbeda untuk berkomunikasi satu sama lain melalui sebuah 
message broker. Protokol ini mendefinisikan format pesan, mekanisme routing, 
antrian (queue), dan pengiriman pesan secara andal antara publisher dan 
subscriber.

### b. What does guest:guest@localhost:5672 mean?

- **guest** (pertama) = username untuk login ke RabbitMQ. 
  Default username RabbitMQ adalah "guest".
- **guest** (kedua) = password untuk login ke RabbitMQ. 
  Default password RabbitMQ adalah "guest".
- **localhost:5672** = alamat dan port tempat RabbitMQ berjalan. 
  "localhost" berarti RabbitMQ dijalankan di komputer yang sama, 
  dan "5672" adalah port default yang digunakan RabbitMQ untuk 
  menerima koneksi AMQP.


## Reflection and Running at least three subscribers

Dengan menjalankan 3 subscriber sekaligus, pemrosesan pesan menjadi 
lebih cepat karena setiap subscriber mengambil pesan dari queue secara 
bergantian. Ini adalah salah satu keunggulan event-driven architecture, 
yaitu kita bisa dengan mudah menambah jumlah subscriber (scaling) untuk 
menangani beban yang tinggi tanpa mengubah kode publisher sama sekali.

Dari kode publisher dan subscriber, hal yang bisa ditingkatkan adalah:
- Publisher bisa menambahkan error handling yang lebih baik
- Subscriber bisa menggunakan connection pooling untuk efisiensi
- Bisa ditambahkan logging yang lebih detail

![Multiple Subscribers](assets/Screenshot%202026-05-11%20124158.png)
![RabbitMQ Multiple](assets/Screenshot%202026-05-11%20123854.png)