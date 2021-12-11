## Membuat Model

Di seri sebelumnya kita menaruh data kita di route, itu tidak boleh. Seharusnya kita masukkan data kita kedalam sebuah model. Untuk laravel kita akan menggunakan sebuah fitur dalam laravel yang dinamakan dengan Eloquent

[Dokumentasi Model](https://laravel.com/docs/8.x/eloquent#generating-model-classes)

Eloquent adalah sebuah objek relasional mapper (ORM) yang membuat kita mudah dalam berinteraksi dengan database kita. Dengan menggunakan eloquenet setiap tabel dalam database bisa di manupulasi insert, update, delete dengan class di dalam "Model"

Membuat Model :

- **Cara Membuat Database**

  Kita bisa membuat database di terminal kita dengan mengetikkan `php artisan make:model Nama`. Atau kita bisa menggunakan sebuah extensions [Laravel Artisan](https://marketplace.visualstudio.com/items?itemName=ryannaddy.laravel-artisan)

  Langkah - Langkah :

  - **Instal Laravel Artisan VS Code**

    [Laravel Artisan](https://marketplace.visualstudio.com/items?itemName=ryannaddy.laravel-artisan)

  - **Gunakan Laravel Artisan**

    Kita bisa menggunakan extensi ini dengan menekan `ctrl+shif+P`. Ketikkan `Artisan: Make Model`. Lalu Enter. Terus Ketikkan nama model `Post` atau terserah kalian. Terus pilih no sampai akhir

- **Masukkan Code Ke `models/Post.php`**

  Masukkan Code Berikut :

  ```
    <?php

    namespace App\Models;


    class Post
    {
        private static $blog_posts = [
            [
                'title' => "Judul Posts Pertama",
                'slug' => "judul-post-pertama",
                'author' => "Fadhlu Ibnu",
                'body' => 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Deserunt, ab ea iusto cupiditate voluptatum id suscipit provident ad, labore laboriosam dignissimos consectetur sit voluptatem omnis sint! Perspiciatis suscipit blanditiis reprehenderit sed obcaecati, sit doloribus, eligendi nostrum maxime at cupiditate molestiae voluptate nihil id, temporibus soluta maiores dolorem non ratione expedita nesciunt. Nisi odio rem vero iure dolore optio, rerum laborum? Nemo, maiores rem. Error, quibusdam. Perferendis veniam, iure iusto impedit placeat modi deleniti labore officiis omnis nemo, porro animi ducimus.'
            ],
            [
                'title' => "Judul Posts Kedua",
                'slug' => "judul-post-Kedua",
                'author' => "Fadhlu Ibnu",
                'body' => 'Lorem ipsum dolor sit, amet consectetur adipisicing elit. Deserunt, ab ea iusto cupiditate voluptatum idmaxime at cupiditate molestiae voluptate nihil id, temporibus soluta maiores dolorem non ratione expedita nesciunt. Nisi odio rem vero iure dolore optio, rerum laborum? Nemo, maiores rem. Error, quibusdam. Perferendis veniam, iure iusto impedit placeat modi deleniti labore officiis omnis nemo, porro animi ducimus.'
            ]
        ];

        public static function all()
        {
            return self::$blog_posts;
        }

        public static function find($slug)
        {
            $posts = self::$blog_posts;
            $post = [];

            foreach ($posts as $p) {
                if ($p['slug'] == $slug) {
                    $post = $p;
                }
            }

            return $post;
        }
    }
  ```

- **Ganti Route**

  Masuk ke `routes/web.php`.

  - **Ganti Route `/blog`**

    Ganti semua kode dari Route `/blog` dengan kode berikut :

    ```
    Route::get('/posts', function () {
        // $blog_posts =
        return view('posts', [
            'title' => 'Posts',
            'posts' => Post::all()
        ]);
    });
    ```

  - **Ganti Route `/posts`**

    Ganti semua kode dari Route `/posts` dengan kode berikut :

    ```
    Route::get('/posts/{slug}', function ($slug) {
        return view('post', [
            'title' => 'Single Post',
            'post' => Post::find($slug)
        ]);
    });
    ```

- **Ganti Link**

  Masuk ke file view `post.blade.php`. Lalu Ganti tag `a` menjadi seperti ini :

  ```
    @extends('layouts.main')

    @section('container')
        <article>
            <h2>{{ $post['title'] }}</h2>
            <h6>{{ $post['author'] }}</h6>
            <p>{{ $post['body'] }}</p>
        </article>
        <a href="/posts">Back To Posts</a>
    @endsection
  ```
