## Implemantasi

Langkah Langkah :

- **Buka file Model Post**

  Buka file `Post.php` tambahkan kode berikut :

  ```
  public function category()
  {
      return $this->belongsTo(Category::class);
  }
  ```

  Kode di atas digunakan untuk memanggil category sesuai dengan post kita

- **Buka file View Post**

  Buka file `post.blade.php` dan edit :

  ```
    @extends('layouts.main')

    @section('container')
        <article>
            <h2>{{ $post->title }}</h2>

            <p>By Fadhlu Ibnu in <a href="/categories/{{ $post->category->slug }}">{{ $post->category->name }}</a></p>

            {!! $post->body !!}
        </article>
        <a href="/posts">Back To Posts</a>
    @endsection
  ```

  Kode di atas digunakan untuk menampilakan category sesuai post kita yang sudah di ambil oleh model Post

- **Buka File `web.php`**

  Tambahkan kode di bawah :

  ```
  Route::get('/categories/{category:slug}', function (Category $category) {
      return view('category', [
          'title' => $category->name,
          'posts' => $category->posts,
          'category' => $category->name
      ]);
  });
  ```

  Kode di atas digunakan untuk menambahkan route category yang mengembalikan view `category`

- **Buka Model Category**

  Tambahkan code berikut ke dalam class Category :

  ```
    public function posts()
    {
        return $this->hasMany(Post::class);
    }
  ```

  Kode di atas digunakan untuk mengambile semua post dengan category yang sama

- **Buat File View `category`**

  Buat view category untuk menampilakan postingan dengan category yang sama. Namai file ini dengan nama `category.blade.php`.

  Pastekan code berikut :

  ```
    @extends('layouts.main')

    @section('container')

        <h1 class="mb-5">Post Category : {{ $category }}</h1>

        @foreach ($posts as $post)
            <article class="mb-4">
                <h2><a href="/posts/{{ $post->slug }}">{{ $post->title }}</a></h2>
                <p>{{ $post->excerpt }}</p>
            </article>
        @endforeach
    @endsection
  ```

- **Masuk File `web.php`**

  Tambhakan kode berikut :

  ```
  Route::get('/categories', function () {
      return view('categories', [
          'title' => "Post Categories",
          'categories' => Category::all()
      ]);
  });
  ```

  Ini route untuk agar bisa menampilkan macam macam kategori

- **Buat File View**

  Buat file view dengan nama `categories.blade.php`, tambahkan code berikut :

  ```
  @extends('layouts.main')

  @section('container')

      <h1 class="mb-5">Post Categories</h1>

      @foreach ($categories as $category)
          <ul>
              <li>
                  <h2><a href="/categories/{{ $category->slug }}">{{ $category->name }}</a></h2>
              </li>
          </ul>
      @endforeach
  @endsection

  ```
