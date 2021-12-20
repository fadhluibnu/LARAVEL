## Database Seeder

Di seri ini kita akan coba menggunakan database seeder

[[dokumentasi]](https://laravel.com/docs/8.x/seeding#introduction)

Seeder adalah fitur miliki laravel untnk mengenerate / mempopulasi dengan folder testing atau dumy

Cara bikinya adalah ketikkan

```
php artisan make:seeder UserSeeder
```

### Ayo Coba

- **Buka File Post Migration**

  Tambhakan code berikut ke function `up()` :

  ```
    Schema::create('posts', function (Blueprint $table) {
        $table->id();
        $table->foreignId('category_id');
        $table->foreignId('user_id');
        $table->string('title');
        $table->string('slug')->unique();
        $table->text('excerpt');
        $table->text('body');
        $table->timestamp('published_at')->nullable();
        $table->timestamps();
    });
  ```

- **Buka File `DatabaseSeeder.php`**

  File ini ada folder `database/seeder`. Masukkan code berikut :

  ```
  public function run()
  {
      // \App\Models\User::factory(10)->create();

      User::create([
          'name' => 'Fadhlu Ibnu',
          'email' => 'fadhluibnu@gmail.com',
          'password' => bcrypt('12345')
      ]);
      User::create([
          'name' => "Fadhlu Ibnu 'Abbbd",
          'email' => 'fadhluibnuabbad@gmail.com',
          'password' => bcrypt('12345')
      ]);

      Category::create([
          'name' => 'Web Programming',
          'slug' => 'web-programming'
      ]);
      Category::create([
          'name' => 'Personal',
          'slug' => 'personak'
      ]);
      Post::create([
          'title' => 'Judul Pertama',
          'slug' => 'judul-pertama',
          'excerpt' => 'Lorem ipsum, dolor sit amet consectetur adipisicing elit',
          'body' => 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sit natus autem saepe esse doloribus dolorem recusandae vitae eaque optio modi fugit id eum temporibus, itaque aut amet consectetur ut! Aperiam distinctio dolore dolorum perspiciatis labore deleniti odio obcaecati culpa sequi, quod quia pariatur aspernatur ad eius tempore adipisci voluptates repellendus quibusdam reiciendis unde blanditiis, ipsam commodi. Voluptatum optio in molestiae maiores accusamus excepturi fuga sit aliquid? Ducimus impedit ullam voluptate modi corporis aspernatur necessitatibus! Ipsa molestiae reiciendis aliquam optio totam. Quibusdam aliquam sunt molestias quas magni iusto, architecto consectetur. Tempora totam facilis veniam commodi nihil ad corporis quas nesciunt non?',
          'category_id' => 1,
          'user_id' => 1
      ]);
      Post::create([
          'title' => 'Judul Ke Dua',
          'slug' => 'judul-ke-dua',
          'excerpt' => 'Lorem ipsum, dolor sit amet consectetur adipisicing elit',
          'body' => 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sit natus autem saepe esse doloribus dolorem recusandae vitae eaque optio modi fugit id eum temporibus, itaque aut amet consectetur ut! Aperiam distinctio dolore dolorum perspiciatis labore deleniti odio obcaecati culpa sequi, quod quia pariatur aspernatur ad eius tempore adipisci voluptates repellendus quibusdam reiciendis unde blanditiis, ipsam commodi. Voluptatum optio in molestiae maiores accusamus excepturi fuga sit aliquid? Ducimus impedit ullam voluptate modi corporis aspernatur necessitatibus! Ipsa molestiae reiciendis aliquam optio totam. Quibusdam aliquam sunt molestias quas magni iusto, architecto consectetur. Tempora totam facilis veniam commodi nihil ad corporis quas nesciunt non?',
          'category_id' => 1,
          'user_id' => 1
      ]);
      Post::create([
          'title' => 'Judul Ke Tiga',
          'slug' => 'judul-ke-tiga',
          'excerpt' => 'Lorem ipsum, dolor sit amet consectetur adipisicing elit',
          'body' => 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sit natus autem saepe esse doloribus dolorem recusandae vitae eaque optio modi fugit id eum temporibus, itaque aut amet consectetur ut! Aperiam distinctio dolore dolorum perspiciatis labore deleniti odio obcaecati culpa sequi, quod quia pariatur aspernatur ad eius tempore adipisci voluptates repellendus quibusdam reiciendis unde blanditiis, ipsam commodi. Voluptatum optio in molestiae maiores accusamus excepturi fuga sit aliquid? Ducimus impedit ullam voluptate modi corporis aspernatur necessitatibus! Ipsa molestiae reiciendis aliquam optio totam. Quibusdam aliquam sunt molestias quas magni iusto, architecto consectetur. Tempora totam facilis veniam commodi nihil ad corporis quas nesciunt non?',
          'category_id' => 2,
          'user_id' => 1
      ]);
      Post::create([
          'title' => 'Judul Ke Empat',
          'slug' => 'judul-ke-empat',
          'excerpt' => 'Lorem ipsum, dolor sit amet consectetur adipisicing elit',
          'body' => 'Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sit natus autem saepe esse doloribus dolorem recusandae vitae eaque optio modi fugit id eum temporibus, itaque aut amet consectetur ut! Aperiam distinctio dolore dolorum perspiciatis labore deleniti odio obcaecati culpa sequi, quod quia pariatur aspernatur ad eius tempore adipisci voluptates repellendus quibusdam reiciendis unde blanditiis, ipsam commodi. Voluptatum optio in molestiae maiores accusamus excepturi fuga sit aliquid? Ducimus impedit ullam voluptate modi corporis aspernatur necessitatibus! Ipsa molestiae reiciendis aliquam optio totam. Quibusdam aliquam sunt molestias quas magni iusto, architecto consectetur. Tempora totam facilis veniam commodi nihil ad corporis quas nesciunt non?',
          'category_id' => 2,
          'user_id' => 2
      ]);
  }
  ```

- **Buka Terminal**

  Ketikkan perintah dibawah :

  ```
  php artisan migrate:fresh --seed
  ```

- **Buka File Model Post**

  Buka file `post.php`, Masukkan code berikut :

  ```
  public function user()
  {
      return $this->belongsTo(User::class);
  }
  ```

- **Buka File Model User**

  Buka file `user.php`, Masukkan code berikut :

  ```
    public function posts()
    {
        return $this->hasMany(Post::class);
    }
  ```

- **Buka File View**

  - Buka file view `posts.blade.php`. Tambhakan code berikut :

    ```
        <article class="mb-4 border-bottom">
            <h2><a href="/posts/{{ $post->slug }}" class="text-decoration-none">{{ $post->title }}</a></h2>
            <p>By <a href="" class="text-decoration-none">{{ $post->user->name }}</a> in <a
                    href="/categories/{{ $post->category->slug }}"
                    class="text-decoration-none">{{ $post->category->name }}</a></p>
            <p>{{ $post->excerpt }}</p>

            <a href="/posts/{{ $post->slug }}" class="text-decoration-none">Read more ...</a>
        </article>
    ```

  - Buka file view `post.blade.php`. Tambahkan code :

    ```
    <article>
        <h2>{{ $post->title }}</h2>

        <p>By <a href="" class="text-decoration-none">{{ $post->user->name }}</a> in <a
                href="/categories/{{ $post->category->slug }}"
                class="text-decoration-none">{{ $post->category->name }}</a></p>

        {!! $post->body !!}
    </article>
    <a href="/posts">Back To Posts</a>
    ```
