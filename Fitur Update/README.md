## Fitur Update Pada Post

mari kita mulai

- **Mengganti link**

  Masuk ke file `index.blade.php` pada folder `/dashboard/posts/` lalu ganti code berikut

  ```
  <a href="" class="badge bg-warning"><span data-feather="edit"></span></a>
  ```

  menjadi :

  ```
  <a href="/dashboard/posts/{{ $post->slug }}/edit" class="badge bg-warning"><span data-feather="edit"></span></a>
  ```

  lalu masuk ke file `show.blade.php` ganti code berikut :

  ```
  <a href="" class="btn btn-warning"><span data-feather="edit"></span> Edit</a>
  ```

  menjadi :

  ```
  <a href="/dashboard/posts/{{ $post->slug }}/edit" class="btn btn-warning"><span data-feather="edit"></span>Edit</a>
  ```

  note: jika ingin tau route list kita, kalian bisa ketikkan perintah pada terminal `php artisan route:list`

- **Membuat View Edit**

  Buat view dengan nama `edit.blade.php`, lalu pastekan code berikut

  ```
  @extends('dashboard.layouts.main')

  @section('container')
      <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
          <h1 class="h2">Edit Post</h1>
      </div>
      <div class="col-lg-8">
          <form method="POST" action="/dashboard/posts/{{ $post->slug }}" class="mb-5">
              @method('put')
              @csrf
              <div class="mb-3">
                  <label for="title" class="form-label">Title</label>
                  <input type="text" class="form-control @error('title') is-invalid @enderror" id="title" name="title"
                      autofocus value="{{ @old('title', $post->title) }}" required>
                  @error('title')
                      <div class="invalid-feedback">
                          {{ $message }}
                      </div>
                  @enderror
              </div>
              <div class="mb-3">
                  <label for="slug" class="form-label">Slug</label>
                  <input type="text" class="form-control @error('slug') is-invalid @enderror" id="slug" name="slug"
                      value="{{ @old('slug', $post->slug) }}" required>
                  <div id="relatime-response" class="mt-1">
                  </div>
                  @error('slug')
                      <div class="invalid-feedback">
                          {{ $message }}
                      </div>
                  @enderror
              </div>
              <div class="mb-3">
                  <label for="Category" class="form-label">Category</label>
                  <select class="form-select" name="category_id">
                      @foreach ($categories as $category)
                          @if (old('category', $post->category->id) == $category->id)
                              <option value="{{ $category->id }}" selected>{{ $category->name }}</option>
                          @endif
                          <option value="{{ $category->id }}">{{ $category->name }}</option>
                      @endforeach
                  </select>
              </div>
              <div class="mb-3">
                  <label for="Body" class="form-label">Body</label>
                  @error('body')
                      <p class="text-danger">{{ $message }}</p>
                  @enderror
                  <input id="body" type="hidden" name="body" value="{{ @old('body', $post->body) }}">
                  <trix-editor input="body"></trix-editor>
              </div>
              <button type="submit" id="submit" class="btn btn-primary">Update post</button>
          </form>
      </div>
  @endsection
  ```

  Penjelasan :

  - `@method('put')` jadi untuk melakuakan edit kita bisa menggunakan sebuah method `put` atau `patch` selengkapnya [klik disini](https://laravel.com/docs/8.x/controllers#actions-handled-by-resource-controller)

  - `@old('title', $post->title)` sebelumnya kita mungkin sudah menggunakan ini tapi dengan bentuk yang berbeda. Jadi sebelumnya kita menggunakan nya yaitu `@old('title')` sekarang kita menggunakan `@old('title', $post->title)` dan tentunya memiliki fungsi yang berbeda.

    cara kerjanya `@old('title', $post->title)` jika tidak ada `@old('title')` dari form nya maka akan menampilkan `@old($post->title)` dan sebaliknya. `@old($post->title)` disini digunakan untuk menampilkan judul kita dari database

- **Membuat Controllernya**

  Masuk ke `DashboardPostController.php` lalu cari method `edit` dan masukkan code berikut :

  ```
  return view('dashboard.posts.edit', [
      'post' => $post,
      'categories' => Category::all()
  ]);
  ```

- **Membuat Validasi**

  Masih di `DashboardPostController.php` dan kita sekrang fokus di method `update`. Tambhakan code dibawah :

  ```
    $rules = [
        'title' => 'required|max:255',
        'category_id' => 'required',
        'body' => 'required',
    ];

    if ($request->slug != $post->slug) {
        $rules['slug'] = 'required|unique:posts';
    }

    $validateData = $request->validate($rules);
    $validateData['user_id'] = auth()->user()->id;
    $validateData['excerpt'] = Str::limit(strip_tags($request->body), 40);

    Post::where('id', $post->id)->update($validateData);

    return redirect('/dashboard/posts')->with('success', 'Post has been update!');
  ```

  Penjelasan :

  Untuk selengkapnya bisa tonton di channel Web Programming UNPAS milik Pak Sandhika Galih

  - Kenapa validasi `slug` di buat pengkondisian

    Pertama, kalo kita tau slug ini harus unik/tidak boleh ada yang sama. Sedangkan saat kita mau mengupdate postingan tersebut mungkin slug itu tidak akan kita ubah, nah masalahnya di sini. Saat slug tidak diubah/sama dengan slug yang lama, laravel akan membacanya bentrok sehingga akan error saat melakukan update karena slug saat kita create dan update di bacanya sama oleh laravel.

    jadi solusinya adalah membuat sebuah pengkondisian :

    ```
    if ($request->slug != $post->slug) {
        $rules['slug'] = 'required|unique:posts';
    }
    ```

    Dibacanya yaitu : Jika slug lama tidak sama dengan slug baru maka tambahkan validasi `$rules['slug'] = 'required|unique:posts';` pada variable `$rules`

  - `Post::where('id', $post->id)->update($validateData);` ini digunakan untuk mengupdate postingan kita
