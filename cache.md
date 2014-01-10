# Cache

- [Konfigurasi](#configuration)
- [Penggunaan](#cache-usage)
- [Increments dan Decrements](#increments-and-decrements)
- [Cache Tags](#cache-tags)
- [Database Cache](#database-cache)

<a name="configuration"></a>
## Konfigurasi

Pada Laravel tersedia API untuk meng-handle berbagai jenis caching systems. Konfigurasi cache dapat anda lihat pada file `app/config/cache.php`. Pada file ini anda bisa menentukan driver cache yang ingin anda gunakan secara default di dalam aplikasi anda. Laravel sudah support terhadap beberapa caching backend yang populer seperti [Memcached](http://memcached.org) dan [Redis](http://redis.io).

Pada file konfigurasi tersebut juga tersedia beberapa opsi lain, dimana dokumentasinya bisa anda lihat dalam file itu sendiri, jadi pastikan anda telah membaca keseluruhan opsi ini. Secara default, Laravel dikonfigurasi untuk menggunakan cache driver bernama `file`, yang menyimpan object cache yang sudah ter-serialize di dalam file sistem.

<a name="cache-usage"></a>
## Penggunaan cache

**Menyimpan sebuah item di dalam cache**

	Cache::put('key', 'value', $minutes);

**Menggunakan carbon object untuk menentukan waktu kadaluarsa**

	$expiresAt = Carbon::now()->addMinutes(10);

	Cache::put('key', 'value', $expiresAt);

**Menyimpan sebuah item di dalam cache apabila tidak exist**

	Cache::add('key', 'value', $minutes);

Method `add` akan mengembalikan nilai `true` apabila item yang bersangkutan benar-benar **ditambahkan** ke dalam  cache. Apabila sebaliknya, method tersebut akan mengembalikan nilai `false`.

**Mengecek ada tidaknya sebuah cache**

	if (Cache::has('key'))
	{
		//
	}

**Mengambil sebuah item dari dalam cache**

	$value = Cache::get('key');

**Mengambil sebuah item atau mengembalikan default value**

	$value = Cache::get('key', 'default');

	$value = Cache::get('key', function() { return 'default'; });

**Menyimpan sebuah item di dalam cache selamanya**

	Cache::forever('key', 'value');

Terkadang anda mungkin ingin mengambil sebuah item dari dalam cache, tetapi juga menyimpan default value jika item tersebut tidak ada. Anda bisa melakukannya dengan menggunakan method `Cache::remember`:

	$value = Cache::remember('users', $minutes, function()
	{
		return DB::table('users')->get();
	});

Anda juga bisa mengkombinasikan method `remember` dan `forever`:

	$value = Cache::rememberForever('users', function()
	{
		return DB::table('users')->get();
	});

Perhatikan bahwa semua item yang disimpan di dalam cache telah di-serialisasi, jadi anda bebas untuk menyimpan berbagai jenis data.

**Menghapus item dari cache**

	Cache::forget('key');

<a name="increments-and-decrements"></a>
## Increments dan Decrements

Semua driver kecuali `file` dan `database` telah mendukung operasi `increment` dan `decrement`:

**Melakukan increment terhadap sebuah value**

	Cache::increment('key');

	Cache::increment('key', $amount);

**Melakukan decrement terhadap sebuah value**

	Cache::decrement('key');

	Cache::decrement('key', $amount);

<a name="cache-tags"></a>
## Cache Tags

> **Catatan:** Cache driver `file` dan `database` tidak mendukung Cache tags. Selain itu, ketika menggunakan multiple tags pada cache yang disimpan secara permanen, performance-nya akan paling bagus jika menggunakan `memcached`, dimana stale records akan secara otomatis dihapus.

Cache tags memungkinkan anda untuk melakukan tagging terhadap item yang saling berkaitan di dalam cache, dan kemudian mengelompokkan semua cache dengan nama yang sama. Untuk mengakses cache yang sudah di-tag, gunakan method `tag`:

**Mengakses cache yang sudah di-tagging**

Anda bisa menyimpan tagged cache dengan cara seperti dibawah ini, dimana `people` dan `artists` adalah nama tag dari cache yang anda buat. Silahkan anda pilih salah satu:

	Cache::tags('people', 'artists')->put('John', $john, $minutes);

	Cache::tags(array('people', 'artists'))->put('Anne', $anne, $minutes);

Anda bisa mengkombinasikan tag dengan beberapa method diantaranya `remember`, `forever`, dan `rememberForever`. Anda juga bisa meng-access item dari dalam tagged cache, maupun menggunakan method cache yang lain seperti `increment` dan `decrement`: 

**Mengakses item di dalam tagged cache**

Untuk mengakses tagged cache, anda dapat menggunakan parameter untuk method `tags` sama seperti saat menyimpan. Hanya saja kali ini anda menggunakan method `get`:

	$anne = Cache::tags('people', 'artists')->get('Anne');

	$john = Cache::tags(array('people', 'authors'))->get('John);

Anda juga bisa menghapus semua item yang di-tag menggunakan sebuah nama atau beberapa nama. Sebagai contoh, statement di bawah ini akan menghapus semua cache yang menggunakan tag `people`, `author` atau keduanya. Jadi, `Anne` dan `John` keduanya akan dihapus.

	Cache::tags('people', 'authors')->flush();

Pernyataan berikut ini akan menghapus hanya cache yang menggunakan tag `author`. Jadi "John" akan dihapus, tetapi "Anne" tidak.

	Cache::tags('authors')->flush();

<a name="database-cache"></a>
## Database Cache

Ketika menggunakan cache driver `database`, anda perlu perlu membuat sebuah table untuk menyimpan item cache. Contoh dari deklarasi `Schema` untuk tabel tersebut bisa anda lihat di bawah ini:

	Schema::create('cache', function($table)
	{
		$table->string('key')->unique();
		$table->text('value');
		$table->integer('expiration');
	});
