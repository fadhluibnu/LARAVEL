## Membuat validasi untuk insert postingan

- **Masuk Ke DashboardPostController**
- **Tambahakan Code**

  Copy dan letakkan pada function store :

  ```php
  $validateData = $request->validate([
    'title' => 'required|max:255',
    'slug' => 'required|unique:posts',
    'category_id' => 'required',
    'body' => 'required',
  ]);
  $validateData['user_id'] = auth()->user()->id;
  $validateData['excerpt'] = Str::limit(strip_tags($request->body), 40);
  Post::create($validateData);
  return redirect('/dashboard/posts')->with('success', 'New post has been added!');
  ```

  Penjelasan

  - Validate

    ```php
    $validateData = $request->validate([
    'title' => 'required|max:255',
    'slug' => 'required|unique:posts',
    'category_id' => 'required',
    'body' => 'required',
    ]);
    ```

    ini digunakan untuk memberi syarat" pada form input data kita

  - `$validateData['user_id'] = auth()->user()->id;` ini digunakan untuk menangkap siapa id user yang sedang login

  - Excerpt

    `$validateData['excerpt'] = Str::limit(strip_tags($request->body), 40);`

    disini menggunakan sebuah fungsi dari laravel yaitu `Str::limit` [[dokumentasu]](https://laravel.com/docs/8.x/helpers#method-str-limit), `Str::limit` ini digunakan untuk memotong sting dari inputan kita tadi.

    sedangkan `strip_tags()` ini digunakan agar data kembali menjadi teks, seperti kita tau saat kita mengetikkan teks di trik maka akan membawa format format tertentu.
