## Route Model Binding

Ini terjadi ketika kita menyuntikkan id ke route atau ke controller kita bisa akan query berdasarkan id yang tadi. Jika pakai ini maka laravel akan otomatis melakuakan query

### Mari Coba

- **Masuk Ke `web.php`**

  Ganti route `/posts/{slug}` menjadi :

  ```
  Route::get('/posts/{post}', [PostController::class, 'show']);
  ```

- **Masuk Ke `PostController.php`**

  Ganti funtion `show` menjadi :

  ```
  public function show(Post $post)
  {
      return view('post', [
          'title' => 'Single Post',
          'post' => $post
      ]);
  }
  ```

  parameter `$post` harus sama dengan `{post}` yang berada pada file `web.php`

- **Buka File Migration `Create_post`**

  Ganti file function `up()` menjadi :

  ```
  public function up()
  {
      Schema::create('posts', function (Blueprint $table) {
          $table->id();
          $table->string('title');
          $table->string('slug')->unique();
          $table->text('excerpt');
          $table->text('body');
          $table->timestamp('published_at')->nullable();
          $table->timestamps();
      });
  }
  ```

- **Masuk Terminal**

  Ketikkan `php artisan migrate:fresh`

- **Insert Data**

  Masuk ke tinker dengan perintah `php artisan tinker`. Lalu pastekan perintah di bawah :

  - Pertama

    ```
    Post::create([
        'title'=>'Judul Pertama',
        'slug'=>'judul-pertama',
        'excerpt'=>'Lorem ipsum pertama',
        'body'=>'<p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p><p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Veritatis repudiandae nemo adipisci blanditiis enim rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut!</p><p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p>'
    ])
    ```

  - Kedua

    ```
    Post::create([
        'title'=>'Judul Ke Dua',
        'slug'=>'judul-ke-dua',
        'excerpt'=>'Lorem ipsum ke dua',
        'body'=>'<p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p><p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Veritatis repudiandae nemo adipisci blanditiis enim rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut!</p><p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p>'
    ])
    ```

  - Ke Tiga

    ```
    Post::create([
        'title'=>'Judul Ke Tiga',
        'slug'=>'judul-ke-tiga',
        'excerpt'=>'Lorem ipsum ke tiga',
        'body'=>'<p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p><p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Veritatis repudiandae nemo adipisci blanditiis enim rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut!</p><p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p>'
    ])
    ```

- **Mengunakan Slug Sebagai URL**

  Masuk ke file view `posts.blade.php` ganti tag `h2` menjadi :

  ```
  <h2><a href="/posts/{{ $post->slug }}">{{ $post->title }}</a></h2>
  ```

- **Mengubah Route**

  Masuk ke file `web.php` ganti route `/posts/{post}` menjadi :

  ```
  Route::get('/posts/{post:slug}', [PostController::class, 'show']);
  ```

  Syntax {post:slug} berarti akan menangkap slug yang ada di table post sedangkan jika menggunakan {post} hanya bisa menangkap id postnya
