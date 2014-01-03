# Artisan Development

- [Intro](#intro)
- [Membuat sebuah command](#membuat-sebuah-command)
- [Mendaftarkan Commands](#mendaftarkan-commands)
- [Memanggil command yang lain](#memanggil-command-yang-lain)

<a name="intro"></a>
## Intro

Selain perintah/command yang disediakan oleh Artisan, anda juga bisa membuat command anda sendiri untuk memudahkan dalam membangun aplikasi anda. Anda bisa menyimpan command buatan anda pada direktori `app/commands`. Namun, sebenarnya anda juga bebas memilih lokasi yang anda inginkan selama command yang anda buat bisa di-autoload melalui setting `composer.json`.

<a name="membuat-sebuah-command"></a>
## Membuat sebuah command

### Meng-generate class

Untuk membuat sebuah command baru, anda bisa menggunakan perintah `command:make` melalui Artisan, dengan begitu sebuah file command baru akan dibuat untuk memudahkan anda membuat sebuah command:

**Meng-generate sebuah command class**

	php artisan command:make CommandBaru

Secara default, command yang tergenerate akan disimpan pada direktori `app/commands`. Namun, anda bisa juga menggenarate pada direktori yang berbeda:

	php artisan command:make CommandBaru --path=app/classes --namespace=Classes

Ketika membuat command, pilihan `--command` bisa digunakan untuk menempatkan command tersebut sebagai bagian dari command lain yang sudah ada:

	php artisan command:make AssignUsers --command=users:assign

Jika anda mau membuat sebuah command untuk [workbench package](/docs/packages), gunakan pilihan `--bench`:

	php artisan command:make AssignUsers --bench="vendor/package"

### Menulis sebuah command

Setelah command tergenerate, anda harus mengisi properti `name` dan `description` pada class yang tergenerate, yang akan digunakan untuk menampilkan command kita pada tampilan `list`.

Method `fire` akan dipanggil ketika command anda dieksekusi. Anda bisa menempatkan logic dari command anda pada method tersebut.

### Argumen & Opsi

Method `getArguments` dan `getOptions` adalah method-method dimana anda bisa mendefinisikan argumen dan opsi dari command yang anda buat. Kedua method ini mengembalikan nilai array.

Ketika mendefinisikan `arguments`, `value` dari definisi array yang anda buat digambarkan sebagai berikut:

	array($name, $mode, $description, $defaultValue)

`mode` dari sebuah argumen bisa berupa  `InputArgument::REQUIRED` atau `InputArgument::OPTIONAL`.

Ketika mendefinisikan sebuah `options`,  `value` dari definisi array yang anda buat bisa digambarkan sebagai berikut:

	array($name, $shortcut, $mode, $description, $defaultValue)

Untuk sebuah `options`, `mode` dari sebuah argumen bisa berupa `InputOption::VALUE_REQUIRED`, `InputOption::VALUE_OPTIONAL`, `InputOption::VALUE_IS_ARRAY`, `InputOption::VALUE_NONE`.

mode `VALUE_IS_ARRAY` menunjukkan bahwa sebuah opsi bisa digunakan beberapa kali ketika menjalankan perintah, contoh:

	php artisan foo --opsi=bar --opsi=baz

mode `VALUE_NONE` menunjukkan bahwa opsi tersebut hanya menggunakan sebuah "switch/nilai" sebagai pilihan:

	php artisan foo --option

### Menangkap Input User

Ketika sebuah command dijalankan, anda tentunya akan perlu mengakses nilai dari sebuth argumen dan opsi yang diterima oleh aplikasi yang anda buat. Untuk itu, anda bisa menggunakan method `argument` dan `option`:

**Menangkap Nilai dari Sebuah Argumen**

	$value = $this->argument('name');

**Mengambil Semua Argumen yang ada**

	$arguments = $this->argument();

**Menangkap Nilai dari Sebuah Opsi**

	$value = $this->option('name');

**Mengambil Semua Opsi yang ada**

	$options = $this->option();

### Menampilkan output

Untuk mengirimkan output ke console, anda bisa menggunakan method `info`, `comment`, `question' dan `error`. Setiap method ini akan menggunakan warna ANSI tertentu untuk tujuan masing-masing.

**Mengirim informasi ke console**

	$this->info('Tampilkan tulisan ini pada layar');

**Mengirim pesan error ke console**

	$this->error('Terjadi Kesalahan!');

### Menampilkan Pertanyaan

Anda juga bisa menggunakan method `ask` dan `confirm` untuk menampilkan pertanyaan untuk user:

**Memberikan pertanyaan kepada user untuk kemudian diinput jawabannya**

	$name = $this->ask('Siapakah nama anda?');

**Menanyakan sebuah pertanyaan rahasia kepada user**

	$password = $this->secret('Apa password anda?');

**Menanyakan sebuah pertanyaan konfirmasi**

	if ($this->confirm('Anda ingin melanjutkan? [yes|no]'))
	{
		//
	}

Anda bisa menentukan `default value` pada method `confirm` berupa `true` atau `false`:

	$this->confirm($question, true);

<a name="mendaftarkan-commands"></a>
## Mendaftarkan Command

Setelah command anda selesai dibuat, anda perlu mendaftarkannya pada Artisan agar bisa digunakan. Anda bisa membuka file `app/start/artisan.php`. Dalam file ini, anda bisa menggunakan method `Artisan::add` untuk mendaftarkan command tersebut:

**Mendaftarkan Command pada Artisan**

	Artisan::add(new CustomCommand);

Jika command anda sudah terdaftar pada [IoC container](/docs/ioc) dari aplikasi, ana bisa menggunakan method `Artisan:resolve` untuk membuatnya bisa diakses melalui Artisan:

**Mendaftarkan command yang berada pada IoC container**

	Artisan::resolve('binding.name');

<a name="memanggil-command-yang-lain"></a>
## Memanggil command yang lain

Terkadang anda mungkin perlu memanggil command yang lain dari command yang anda buat. Anda bisa menggunakan method `call`:

**Memanggil Command yang lain**

	$this->call('command.name', array('argument' => 'foo', '--option' => 'bar'));
