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
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. Menurut saya, single Model struct sudah cukup pada kasus BambangShop ini. Hal ini dikarenakan belum terlalu kompleks, sehingga tidak diperlukan interface atau trait karena hanya ada tepat 1 tipe subscriber. Note that pada kasus lain yang lebih kompleks, misal subscriber perlu dipecah menjadi beberapa tipe, maka interface bisa menjadi pilihan yang tepat. Tetapi, pada kasus BambangShop, single Model struct sudah cukup.

2. Menurut saya, penggunaan DashMap sudah tepat karena dapat mengakses valuenya dengan key, tidak harus index berupa integer dari 0,1,... Jika menggunakan Vec, akan menjadi tidak efisien karen ahrus melakukan iterasi satu per satu hingga menemukan key yang diinginkan. Dengan demikian, DashMap menjadi lebih efisien. Note that tetap bisa menggunakan Vec, tetapi untuk efisiensi, lebih baik menggunakan DashMap.

3. Menurut saya, singleton maupun dashmap sama-sama diperlukan. Hal ini dikarenakan Singleton akan assure bahwa terdapat tepat satu instance dari interface dan DashMap akan assure bahwa data subscriber tetap safe untuk digunakan secara concurrent (oleh beberapa thread).

#### Reflection Publisher-2

1. Menurut saya, Service dan Repository perlu dipisah untuk separation of concerns (single responsibility principle). Dengan dipisahnya Service dan Repository, jika ingin memodifikasi bagian yang berhubungan dengan program logic, kita hanya perlu mengubah bagian Service tanpa harus ikut mengubah Repository. Begitupun sebaliknya, ketika ingin mengubah bagian storage (Repository), kita tidak perlu mengubah bagian Service (program logic). Dengan demikian, program menjadi lebih maintainable dan mudah untuk dimodifikasi. Tanpa memisahkan Service dan Repository dari Model, akan ada terlalu banyak responsibility pada Model, sehingga sulit untuk dimaintain.

2. Menurut saya, apabila kita memaksakan hanya menggunakan Model saja (tanpa pemisahan Service dan Repository), maka maintainability kode akan berkurang secara signifikan. Moreover, kita memiliki 3 Model (program, subscriber, dan notification). Sehingga, kompleksitas kode akan bertambah karena ketika ingin memodifikasi bagian logic, terpaksa harus mereview bagian storage, dan sebaliknya.

3. Setelah saya explore, Postman dapat digunakan untuk testing API dari suatu aplikasi. Dapat digunakan dengan method GET, POST, dan lain sebagainya untuk memeriksa apakah aplikasi berjalan dengan benar sesuai ekspektasi. Note that kita dapat membuat custom body request, token, dan lain sebagainya. Maka dari itu, menurut saya Postman adalah salah satu tools yang berguna untuk pengembangan aplikasi. 


#### Reflection Publisher-3
