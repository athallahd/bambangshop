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
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [v] Commit: `Create Subscriber model struct.`
    -   [v] Commit: `Create Notification model struct.`
    -   [v] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [v] Commit: `Implement add function in Subscriber repository.`
    -   [v] Commit: `Implement list_all function in Subscriber repository.`
    -   [v] Commit: `Implement delete function in Subscriber repository.`
    -   [v] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [v] Commit: `Create Notification service struct skeleton.`
    -   [v] Commit: `Implement subscribe function in Notification service.`
    -   [v] Commit: `Implement subscribe function in Notification controller.`
    -   [v] Commit: `Implement unsubscribe function in Notification service.`
    -   [v] Commit: `Implement unsubscribe function in Notification controller.`
    -   [v] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [v] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [v] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [v] Commit: `Implement publish function in Program service and Program controller.`
    -   [v] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [v] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
1. In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?

   - Dalam pola desain Observer, penggunaan interface (atau trait dalam Rust) biasanya diperlukan untuk mendefinisikan kontrak yang harus diimplementasikan oleh semua observer, sehingga memungkinkan fleksibilitas dan konsistensi dalam interaksi antara subject dan observer. Namun, dalam kasus BambangShop, Jika BambangShop hanya memiliki satu jenis observer, seperti hanya menggunakan Model, maka interface tidak diperlukan dan bisa langsung menggunakan struct tersebut. Namun, jika ada atau berpotensi ada beberapa jenis observer yang perlu menerima notifikasi dari subject dengan cara yang berbeda, maka penggunaan interface atau trait sangat disarankan untuk memastikan semua observer mematuhi kontrak yang sama. Dengan demikian, hal ini bergantung pada kompleksitas dan kebutuhan fleksibilitas proyek BambangShop

2. <b>id</b> in <b>Program</b> and <b>url</b> in Subscriber is intended to be unique. Explain based on your understanding, is using <b>Vec</b> (list) sufficient or using <b>DashMap</b> (map/dictionary) like we currently use is necessary for this case?

   - Jika keunikan <b>id</b> pada <b>Program</b> dan <b>url</b> pada Subscriber penting, <b>Vec</b> bisa digunakan untuk kasus sederhana dengan data kecil. Namun, <b>Vec</b> kurang efisien karena membutuhkan iterasi manual untuk memeriksa duplikat. Sebaliknya, <b>DashMap</b> lebih cocok karena key-value-nya secara otomatis memastikan keunikan. Selain itu, <b>DashMap</b> lebih cepat untuk operasi pencarian, penambahan, dan penghapusan data. Dalam aplikasi multi-threaded, <b>DashMap</b> juga unggul karena mendukung thread-safe secara bawaan. Jadi, jika performa dan keunikan data menjadi prioritas, <b>DashMap</b> adalah pilihan yang lebih tepat dibandingkan <b>Vec</b>.

3. When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers <b>(SUBSCRIBERS)</b> static variable, we used the <b>DashMap</b> external library for <b>thread safe HashMap</b>. Explain based on your understanding of design patterns, do we still need <b>DashMap</b> or we can implement Singleton pattern instead?

   - penggunaan <b>DashMap</b> untuk variabel statis seperti <b>SUBSCRIBERS</b> sudah memberikan thread-safety secara bawaan, sehingga cocok untuk menangani akses bersamaan di lingkungan multi-threaded. Namun, jika tujuan utamanya adalah memastikan hanya ada satu instance dari daftar subscriber di seluruh aplikasi, pola desain Singleton juga bisa digunakan. Dengan Singleton, kita dapat mengontrol akses ke instance tunggal tersebut, tetapi kita tetap perlu memastikan thread-safety secara eksplisit. Di sisi lain, <b>DashMap</b> sudah menggabungkan thread-safety dan efisiensi untuk operasi seperti pembacaan dan penulisan data, sehingga lebih praktis dibandingkan mengimplementasikan Singleton dari awal. Jika kebutuhan utama adalah efisiensi dan kemudahan dalam menangani akses bersamaan, <b>DashMap</b> tetap menjadi pilihan yang lebih baik. Namun, jika ada kebutuhan tambahan untuk mengontrol instansiasi atau perilaku global lainnya, Singleton bisa dipertimbangkan. Jadi, keputusan antara <b>DashMap</b> dan Singleton bergantung pada kebutuhan spesifik aplikasi, tetapi dalam kasus ini, <b>DashMap</b> sudah memenuhi kebutuhan dengan baik.

#### Reflection Publisher-2
1. In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?

   - Dalam desain prinsip modern, memisahkan "Service" dan "Repository" dari "Model" bertujuan untuk meningkatkan modularitas, keterbacaan, dan skalabilitas kode. Dalam pola MVC tradisional, Model mencakup logika bisnis dan akses data, tetapi ini dapat menyebabkan kode menjadi terlalu kompleks dan sulit dikelola, terutama dalam aplikasi yang besar. Dengan memisahkan "Service", logika bisnis dapat ditempatkan di satu lapisan khusus, sehingga lebih mudah untuk diuji dan diubah tanpa memengaruhi lapisan lainnya. Sementara itu, "Repository" bertanggung jawab hanya untuk akses data, seperti berinteraksi dengan database, sehingga memisahkan tanggung jawab ini dari logika bisnis. Pemisahan ini juga mendukung prinsip Single Responsibility, di mana setiap komponen memiliki satu tanggung jawab spesifik. Selain itu, pendekatan ini mempermudah penggantian atau pengembangan ulang salah satu lapisan tanpa memengaruhi lapisan lainnya, misalnya mengganti database tanpa mengubah logika bisnis. Dengan demikian, pemisahan ini membantu menciptakan arsitektur yang lebih bersih, fleksibel, dan mudah dipelihara.

2. What happens if we only use the Model? Explain your imagination on how the interactions between each model <b>(Program, Subscriber, Notification)</b> affect the code complexity for each model?

   - Jika kita hanya menggunakan Model tanpa memisahkan logika bisnis dan akses data ke dalam lapisan seperti "Service" dan "Repository", interaksi antara model seperti `Program`, `Subscriber`, dan `Notification` akan membuat kode menjadi semakin kompleks. Semua tanggung jawab, mulai dari pengelolaan data, validasi, hingga logika bisnis, akan bercampur dalam setiap model. Misalnya, `Program` harus menangani bagaimana menambahkan subscriber, mengirim notifikasi, dan mungkin juga berinteraksi langsung dengan database. Hal ini akan membuat kode sulit dibaca, sulit diuji, dan rentan terhadap bug karena setiap perubahan kecil di satu model dapat memengaruhi model lainnya. Selain itu, ketergantungan antar model akan meningkat, sehingga sulit untuk memodifikasi atau mengganti salah satu model tanpa memengaruhi keseluruhan sistem. Dengan semua logika bercampur di dalam model, pengembangan fitur baru juga akan menjadi lebih lambat karena pengembang harus memahami seluruh kompleksitas model sebelum melakukan perubahan. Akibatnya, kode menjadi tidak terstruktur dan sulit dipelihara dalam jangka panjang.

3. Have you explored more about `Postman`? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.

   - Sudah, `Postman` sangat membantu untuk pengujian API, terutama dalam memastikan bahwa endpoint yang telah dibuat berfungsi dengan benar. Fitur pengiriman permintaan HTTP, seperti POST dapat langsung memberikan respons dari server, lengkap dengan status kode nya. Fitur ini sangat berguna untuk memverifikasi apakah API kita bekerja sesuai dengan yang diharapkan, tanpa perlu membuat aplikasi front-end terlebih dahulu. Dalam konteks group project atau proyek software engineering, fitur ini memungkinkan tim backend untuk menguji dan memvalidasi API mereka secara mandiri sebelum diintegrasikan dengan bagian lain dari sistem. Selain itu, Postman juga mempermudah dokumentasi API karena setiap permintaan dan respons dapat disimpan dan dibagikan dengan anggota tim lain. Dengan kemampuan ini, kolaborasi antar anggota tim menjadi lebih efisien, karena semua orang dapat memahami dan menguji API tanpa harus bergantung pada kode tambahan. Secara keseluruhan, Postman membantu mempercepat pengembangan, mengurangi potensi bug, dan memastikan kualitas API yang lebih baik dalam proyek tim.

#### Reflection Publisher-3
1. Observer Pattern has two variations: <b>Push model</b> (publisher pushes data to subscribers) and <b>Pull model</b> (subscribers pull data from publisher). In this tutorial case, which variation of Observer Pattern that we use?

   - Dalam kasus tutorial ini, variasi Observer Pattern yang digunakan adalah Push model, di mana publisher secara aktif mengirimkan data ke para subscriber. Hal ini terlihat dari implementasi fitur `Notification`, di mana notifikasi dikirim menggunakan HTTP POST request ke alamat `receive notification` milik subscriber. Dengan pendekatan ini, subscriber tidak perlu meminta data secara manual dari publisher, karena data langsung dikirimkan ketika ada perubahan seperti pembuatan, promosi, atau penghapusan produk. Push model cocok digunakan dalam skenario ini karena memastikan subscriber mendapatkan informasi secara real-time tanpa perlu melakukan polling. Pendekatan ini juga lebih efisien untuk mengurangi beban komunikasi antara publisher dan subscriber.

2. What are the advantages and disadvantages of using the other variation of Observer Pattern for this tutorial case? (example: if you answer Q1 with Push, then imagine if we used Pull)

   - Jika kita menggunakan Pull model dalam kasus tutorial ini, di mana subscriber harus secara aktif meminta data dari publisher, ada beberapa kelebihan dan kekurangan. Keuntungannya adalah subscriber memiliki kontrol penuh atas kapan dan bagaimana mereka mengambil data, sehingga dapat mengurangi beban komunikasi jika subscriber hanya membutuhkan data pada waktu tertentu. Namun, kelemahannya adalah subscriber harus terus melakukan polling ke publisher untuk memeriksa perubahan, yang dapat meningkatkan latensi dan membebani server dengan permintaan berulang, terutama jika tidak ada perubahan data. Selain itu, Pull model kurang efisien untuk skenario yang membutuhkan notifikasi real-time, seperti dalam kasus notifikasi produk di BambangShop. Oleh karena itu, meskipun Pull model memberikan fleksibilitas lebih kepada subscriber, pendekatan ini kurang cocok untuk kebutuhan tutorial ini yang mengutamakan pengiriman data secara langsung dan real-time.

3. Explain what will happen to the program if we decide to not use multi-threading in the notification process.

   - Jika kita memutuskan untuk tidak menggunakan multi-threading dalam proses notifikasi, program akan mengirimkan notifikasi ke setiap subscriber secara berurutan dalam satu thread. Hal ini dapat menyebabkan keterlambatan, terutama jika jumlah subscriber banyak atau jika ada subscriber yang lambat merespons, karena proses pengiriman ke subscriber berikutnya harus menunggu hingga pengiriman sebelumnya selesai. Selain itu, performa aplikasi secara keseluruhan dapat terpengaruh, karena thread utama akan sibuk menangani notifikasi dan tidak dapat menangani tugas lain secara paralel. Akibatnya, pengalaman pengguna dapat terganggu, terutama jika aplikasi memiliki fitur lain yang membutuhkan respons cepat. Oleh karena itu, tanpa multi-threading, efisiensi dan skalabilitas sistem akan menurun secara signifikan.