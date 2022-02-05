## Membuat Form Input Image

[Dokumentasi Laravel](https://laravel.com/docs/8.x/filesystem#introduction)

- **Masuk Ke File View `create.blade.php`**

  Masukkan kode dibawah di atas form body :

  ```blade
  <div class="mb-3">
    <label for="image" class="form-label">Post Image</label>
    <input
      class="form-control  @error('image') is-invalid @enderror"
      type="file"
      id="image"
      name="image"
    />
    @error('image')
    <div class="invalid-feedback">{{ $message }}</div>
    @enderror
  </div>
  ```

- **Masuk Ke `DashboardPostController.php`**

  Tambahkan code berikut pada method store :

  ```php
  return $request->file('image')->store('post-images');
  ```

### Mengubah Direktori Penyimpanan Gambar

Untuk insert gambar tentunya kita butuh direktori untuk menyimpan gambar tersebut. Nah untuk costomisai penyimanan gambar kita bisa lakukan langkah - langkah berikut

- **Buka File `.env`**

  Tambahkan :

  ```
  FILESYSTEM_DRIVER=public
  ```

  ini dugunakan untuk mengubah konfigurasi pada file `filesystems.php`. Maka file gambar itu akan tersimpan pada directory `storage\post-images`

- **Menghubungakan File Storage**

  Nah jika kita menyimpan file kita di `storage\post-images` maka gambar itu tidak dapat di panggil oleh laravel. Untuk solusinya kita bisa menggunakan [Symbolic link](https://laravel.com/docs/8.x/filesystem#the-public-disk).

  Lalu ketikkan pada terminal :

  ```artisan
  php artisan storage:link
  ```

  jadi perintah itu akan menjadikan gambar yang tadinya tersimpan di `storage\post-images` jadi tersimpan `public\storage\post-images`

### Edit Struktur Table Database

- **Buka create_posts_table Migrations**

  Tambahkan code berikut pada methd up di dalam create :

  ```php
  $table->string('image')->nullable();
  ```

- **Buka create_users_table Migrations**

  Lalu tambahkan code dibawah pada method up di dalam create :

  ```php
  $table->string('username');
  ```

- **Mambuat Database Seeder Baru**

  Buka file `DatabaseSeeder.php` dan ubah pada method run dengan code berikut :

  ```php
    User::create([
        'name' => "namakalian(babas)",
        'username' => 'username_kalian(babas)',
        'email' => 'email_kalian@gmail.com(babas)',
        'password' => bcrypt('password_kalian(babas)')
    ]);

    User::factory(3)->create();

    Category::create([
        'name' => 'Web Programming',
        'slug' => 'web-programming'
    ]);
    Category::create([
        'name' => 'Web Design',
        'slug' => 'web-design'
    ]);
    Category::create([
        'name' => 'Personal',
        'slug' => 'personal'
    ]);

    Post::factory(20)->create();
  ```

  lalu ketikkan :

  ```artisan
  php artisan migrate:fresh --seed
  ```

### Membuat Validasi Untuk Image

- **Buka Dashboard Post Controller**

  Disini kita akan mengubah bagian validasi untuk image yang akan kita lakukan pada method store.

  - Hapus code lama

    code yang lama yaitu :

    ```php
    return $request->file('image')->store('post-images');
    ```

    code ini terletak pada method store.

- **Menambahkan Validasi**

  Tambahkan validasi :

  ```php
  'image' => 'image|file|max:1024',
  ```

  letakkan di dalam `validate()`

  penjelasan :

  - `image` ini adalah validasi yang wajib pada inputan image
  - `file` agar inputan terdeteksi sebagai file bukan teks dan agar tedeteksi sebagai kilobyte
  - `max:1024` untuk menentukan maksimal ukuran image yang boleh di inputkan

- **Membuat Pengecekan**

  Kita akan membuat pengecekan, apakah user menginputkan gambar atau tidak. Jika tidak kita bisa menggunakan API dari unsplash.com.

  Tambahkan code, di bawah validasi :

  ```php
  if ($request->file('image')) {
    $validateData['image'] = $request->file('image')->store('post-images');
  };
  ```

### Menampilkan Image

- **Edit File `show.blade.php`**

  Ganti code :

  ```php
  <img src="https://source.unsplash.com/1200x400?{{ $post->category->name }}" class="img-fluid mt-2" alt="{{ $post->category->name }}">
  ```

  Menjadi :

  ```blade
    @if ($post->image)
        <div style="max-height: 350px; overflow:hidden;">
            <img src="{{ asset('storage/' . $post->image) }}" class="img-fluid mt-2"
                alt="{{ $post->category->name }}">
        </div>
    @else
        <img src="https://source.unsplash.com/1200x400?{{ $post->category->name }}" class="img-fluid mt-2"
            alt="{{ $post->category->name }}">
    @endif
  ```

  code di atas digunakan untuk pengkondisian jika ada dalam post itu ada image yang di upload maka akan menjalankan :

  ```blade
  <div style="max-height: 350px; overflow:hidden;">
    <img src="{{ asset('storage/' . $post->image) }}" class="img-fluid mt-2"
        alt="{{ $post->category->name }}">
  </div>
  ```

  Jika tidak ada akan menjalankan :

  ```blade
  <img src="https://source.unsplash.com/1200x400?{{ $post->category->name }}" class="img-fluid mt-2" alt="{{ $post->category->name }}">
  ```

- **Edit `posts.blade.php`**

  Ganti code :

  ```blade
  <img src="https://source.unsplash.com/1200x400?{{ $posts[0]->category->name }}" class="card-img-top"alt="{{ $posts[0]->slug }}">
  ```

  Menjadi :

  ```blade
    @if ($posts[0]->image)
        <div style="max-height: 350px; overflow:hidden;">
            <img src="{{ asset('storage/' . $posts[0]->image) }}" class="img-fluid mt-2"
                alt="{{ $posts[0]->category->name }}">
        </div>
    @else
        <img src="https://source.unsplash.com/1200x400?{{ $posts[0]->category->name }}" class="card-img-top"
            alt="{{ $posts[0]->slug }}">
    @endif
  ```

  dan ganti juga code :

  ```blade
  <img src="https://source.unsplash.com/500x400?{{ $post->category->name }}" class="card-img-top" alt="{{ $post->slug }}">
  ```

  menjadi :

  ```
  @if ($post->image)
    <img src="{{ asset('storage/' . $post->image) }}" class="img-fluid"
        alt="{{ $post->category->name }}">
  @else
    <img src="https://source.unsplash.com/500x400?{{ $post->category->name }}"
        class="card-img-top" alt="{{ $post->slug }}">
  @endif
  ```

- **Edit `post.blade.php`**

  Ganti code :

  ```blade
  <img src="https://source.unsplash.com/1200x400?{{ $post->category->name }}" class="img-fluid mt-2" alt="{{ $post->category->name }}">
  ```

  menjadi :

  ```blade
  @if ($post->image)
    <div style="max-height: 350px; overflow:hidden;">
        <img src="{{ asset('storage/' . $post->image) }}" class="img-fluid"
            alt="{{ $post->category->name }}">
    </div>
  @else
    <img src="https://source.unsplash.com/1200x400?{{ $post->category->name }}" class="img-fluid mt-2"
        alt="{{ $post->category->name }}">
  @endif
  ```
