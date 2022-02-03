## Membuat Tampilan Form Post

- **Masuk Ke File View `/dashboard/posts/index`**

  Tambahkan code berikut di atas table kita :

  ```
  <a href="/dashboard/posts/create" class="btn btn-primary mb-3 mt-1">Create new post</a>
  ```

- **Menambahkan Controller**

  Masuk ke `DashboardPostController` lalu copy kode berikut ke dalam method `create` :

  ```
  return view('dashboard.posts.create', [
    'categories' => Category::all()
  ]);
  ```

- **Membuat View Create**

  Buat sebuah view dengan nama `create.blade.php` di dalam folder `/dashboard/posts`. Setelah itu masukkan code berikut :

  ```
  @extends('dashboard.layouts.main')
  @section('container')
        <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
            <h1 class="h2">Create New Post</h1>
        </div>
        <div class="col-lg-8">
        <form method="POST" action="/dashboard/posts" class="mb-5">
            @csrf
            <div class="mb-3">
                <label for="title" class="form-label">Title</label>
                <input type="text" class="form-control @error('title') is-invalid @enderror" id="title" name="title"
                    autofocus value="{{ @old('title') }}" required>
                @error('title')
                    <div class="invalid-feedback">
                        {{ $message }}
                    </div>
                @enderror
            </div>
            <div class="mb-3">
                <label for="slug" class="form-label">Slug</label>
                <input type="text" class="form-control @error('slug') is-invalid @enderror" id="slug" name="slug"
                    {{ @old('slug') }} required>
                @error('slug')
                    <div class="invalid-feedback">
                        {{ $message }}
                    </div>
                @enderror
            </div>
            <div class="mb-3">
                <label for="Category" class="form-label">Category</label>
                @error('body')
                    <p class="text-denger">{{ $message }}</p>
                @enderror
                <select class="form-select" name="category_id">
                    @foreach ($categories as $category)
                        @if (old('category') == $category->id)
                            <option value="{{ $category->id }}" selected>{{ $category->name }}</option>
                        @endif
                        <option value="{{ $category->id }}">{{ $category->name }}</option>
                    @endforeach
                </select>
            </div>
            <div class="mb-3">
                <label for="Body" class="form-label">Body</label>
                <input id="body" type="hidden" name="body" value="{{ @old('body') }}">
                <trix-editor input="body"></trix-editor>
            </div>
            <button type="submit" id="submit" class="btn btn-primary">Create post</button>
        </form>
    </div>

    <script>
        const title = document.querySelector('#title');
        const slug = document.querySelector('#slug');

        title.addEventListener('input', function() {
            var titleLower = title.value.toLowerCase()
            slug.value = titleLower.split(' ').join('-')
        })

        document.addEventListener('trix-file-accept', function() {
            e.preventDefault();
        })
    </script>
  @endsection
  ```

  Penjelasan :

  - `@error('slug') is-invalid @enderror` ini akan menampilkan class is invalid dan fungsi ini berjalan ketikka kita insert data namun terdadi kesalahan.
  - `{{ @old('title') }}` disini digunakan untuk menangkap sebuah value, sehinnga saat kita medapatkan sebuah error maka value sebelumnya akan terekan oleh `@old()`
