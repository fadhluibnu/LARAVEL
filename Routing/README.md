<h2>⁋ Routing</h2>
<p>Untuk melakukan routung pada web kita bisa masuk ke folder <code>routes/web.php</code>.</p>
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
