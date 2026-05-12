# Subscriber - Event Driven Architecture

## Understanding Subscriber and Message Broker

### a. What is AMQP?

AMQP (Advanced Message Queuing Protocol) adalah protokol komunikasi jaringan
terbuka yang dirancang untuk message-oriented middleware. AMQP memungkinkan
aplikasi yang berbeda untuk berkomunikasi satu sama lain melalui sebuah
message broker secara andal dan terstandarisasi. Protokol ini mendefinisikan
format pesan, mekanisme routing, antrian (queue), dan pengiriman pesan secara
andal antara publisher dan subscriber. AMQP bersifat platform-agnostic,
artinya aplikasi yang ditulis dalam bahasa pemrograman berbeda pun dapat
saling berkomunikasi selama keduanya menggunakan protokol AMQP. RabbitMQ
adalah salah satu implementasi message broker yang paling populer yang
mendukung protokol AMQP ini.

### b. What does guest:guest@localhost:5672 mean?

- **guest** (pertama) = username untuk login ke RabbitMQ.
  Default username RabbitMQ adalah "guest" yang dibuat secara otomatis
  saat RabbitMQ pertama kali dijalankan.
- **guest** (kedua) = password untuk login ke RabbitMQ.
  Default password RabbitMQ adalah "guest" yang berpasangan dengan
  username di atas untuk keperluan autentikasi.
- **localhost** = alamat host tempat RabbitMQ berjalan. "localhost"
  berarti RabbitMQ dijalankan di komputer yang sama dengan aplikasi
  yang mengaksesnya.
- **5672** = port default yang digunakan RabbitMQ untuk menerima
  koneksi AMQP dari publisher maupun subscriber.

## Simulation Slow Subscriber

Dengan menambahkan `thread::sleep` selama 1000ms pada subscriber, subscriber
menjadi lambat dalam memproses setiap pesan. Ketika publisher mengirimkan
banyak pesan sekaligus secara berulang, pesan-pesan tersebut menumpuk di
queue RabbitMQ karena subscriber tidak dapat memproses pesan secepat
publisher mengirimkannya. Hal ini terlihat dari chart Queued messages yang
naik secara signifikan setiap kali publisher dijalankan. Jumlah queue yang
menumpuk bergantung pada berapa kali publisher dijalankan dikalikan 5 pesan
per run dikurangi jumlah pesan yang sudah berhasil diproses subscriber.
Fenomena ini mirip dengan kondisi SIAK War dimana permintaan sangat tinggi
namun server tidak mampu memprosesnya dengan cepat sehingga terjadi
penumpukan antrian dan sistem menjadi lambat bahkan crash.

![Slow Subscriber](assets/Screenshot%202026-05-11%20123854.png)

## Reflection and Running at least three subscribers

Dengan menjalankan 3 subscriber sekaligus yang terhubung ke queue yang sama,
pemrosesan pesan menjadi jauh lebih cepat karena beban kerja dibagi secara
otomatis oleh RabbitMQ kepada masing-masing subscriber yang tersedia. Setiap
subscriber mengambil pesan dari queue secara bergantian sehingga pesan yang
tadinya menumpuk dapat diproses lebih cepat. Ini adalah salah satu keunggulan
utama event-driven architecture, yaitu kemampuan horizontal scaling dimana
kita bisa dengan mudah menambah jumlah subscriber (consumer) untuk menangani
beban yang tinggi tanpa mengubah kode publisher sama sekali. Dari grafik
RabbitMQ terlihat bahwa spike pada Queued messages turun lebih cepat
dibandingkan saat hanya ada satu subscriber, membuktikan bahwa multiple
subscriber efektif mengatasi masalah slow consumer.

Dari kode publisher dan subscriber, beberapa hal yang bisa ditingkatkan
adalah sebagai berikut. Publisher saat ini tidak memiliki error handling
yang memadai, sehingga jika RabbitMQ tidak tersedia maka program akan
langsung crash tanpa memberikan pesan error yang informatif. Subscriber
juga bisa ditingkatkan dengan menggunakan connection pooling agar lebih
efisien dalam mengelola koneksi ke RabbitMQ. Selain itu, bisa ditambahkan
logging yang lebih detail untuk memudahkan debugging dan monitoring di
lingkungan produksi.

![Multiple Subscribers](assets/Screenshot%202026-05-11%20124158.png)
![RabbitMQ Multiple](assets/Screenshot%202026-05-11%20123854.png)