## Metode Mess Asignment

Ini adalah metode untuk memudahkan saat melakuakn insert database secara manual

### Ayo Mulai!!

- **Buat Kode**

  Buat kode seperti dibawah :

  ```
  Post::create([
      'title'=>'Judul Ke Empat',
      'excerpt'=>'Rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut',
      'body'=>'<p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p><p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Veritatis repudiandae nemo adipisci blanditiis enim rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut!</p><p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p>'
  ])
  ```

- **Atur Mess Assignment**

  Masuk ke model `Post.php` masukkan kode dibawah ke `Class Post` :

  ```
    protected $fillable = [
        'title', 'excerpt', 'body'
    ];
  ```

  Atau

  ```
    protected $guarded = [
        'id'
    ];
  ```

  `$fillable` artinya fild mana aja yang boleh di isi
  `$guarded` artinya file mana aja yang gak boleh di isi, ini lebih simpel penggunaanya dibanding `$fillable`

- **Masuk Ke Tinker**

  Masuk dulu ke terminal tempat menyimpan file laravel, Lalu ketikkan perintah `php artisan tinker`. Setelah itu paste kode di atas

### Macam Macam Mess Assignment

- `Update`
  Ikuti perintah di bawah :

  ```
  Post::find(3)->update(['title'=>'Judul Ke Tiga Berubah'])
  ```

  pakai `update` hanya bisa mencari id

- `where`

  Ikuti perintah di bawah :

  ```
  Post::where('title','Judul Ke Tiga Berubah')->update(['excerpt'=>'Excerpt post 3 berubah'])
  ```

  pakai `where` bisa masukkan apapun
