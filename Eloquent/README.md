## Eloquent

Ini akan membuat kita mudah dalam mengisikan database.

[Eloquent](https://laravel.com/docs/8.x/eloquent#introduction) adalah obejek-relational-mapper Sehingga kita mudah dalam mengakses table database kita. Kita bisa melakukan CRUD dengan class dari file model kita

Active Record Pattern adalah sebuah pendekatan untuk mengakses sebuah database, database kita dibungkus oleh class. Jika sudah ada class yang mereprentasikan table maka instans akan terhubung ke objek

Istilah - istilah :

- `$fillable` adalah fild mana aja yang boleh di isi saat akan memasukkan data
- `$

### Mari Coba

- **Buka Terminal**

  Buka terminal dan masuk ke folder laravel kita, lalu ketikkan

  ```
  php artisan tinker
  ```

- **Buat Variable**

  Ketikkan variable untuk menginstasiasi modelnya

  Misal :

  ```
  $user = new User;
  ```

- **Memasukkan Data**

  Ketikkan perintah dibawah :

  Gambaran perintah `$instansi_objek->Filed_pada_database = "Value"`

  - `$user->name = "Nama Kalian"`

    Untuk mengisi fild name

  - `$user->email = "Email Kalian"`

    Untuk mengisi fild email

  - `$user->password = bcrypt("Nama Kalian")`

    Untuk mengisi fild password. `bcrypt()` sendiri digunakan untuk mengenkripsi password

  - `$user->save()`

    Untuk menyimpan semua yang sudah kita lakukan

  - `$user->all()`

    Untuk mengambil semua data

  - `$user->find()`

    Cara mengunnakan `find(parameter)`. Ini digunakan untuk menemukan data sesuai parameter, kelemahanya jika parameter tidak ditemukan akan mengembalikan `null`

  - `$user->findOrFail()`

    Ini sama dengan `find()` hanya bedanya jika parameter tidak ditemukan maka akan muncul pesan kesalahan. dan yang ini sering di gunakan
