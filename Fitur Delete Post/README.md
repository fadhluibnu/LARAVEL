## Membuat Fitur Delete Postingan

- **Membuat Form**

  Jadi untuk membuat sebuah fitur delete di laravel kita membutuhkan sebuah request method `delete` serta `@csrf` dan itu harus di bungkus oleh form. Untuk penjelasanya bisa [klik disini](https://laravel.com/docs/8.x/controllers#actions-handled-by-resource-controller)

  - Pertama

    Buka file `index.blade.php` yang berada pada folder `/dashboard/posts/`

    Ganti code dibawah :

    ```html
    <a href="" class="badge bg-danger"><span data-feather="x-circle"></span></a>
    ```

    menjadi :

    ```blade
    <form action="/dashboard/posts/{{ $post->slug }}" method="POST" class="d-inline">
        @method('delete')
        @csrf
        <button class="badge bg-danger border-0" onclick="return confirm('Yakin ?')"><span
                data-feather="x-circle"></span></button>
    </form>
    ```

- **Mengedit Dashboard Post Controller**

  Buka `DashboardPostController.php` lalu tambahkan code berikut pada method `destroy` :

  ```php
  Post::destroy($post->id);
  return redirect('/dashboard/posts')->with('success', 'New post has been deleted!');
  ```

- **Edit Button**

  Buka file `show.blade.php` lalu ganti code berikut :

  ```html
  <a href="" class="btn btn-danger"><span data-feather="x-circle"></span> Delete</a>
  ```

  menjadi :

  ```blade
  <form action="/dashboard/posts/{{ $post->slug }}" method="POST" class="d-inline">
    @method('delete')
    @csrf
    <button class="btn btn-danger" onclick="return confirm('Yakin ?')"><span data-feather="x-circle"></span>
        Delete</button>
  </form>
  ```

  ~Fitur delete Selesi~
