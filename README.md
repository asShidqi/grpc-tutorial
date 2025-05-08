1. Unary RPC memungkinkan client mengirim satu permintaan dan menerima satu tanggapan. Server streaming memungkinkan client mengirim satu permintaan dan menerima banyak tanggapan secara bertahap. Bidirectional streaming memungkinkan komunikasi dua arah di satu koneksi, yang sangat cocok untuk aplikasi real-time seperti fitur chat.

2. Autentikasi: Pastikan hanya klien sah yang bisa akses.
Otorisasi: Pastikan pengguna hanya akses data/layanan yang diizinkan.
Enkripsi: Gunakan TLS agar komunikasi aman dari sadapan.
Validasi Input: Cegah data berbahaya dari klien.


3. Masalah utama adalah menjaga komunikasi dua arah tetap stabil dan sinkron, terutama jika ada gangguan koneksi atau kesalahan baca/tulis. Selain itu, penting untuk menjaga performa agar tetap responsif, misalnya dengan manajemen buffer yang baik.

4. Penggunaan tokio_stream::wrappers::ReceiverStream memiliki kelebihan dalam menyederhanakan konversi dari channel asynchronous ke bentuk stream yang bisa dikonsumsi gRPC. Hal ini memudahkan integrasi antara logika aplikasi internal dengan protokol komunikasi eksternal. Namun, ada kekurangan seperti keterbatasan kontrol terhadap backpressure, buffer channel yang bisa penuh jika receiver lambat, serta penanganan channel yang harus ditutup dengan hati-hati agar tidak menyebabkan kebocoran resource atau panic mendadak.

5. Pisahkan setiap layanan (misalnya Pembayaran, Transaksi, Chat) ke dalam modul atau file sendiri. Satukan konfigurasi umum (alamat server, token) di satu tempat. Gunakan trait untuk mendefinisikan logika.

6. Untuk pembayaran yang lebih kompleks, tambahkan: validasi pengguna dan jumlah, simpan status ke database, pastikan transaksi konsisten (atomik), catat aktivitas (logging/audit), dan sistem coba ulang otomatis jika gagal.

7. gRPC membuat layanan lebih mudah terhubung meski beda bahasa (interoperabilitas), kontrak layanan jadi jelas dan aman (berbasis skema), dan komunikasi antar microservice lebih cepat dan efisien karena menggunakan HTTP/2.

8. * Kelebihan HTTP/2: Mendukung banyak permintaan sekaligus dalam satu koneksi (multiplexing), kompresi header, dan streaming bawaan, bagus untuk komunikasi real-time.
* Kekurangan: Lebih rumit disiapkan dan dukungan langsung di browser terbatas dibandingkan WebSocket atau HTTP/1.1 yang lebih sederhana untuk aplikasi web ringan.

9. * REST (Request-Response): Klien minta, server jawab. Untuk real-time, biasanya mengandalkan polling yang tidak efisien.
* gRPC (Bidirectional Streaming): Koneksi selalu terbuka, klien dan server bisa kirim pesan kapan saja. Lebih cocok untuk aplikasi yang butuh komunikasi dua arah terus-menerus (misalnya chat).

10. * gRPC (Protocol Buffers): Struktur data ketat, cepat, dan hemat ukuran. Cocok untuk sistem besar dan lintas platform.
* REST (JSON): Fleksibel dan mudah dibaca manusia, tapi kurang efisien dan rentan salah karena tidak ada validasi skema bawaan. Singkatnya: gRPC unggul di efisiensi, REST unggul di fleksibilitas.