<h2>⁋ Routing</h2>
<p>Untuk melakukan routing pada web, kita bisa masuk ke folder <code>routes/web.php</code>.</p>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/web.php.JPG"/>
<p>Di disini kita menemukan sebuah code bertuliskan</p>
<code>
Route::get('/', function () {
    return view('welcome');
})
</code>
<p>Itu dibaca Jika ada route dengan metode request get dengan alamat url adalah <code>/</code> maka akan menjalankan view welcome</p>
<p>Jika kita jalankan dengan memasukkan url <code>/</code> akan muncul</p>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/reoute%20slash.JPG"/>
<p>Tapi jika kita ubah Routenya get menjadi <code>/about</code></p>
<code>
Route::get('/about', function () {
    return view('welcome');
})
</code>
<p></p>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/route%20slash%20about.JPG"/>
<p>tetapi kita memasukkan url <code>/</code> pada browser maka yang terjadi adalah <code>404 | Not Found</code> seperti gambar di bawah</code></p>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/slash%20not%20found.JPG"/>

itu terjadi karena tidak ada route get yang menangani `/`. Tapi jika kita Menuliskan Route Url nya `/About` maka halaman Tidak akan memunculkan `404 | Not Found`.

Oke Sekarang kita kembalikan lagi routenya mendadi `/`.

```
Route::get('/', function () {
    return view('welcome');
})
```

Sekarang dibagian return. Sebenarnya di bagian return kita bisa menuliskan apapun seperti :

```
Route::get('/', function () {
    return "Hello Laravel";
});
```

Maka jika kita buka di browser dengan mengetikkan url `/` akan muncul Hello Laravel, seperti gambar berikut

![Gambar](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/Hello%20Laravel.JPG)

Oke sekarang kita kembalikan lagi menjadi

```
Route::get('/', function () {
    return view('welcome');
})
```

Nah cara baca returnya gimana sih? Caranya yaitu saat kita mengetikkan url `/` maka function akan dijalankan dan mengembalikan `view('welcome')`.

Tapi kok bisa tampil do browser? nah kalau kalian sudah paham konsep MVC `welcome` yang ada di dalam kurung itu sebenernya adalah sebuah file yang tersimpan di folder `resources/view` dengan nama `welcome.blade.php`

![welcome.blade.php](https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/welcome.blade.php.JPG)

itulah yang membuat aplikasi kita tampil di browser. Yang terjadi sebenarnya adalah Laravelnya mencari sebuah file yang bernama `welcome`
