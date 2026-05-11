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