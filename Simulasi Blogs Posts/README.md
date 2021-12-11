## Membuat Simulasi Post

Jadi kita akan membuat sebuah postingan dengangan array terlebih dahulu dan nanti tinggal kita buat dengan database

Langkah - langkah :

- **Buat Variable Post**

  Buat Variable ini di file `routes/web.php` Pada Route `/blog`

  Salain Kode Berikut :

  ```
  Route::get('/blog', function () {
    $blog_posts = [
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
    return view('posts', [
        'title' => 'Posts',
        'posts' => $blog_posts
    ]);
  });
  ```

- **Edit File View `posts.blade.php`**

  Ubah Code nya menjadi seperti di bawah

  ```
  @extends('layouts.main')

  @section('container')
      @foreach ($posts as $post)
          <article class="mb-4">
              <h2><a href="/posts/{{ $post['slug'] }}">{{ $post['title'] }}</a></h2>
              <h6>{{ $post['author'] }}</h6>
              <p>{{ $post['body'] }}</p>
          </article>
      @endforeach
  @endsection
  ```

- **Edit File Route `routes/web.php`**

  Setelah itu kita kembali lagi ke file `routes/web.php` untuk menambahkan route baru yang akan menangani postingan setelah di klik

  Tambahkan route seperti code berikut :

  ```
  Route::get('/posts/{slug}'/* {slug} digunakan untuk menangkap seluruh slash setelah posts */, function ($slug) {
      $blog_posts = [
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

      // Digunakan untuk mencocokan apakah slug yang di klik sama
      foreach ($blog_posts as $post) {
          if ($post['slug'] == $slug) {
              $new_post = $post;
          }
      }

      // Mengarahkan Ke File post.blade.php
      return view('post', [
          'title' => 'Single Post',
          'post' => $new_post
      ]);
  });
  ```

- **Buat File Post**

  Ini adalah file yang kita buat di folder view. Buat dengan nama `post.blade.php`. Isi file tersebut dengan code berikut :

  ```
  @extends('layouts.main')

  @section('container')
      <article>
          <h2>{{ $post['title'] }}</h2>
          <h6>{{ $post['author'] }}</h6>
          <p>{{ $post['body'] }}</p>
      </article>
      <a href="/blog">Back To Posts</a>
  @endsection

  ```
