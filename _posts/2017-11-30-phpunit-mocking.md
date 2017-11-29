---
layout: post
title:  "PHPUnit - Mocking"
date:   2017-11-30 02:00:00 +0700
categories: programming php
tags: php unit-test programming tdd mock test
---

Apabila kelas yang akan ditest membutuhkan dependensi external (kelas/objek di luar kelas yang ditest). Karena ketika melakukan test untuk sebuah kelas, kita tidak perlu mengetes kelas yang lain, maka kita dapat melakukan _mocking_ untuk membuat kelas 'palsu' untuk menggantikan kelas yang dibutuhkan tersebut.

Metode *mocking* ini biasa digunakan salah satunya apabila kita menggunakan teknik *dependency injection*.

## Contoh: Kelas dengan *dependency injection* (DI)

```php
<?php
namespace App\SalaryV2;

use App\EmployeeRepository;
use App\MonthlyPresenceRepository;

class SalaryCalculator
{
    private $employeeRepo;
    private $presenceRepo;

    public function __construct(
        EmployeeRepository $employeeRepo,
        MonthlyPresenceRepository $presenceRepo
    ){
        $this->employeeRepo = $employeeRepo;
        $this->presenceRepo = $presenceRepo;
    }

    public function calculate($employeeId, $fromDate, $toDate)
    {
        $employee = $this->employeeRepo->find($employeeId);
        $presence = $this->presenceRepo->query($employeeId, $fromDate, $toDate);

        $totalWorkingDay = floatval($presence->getTotalWorkingDay());
        $presenceDay = floatval($presence->getPresenceDay());
        $basicSalary = floatval($employee->getBasicSalary());

        $ratio = $presenceDay / $totalWorkingDay;

        $proRateSalary = $ratio * $basicSalary;

        return intval(ceil($proRateSalary));
    }
}
```
file: `SalaryCalculator.php`

Teknik *dependency injection* adalah salah satu teknik simpel yang dapat digunakan untuk membuat kode lebih bersih dengan cara meng-abstraksi beberapa komponen dari aplikasi secara fungsionalnya. Pada contoh di atas, fungsi logika perhitungan *salary* dipisahkan dari fungsi akses database (repository).

Pada contoh ini, fungsi akses database di-abstraksi dengan menggunakan interface:

### Employee Repository

```php
<?php
namespace App;

interface EmployeeRepository
{
    public function find($id) : Employee;
}
```

### Monthly Presence Repository

```php
<?php
namespace App;

interface MonthlyPresenceRepository
{
    public function query($employeeId, $fromDate, $toDate) : MonthlyPresence;
}
```

Untuk mengetes kelas ini, kita tidak perlu mengetes implementasi dari `EmployeeRepository` dan `MonthlyPresenceRepository`, tetapi kita cukup membuat *mock* dari interface tersebut dan mendefinisikan *behavior*-nya sesuai dengan skenario tes yang akan kita buat.

Catatan: sebelum masuk ke bagian selanjutnya, buat terlebih dahulu kelas unit test untuk kelas di atas, dengan mengikuti langkah seperti yang sudah dijelaskan di [sini]({% post_url 2017-11-21-phpunit-unit-testing-dasar %}).

## Buat Mock Repository

Terlebih dahulu kita buat *mock* untuk repository.

```php
...

$employeeRepo = $this->getMockBuilder(EmployeeRepository::class)
    ->getMock();

$presenceRepo = $this->getMockBuilder(MonthlyPresenceRepository::class)
    ->getMock();

...
```

Untuk membuat *mock* dari suatu interface, kita bisa menggunakan fungsi dari PHPUnit, yaitu `getMockBuilder`. Kelas *mock* tersebut kemudian dapat di inject ke dalam constructor kelas `SalaryCalculator`.

```php
...
$calculator = new SalaryCalculator($employeeRepo, $presenceRepo);
...
```

## Definisikan Behavior dari Kelas *Mock*

Kita sudah mendefiniskan *mock* dari kelas dependensi kelas yang akan kita test. Namun, karena kita belum mendefinisikan behavior dari kelas *mock* tersebut, yang terjadi adalah behavior dari kelas *mock* tersebut masih *default*. Hal ini bisa kita lihat kalau kita jalankan fungsi yang akan kita tes di file unit test tersebut.

```php
...
$calculator = new SalaryCalculator($employeeRepo, $presenceRepo);
$calculator->calculate(1, date('Y-m-d'), date('Y-m-d'));
...
```

Akan menghasilkan *division by zero* karena keluaran salah satu fungsi yang digunakan sebagai pembagi adalah 0 (nilai default).

```
PHPUnit 6.4.4 by Sebastian Bergmann and contributors.

Runtime:       PHP 7.1.12 with Xdebug 2.5.5
Configuration: /home/arseto/Projects/github-shared/php-test-example/phpunit.xml

..E                                                                 3 / 3 (100%)

Time: 49 ms, Memory: 6.00MB

There was 1 error:

1) SalaryCalculatorV2Test::shouldCalculateSalary
Division by zero
```

Dengan melihat fungsi `calculate` di atas, terlihat bahwa `$employeeRepo` memanggil fungsi `find`, dan menghasilkan suatu keluaran yang dimasukkan ke dalam variabel `$employee`. Kita coba definisikan behavior dari fungsi `find` tersebut dengan cara:

```php
...
$employeeRepo->expects($this->once())
    ->method('find')
    ->willReturn(true);

$calculator = new SalaryCalculator($employeeRepo, $presenceRepo);
$calculator->calculate(1, date('Y-m-d'), date('Y-m-d'));
...
```

Yang ternyata menghasilkan error:

```
There was 1 error:

1) SalaryCalculatorV2Test::shouldCalculateSalary
TypeError: Return value of Mock_EmployeeRepository_d2c8849f::find() must be an instance of App\Employee, boolean returned
```

Periksa kembali definisi interface [`EmployeeRepository`](#employee-repository), ternyata mengharuskan return type `Employee`. Maka kita bisa ubah behaviornya sesuai yang dibutuhkan oleh interface tersebut, demikian pula untuk interface [`MonthlyPresenceRepository`](#monthly-presence-repository). Sehingga menjadi:

```php
...
$employeeRepo->expects($this->once())
	->method('find')
	->willReturn(new Employee(1, 'John Smith', 22000000));

$presenceRepo->expects($this->once())
	->method('query')
	->willReturn(new MonthlyPresence(22, 20));
...
```

Rumus yang didefinisikan oleh fungsi adalah (hari masuk kerja/total hari kerja) * gaji pokok. Sehingga kita bisa assert hasil untuk fungsi ini seharusnya adalah `20000000`.

```php
...
$calculator = new SalaryCalculator($employeeRepo, $presenceRepo);
$this->assertEquals(20000000, $calculator->calculate(1, date('Y-m-d'), date('Y-m-d')));
```

Dan test berhasil dengan sukses:

```
...                                                                 3 / 3 (100%)

Time: 47 ms, Memory: 6.00MB
```

## Selanjutnya?

Tes ini masih bisa diperbaiki lagi, di antaranya:

* Karena kelas `Employee` dan `MonthlyPresence` di dalam fungsi `calculate` hanya memanggil method juga maka bisa juga dibuat *mock* untuk kedua kelas di atas dan gunakan teknik *mocking* untuk fungsi yang digunakan saja (tidak perlu semuanya).
* Parameter untuk fungsi `find` dan `query` juga bisa di *assert* dengan memanggil method `with($param1, $param2, ...)`, selengkapnya bisa dicek di dokumentasi [PHPUnit](https://phpunit.de/manual/3.0/en/mock-objects.html).

Source code bisa dilihat di [sini](https://github.com/arseto/php-test-example). Semoga bermanfaat, happy coding!
