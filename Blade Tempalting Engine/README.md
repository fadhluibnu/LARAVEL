## Blade Templating Engine

Sebuah fitur atau tools dalam mengelola tampilan dalam web khususnya dalam framework. Laravel sendiri menamainya dengan Blade

Cara penggunaan lengkap Blade Tempalting Engine Ada di [Dokumentasi Laravel](https://laravel.com/docs/8.x/blade#introduction)

Di sini kita bisa mengganti

```
<h1>Hell, <?php echo $name ?></h1>
```

Menjadi

```
<h1>Hell, {{ $name }}</h1>
```

Syntak `{{ }}` adalah milik Blade dan dengan menggunakan itu, juga sudah di tambahkan sebuah fungsi `htmlspecialchars` untuk menghindari dari serangan XSS.

### Ayo Kita Coba

- **Buka File `about.blade.php`**
- **Ganti Syntax**

Ganti Syntax yang ada di file tersebut:

```
<h1>Halaman About</h1>
<h3><?php echo $name; ?></h3>
<h3><?php echo $email; ?></h3>
<img src="img/<?php echo $img; ?>" alt="">
```

Ganti menjadi

```
<h1>Halaman About</h1>
<h3>{{ $name }}</h3>
<h3>{{ $email }}</h3>
<img src="img/{{ $img }}" alt="{{ $name }}">
```

note: syntax `{{ }}` hanya akan jalan jika kita menambhakan blade di file views kita,, contoh:

`nama_file.blade.php`

## Membuat Sistem Layout Sederhana

- **[Buka Bootstrap](https://getbootstrap.com/docs/5.0/getting-started/introduction/)**
- **[Mengunakan Extending A Layout](https://laravel.com/docs/8.x/blade#extending-a-layout)**

  Yaitu menggunakan keyword `@extends` milik Blade

  `@extends` adalah ini akan di pakai oleh child view, nanti halaman child menggunakan keyword ini

- **Buat Folder `layouts` Di Folder `view`**
- **Buat File `main.blade.php`**

  Buat File ini di folder `layouts` yang barusan di buat.

  Terus Copy dan pastekan code berikut

  ```
  <!doctype html>
  <html lang="en">

  <head>
      <!-- Required meta tags -->
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1">

      <!-- Bootstrap CSS -->
      <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
          integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">

      <title>WPU Blog | Home</title>
  </head>

  <body>
      <nav class="navbar navbar-expand-lg navbar-dark bg-danger">
          <div class="container">
              <a class="navbar-brand" href="#">WPU Blog</a>
              <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
                  aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                  <span class="navbar-toggler-icon"></span>
              </button>
              <div class="collapse navbar-collapse" id="navbarNav">
                  <ul class="navbar-nav">
                      <li class="nav-item">
                          <a class="nav-link active" href="/">Home</a>
                      </li>
                      <li class="nav-item">
                          <a class="nav-link" href="/about">About</a>
                      </li>
                      <li class="nav-item">
                          <a class="nav-link" href="/blog">Blog</a>
                      </li>
                  </ul>
              </div>
          </div>
      </nav>

      <div class="container mt-4">
          @yield('container')
      </div>

      <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
          integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous">
      </script>

  </body>

  </html>
  ```

  @yield('name') berguna untuk menandai dan menangkap komponenyang ada di file child nya

- **Membuat Halaman Child**

  Untuk pertama Buka file view. Dan tambahkan kode berikut

  - `home.blade.php`

    ```
    @extends('layouts.main')

    @section('conteiner')
        <h1>Halaman Home</h1>
    @endsection
    ```

  - `about.blade.php`

    ```
    @extends('layouts.main')

    @section('container')
        <h1>Halaman About</h1>
        <h3>{{ $name }}</h3>
        <h3>{{ $email }}</h3>
        <img src="img/{{ $img }}" alt="{{ $name }}">
        <script src="script.js"></script>
    @endsection
    ```

  - `blog.blade.php`

    ```
    @extends('layouts.main')

    @section('conteiner')
        <h1>Halaman Home</h1>
    @endsection
    ```

## Membuat Partials Untuk Navbar

- **Buat Folder `partials` di folder `public`**

- **Buat File `navbar.blade.php`**

  Cut semua bagian navbar yang ada di file `layouts/main.blade.php`. Pindahkan ke file `partials/navbar.blade.php`

  code :

  ```
    <nav class="navbar navbar-expand-lg navbar-dark bg-danger">
        <div class="container">
            <a class="navbar-brand" href="#">WPU Blog</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
                aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav">
                    <li class="nav-item">
                        <a class="nav-link active" href="/">Home</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="/about">About</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="/blog">Blog</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
  ```

- **Tambahkan @include pada file `layouts/main.blade.php`**

  Tambahkan:

  ```
      @include('partials.navbar')
  ```

  ditaruh di bawah body sebelum Class Container. Ini digunakan untuk menendai bahwa `layouts/main.blade.php` mengambil sebuah nabvar dari `partials/navbar.blade.php`

## Membuat Semua View Dinamis

Saat kita sadari bahwa kalo kita masuk ke halaman `about` tapi title tatap `WPU Blogs | Home`, seharusnya kan `WPU Blogs | About`, dan link yang active

- **Masuk Ke `web.php`**
  Masukkan kode berikut

  - Route `/`

        ```
        Route::get('/', function () {
            return view('home', [
                'title' => 'Home'
            ]);
        });
        ```

  - Route `/about`

        ```
        Route::get('/about', function () {
            return view('about', [
            'title' => 'About',
            'name' => 'Fadhlu Ibnu',
            'email' => 'f.ibnu.a1@gmail.com',
            'img' => 'fadhluibnu.png'
            ]);
        });
        ```

  - Route `/posts`

    ```
    Route::get('/blog', function () {
        return view('posts', [
        'title' => 'Posts'
        ]);
    });
    ```

- **Ganti Title Di file `layouts/main.blade.php`**

  Gantikan dengan code berikut :

  ```
  <title>WPU Blog | {{ $title }}</title>
  ```

- **Ganti Link Agar Active Menjadi Dinamis pada file `partials/navbar.blade.php`**

  Ganti seluruh tag `ul` menjadi :

  ```
  <ul class="navbar-nav">
      <li class="nav-item">
          <a class="nav-link {{ $title === 'Home' ? 'active' : '' }}" href="/">Home</a>
      </li>
      <li class="nav-item">
          <a class="nav-link {{ $title === 'About' ? 'active' : '' }}" href="/about">About</a>
      </li>
      <li class="nav-item">
          <a class="nav-link {{ $title === 'Posts' ? 'active' : '' }}" href="/blog">Blog</a>
      </li>
  </ul>
  ```
