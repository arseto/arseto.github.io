---
layout: post
title:  "Bagaimana Hidup Dengan Vim?"
date:   2017-12-30 09:00:00 +0700
categories: editor
tags: editor vim
---

Ketika ada yang heran atau bertanya tanya, bagaimana pengguna vim dapat _hidup_ dengan editor vim untuk coding. Maka kemungkinan besar mereka belum pernah merasakan _power of vim_.

Saya adalah seorang programmer, maka tulisan ini saya ambil dari sudut pandang programmer. Ketika membuat coding, ada kasus yang sering dialami seorang programmer, yaitu: mengetik baris kode dengan struktur yang sama, tetapi isi berbeda.

Langkah yang diambil biasanya adalah:

* Mengetik baris kode pertama
* Copy
* Paste di baris baru
* Ubah bagian tertentu yang perlu diubah


Mari kita bandingkan antara VIM dengan text editor lain, katakanlah namanya editor **A**. Sebagai pembanding kita gunakan jumlah _keystroke_, setiap karakter dihiting 1 keystroke, untuk key combinatian/chord disesuaikan dengan jumlah tombol dalam kombinasinya (misal Shift+End adalah 2 keystroke).

## New Line

VIM: `o` (1 keystroke)

![Vim open line]({{ "/assets/img/vim_open_line.gif" }})

vs

**A**: `[End][Enter]` (2 keystroke)

![Atom open line]({{ "/assets/img/atom_open_line.gif" }})

## Copy line

VIM: `yyp` (3 keystroke)

![Vim copy line]({{ "/assets/img/vim_copy_line.gif" }})

vs

**A**: `[Home][Shift+End][Ctrl+C][End][Enter][Ctrl+v]` (9 keystroke)

![Atom copy line]({{ "/assets/img/atom_copy_line.gif" }})

## Rename assignment

VIM: `wwwce[Ketik nama baru:x karakter][Esc]www.` (10 + x keystroke)

![Vim repeat]({{ "/assets/img/vim_repeat.gif" }})

vs

**A**: `[Ctrl+->][Ctrl+->][Ctrl+->][Shift+Ctrl+->][Ketik nama baru:x karakter][Shift+Ctrl+->][Shift+Ctrl+->][Shift+Ctrl+->][Shift+Ctrl+->][Ketik nama baru:x karakter lagi]` (18 + 2x keystroke)

![Atom repeat]({{ "/assets/img/atom_repeat.gif" }})

## Result

**A** - VIM = (29 + 2x) - (14 + x) = 15 + x

Hasilnya adalah 15 + x keystroke lebih sedikit untuk VIM. Jika ini dilakukan 4 kali maka perbedaannya adalah 60 keystroke lebih. Ini hanyalah sebagian kecil dan perhitungan kasar dari sebuah contoh kasus, yang terjadi mungkin kurang dari 2 menit (dengan VIM mungkin kurang dari 1 menit untuk melakukan semua). Bayangkan penghematan yang dapat dilakukan dalam sesi coding dalam 1 hari.

Bagaimana bisa seperti ini? Fitur dari VIM yang ditonjolkan pada post ini adalah _modal editing_, yang tulisan mengenainya Insya Allah menyusul.

Dan ini hanya sebagian kecil dari kemampuan VIM, masih banyak fitur-fitur lainnya yang mempermudah _hidup_ sebagai programmer, seperti syntax highlighting, auto complete, fuzzy search, git integration, dan lain lain.

Maka dari itu, kebanyakan orang yang sudah terbiasa dengan VIM akan balik bertanya: kok kalian bisa sih, hidup tanpa VIM? hehe. Saking bingungnya, text editor lain pun mengadopsi [vim mode](https://stackoverflow.com/questions/700186/text-editors-with-vim-mode) meskipun tidak se-powerful aslinya.

Semoga bermanfaat. Happy Hacking!
