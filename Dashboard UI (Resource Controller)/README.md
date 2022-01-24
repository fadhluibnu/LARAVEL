## Resourece Controllers

[dokumentasi](https://laravel.com/docs/8.x/controllers#resource-controllers)

Resoruce Controller adalah Sebuah controller yang otomatis mengelola data CRUD. Sehingga kita tidak usah menambahkan route satu persatu.

### Mari Kita Coba

- **Buka Terminal**

  Ketikkan perintah :

  ```
  php artisan make:controller DashboardPostController --model=Post --resource
  ```

  Perintah di atas akan membuat sebuah resource controller dan sekaligus mereference ke model post

- **Buka DashboardPostController.php**

  Penjelasan :

    <table>
    <tr>
    <th>Method</th>
    <th>Penjelasan</th>
    </tr>
    <tr>
    <td>index</td>
    <td>Untuk menampilkan data posts berdasarkan user tertentu</td>
    </tr>
    <tr>
    <td>create</td>
    <td>Untuk menampikan tambah postingan</td>
    </tr>
    <tr>
    <td>store</td>
    <td>Untuk Menjalankan fungsi tambah data</td>
    </tr>
    <tr>
    <td>show</td>
    <td>Untuk melihat detail dari postingan</td>
    </tr>
    <tr>
    <td>Edit</td>
    <td>Untuk halaman ubah data</td>
    </tr>
    <tr>
    <td>Update</td>
    <td>Untuk halaman proses ubah data</td>
    </tr>
    <tr>
    <td>Delete</td>
    <td>Untuk mengkapus data</td>
    </tr>
    </table>

- **Membuat Route**

  Buka web.php lalu tambahkan :

  ```
  Route::resource('/dashboard/posts', DashboardPostController::class)->middleware('auth');
  ```

  Jika ada route `/dashboard/posts` maka method index akan otomatis di jalankan

- **Edit Method Index**

  Masuk ke Dashbord Post Controller lalu tambahkan code berikut :

  ```
  return view('dashboard.posts.index', [
          'posts' => Post::where('user_id', auth()->user()->id)->get()
      ]);
  ```

  `Post::where('user_id', auth()->user()->id)->get()` akan mengambil data posts dengan id tertentu

- **Membuat View Untuk Posts**

  - **Buat Folder posts**

    Buat folder ini di dalam folderd dashbord

  - **Buat View**

    Buat file index.blade.php di dalam folder `dashboard/posts`

    Masukkan Code berikut :

    ```
    @extends('dashboard.layouts.main')
    @section('container')
        <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
            <h1 class="h2">My Posts</h1>
        </div>
        <div class="table-responsive col-lg-10">
            <a href="/dashboard/posts/create" class="btn btn-primary mb-3 mt-1">Create new post</a>
            <table class="table table-striped table-sm">
                <thead>
                    <tr>
                        <th scope="col">#</th>
                        <th scope="col">Title</th>
                        <th scope="col">Category</th>
                        <th scope="col">Action</th>
                    </tr>
                </thead>
                <tbody>
                    @foreach ($posts as $post)
                        <tr>
                            <td>{{ $loop->iteration }}</td>
                            <td>{{ $post->title }}</td>
                            <td>{{ $post->category->name }}</td>
                            <td>
                                <a href="/dashboard/posts/{{ $post->slug }}" class="badge bg-info"><span
                                        data-feather="eye"></span></a>
                                <a href="" class="badge bg-warning"><span data-feather="edit"></span></a>
                                <a href="" class="badge bg-danger"><span data-feather="x-circle"></span></a>
                            </td>
                        </tr>
                    @endforeach
                </tbody>
            </table>
        </div>
    @endsection
    ```

    [`$loop->iteration`](https://laravel.com/docs/8.x/blade#the-loop-variable) digunakan untuk membuat iteration, sehingga kita tidak membuat secara manual lagi

#### Membuat Halaman Detail Postingan

- **Membuat Route**

  [Dokumentasi](https://laravel.com/docs/8.x/routing#customizing-the-default-key-name)

  Masuk ke Model Post, lalu tambahkan code :

  ```
  public function getRouteKeyName()
  {
      return 'slug';
  }
  ```

- **Edit Method Show**

  Buka dashboard post controller lalu pastekan code berikut pada method show :

  ```
  return view('dashboard.posts.show', [
      'post' => $post
  ]);
  ```

- **Membuat View Detail**

  Buat file ini di dalam folder `/dashborad/posts` dengan nama `show.blade.php`. Lalu tambahkan code berikut :

  ```
  @extends('dashboard.layouts.main')
  @section('container')
      <div class="container">
          <div class="row my-3">
              <div class="col-lg-8">
                  <h2 class="mb-3">{{ $post->title }}</h2>
                  <a href="/dashboard/posts" class="btn btn-success"><span data-feather="arrow-left"></span> Back to all my
                      posts</a>
                  <a href="" class="btn btn-warning"><span data-feather="edit"></span> Edit</a>
                  <a href="" class="btn btn-danger"><span data-feather="x-circle"></span> Delete</a>
                  <img src="https://source.unsplash.com/1200x400?{{ $post->category->name }}" class="img-fluid mt-2"
                      alt="{{ $post->category->name }}">
                  <article class="my-3 fs-6">
                      {!! $post->body !!}
                  </article>
              </div>
          </div>
      </div>
  @endsection
  ```
