## Migration

Untuk membuat table kita akan menggunakan [Migration](https://laravel.com/docs/8.x/migrations#introduction)

Migrations adalah Version Control untuk melacak perubahan database lewat code laravel, yang memungkinkan kita dan tim untuk membagikan skema database kita

Untuk menggunakan ini kita menggunakan terminal

```
php artisan make:migration nama_table
```

Untuk mengeksekusi migration kita menggunakan

```
php artisan migrate
```

perintah di atas akan mengesekusi semua perubahan pada file yang ada di folder `database/migrations`

Untuk sekarang coba kita buka File `2014_10_12_000000_create_users_table.php` di folder tersebut.

Di dalam file tersebut terdapat 2 function yaitu `up()` (Digunakan saat kita ingin membuat suatu Schema database dan di eksekusi ketika kita ketikkan perintah `migrate`) dan `down()` (Untuk menghilangka Schema yang sudah kita buat dan di eksekusi ketika kita ketikkan perintah `migrate:rollback`, Sehingga semua table hilang)

`php artisan migrate:fresh` digunakan untuk mengajalankan `migrate` dan `migrate:rollback` secara bersamaan

note: `php artisan migrate` tidak akan berjalan ketika `APP_ENV=production` karena laravel membaca aplikasi kita dalam production

## Mecoba Menambahkan Kolom Pada Table

- **Edit File `2014_10_12_000000_create_users_table.php`**

  Edit functon up(), ganti dengan code dibawah :

  ```
  public function up()
  {
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('email')->unique();
        $table->timestamp('email_verified_at')->nullable();
        $table->string('password');
        $table->rememberToken();
        $table->timestamps();
    });
  }
  ```

  Untuk macam - macam column method ada [disini](https://laravel.com/docs/8.x/migrations#creating-columns)

- **Jalankan Perintah**

  Jalankan perintah tersebut pada terminal

  ```
  php artisan migrate:fresh
  ```
