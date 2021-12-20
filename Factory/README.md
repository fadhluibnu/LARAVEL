## Menggunakan Factory & Faker

Factory digunakan untuk menambhakan data baru yang biasanya digunakan untuk testing

File ini terdapat pada folder `database/factories`. Saat menggunakan factory ini kita akan menggunakan library yang bernama faker yang digunakan untuk mengenerate data random

## Mari Mulai

- **Edit File `UserFactory.php`**

  Edit function `definition()` menjadi :

  ```
  public function definition()
  {
      return [
          'name' => $this->faker->name(),
          'username' => $this->faker->userName(),
          'email' => $this->faker->unique()->safeEmail(),
          'email_verified_at' => now(),
          'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
          'remember_token' => Str::random(10),
      ];
  }
  ```

- **Buat File `PostFactory.php`**

  Buat file tersebut dengan mengetikkan `php artisan make:factory PostFactory`. Lalu buka file tersebut dan edit function `definition()` :

  ```
  public function definition()
  {
      return [
          'title' => $this->faker->sentence(mt_rand(2, 8)),
          'slug' => $this->faker->slug(),
          'excerpt' => $this->faker->paragraph(),
          'body' => $this->faker->paragraph(mt_rand(5, 10)),
          'user_id' => mt_rand(1, 3),
          'category_id' => mt_rand(1, 2)
      ];
  }
  ```

- **Buka File `app.php`**

  Cari key dengan nama `faker_locale`. lalu edit menjadi :

  ```
  'faker_locale' => env('FAKER_LOCALE', 'en_US'),
  ```

- **Buka File `.env`**

  Tambhakan code di bawah :

  ```
  FAKER_LOCALE=id_ID
  ```

- **Buka File `DatabaseSeeder.php`**

  Edit function `up()` menjadi :

  ```
    public function run()
    {
        User::factory(3)->create();

        Category::create([
            'name' => 'Web Programming',
            'slug' => 'web-programming'
        ]);
        Category::create([
            'name' => 'Personal',
            'slug' => 'personak'
        ]);

        Post::factory(20)->create();
    }
  ```

- **Buka Terminal**

  Ketikkan perintah `php artisan migrate:fresh --seed`

- **Buka File `web.php`**

  Tambhakan route seperti di bawah :

  ```
  Route::get('/authors/{author:username}', function (User $author) {
      return view('posts', [
          'title' => 'User Post',
          'posts' => $author->posts
      ]);
  });
  ```

- **Buka File `PostController.php`**

  Edit function `index()` yang mulanya :

  ```
  'posts' => Post::all()
  ```

  Menjadi :

  ```
  'posts' => Post::latest()->get()
  ```

  `Post::latest()->get()` digunakan untuk mengurutkan postingan yang tampil dari yang terbaru
