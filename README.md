# LARAVEL

Belajar Laravel 8
<a href="https://laravel.com/docs/8.x" target="_blank">Laravel Documentation</a>

<hr>
<h2>⁋ Struktur Folder</h2>
<img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/Struktur%20Folder.JPG"/>
<p>Untuk penjelasan lebih lengkap Struktur Directory Laravel <a href="https://laravel.com/docs/8.x/structure" target="_blank">ada disini </a></p>
<ol>
  <li>
    <h3>Folder App</h3>
    <p>Menyimpan kode utama dalam aplikasi kita<p>
    <img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/folder%20App.JPG"/>
  </li>
  
   <li>
    <h3>Folder Controller</h3>
    <p>Folder ini berada di dalam folder App terus di Folder Http. Folder ini berguna untuk menghubungka model dan view pada aplikasi kita<p>
    <p><a href="https://www.niagahoster.co.id/blog/mvc-adalah/#Apa_itu_MVC" target="_blank">Penjelasan Controller Dalam Konsep MVC</a><p>
    <img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/Folder%20Controller.JPG"/>
  </li>
  
  <li>
    <h3>Folder Model</h3>
    <p>Folder ini berada di dalam folder App. Folder ini berguna untuk mengelola model seperti folder registrasi atau yang lain<p>
    <p><a href="https://www.niagahoster.co.id/blog/mvc-adalah/#Apa_itu_MVC" target="_blank">Penjelasan Model Dalam Konsep MVC</a><p>
    <img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/folder%20Model.JPG"/>
  </li>
  
  <li>
    <h3>Folder View</h3>
    <p>Folder ini berada di dalam folder resource. Folder ini berguna untuk mengelola view pada aplikasi kita<p>
    <p><a href="https://www.niagahoster.co.id/blog/mvc-adalah/#Apa_itu_MVC" target="_blank">Penjelasan view Dalam Konsep MVC</a><p>
    <img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/folder%20View.JPG"/>
  </li>
  
  <li>
    <h3>Folder Route</h3>
    <p>Foder ini menyimpan file routing atau penaluran, seperti url yang kita masukkan agar bisa masuk ke halaman<p>
    <img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/folder%20route.JPG"/>
  </li>
  
  <li>
    <h3>Folder Public</h3>
    <p>Foder ini adalah tempat asset statis kita seperti file CSS, JavaScript dll<p>
    <img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/folder%20public.JPG"/>
  </li>
  
  <li>
    <h3>File .env</h3>
    <p>File ini berguna untuk menentuka environment dari aplikasi kita, misal kita akan terkoneksi ke database MYSQL<p>
    <img src="https://github.com/fadhluibnu/LARAVEL/blob/main/Asset%20GitHub/file.env.JPG"/>
  </li>  
  
  <p>Penjelasan lebih lengkap Struktur Directory Laravel <a href="https://laravel.com/docs/8.x/structure" target="_blank">ada disini</a></p>  
</ol>
<hr>
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
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  















