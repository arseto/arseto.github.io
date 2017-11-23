---
layout: post
title:  "PHPUnit - Mocking"
date:   2017-11-21 20:00:00 +0700
categories: programming php
tags: php unit-test programming tdd mock test
---

Apabila kelas yang akan ditest membutuhkan dependensi external (kelas/objek di luar kelas yang ditest). Karena ketika melakukan test untuk sebuah kelas, kita tidak perlu mengetes kelas yang lain, maka kita dapat melakukan _mocking_ untuk membuat kelas 'palsu' untuk menggantikan kelas yang dibutuhkan tersebut.

