# Instalasi

- [Instal Composer](#instal-composer)
- [Instal Laravel](#instal-laravel)
- [Server Requirements](#server-requirements)
- [Konfigurasi](#konfigurasi)
- [Pretty URLs](#pretty-urls)

<a name="instal-composer"></a>
## Instal Composer

Laravel menggunakan [Composer](http://getcomposer.org) untuk memanage dependensi-dependensinya. Pertama-tama, unduh `composer.phar`. Selanjutnya setelah di-unduh anda dapat memindahkannya ke dalam folder proyek atau memindahkannya ke `usr/local/bin` untuk menggunakannya secara global. Sedangkan, pada Windows anda bisa menggunakan [Windows installer](https://getcomposer.org/Composer-Setup.exe).

<a name="instal-laravel"></a>
## Instal Laravel

### Melalui Laravel Installer

Pertama-tama, unduh [Laravel installer PHAR archive](http://laravel.com/laravel.phar). Untuk memudahkan, ubah nama file menjadi `laravel` dan pindahkan ke `/usr/local/bin`. Setelah terinstal, gunakan command `laravel new` didalam direktori yang anda pilih untuk membuat proyek baru. Misalnya, dengan mengetikan `laravel new blog` akan dibuat sebuah direktori baru bernama `blog` yang berisi instalasi Laravel yang masih baru dengan seluruh dependensi yang sudah diinstal. Cara instalasi seperti ini jauh lebih cepat daripada instalasi melalui Composer.

### Via Composer Create-Project

Anda juga bisa meng-install Laravel dengan menggunakan command `create-project` melalui terminal:

	composer create-project laravel/laravel --prefer-dist

### Via Download


Setelah anda melakukan instalasi Composer, download [versi terbaru](https://github.com/laravel/laravel/archive/master.zip) Laravel framework, kemudian extract dan simpan di direktori server anda. Selanjutnya, didalam folder utama aplikasi Laravel anda, jalankan perintah `php composer.phar install` (atau `composer install`) untuk memasang semua dependensi-nya. Proses ini mengharuskan adanya Git yang sudah terinstall pada komputer anda untuk menyelesaikan proses instalasi.

Jika anda ingin melakukan update Laravel framework, anda bisa menjalankan perintah `php composer.phar update`.

<a name="server-requirements"></a>
## Server Requirements

Framework Laravel membutuhkan beberapa persyaratan supaya dapat berjalan dengan baik, diantaranya:

- PHP >= 5.3.7
- MCrypt PHP Extension

Apabila menggunakan PHP 5.5, beberapa OS mengharuskan anda untuk melakukan instalasi secara manual terhadap PHP JSON extension. Ketika menggunakan Ubuntu, proses ini bisa dilakukan dengan menjalankan perintah `apt-get install php5-json`.

<a name="konfigurasi"></a>
## Konfigurasi

Laravel hampir tidak membutuhkan konfigurasi apapun saat dilakukan instalasi. Anda bebas untuk langsung mengembangkan aplikasi anda segera setelah melakukan instalasi! Namun demikian, anda mungkin perlu melihat file `app/config/app.php` dan dokumentasinya. Pada file tersebut terdapat beberapa opsi seperti `timezone` dan `locale` yang mungkin anda ingin ubah sesuai keperluan aplikasi anda.

<a name="permissions"></a>
### Permissions

Laravel terkadang membutuhkan sebuah permissions untuk dikonfigurasi: folder-folder yang ada di app/storage membutuhkan `write access` terhadap web server.

<a name="paths"></a>
### Paths

Beberapa direktori yang ada di framework ini bisa dikonfigurasi. Untuk mengubah lokasi dari direktori-direktori tersebut, cek file `bootstra/paths.php`.

<a name="pretty-urls"></a>
## Pretty URLs

Di dalam framework ini sudah terdapat file `public/.htaccess` yang memungkinkan sebuah URL tidak menggunakan `index.php`. Jika anda menggunakan Apache sebagai web server, pastikan agar modul `mod_rewrite` anda sudah di enable.

Jika file `.htaccess` yang sudah termasuk dalam Laravel tidak bisa berjalan pada instalasi Apache anda, cobalah yang berikut ini:

	Options +FollowSymLinks
	RewriteEngine On

	RewriteCond %{REQUEST_FILENAME} !-d
	RewriteCond %{REQUEST_FILENAME} !-f
	RewriteRule ^ index.php [L]
