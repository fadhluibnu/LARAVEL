## N + 1 Problem

ini terjadi ketika aplikasi kita mengambil data dari database dengan menggunakan looping, dan ini akan menjadikan aplikasi menjalankan query ke database secara berulang ulang, sehingga performa aplikasi kita menjadi lambat

Sekarang kita coba merapihkan bagian category

Ini akan menjadikan halaman category dan posts menjadi satu view

### Mari Coba

- **Masuk Ke Veiw `web.php`**

  - Ubah codingan dibawah :

        ```
        Route::get('/categories/{category:slug}', function (Category $category) {
            return view('category', [
                'title' => "Post by category : $category->name",
                'posts' => $category->posts->load('category', 'author')
            ]);
        });
        ```

        Menjadi

        ```
            Route::get('/categories/{category:slug}', function (Category $category) {
                return view('posts', [
                    'title' => "Post by category : $category->name",
                    'posts' => $category->posts->load('category', 'author')
                ]);
            });
        ```

  - Ubah Codingan dibawah :

        ```
        Route::get('/authors/{author:username}', function (User $author) {
            return view('posts', [
                'title' => "User Post",
                'posts' => $author->posts)
            ]);
        });
        ```

        Menjadi

        ```
        Route::get('/authors/{author:username}', function (User $author) {
            return view('posts', [
                'title' => "Post by author : $author->name",
                'posts' => $author->posts->load('category', 'author')
            ]);
        });
        ```

- **Masuk Post Controller**

  Ganti `'title'` pada method `index()` menjadi :

  ```
  'title' => 'All Post',
  ```

- **Masuk Ke View `posts.blade.php`**

  Edit `h1` nya menjadi :

  ```
  <h1 class="mb-5">{{ $title }}</h1>
  ```

**SELESAI**

Sekarang kita masuk ke `N + 1` Problem

kasus kita berapa pada file `posts.blade.php`, di situ kita melakukan looping dengan memanggil tabel yang lain, dan itu dilakuakan secara berkali kali

untuk melihat berapa kali query pada aplikasi, kita bisa install [clockwork](https://github.com/itsgoingd/clockwork#installation) dan instal [ekstesion clockwork](https://chrome.google.com/webstore/detail/clockwork/dmggabnehkmmfmdffgajcflpdjlnoemp?hl=id) pada browser (hanya support pada : chrome, mozila, safari).

Cara menggunakanya adalah :

- Klik kanan dan pilih inspect element
- cari clockwork

Sekarang kita akan melakukan sebuah fitur [Eager Loading](https://laravel.com/docs/8.x/eloquent-relationships#eager-loading)

Eager Loading adalah akan melakukan query saat parentnya di muat, sehingga memungkinkan aplikasi kita melakukan hanya 2 query

cara membuatnya adalah dengan menggunakan method `with([])`

### Mari Kita Coba

- **Bagian Postingan**

  Masuk ke post controller

  Edit key `'posts'` pada method `index()` menjadi :

  ```
  'posts' => Post::with(['author', 'category'])->latest()->get()
  ```

  Jadi sekarang akan memuat author dan category bersamaan sehingga pas melakukan looping ini hanya akan memanggil data yang sudah ada

- **Bagian Author Dan Category**

  Ini akan berbeda dengan bagian postingan karena, di sini kita menggunakan sebuah metode model binding. Nah kita akan menggunakan Lazy Eager Loading.

  Lazy Eager Loading adalah ini akan melakukan eager loading setelah parentnya sudah di dapatkan. Ini menggunakan method `load()`

  - Masuk ke `web.php`

    Pada route `'/authors/{author:username}'` di bagian key `posts` ganti menjadi :

    ```
    'posts' => $author->posts->load('category', 'author')
    ```

  - Masuk ke `web.php`

    Pada route `'/categories/{category:slug}'` di bagian key `posts` ganti menjadi :

    ```
    'posts' => $category->posts->load('category', 'author')
    ```
