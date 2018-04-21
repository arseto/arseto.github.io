---
layout: post
title:  "Clean Architecture: Konsep Dasar"
date:   2018-03-28 16:00:00 +0700
categories: programming
comments: true
tags: programming clean-architecture inversion-of-control
---

> Framework and driver shouldn't dictate how we build our application logic. It's the reverse, the framework and driver should do what the application logic says.

## Keywords

> Clean Code, Arhitecture, Clean Architecture, Dependency Injection, Inversion of Control

## Problem

Seringkali ketika membuat aplikasi, kita terlalu memprioritaskan untuk menentukan framework di depan. Misal: menggunakan Laravel dengan database Postgres, atau Spring dengan database MySql. Padahal ketika membuat aplikasi, framework hanyalah platform tempat aplikasi tersebut berjalan, faktanya:

* Web Framework hanyalah metode untuk menerima request dan memberi response.
* Database Driver hanyalah metode untuk menyimpan data. Sedangkan data adalah property dari aplikasi, bukan database.

Akibat dari menentukan framework di depan, tidak jarang kode aplikasi menjadi *tight-coupled* ke framework/driver yang dipakai. Sehingga apabila diperlukan porting atau perubahan framework/driver (misal karena ada kebutuhan performa atau *obsolescence*) akan memakan waktu dan usaha yang banyak.

## Bagaimana Seharusnya

Ketika membicarakan aplikasi (*business rule*) maka logika aplikasi harus lebih penting daripada penentuan framework/driver. Sehingga keputusan mengenai framework/driver yang digunakan dapat di-defer (tunda) di belakang.

Hal terpenting yang perlu diperhatikan adalah memisahkan antara logika bisnis aplikasi dengan framework yang menjadi platform dari aplikasi. Misal:

* Web framework tidak mempengaruhi logika aplikasi, tetapi logika aplikasi-lah yang menggunakan web framework untuk menerima request dan memberi response.
* Database Driver tidak mempengaruhi data yang disimpan aplikasi, hanya menulis data yang diminta oleh aplikasi, dan memberi data yang diminta oleh aplikasi.

Arsitektur yang mengakomodasi aturan tersebut selanjutnya dapat disebut *Clean Architecture* karena kode aplikasi *bersih* dari kode-kode framework dan driver.

## *Decoupling*: Gunakan *Layers*

Secara global, arsitektur ini dapat dibagi menjadi 4 *layer* (tidak harus seperti ini):

* Entities
* Use Cases
* Interface Adapters
* Framework and Drivers

<div align="center"><img src="https://8thlight.com/blog/assets/posts/2012-08-13-the-clean-architecture/CleanArchitecture.jpg" alt="clean architecture" width="550px" /></div>

Source: [8th Light](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)

### Entities

Mendefinisikan *business rule* yang dipakai bersama di keseluruhan aplikasi enterprise. Bisa berupa object dengan method, atau struktur data. Misal:

* Struktur data `User` yang digunakan di seluruh aplikasi.
* Kelas `Date` yang digunakan untuk melakukan operasi terkait tanggal, yang digunakan di seluruh aplikasi.

Kode di *layer* ini jarang berubah, karena merupakan aturan yang ada di inti aplikasi enterprise. Kalau berubah di sini, bisa jadi *layer-layer* di luarnya akan terkena dampaknya. Tapi sebaliknya, perubahan di *layer* luarnya tidak boleh berdampak pada kode di *layer* ini. Kode di *layer* ini boleh berubah bila ada perubahan *business rule* yang terkait dengan keseluruhan aplikasi enterprise.

### Use Cases

Mendefinisikan logika kasus spesifik penggunaan aplikasi dalam bentuk use case. Misal:

* Create Order. Khusus digunakan untuk membuat order, di dalamnya ada aturan-aturan bagaimana order dibuat.

Perubahan di *layer* ini akan terjadi apabila ada perubahan di behavior use case aplikasi.

### Interface Adapters

Mengkomunikasikan antara framework/driver dengan aplikasi, dan sebaliknya. Kalau kita menggunakan framework seperti Web MVC, maka letaknya hanya di *layer* ini. Data akan dikonversi menjadi bentuk yang dapat digunakan oleh aplikasi (use case/entities).

* Ketika menyimpan data, *layer* ini berisi SQL Command untuk MySQL
* Ketika menampilkan hasil operasi/fungsi, *layer* ini berisi kode bagaimana data direpresentasikan dalam bentuk yang dapat dibaca.

### Framework and Driver

Layer ini berisi framework dan driver, misal web framework dan database driver. Yang dilakukan di *layer* ini seharusnya cukup menyambungkan antara framework/driver dengan *layer-layer* di dalamnya. Misal:

* Ketika menggunakan framework dengan dependency injection, maka di sini adalah service containernya.
* Untuk web framework, di sini didefinisikan routing ke controller yang dipakai. Di mana controller masuk ke *layer* di bawahnya (interface adapters).

## Dependency Rule

* Arah dependency harus dari luar ke dalam, contoh:
    * Framework ke Interface Adapter
    * Interface Adapter ke use case
* Layer tidak harus sama seperti di gambar, tetapi aturan dependency tetap sama.

### Dependency Inversion Principle

Bagaimana jika output *business rule* harus direpresntasikan sebagai aksi di halaman web, misal me-render halaman dengan parameter data tertentu? Bukankah hal ini menyebabkan business rule harus depend ke fungsi render halaman web? Hal ini tentu saja melanggar *dependency rule* di atas.

Untuk kasus semacam ini, cara mengatasinya bisa menggunakan *Dependency Inversion Principle*, atau biasa disebut juga *Inversion of Control*, karena *flow of control* dibuat berlawanan dengan arah dependency.

<div align="center"><img src="https://image.slidesharecdn.com/designpatternintegrationdip-090913141412-phpapp01/95/dependency-inversion-principle-7-728.jpg?cb=1252864904" width="550px" /></div>

Source: [Slideshare](https://www.slideshare.net/MarcoManga/dependency-inversion-principle)

Prinsip *dependency inversion*:

* Depend ke abstraksi
* Jangan depend ke *concrete class*
* Inject dependendency di runtime
    * Manual (di fungsi main/entry point), atau
    * Menggunakan *dependency injection container*

## Kesimpulan

* Kode *business rule* yang terlalu *tight-coupled* ke framework/driver akan mengalami kesulitan ketika ingin mengubah/porting framework/driver yang digunakan.
* Solusi untuk mengatasi tight coupling antara framework dengan *business rule* adalah dengan membagi menjadi beberapa *layer* sesuai dengan kompleksitas aplikasi, dan menerapkan *dependency rule* di mana kode *business rule* berada di lapisan paling dalam pada aplikasi.
* Dengan demikian akan terbentuk arsitektur kode yang testable dan mudah di-substitusi komponen framework/driver-nya tanpa mengganggu *business rule*

## Glossary

* [8th Light: Clean Architecure by Bob Martin](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)
* [Martin Fowler: Inversion of Control](https://martinfowler.com/articles/injection.html)
