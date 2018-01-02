---
layout: post
title:  "VIM: Modal Editing"
date:   2018-01-02 21:00:00 +0700
categories: editor
tags: editor vim
---

Ketika pertama kali membuka vim, orang yang tidak terbiasa akan bingung karena ketika tombol keyboard ditekan, bukannya karakter yang tertulis di layar, tetapi malah kursor berpindah entah ke mana, atau malah ada karakter yang terhapus, atau ter-copy. Saking bingungnya, sejuta orang (sampai terakhir data diambil) [mencari jalan keluar dari vim](https://stackoverflow.blog/2017/05/23/stack-overflow-helping-one-million-developers-exit-vim/).

![exit vim](https://www.memecreator.org/static/images/memes/4664109.jpg)

*Exit VIM*

## Modal Editing

Editor VIM memiliki beberapa mode dalam penggunaannya. Dari situlah muncul istilah _modal editing_. Dalam VIM kita mengenal 3 mode utama:

* Normal mode
* Insert mode
* Visual/Selection mode
* Command mode

Inilah yang biasa membuat orang bingung. Karena mayoritas editor tidak memiliki _multi-mode_ seperti ini. Sehingga ekspektasinya adalah ketika diketik sobuah karakter, maka karakter tersebut akan tertulis di layar. Sedangkan dalam VIM, akan tergantung dari mode yang sedang aktif pada saat itu.

### Normal Mode

Dalam normal mode, ketika kita input karakter dengan keyboard, VIM akan menangkapnya sebagai _command_, bukan sebagai input karakter. Input _command_ di dalam command mode menggunakan _mnemonic_ atau kata yang disingkat, lebih jelas nanti kita lihat pada contoh.

#### Movement Commands

Di dalam normal mode, kita bisa menggunakan _movement command_ untuk melakukan navigasi di dalam teks yang sedang di edit lebih efisien daripada menggunakan navigasi dengan tombol arah/panah. Contoh:

* `e` - go to **e**nd of word
* `b` - go to **b**eginning of word
* `w` - go to next **w**ord

Bisa juga dikombinasikan dengan angka, seperti:

* `4w` - go to **4** next **w**ords
* `5b` - go to **5** words **b**ack

Bagaimana kalau kita ingin menggeser kursor dengan lebih presisi, misal per karakter, seperti halnya ketika kita menggunakan _arrow keys_ di editor pada umumnya?

* `h` - move cursor left
* `j` - move cursor down
* `k` - move cursor up
* `l` - move cursor right

Di VIM digunakan `hjkl` sebagai pengganti tanda panah. Sehingga pergerakan tangan menjadi lebih efisien (tidak perlu meninggalkan _home row_).

`hjkl` = `wasd` of vim

#### Modifying Commands

Kita dapat melakukan modifikasi teks, seperti hapus, copy, paste, dan sebagainya di dalam normal mode juga. Modifying command ini ada yang stand alone dan ada yang harus menggunakan kombinasi movement.

Stand alone:

* `p` - **p**aste
* `x` - delete
* `~` - change case
* `u` - **u**ndo

Beberapa perintah _modifying commands_ membutuhkan kombinasi, misal dengan movement command.

* `de` - **d**elete to the **e**nd of the word
* `d4w` - **d**elete **4** **w**ords
* `y5j` - **y**ank (copy) **5** lines downward

Saya lebih suka menyebutnya **combo commands**.

<img src="http://darkdemon.org/user-files/37704/1352017312.gif" alt="stickman combo" style="height: 250px;"/>

*Block punch kick run, repeat*

### Insert Mode

Di dalam insert mode inilah kita melakukan editing text. Ketika di dalam mode ini, kebanyakan pengguna akan merasa familiar karena behavior yang hampir sama dengan text editor lainnya, _see as you type_. Enter, backspace, arrow keys, semua berlaku seperti pada text editor pada umumnya. Insert mode dapat diakses dari normal mode dengan menekan:

* `i` - **i**nsert (kursor mulai sebelah kiri _highlight_)
* `a` - **a**ppend (kursor mulai sebelah kanan _highlight_)

Beberapa command lain, seperti command `c[movement]` (**c**hange sesuai _movement_), juga secara otomatis juga akan masuk ke dalam insert mode.

* `ce` - **c**hange to the **e**nd of the word
* `ci'` - **c**hange **i**nside `'`

### Visual Mode

Visual mode adalah mode untuk melakukan selection pada text, misal untuk copy/cut/delete. Visual mode dapat diakses dari normal mode dengan menekan:

* `v` - masuk ke visual mode standar, selection dilakukan per karakter
* `[Shift+v]` - masuk ke line visual mode, selection akan dilakukan per _line_
* `[Ctrl+v]` - masuk ke block visual mode, selection akan dilakukan dengan cara _block_, seperti `[Alt+click+drag]` di editor lain

### Command Mode

VIM memiliki perintah tertulis (_command line_) untuk mengakses fitur-fitur di dalamnya. Command mode dapat diakses dari normal mode dengan menekan `:`.

* `:q` - **q**uit active window
* `:w` - **w**rite to file

Perintah yang diketik dalam command mode juga dapat dikombinasikan.

* `:qa` - **q**uit **a**ll windows
* `:wq` - **w**rite then **q**uit
* `:wqa` - **w**rite then **q**uit **a**ll windows

## Lebih Lanjut

Bingung? Atau merasakan awal yang berat dalam mempelajari VIM? Instalasi VIM secara otomatis sudah memasukkan aplikasi `vimtutor` yang di dalamnya sudah terdapat berbagai tutorial VIM yang mudah dan menyenangkan. Cukup ketikkan `vimtutor` di terminal (Linux & OSX). Untuk pengguna Windows dapat mencari tahu lebih lanjut di versi windows dari VIM.

Semoga bermanfaat. Happy hacking!
