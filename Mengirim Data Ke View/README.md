## Mengirim Data Ke View

Ini digunakan agar kita tidak menambahkan secara manual semua data ke view kita. Dan data ini dikirim dari route kita

caranya:

**1. Masuk Ke File `routes/web.php`**

**2. Masukkan parameter Tambahan**

Contoh:

```
Route::get('/about', function () {
    return view('about', [
        'name' => 'nama kalian',
        'email' => 'nama_kalian@gmail.com',
        'img' => 'nama_image_kalian.png'
    ]);
});
```

**3. Panggil Key-nya Di File `about.blade.php`**

Tambhakan code berikut :

```
<h1>Halaman About</h1>
<h3><?php echo $name; ?></h3>
<h3><?php echo $email; ?></h3>
<img src="img/<?php echo $img; ?>" alt="">
```

pastekan kode itu ke file `about.blade.php` di dalam body nya. Jadi sebenarnya yang terjadi adalah file `about.blade.php` memanggil key yang ada di file `routes/web.php` dengan route `/about`.

File `about.blade.php` memanggil key nya dengan cara menuliskan sebuah variable yang sama dengan key yang ada di `routes/web.php` yaitu `$name` untuk key `name`, `$email` untuk key `email`, `$img` untuk key `img`.
