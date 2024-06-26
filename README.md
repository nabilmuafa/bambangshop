# BambangShop Publisher App
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
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [x] Commit: `Create Subscriber model struct.`
    -   [x] Commit: `Create Notification model struct.`
    -   [x] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [x] Commit: `Implement add function in Subscriber repository.`
    -   [x] Commit: `Implement list_all function in Subscriber repository.`
    -   [x] Commit: `Implement delete function in Subscriber repository.`
    -   [x] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [x] Commit: `Create Notification service struct skeleton.`
    -   [x] Commit: `Implement subscribe function in Notification service.`
    -   [x] Commit: `Implement subscribe function in Notification controller.`
    -   [x] Commit: `Implement unsubscribe function in Notification service.`
    -   [x] Commit: `Implement unsubscribe function in Notification controller.`
    -   [x] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [x] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [x] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [x] Commit: `Implement publish function in Program service and Program controller.`
    -   [x] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [x] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. Dalam kasus ini, penggunaan struct saja sudah cukup. Hal ini karena hanya ada satu jenis/class observer pada kasus BambangShop ini, yaitu Subscriber. Namun, apabila kedepannya akan ada jenis observer lain, maka penggunaan trait akan menjadi lebih strategis. Dengan trait atau interface, setiap jenis observer yang berbeda dapat diberikan definisi methods yang sama untuk menerima data dari publisher. Aplikasi menjadi lebih terbuka terhadap modifikasi.
2. Penggunaan DashMap disini diperlukan, karena DashMap dapat memberikan pemetaan yang lebih baik antara id Program dan url Subscriber. Apabila kita hanya menggunakan Vec saja yang hanya bisa menyimpan data satu dimensi, maka akan diperlukan lebih dari satu Vec untuk pemetaan antara id Program dan url Subscriber. Hal ini dapat menghambat modifikasi yang perlu dilakukan pada data.
3. DashMap masih lebih diperlukan karena DashMap memang dioptimisasi sedemikian rupa untuk bisa digunakan dalam multithreading. Dengan thread safetynya, operasi yang multithreaded pada aplikasi BambangShop dapat berjalan dengan lebih aman meskipun map SUBSCRIBER akan diakses oleh banyak thread. Di sisi lain, Singleton pattern memang akan memastikan bahwa hanya akan ada satu instance dari objek yang dibuat, akan tetapi implementasi Singleton pattern secara umum tidak menjamin thread safety, tidak seperti DashMap.

#### Reflection Publisher-2
1. Hal ini berkaitan dengan _separation of concerns_, dimana masing-masing dari service dan repository punya kepentingannya sendiri, begitupun dengan model. Dalam hal ini, service seringkali digunakan sebagai lapisan terluar (setelah controller) yang menerima segala instruksi. Service digunakan untuk mengolah data dan mengambil atau mengirimkannya ke repository. Repository sendiri bersifat sebagai lapisan lebih dalam yang berhubungan langsung dengan database dan menjalankan instruksi-instruksi hanya seperti menambahkan, menghapus, dan modifikasi-modifikasi lainnya yang dilakukan secara langsung tanpa ada pengolahan dan sebagainya. Dari dua tugas ini saja sudah berbeda karena yang satu lebih low level dan yang satunya tidak, sehingga kurang baik jika digabungkan. Dengan memisahkan service dan repository, kode akan menjadi lebih mudah di-maintain karena adanya pemisahan "tugas" dari kode. Selain itu, akan memenuhi salah satu prinsip SOLID yaitu Single Responsibility Principle.
2. Jika kita hanya menggunakan Model, akan terjadi interaksi yang menumpuk dan berat pada tiap-tiap model yang kita definisikan, karena semua operasi dilakukan di satu berkas yang sama. Selain itu, dengan menumpuknya kode, akan ada ketergantungan yang tinggi pada kode sehingga satu modifikasi saja bisa memberikan efek domino (satu modifikasi memerlukan modifikasi lain yang berjumlah banyak di berkas yang sama.)
3. Postman sendiri adalah aplikasi yang saya anggap sebagai "browser simulator", dimana bisa kita gunakan untuk melakukan test pada behavior suatu website yang sudah dibuat tapi tanpa perlu memunculkan tampilannya dan lebih berfokus ke raw outputnya, baik itu HTML atau data seperti JSON. Postman bisa berguna untuk melakukan debugging dan testing, misalnya untuk melihat apakah data yang dikembalikan sebagai response melalui suatu endpoint API sudah benar sesuai format atau belum. Untuk fitur-fiturnya sendiri saya belum terlalu eksplorasi, sehingga saya belum tahu fitur-fitur apa saja yang menurut saya menarik dan bisa diutilisasi untuk tugas kelompok atau proyek software engineering kedepannya.

#### Reflection Publisher-3
1. Pada BambangShop ini, model yang digunakan adalah Push model. Hal ini terlihat dari behavior main app nya dimana ketika terjadi CRUD pada produk dengan suatu type, maka yang akan memberikan update tentang perubahan yang ada adalah service Notification kepada semua subscriber.
2. Apabila kita menggunakan Pull model, maka setiap subscriber-lah yang akan secara rutin memeriksa apakan ada notifikasi yang terkait dengan apa yang mereka subscribe. Keuntungannya adalah observer, atau dalam kasus ini Subscriber, memiliki kendali penuh atas kapan mereka ingin mengambil suatu data dan data apa saja yang mereka ambil. Kekurangannya adalah observer perlu mengetahui implementasi struktur data dari source-nya untuk bisa melakukannya dengan baik.
3. Jika kita tidak menggunakan multithreading, ada beberapa hal yang akan terjadi. Salah satunya adalah menumpuknya instruksi pengiriman notifikasi-notifikasi untuk setiap subscriber karena pengiriman setiap notifikasi akan saling menunggu satu sama lain selesai. Hal ini dapat menghambat jalannya program terutama apabila terdapat cukup banyak subscriber yangperlu di-notify terhadap perubahan data yang ada.
