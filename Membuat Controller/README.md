## Membuat Controller

Daripada kita membuat sebuah closure atau anonimus Function kita bisa mengganti dengan sebuah class controller. Controller bisa menggabungkan request yanng serupa

[Dokumentasi Controller](https://laravel.com/docs/8.x/controllers#introduction)

Langkah - Langkah :

- **Buat Controller Post**

  Jika kita sudah menginstall extension Laravel [Laravel Artisan](https://marketplace.visualstudio.com/items?itemName=ryannaddy.laravel-artisan). Kita klik shortcut `CTRL + SHIFT + P`. Lalu ketikkan `Artisan: Make Controller`. Buat dengan nama `PostController`, lalu enter. Setelah itu pilih basic.

- **Edit File `PostController.php`**

  Copy code dibawah :

  ```
  <?php

  namespace App\Http\Controllers;

  use App\Models\Post;
  use Illuminate\Http\Request;

  class PostController extends Controller
  {
      public function index()
      {
          return view('posts', [
              'title' => 'Posts',
              'posts' => Post::all()
          ]);
      }

      public function show($slug)
      {
          return view('post', [
              'title' => 'Single Post',
              'post' => Post::find($slug)
          ]);
      }
  }

  ```

- **Edit Route File `web.php`**

  - Route `/posts`

    ```
    Route::get('/posts', [PostController::class, 'index']);
    ```

  - Route `/posts/{slug}`

    ```
    Route::get('/posts/{slug}', [PostController::class, 'show']);
    ```
