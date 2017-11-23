---
layout: post
title:  "PHPUnit - Unit Testing Dasar"
date:   2017-11-21 09:21:28 +0700
categories: programming php
tags: php unit-test programming tdd test
---

[PHPUnit](https://phpunit.de) adalah tools yang populer untuk melakukan unit testing di PHP. Library yang ada cukup lengkap mulai dari _assertion_ dan _mocking_. Setupnya sendiri cukup mudah dengan menggunakan tools [composer](https://getcomposer.org).

## Persiapan: Buat Project

### Direktori project

Direktori project yang akan dibuat adalah `php-test-example`.

```sh
~$ mkdir -p php-test-example
~$ cd php-test-example
```

### Generate composer.json

```sh
~$ composer init
```

Akan muncul _wizard_ di command line yang akan memandu pembuatan file `composer.json`.

![Composer Init]({{ "/assets/img/composer_init.gif" }})

### Install PHPUnit menggunakan composer

```sh
~$ composer require --dev phpunit/phpunit
```

Parameter `--dev` digunakan untuk memberi tahu composer bahwa package ini diinstall untuk keperluan development saja (require-dev). Secara _default_, _executable_ PHPUnit akan terinstall di folder `vendor/bin/phpunit`.

### Direktori source code dan namespace

Source code program akan kita letakkan di sub-direktori `src`.

```sh
~$ mkdir src
```

Root namespace untuk program kita tentukan `App`, di mapping ke semua source code di sub-direktori `src`, kita tuliskan konfigurasi ini di `composer.json`.

```json
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
```

Cukup tambahkan `autoload` di `composer.json`. Dan jangan lupa untuk generate autoload dengan menggunakan perintah:

```sh
~$ composer dumpautoload
```

## Development

### Buat project dan kelas yang akan di-test

Di dalam direktori `src` buatlah kelas `Employee` sebagai berikut:

```php
<?php
namespace App;

class Employee
{
    private $id;
    private $name;
    private $basicSalary;

    public function __construct($id, $name, $basicSalary)
    {
        $this->id = $id;
        $this->name = $name;
        $this->basicSalary = $basicSalary;
    }

    public function getId()
    {
        return $this->id;
    }

    public function getName()
    {
        return $this->name;
    }

    public function getBasicSalary()
    {
        return $this->basicSalary;
    }
}
```

Untuk kelas ini akan kita buat test untuk semua public methodnya.

### Buat file test

Untuk memudahkan eksekusi test nantinya, buat direktori khusus `tests` untuk semua test file yang akan dibuat.

```sh
~$ mkdir tests
```

Di dalam direktori `tests` buatlah kelas `EmployeeTest` sebagai berikut:

```php
<?php

use App\Employee;

class EmployeeTest extends PHPUnit\Framework\TestCase
{
    /** @test */
    public function shouldCreateObject()
    {
        $id = 1;
        $name = 'John Smith';
        $basicSalary = 1000000;

        $obj = new Employee(
            $id,
            $name,
            $basicSalary
        );

        $this->assertEquals($id, $obj->getId());
        $this->assertEquals($name, $obj->getName());
        $this->assertEquals($basicSalary, $obj->getBasicSalary());
    }
}
```

Pada test ini kita menggunakan _assertion_ `assertEquals` yang membandingkan antara nilai ekspektasi dengan nilai aktual. Nilai ekspektasi dapat diambil dari variabel langsung, karena pada kasus ini kita hanya mengetes fungsio _getter_ yang fungsinya mengambil atribut yang ada di dalam objek. Dokumentasi apa saja _assertion_ yang dapat digunakan bisa dilihat lebih lengkap di situs [PHPUnit](https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html).

## Menjalankan Test

Menjalankan test dengan PHPUnit sebelumnya harus mengkonfigurasi file mana yang akan di-_execute_ ketika akan menjalankan test. Cara paling _basic_ adalah menjalankan test di dalam struktur direktori, atau jika dibutuhkan konfigurasi yang lebih lengkap bisa dengan menggunakan f ile konfigurasi `phpunit.xml`.

### Menggunakan struktur direktori

Untuk menjalankan test di dalam direktori tertentu, gunakan perintah:

```sh
~$ vendor/bin/phpunit tests
```

![Run phpunit subdir]({{ "/assets/img/phpunit_run_subfolder.gif" }})

Atau jika membutuhkan file bootstrap/autoload bisa menambahkan parameter `--bootstrap`.

```sh
~$ vendor/bin/phpunit --bootstrap vendor/autoload.php tests
```

### Menggunakan phpunit.xml

PHPUnit bisa menggunakan file `phpunit.xml` sebagai konfigurasi test. File standar `phpunit.xml` berisi kurang lebih seperti ini:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit backupGlobals="false"
         backupStaticAttributes="false"
         bootstrap="vendor/autoload.php"
         colors="true"
         convertErrorsToExceptions="true"
         convertNoticesToExceptions="true"
         convertWarningsToExceptions="true"
         processIsolation="false"
         stopOnFailure="false"
         verbose="true"
         syntaxCheck="false">
    <testsuites>
        <testsuite name="Unit Test">
            <directory suffix="Test.php">./tests/</directory>
        </testsuite>
    </testsuites>
</phpunit>
```

Perhatikan parameter `bootstrap` di file xml di atas, fungsinya sama dengan parameter `--bootstrap` apabila menggunakan metode test di dalam direktori.

Di dalam file xml ini terdapat elemen `<testsuites>` yang berfungsi menunjukkan detail konfigurasi per `test-suite`. Di dalam `testsuites` terdapat beberapa `testsuite` yang independen. Di dalamnya memuat direktori yang akan dijalankan ketika test, serta terdapat atribut `suffix` untuk mem-_filter_ file dengan akhiran tertentu saja yang akan dijalankan sebagai test.

Konfigurasi dengan `phpunit.xml` lebih disarankan ketika dibutuhkan konfigurasi testing yang lebih kompleks dari sekedar struktur direktori saja. Serta memudahkan untuk kolaborasi/sharing karena cara untuk menjalankan test menjadi tidak perlu tambahan lagi, cukup jalankan:

```sh
~$ vendor/bin/phpunit
```

Test suite bisa didefinisikan lebih dari satu, jika tidak diberi parameter ketika menjalankan, maka semua test suite akan dijalankan. Untuk menjalankan satu test suite saja, gunakan perintah:

```sh
~$ vendor/bin/phpunit --testsuite "Unit Test"
```

## Kesimpulan

Dengan menggunakan unit test kita dapat melakukan testing logika aplikasi tanpa harus menjalankan program secara keseluruhan (misal http server). PHPUnit sudah menyediakan library yang lumayan lengkap untuk melakukan unit test ini. Source code lengkap bisa didapat di [sini](
https://github.com/arseto/php-test-example).

Semoga bermanfaat, Happy Coding!
