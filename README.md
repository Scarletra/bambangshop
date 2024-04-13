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
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

Umumnya, penggunaan trait (interface) pada Rust dalam konteks pengimplementasian Observer design pattern adalah untuk mendefinisikan fungsi-fungsi yang akan dilakukan (service). Namun, pada konteks aplikasi ini, BambangShop, penggunaan trait (interface) tidak begitu diperlukan karena dengan menggunakan satu struktur Model untuk observer saja sudah cukup untuk mencakup data dan juga behaviour nya.

2. id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?

Penggunaan Vec (list) dalam konteks aplikasi ini sebenarnya cukup jika kita hanya ingin menyimpan ID dan URL yang unik. Namun, kalau dipikirkan lebih lanjut, akan lebih baik lagi jika kita menggunakan DashMap karena alasan tertentu. Dengan adanya penggunaan DashMap, modifikasi atau manipulasi terhadap data seperti ID dan URL menjadi lebih efisien, sehingga secara tidak langsung akan meningkatkan kinerja dari aplikasi ini, terlebih saat aplikasi ini menjadi kompleks.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?

Pada Singleton Pattern, satu class hanya memiliki satu instance di seluruh aplikasi. Dalam konteks aplikasi ini, menggunakan DashMap untuk SUBSCRIBERS static variable adalah pilihan terbaik karena memudahkan kita untuk mengakses data dari manapun dan memastikan bahwa itu aman secara bersamaan. DashMap menyediakan solusi yang baik untuk keamanan concurrency dalam multithreaded environment, yang mana sangat penting dalam Rust. Meskipun Singleton pattern dapat diimplementasikan dalam Rust, penggunaannya mungkin tidak diperlukan dalam kasus ini karena DashMap secara efektif menyediakan fungsionalitas yang diperlukan sambil mempertahankan keamanan bersama.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

Seandainya kita menggabungkan antara data storage dengan business logic dalam suatu Model, hal ini bisa melanggar dari prinsip SOLID, seperti SRP dimana Model disini memiliki dua tanggung jawab yaitu menyimpan data dan juga business logic. Tidak hanya itu, jika kita tinjau dari sisi design principle, kita harus memisahkan segala sesuatu yang berbeda antara yang satu dengan yang lain. Dengan adanya pemisahan data storage pada Repository dan business logic ke Service, sekarang Model hanya memiliki satu tanggung jawab yaitu merepresentasikan data yang akan digunakan pada aplikasi kita. Tersisanya satu tanggung jawab ini juga bisa memudahkan kita dalam mendefinisikan data kita dalam suatu database (SQL).

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?

Tanpa adanya pemisahan tugas pada Model, hal ini bisa menyebabkan masalah seperti yang sudah disebutkan pada poin (1). Hal ini dapat menyebabkan peningkatan kompleksitas, keterkaitan erat antara komponen, dan maintainablity. Selain itu, interaksi antara model-model yang berbeda akan menjadi lebih rumit dan terkait erat. Misalnya, jika sebuah Product perlu memberi tahu Subscriber tentang perubahan, itu mungkin langsung berinteraksi dengan objek Subscriber untuk mengirimkan Notification, melanggar enkapsulasi dan menghasilkan kode yang sulit dipahami. Secara keseluruhan, tanpa pemisahan concern yang tepat, kompleksitas kode untuk setiap model akan meningkat secara signifikan, membuatnya lebih sulit dipahami, diuji, dan dipelihara.

3. Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

Sejauh ini Postman membantu saya untuk mengetest program yang sedang dibuat, baik tutorial ini, maupun proyek lainnya. Saya menggunakan Postman untuk menguji request-request yang tersedia yang biasanya berkaitan dengan API. Kita bisa mengirimkan request sesuai endpoint yang tersedia pada program kita. Ada beberapa fitur yang mungkin akan saya gunakan nantinya yaitu **Sending Requests**, **Automated Testing**, dan **Environment Variables**.


#### Reflection Publisher-3
1. Observer Pattern has two variations: Push model (publisher pushes data to subscribers) and Pull model (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

Pada tutorial ini kita menggunakan observer pattern dengan variasi push model dimana notifikasi akan dikirimkan ke subscriber

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

Variasi lainnya adalah pull model

Kelebihan:
- Subscriber hanya akan mengambil data saat mereka memerlukannya, mengurangi penggunaan sumber daya jaringan dan komputasi.
- Subscriber memiliki kontrol penuh atas kapan mereka ingin mengambil data, sehingga dapat menghindari pengambilan data yang tidak perlu.

Kekurangan:
- Pelanggan membutuhkan pembaruan secara berkala, yang dapat menyebabkan keterlambatan dalam mendapatkan informasi baru. 
- Implementasi Pull model mungkin memerlukan lebih banyak logika pada sisi pelanggan untuk mengelola permintaan dan pembaruan data secara efisien.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

Jika kita tidak menggunakan multi-threading dalam proses notification, pengiriman notifikasi akan berjalan secara berurutan dan satu per satu, sehingga tiap pengiriman notifikasi harus menunggu pengiriman notifikasi sebelumnya selesai dijalankan. Karena hal tersebut, proses pengiriman notifikasi dengan jumlah yang besar akan menjadi lambat untuk diselesaikan dan bisa berakibat tertundanya penerimaan notifikasi oleh pengguna (subscriber). Dengan adanya penggunaan multi-threading, masalah seperti itu bisa diselesaikan karena pengiriman notifikasi dapat dilakukan secara bersamaan dalam satu waktu.

