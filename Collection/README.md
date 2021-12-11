## Collection

[Collection Laravel](https://laravel.com/docs/8.x/collections#introduction) adalah Pembungkus sebuah array agar arrya lebih hebat lagi sehingga array kita bisa menggunakan banyak method dari laravel

misal :

- `return $coba->first()` maka method `first()` akan mengambil data pertama
- `return $coba->firstWhere('slug', $slug)` maka method `firstWhere()` akan mengambil data pertama yang `slug` nya mirip dengan `$slug`

Untuk method lainya ada [disini](https://laravel.com/docs/8.x/collections#available-methods)

### Mari Coba

- **Ganti Function all(), dan find()**

  Buka file `moduls/Post.php` Lalu ganti dengan kode berikut :

  - all()

    ```
    public static function all()
    {
        return collect(self::$blog_posts);
    }
    ```

  - find()

    ```
    public static function find($slug)
    {
        $posts = static::all();

        return $posts->firstWhere('slug', $slug);
    }
    ```
