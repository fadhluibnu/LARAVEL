<h2>⁋ Routing</h2>
<p>Untuk melakukan routing pada web, kita bisa masuk ke folder <code>routes/web.php</code>.</p>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/web.php.JPG"/>
<p>Di disini kita menemukan sebuah code bertuliskan</p>
<code>
Route::get('/', function () {
    return view('welcome');
})
</code>
<p>Itu dibaca Jika ada route dengan metode request get dengan alamat url adalah <code>/</code> maka akan menjalankan view welcome</p>
<p>Jika kita jalankan dengan memasukkan url <code>/</code> akan muncul</p>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/reoute%20slash.JPG"/>
<p>Tapi jika kita ubah Routenya get menjadi <code>/about</code></p>
<code>
Route::get('/about', function () {
    return view('welcome');
})
</code>
<p></p>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/route%20slash%20about.JPG"/>
<p>tetapi kita memasukkan url <code>/</code> pada browser maka yang terjadi adalah <code>404 | Not Found</code> seperti gambar di bawah</code></p>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/slash%20not%20found.JPG"/>

itu terjadi karena tidak ada route get yang menangani `/`. Tapi jika kita Menuliskan Route Url nya `/About` maka halaman Tidak akan memunculkan `404 | Not Found`.

Oke Sekarang kita kembalikan lagi routenya mendadi `/`.

```
Route::get('/', function () {
    return view('welcome');
})
```

Sekarang dibagian return. Sebenarnya di bagian return kita bisa menuliskan apapun seperti :

```
Route::get('/', function () {
    return "Hello Laravel";
});
```

Maka jika kita buka di browser dengan mengetikkan url `/` akan muncul Hello Laravel, seperti gambar berikut

![Gambar](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/Hello%20Laravel.JPG)

Oke sekarang kita kembalikan lagi menjadi

```
Route::get('/', function () {
    return view('welcome');
})
```

Nah cara baca returnya gimana sih? Caranya yaitu saat kita mengetikkan url `/` maka function akan dijalankan dan mengembalikan `view('welcome')`.

Tapi kok bisa tampil do browser? nah kalau kalian sudah paham konsep MVC `welcome` yang ada di dalam kurung itu sebenernya adalah sebuah file yang tersimpan di folder `resources/view` dengan nama `welcome.blade.php`

![welcome.blade.php](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/welcome.blade.php.JPG)

itulah yang membuat aplikasi kita tampil di browser. Yang terjadi sebenarnya adalah Laravelnya mencari sebuah file yang bernama `welcome`

### Mengubah tampilan

cara mengubah tampilan yaitu dengan cara:

- **Ganti Return**

  Ganti menjadi, Misal `return 'Halaman Utama';`

  ![Halaman Utama](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/return%20halaman%20utama.JPG)

Nah kalo mau buat banyak halaman tinggal copas aja seperti gambar di bawah dan ganti return sama Routenya

![gambar copas](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/gambar%20copas.JPG)

Sekarang kita coba buka halaman dengan mengetikan:

- **`/`**

  Maka akan mengeksekusi muncul route dengan url `/` yaitu

  ```
  Route::get('/', function () {
    return 'Halaman Home';
  });
  ```

  ![Gambar route /](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/halaman%20home.JPG)

- **`/about`**

  Maka akan mengeksekusi muncul route dengan url `/about` yaitu

  ```
  Route::get('/about', function () {
    return 'Halaman About';
  });
  ```

  ![Gambar route about](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/Halaman%20About.JPG)

- **`/blog`**

  Maka akan mengeksekusi muncul route dengan url `/blog` yaitu

  ```
  Route::get('/blog', function () {
    return 'Halaman Blog';
  });
  ```

  ![Gambar route blog](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/halaman%20blog.JPG)

Nah Sekarang kita coba untuk menampilkan sebuha view, caranya yaitu kita ubah return nya menjadi :

```
Route::get('/', function () {
    return view('home');
});
```

```
Route::get('/about', function () {
    return view('about');
});
```

```
Route::get('/blog', function () {
    return view('posts');
});
```

Selanjutnya kita buat sebuah File di `resources/views`

![Gambar views](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/views.JPG)

Setelah itu masukan kode html pada setiap file nya:

- **`home.blade.php`**

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WPU Blog | Home</title>
</head>

<body>
    <h1>Halaman Home</h1>
</body>

</html>
```

- **`about.blade.php`**

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WPU Blog | About</title>
</head>

<body>
    <h1>Halaman About</h1>
</body>

</html>
```

- **`posts.blade.php`**

```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>WPU Blog | Posts</title>
</head>

<body>
    <h1>Halaman Posts</h1>
</body>

</html>
```

**Menambahkan file CSS**

Buat filenya di `public` dengan nama `style.css` kemudian tambahakan code:

```
body {
    background-color: salmon;
}
```

Bagaimana cara menghubungkanya ke file `home.blade.php`, `about.blade.php`, `posts.blade.php`. Kita tinggal tambahakan code

```
<link rel="stylesheet" href="style.css">
```

di setiap file views nya. Nah, kok bisa terhubung? itu karena tag `link` sudah relative ke folder publicnya. tekhnik ini juga berlaku pada penghubungan file JavaScript dengan menggunakan tag `script` di akhir body.

Dan jika ingin menambahkan gambar kita bisa buat di folder `public` dengan nama `image`/`img`. Cara mengubungkanya yaitu

Contoh:

```
<img src="img/nama_gambar_kalian.png" alt="">
```
