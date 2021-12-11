## Environment Variables

[Dokumentasi Environment Variables](https://laravel.com/docs/8.x/configuration#environment-configuration)

Ini digunakan untuk melakukan setting konfigurasi pada aplikasi kita agar konfigurasi tidak bisa di lihat oleh orang lain.

Nama file nya adalah :

- `.env`

  File yang akan digunakan. Dan ini menyimpan banyak variable

- `.env.example`

  File ini hanyalah templatenya

### Macam - Macam Variable Di File `.env`

- `APP_KEY`

  Sebuah key yang otomstis di generate oleh laravel saat kita mendownloadnya

- `APP_NAME`

  Untuk mengetahui nama aplikasi kita

- `APP_ENV`

  Untuk memberi ngasih tau aplikasi dalam tahap local/development/prodaction

- `APP_URL`

  Untuk memberi ngasih tau URL aplikasi Kita

- `DB_CONNECTION`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`

  Untuk menentukan konfigurasi Database kita. Untuk `DB_DATABASE` akan saya ganti menjadi `DB_DATABASE=wpu_blog1`
