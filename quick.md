# Laravel Quickstart

- [Instalasi](#instalasi)
- [Routing](#routing)
- [Membuat View](#creating-a-view)
- [Membuat migrasi](#creating-a-migration)
- [Eloquent ORM](#eloquent-orm)
- [Menampilkan data](#displaying-data)

<a name="installation"></a>
## Instalasi

Framework Laravel menggunakan [Composer](http://getcomposer.org) untuk melakukan instalasi dan manajemen dependensi. Jika anda masih belum melakukan instalasi berikut [Panduannya](http://getcomposer.org/doc/00-intro.md).

Sekarang anda bisa menginstal Laravel dengan menjalankan perintah berikut melalui terminal:

	composer create-project laravel/laravel nama-proyek-anda --prefer-dist

Dengan perintah tersebut, Laravel akan di-instal di dalam folder `nama-proyek-anda` di dalam direktori dimana anda berada.

Jika anda mau, anda juga bisa mengunduh melalui [Repository Laravel di Github](https://github.com/laravel/laravel/archive/master.zip) secara manual. Kemudian jalankan perintah `composer install` pada folder yang anda buat secara manual. Perintah ini akan mengunduh dan menginstal seluruh dependensi Laravel.

### Permissions

Setelah melakukan instalasi Laravel, anda kadang perlu menambahkan `write permission` oleh web server terhadap direktori `app/storage`. Lihat dokumentasi [Instalasi](/docs/installation) untuk lebih jelasnya pada bagian konfigurasi.

<a name="directories"></a>
### Struktur Direktori

Setelah melakukan instalasi Laravel, anda bisa melihat-lihat seluruh folder proyek agar lebih familiar. Pada direktori `app`, terdapat folder seperti `view`, `controllers, dan `models`. Kebanyakan kode program dari aplikasi yang anda buat akan berada pada folder ini. Anda juga bisa melihat-lihat ke dalam direktori `app/config` yang berisi pilihan konfigurasi yang tersedia untuk anda.

<a name="routing"></a>
## Routing

Untuk memulai, mari kita membuat route pertama di Laravel. Buka file `app/routes.php` dan tambahkan route sebagai berikut pada bagian paling bawah file tersebut:

	Route::get('users', function()
	{
		return 'Users!';
	});

Sekarang, jika anda mengakses route `/users` melalui web browser, anda akan melihat tulisan `Users! sebagai response. Keren kan! anda baru saja membuat route pertama anda.

Route juga bisa diarahkan ke class controller. Misalnya:

	Route::get('users', 'UserController@getIndex');

Dengan menggunakan route diatas, Laravel akan mengarahkan request terhadap `/user` kepada method `getIndex` yang terdapat pada class `UserController`. Untuk informasi lebih lengkap mengenai routing controller, silahkan check [dokumentasi controller](/docs/controllerss).

<a name="creating-a-view"></a>
## Membuat view

Selanjutnya, kita juga akan membuat view sederhana untuk menampilkan data user. View disimpan pada direktori `app/views` dan berisi HTML untuk aplikasi yang sedang kita buat. Kita akan membuat dua view baru pada direktori tersebut yaitu `layout.blade.php` dan `users.blade.php`. Pertama-tama mari kita buat file `layout.blade.php`:

	<html>
		<body>
			<h1>Laravel Quickstart</h1>

			@yield('content')
		</body>
	</html>

Selanjutnya kita akan membuat view `users.blade.php`:

	@extends('layout')

	@section('content')
		Users!
	@stop

Beberapa syntax mungkin terlihat asing buat anda. Itu karena kita menggunakan templating system-nya Laravel yang disebut dengan Blade. Blade sangatlah cepat, karena merupakan kumpulan dari regular expressions yang mengubah template yang kita buat menjadi kode PHP normal. Blade menyediakan fungsionalitas yang powerful seperti template inheritance, maupun syntax yang mempersingkat syntax PHP seperti `if` dan `for`. Silahkan cek [Dokumentasi Blade](/docs/templates) untuk lebih jelasnya.

Sekarang setelah view-nya kita buat, mari tampilkan view tersebut melalui route `/users`. Route user sebelumnya mengembalikan string `Users!`, sekarang kita ubah nilai kembalian tersebut berupa sebuah view.

	Route::get('users', function()
	{
		return View::make('users');
	});

Bagus! sekarang anda sudah membuat view sederhana berdasarkan layout yang sudah ditentukan. Selanjutnya, mari kita bersentuhan dengan database layer.

<a name="creating-a-migration"></a>
## Membuat Migrasi

Untuk membuat table dan menyimpan data, kita akan menggunakan sistem migrasinya Laravel. Migrasi memudahkan kita ketika memodifikasi database, dan memudahkan pula untuk membagikannya kepada seluruh anggota tim.

Pertama-tama, mari kita mengkonfigurasi koneksi database. Anda bisa mengkonfigurasi semua koneksi database melalui file `app/config/database.php`. Secara default, Laravel dikonfigurasi untuk menggunakan MySQL, dan anda perlu untuk memasukan hak akses seperti `username` dan `password` ke dalam file konfigurasi tersebut. Jika anda mau, anda juga bisa mengubah opsi `driver` menjadi `sqlite` dan Laravel akan menggunakan database SQLite yang ada dalam direktori `app/database`.
	
Selanjutnya, untuk membuat migrasi, kita akan menggunakan [Artisan CLI](/docs/artisan). Dari dalam root project anda, jalankan perintah berikut melalui terminal:

	php artisan migrate:make create_users_table

Kemudian, sebuah file migrasi baru akan dibuat secara otomatis pada folder `app/database/migrations`. File tersebut berisi sebuah class yang terdiri dari 2 method yaitu `up` dan `down`. Di dalam method `up`, anda bisa membuat perubahan yang diinginkan berkaitan dengan table, dan di dalam method `down` anda bisa menghapusnya.

Mari kita buat sebuah migrasi sebagai berikut:

	public function up()
	{
		Schema::create('users', function($table)
		{
			$table->increments('id');
			$table->string('email')->unique();
			$table->string('name');
			$table->timestamps();
		});
	}

	public function down()
	{
		Schema::drop('users');
	}

Selanjutnya, kita akan menjalankan migrasi melalui terminal dengan menggunakan perintah `migrate`. Jalankan perintah berikut melalui direktori utama:

	php artisan migrate

Jika anda mau melakukan rollback, anda bisa menjalankan perintah `migrate:rollback`. Sekarang setelah kita mempunyai sebuah tabel dalam database, mari kita mulai menarik data dari database tersebut.s

<a name="eloquent-orm"></a>
## Eloquent ORM

Di dalam Laravel, sudah termasuk sebuah ORM yang bagus, yaitu Eloquent. Jika anda pernah menggunakan framework Ruby on Rails, anda akan merasa familiar dengan Eloquent, karena Eloquent menggunakan interaksi database bergaya ActiveRecord ORM.

Pertama-tama, mari kita definisikan sebuah model. Sebuah model Eloquent dapat digunakan untuk meng-query sebuah tabel maupun menampilkan baris tertentu dalam tabel tersebut. Jangan khawatir, anda akan mudah memahami hal ini dengan segera! Model biasanya disimpan dalam direktori `app/models`. Mari kita buat model `User.php` seperti berikut:

	class User extends Eloquent {}

Perhatikan bahwa kita tidak perlu menulis nama tabel yang akan kita gunakan. Karena Eloquent akan membaca model yang digunakan, misalkan nama model kita adalah `User` maka tabel yang akan digunakan secara otomatis adalah `users`. Lebih terlihat rapi bukan?

Dengan menggunakan aplikasi database yang biasa anda gunakan, masukan beberapa baris data ke dalam tabel `users`, dan kemudian kita akan mengambil data tersebut dan menampilkannya pada view.

Sekarang mari kita modifikasi route `/users` menjadi sebagai berikut:

	Route::get('users', function()
	{
		$users = User::all();

		return View::make('users')->with('users', $users);
	});

Let's walk through this route. First, the `all` method on the `User` model will retrieve all of the rows in the `users` table. Next, we're passing these records to the view via the `with` method. The `with` method accepts a key and a value, and is used to make a piece of data available to a view.

Awesome. Now we're ready to display the users in our view!

<a name="displaying-data"></a>
## Menampilkan Data

Sekarang setelah tabel `users` bisa diakses melalui view. Kita dapat menampilkannya sebagai berikut:

	@extends('layout')

	@section('content')
		@foreach($users as $user)
			<p>{{ $user->name }}</p>
		@endforeach
	@stop

Anda mungkin bingung kenapa tidak ada perintah `echo` pada kode diatas. Ketika menggunakan Blade kita bisa menampilkan data dengan memasukannya ke dalam kurung kurawal dobel. Jadi tidak perlu menggunakan `echo` lagi. Jadi lebih simpel. Sekarang anda bisa membuka route `/users` melalui browser dan nama dari user yang anda masukan ke dalam database akan ditampilkan.

Ini baru permulaan. Pada tutorial ini, anda telah mempelajari hal yang sangat mendasar pada Laravel, dan masih banyak hal menyenangkan yang bisa dipelajari. Teruslah membaca dokumentasi ini dan teruslah menggali lebih dalam fitur-fitur yang tersedia untuk anda pada [Eloquent](/docs/eloquent) dan [Blade](/docs/templates). Atau anda mungkin tertarik membaca tentang [Queues](/docs/queues) dan [Unit Testing](/docs/testing). Dan lagi, anda mungkin ingin mempelajari tentang arsitektur aplikasi melalui [IoC COntainer](/docs/ioc). Pilihan ada di tangan anda!
