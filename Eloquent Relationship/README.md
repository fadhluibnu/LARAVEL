## Eloquent Relationship

[Dokumentasi Lengkap](https://laravel.com/docs/8.x/eloquent-relationships#introduction)

### Mari Mulai

- **Buat Model Category**

  Buka terminal, ketikkan `php artisan make:model -m Category`

- **Buka Category Migration**

  Edit funtion `up()` menjadi :

  ```
  public function up()
  {
      Schema::create('categories', function (Blueprint $table) {
          $table->id();
          $table->string('name')->unique();
          $table->string('slug')->unique();
          $table->timestamps();
      });
  }
  ```

- **Buka Post Migration**

  Tambahkan code berikut pada bagian Schema di function `up()` :

  ```
  $table->foreignId('category_id');
  ```

- **Simpan Table**

  Buka terminal ketikkan `php artisan migrate:fresh`

- **Buka File Category Model**

  Masukkan code berikut ke dalam classnya :

  ```
  protected $guarded = ['id'];
  ```

- **Isi Data Category**

  Buka terminal ketikkan `php artisan tinker`, lalu inisialisasi Class Category, Masukkan category berikut

  - Programming

    Category::create([
    'name'=>"Programming",
    'slug'=>"programming"
    ])

  - Web Design

    Category::create([
    'name'=>"Web Design",
    'slug'=>"web-design"
    ])

  - Personal

    Category::create([
    'name'=>"Personal",
    'slug'=>"personal"
    ])

- **Isi Data Post**

  Isikan perintah tersebut ke terminal yang masih terhubung dengan tinker :

  - Judul Pertama
    Post::create([
    'title'=>'Judul Pertama',
    'slug'=>'judul-pertama',
    'category_id'=>1,
    'excerpt'=>'Rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut',
    'body'=>'<p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p><p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Veritatis repudiandae nemo adipisci blanditiis enim rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut!</p><p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p>'
    ])

  - Judul Kedua
    Post::create([
    'title'=>'Judul Ke Dua',
    'slug'=>'judul-ke-dua',
    'category_id'=>1,
    'excerpt'=>'Rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut',
    'body'=>'<p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p><p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Veritatis repudiandae nemo adipisci blanditiis enim rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut!</p><p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p>'
    ])

  - Judul Ke Tiga
    Post::create([
    'title'=>'Judul Ke Tiga',
    'slug'=>'judul-ke-tiga',
    'category_id'=>3,
    'excerpt'=>'Rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut',
    'body'=>'<p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p><p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Veritatis repudiandae nemo adipisci blanditiis enim rerum architecto quisquam, molestias esse unde, nostrum ut ullam sunt quae eos numquam delectus ratione aut!</p><p>ipsum dolor, sit amet consectetur adipisicing elit. Explicabo doloribus praesentium sed, sapiente nemo labore beatae fuga eum modi voluptates, reiciendis eaque perspiciatis fugiat? Magnam ab ad laboriosam qui officia omnis possimus rerum exercitationem, doloremque dicta impedit dolorem culpa, non reiciendis, sint incidunt. Fugiat dolorum repellat omnis adipisci optio dolore dicta harum magnam temporibus. Culpa amet doloremque nam hic consectetur, natus doloribus nisi voluptatibus ad at. Reiciendis amet doloremque ex. Cumque pariatur architecto esse doloribus, suscipit minima labore maiores impedit voluptates laudantium asperiores libero aliquam excepturi in error iusto vel quod ipsa nemo rerum. Sunt et facilis impedit officiis laborum! </p>'
    ])

### Pembahasan Cara Melakukan Realtionship

- Cardinalitas
  Misal satu caetgory memiliki banya post maka kardinalitasnya adalah `1` ke `n` atau One-To-Many

- Cardinalitas (inverse)

  Misal `1` post dimili oleh `1` category atau One-To-One

Penguhungan Eloquent adalah :

- hasMany

  Satu category punya banyak post

- belongsTo

  Satu post hanya punya satu category
