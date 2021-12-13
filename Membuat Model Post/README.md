## Membuat Model Post

Di seri ini akan banyak menggunakan terminal

### Ayo Coba

- **Rename File**

  Rename file `Post.php` yang berada di folder `models` agar file tersebut tidak hilang

- **Buka Terminal**

  Ketikkan perintah di bawah :

  ```
  php artisan make:model -m Post
  ```

  itu akan menghasilkan file yang tersimpan di folder `Models` dan otomatis membuat sebuah migration

- **Masuk Ke File Migration**

  Edit file migration `create_post_table.php` pada bagian `function up()` menjadi kode dibawah :

  ```
  public function up()
  {
      Schema::create('posts', function (Blueprint $table) {
          $table->id();
          $table->string('title');
          $table->text('excerpt');
          $table->text('body');
          $table->timestamp('published_at')->nullable();
          $table->timestamps();
      });
  }
  ```

- **Create Table**

  Ketikkan perintah di Terminal :

  ```
  php artisan migrate:fresh
  ```

- **Menggunakan Tinker**

  Ketikkan di terminal :

  ```
  php artisan tinker
  ```

  - **Inisialisai Class Post**

    Ketikkan perintah ini untuk inisialisai Class nya `$post = new Post` Atau `$post = new App\Models\Post`

  - **Inputkan Data**

    Ketikkan perintah dibawah untuk insert data :

    - Post Pertama

      ```
      $post = new App\Models\Post
      $post->title="Judul Pertama"
      $post->excerpt="sto ratione soluta fuga repellendus, consequatur maiores reiciendis dolore cupiditate, libero vitae do"
      $post->body="Lorem ipsum dolor sit amet consectetur adipisicing elit. Asperiores quisquam quia odit sapiente necessitatibus culpa suscipit similique iusto? Illo nihil, accusantium consequatur mollitia ad enim, dolor quisquam tenetur ex officia omnis autem asperiores nulla, nemo doloremque. Sequi repellat quae iusto ratione soluta fuga repellendus, consequatur maiores reiciendis dolore cupiditate, libero vitae doloremque provident odit voluptatibus ut perferendis officia id. Repellendus sed nostrum reprehenderit laudantium? Maiores qui laborum repellat, id impedit mollitia explicabo ratione odit deleniti magnam autem facilis error! Dolorum laboriosam vitae possimus aliquam. Unde voluptatum aliquid sunt, ipsum doloremque ab tenetur maxime quasi temporibus optio officiis possimus fuga itaque?"
      $post->save()
      ```

    - Post Kedua

      ```
      $post = new App\Models\Post
      $post->title="Judul Kedua"
      $post->excerpt="sto ratione soluta fuga repellendus, consequatur maiores reiciendis dolore cupiditate, libero vitae do"
      $post->body="Lorem ipsum dolor sit amet consectetur adipisicing elit. Asperiores quisquam quia odit sapiente necessitatibus culpa suscipit similique iusto? Illo nihil, accusantium consequatur mollitia ad enim, dolor quisquam tenetur ex officia omnis autem asperiores nulla, nemo doloremque. Sequi repellat quae iusto ratione soluta fuga repellendus, consequatur maiores reiciendis dolore cupiditate, libero vitae doloremque provident odit voluptatibus ut perferendis officia id. Repellendus sed nostrum reprehenderit laudantium? Maiores qui laborum repellat, id impedit mollitia explicabo ratione odit deleniti magnam autem facilis error! Dolorum laboriosam vitae possimus aliquam. Unde voluptatum aliquid sunt, ipsum doloremque ab tenetur maxime quasi temporibus optio officiis possimus fuga itaque?"
      $post->save()
      ```

- **Edit File View**

  - `posts.blade.php`
    Buka file `posts.blade.php` yang berada pada folder view

    Ganti tag `article` dengan code berikut :

    ```
    <article class="mb-4">
        <h2><a href="/posts/{{ $post->id }}">{{ $post->title }}</a></h2>
        <h6>{{ $post->author }}</h6>
        <p>{{ $post->body }}</p>
    </article>
    ```

  - `post.blade.php`
    Buka file `post.blade.php` yang berada pada folder view

    Ganti tag `article` dengan code berikut :

    ```
    @extends('layouts.main')

    @section('container')
        <article>
            <h2>{{ $post->title }}</h2>
            {!! $post->body !!}
        </article>
        <a href="/posts">Back To Posts</a>
    @endsection
    ```

    **Note:** Syntax `{!! !!}` adalah blade escape character yang berarti tidak ditambahkan sebuah fungsi `htmlspecialchars`, ini adalah kebalikan dari syntax `{{ }}`
