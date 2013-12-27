# Artisan CLI

- [Introduction](#introduction)
- [Usage](#usage)

<a name="introduction"></a>
## Introduction

Artisan adalah sebuah command-line interface yang sudah termasuk di dalam setiap source code Laravel. Didalamnya terdapat sejumlah perintah yang berguna selama membuat aplikasi dengan menggunakan Laravel. Artisan CLI dijalankan dengan menggunakan Symfony Console component yang powerful.

<a name="usage"></a>
## Cara penggunaan

Untuk melihat perintah-perintah yang tersedia, anda bisa menggunakan command `list` seperti ditunjukkan pada contoh di bawah ini:

**Menampilkan semua command yang tersedia**

	php artisan list

# Every command also includes a "help" screen which displays and describes the command's available arguments and options. To view a help screen, simply precede the name of the command with `help`:

Setiap command juga memiliki fitur bantuan (help screen) yang menampilkan dan menjelaskan argumen dan opsi yang tersedia dalam menjalankan command tersebut. Untuk menampilkan help screen, tambahkan kata `help` dibelakang command yang anda inginkan. Sebagai contoh:

**Menampilkan help screen untuk sebuah command**

	php artisan help migrate

Anda juga bisa memilih pada environment mana sebuah command akan dijalankan dengan menggunakan `--env`, misal:

**Menentukan Environment**

	php artisan migrate --env=local

Anda juga bisa melihat versi dari laravel yang saat ini sedang anda gunakan dengan menggunakan opsi `--version`:

**Menentukan versi laravel anda saat ini**

	php artisan --version
