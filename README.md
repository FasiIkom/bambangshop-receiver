# BambangShop Receiver App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable                | type   | description                                                     |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT             | string | Port number that will be listened by this receiver instance.    |
    | APP_INSTANCE_ROOT_URL   | string | URL address where this receiver instance can be accessed.       |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed.       |
    | APP_INSTANCE_NAME       | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:
    -   Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    -   Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    -   Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create SubscriberRequest model struct.`
    -   [ ] Commit: `Create Notification database and Notification repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Notification repository.`
    -   [ ] Commit: `Implement list_all_as_string function in Notification repository.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
-   **STAGE 3: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Commit: `Implement receive_notification function in Notification service.`
    -   [ ] Commit: `Implement receive function in Notification controller.`
    -   [ ] Commit: `Implement list_messages function in Notification service.`
    -   [ ] Commit: `Implement list function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

**Jawaban:**

1. **Penggunaan RwLock<> pada Vec Notifications**  
   Pada tutorial ini, kita menggunakan `RwLock<Vec<Notification>>` untuk menyinkronkan akses ke daftar notifikasi. Hal ini diperlukan karena:
   - Akses ke resource bersama (dalam hal ini, Vec notifikasi) harus dijaga agar aman dari race condition di lingkungan multi-thread.  
   - Dengan `RwLock`, kita dapat memberikan akses baca (read) secara bersamaan ke banyak thread ketika tidak terjadi modifikasi, sehingga meningkatkan performa.  
   - Jika kita hanya menggunakan `Mutex<>`, maka setiap akses—baik baca maupun tulis—akan mengunci seluruh resource, yang dapat menurunkan performa dengan mengurangi paralelisme.

2. **Alasan Rust Menggunakan lazy_static dan Pembatasan Mutasi pada Static Variable**  
   Di Rust, variabel global atau "static" memiliki aturan kepemilikan yang ketat untuk menjaga keamanan memori.  
   - Dengan menggunakan external library seperti `lazy_static`, kita dapat mendefinisikan variabel static yang diinisialisasi secara lazy saat pertama kali diakses.  
   - Rust tidak mengizinkan mutasi pada static variable secara langsung (seperti di Java) karena akan mengabaikan aturan safety dan concurrency yang dijamin oleh compiler.  
   - Melalui `lazy_static`, kita dapat membungkus variabel tersebut dengan _synchronization primitive_ seperti `RwLock` atau `Mutex` sehingga mutasi dapat dilakukan dengan cara yang aman dan terkontrol, serta menghindari data race di waktu kompilasi maupun waktu run.


#### Reflection Subscriber-2

**Jawaban:**

1. **Eksplorasi Kode di Luar Tutorial:**  
   Saya memang sempat mengeksplorasi kode di luar langkah-langkah tutorial, misalnya melihat isi `src/lib.rs` dan modul-modul utilitas lainnya. Dari situ, saya belajar bagaimana struktur project dikomposisikan, bagaimana modul-modul dipecah agar tidak saling bergantung secara erat, dan bagaimana penggunaan konfigurasi serta error handling diintegrasikan dengan framework Rocket. Pemahaman ini membantu saya menyusun aplikasi yang lebih terstruktur dan scalable.

2. **Observer Pattern dan Penambahan Subscriber:**  
   Dalam tutorial ini, kita menerapkan Observer pattern dengan model push, di mana publisher secara langsung mengirim notifikasi kepada setiap subscriber yang telah terdaftar. Hal ini membuat penambahan subscriber menjadi mudah, karena publisher tidak perlu tahu detail implementasi masing-masing subscriber. Setiap subscriber cukup mendaftar dan secara otomatis akan menerima pemberitahuan.  
   Jika kita menjalankan lebih dari satu instance Main App, secara desain hal ini masih mudah dikelola karena setiap instance publisher akan bekerja secara independen untuk mengirim notifikasi. Namun, untuk memastikan konsistensi antar publisher, mungkin diperlukan mekanisme koordinasi tambahan seperti shared messaging system atau load balancing.

3. **Pengujian dan Peningkatan Dokumentasi Postman:**  
   Saya juga mencoba membuat test sendiri serta meningkatkan dokumentasi dalam Postman collection. Pembuatan test membantu memastikan setiap endpoint berjalan sesuai harapan dan memudahkan deteksi error sedini mungkin. Sementara itu, peningkatan dokumentasi dalam Postman, seperti menambahkan deskripsi endpoint, variabel lingkungan, dan skrip otomatisasi testing, sangat membantu dalam validasi API serta kolaborasi tim pada project kelompok. Pengalaman ini memberikan saya gambaran bagaimana proses pengujian dan dokumentasi dapat meningkatkan kualitas dan keandalan aplikasi.