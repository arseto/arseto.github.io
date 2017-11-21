---
layout: post
title:  "PHPUnit (2): Generate Test Data"
date:   2017-11-21 16:37:28 +0700
categories: programming php
tags: php unit-test programming tdd faker test
---

Test data diperlukan untuk memastikan test bisa dijalankan (karena fungsi pada hakikatnya hanya menerima parameter dan mengembalikan hasil tertentu). Test data dapat dibuat secara manual dengan cara _hardcode_ di file test. Namun hal ini kadang menjadi kendala apabila:

* Membutuhkan random value yang harus diuji cukup bervariasi
* Perlu meng-generate data yang sangat banyak untuk test case

Sehingga untuk generate test data bisa kita serahkan pada [faker](https://github.com/fzaninotto/faker).

Catatan: jika belum membaca [post sebelumnya tentang PHPUnit]({% post_url 2017-11-21-phpunit-unit-testing-dasar %}), disarankan untuk membacanya terlebih dahulu, karena post ini adalah lanjutan dari post tersebut.

## Install Faker

```sh
~$ composer require --dev fzaninotto/faker
```

## Gunakan Dalam Test

Ubah file test `ExampleTest.php` menjadi:

```php
<?php

use App\Employee;
use Faker\Factory as Faker;

class EmployeeTest extends PHPUnit\Framework\TestCase
{
    private $faker;

    protected function setup()
    {
        $this->faker = Faker::create();
    }

    /** @test */
    public function shouldCreateObject()
    {
        $obj = new Employee(
            $id = $this->faker->numberBetween(1, 1000),
            $name = $this->faker->name,
            $basicSalary = $this->faker->randomFloat
        );

        $this->assertEquals($id, $obj->getId());
        $this->assertEquals($name, $obj->getName());
        $this->assertEquals($basicSalary, $obj->getBasicSalary());
    }

}
```

Perhatikan bahwa data test tidak lagi ditulis secara _hardcode_ tetapi diambil dari library `faker`. Untuk API lengkap dari faker dapat dicek di [homepage faker](https://github.com/fzaninotto/Faker#basic-usage).

Kemudian jalankan test dengan perintah seperti biasa.

```sh
~$ vendor/bin/phpunit
```
