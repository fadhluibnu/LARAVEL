## Form Registrasi

Sekarang kita akan masuk ke seri membuat sebuah form registrasi yang nantinya akan di gunakan oleh para author

- **Buat Controller**

  Buat sebuah controller yang akan digunakan untuk controller registrasi. Untuk membuat controller bisa ketikkan `php artisan make:controller RegisterController`

- **Buat Route Baru**

  Masuk ke `web.php` tambahkan route di bawah :

  - Register View

    ```
    Route::get('/register', [RegisterController::class, 'index']);
    ```

    Untuk route ini dugunakan untuk menampilakan view form registrasi;

  - Register Validasi

    ```
    Route::post('/register', [RegisterController::class, 'store']);
    ```

    Untuk route ini dugunakan untuk mengkap data dengan method post yang di kirim melalui form. Route ini juga akan digunakan untuk menyimpan semua data yang sudah dikirim melalui form post tadi

- **Masuk Ke Register Controller**

  - Untuk View

    Buat sebuah method `index()` untuk menampilakan view dari registrasi :

    ```
    public function index()
    {
        return view('register.index', [
            'title' => 'Register',
            'active' => 'register'
        ]);
    }
    ```

    `view('register.index')` berarti kita akan membuat sebuah view index di dalam folder register

  - Untuk meyimpan data

    Buat metdod dengan nama `store` :

    ```
    public function store(Request $request)
    {
        // manangkap data
        $validatedData = $request->validate([
            'name' => 'required|max:255',
            'username' => ['required', 'min:3', 'max:255', 'unique:users'],
            'email' => 'required|email:dns|unique:users',
            'password' => 'required|min:5|max:255'
        ]);

        $validatedData['password'] = Hash::make($validatedData['password']);

        User::create($validatedData);

        return redirect('/login')->with('success', 'Registration successfull!');
    }
    ```

    Akan sedikit saya jelaskan code code yang baru di method `store`. Jadi method ini selain digunakan untuk menyimpan data, method ini juga digunakan sebagai sebuah [validasi data](https://laravel.com/docs/8.x/validation) yang dilakukan oleh laravel.

    `required` digunakan supaya data form tidak null,

    `min:` & `max:` digunakan untuk membatasi minimal dan maksimal data/teks yang di masukkan

    `email:dns` ini digunakan supaya data yang dimasukkan berupa email yang valid, cth `name@gmail.com`

    `Hash::make()` digunakan untuk meng-eksripsi pasword yang telah di masukkan oleh user

    `with()` disini digunakan untuk mengirimkan sebuah session yang nantinya digunakan untuk menampilkan flash message saat registrasi berasil

- **Membuat View Register**

  Masuk ke folder view. Lalu tambahkan folder dengan nama `register` dan di dalamnya buat sebuah file dengan nama `index.blade.php`.

  Buka file tersebut dan tambhakan code di bawah :

  ```
  @extends('layouts.main')

  @section('container')

      <div class="row justify-content-center">
          <div class="col-lg-5">
              <main class="form-registration">
                  <h1 class="h3 mb-3 fw-normal">Registration Form</h1>
                  <form action="/register" method="POST">
                      @csrf
                      <div class="form-floating">
                          <input type="text" name="name" class="form-control rounded-top @error('name') is-invalid @enderror"
                              id="name" placeholder="Name" required value="{{ old('name') }}">
                          <label for="name">Name</label>
                          @error('name')
                              <div id="validationServerUsernameFeedback" class="invalid-feedback">
                                  {{ $message }}
                              </div>
                          @enderror
                      </div>
                      <div class="form-floating">
                          <input type="text" name="username" class="form-control @error('username') is-invalid @enderror"
                              id="username" placeholder="username" required value="{{ old('username') }}">
                          <label for=" username">Username</label>
                          @error('username')
                              <div id="validationServerUsernameFeedback" class="invalid-feedback">
                                  {{ $message }}
                              </div>
                          @enderror
                      </div>
                      <div class="form-floating">
                          <input type="email" name="email" class="form-control @error('email') is-invalid @enderror"
                              id="email" placeholder="name@example.com" required value="{{ old('email') }}">
                          <label for=" email">Email address</label>
                          @error('email')
                              <div id="validationServerUsernameFeedback" class="invalid-feedback">
                                  {{ $message }}
                              </div>
                          @enderror
                      </div>
                      <div class="form-floating">
                          <input type="password" name="password"
                              class="form-control rounded-bottom @error('password') is-invalid @enderror" id="password"
                              placeholder="Password" required>
                          <label for="password">Password</label>
                          @error('password')
                              <div id="validationServerUsernameFeedback" class="invalid-feedback">
                                  {{ $message }}
                              </div>
                          @enderror
                      </div>
                      <button class="w-100 btn btn-lg btn-primary mt-3" type="submit">Register</button>
                  </form>
                  <small class="d-block text-center mt-3">
                      Alredy registered? <a href="/login" class="text-decoration-none">Login</a>
                  </small>
              </main>
          </div>
      </div>

  @endsection
  ```

  Saya akan sedikit code code baru yang digunakan di situ :

  - `@csrf`

    Fungsi ini digunakan untuk membuat sebuah token yang di generate oleh aplikasi kita dan akan digunakan untuk melakukan registrasi. Dengan menggunakan fungsi ini, laravel membantu security kita menjadi aman. Secara spesifik fungsi ini digunakan agar terhindar dari serangan cross-site request forgery (CSRF). Jadi CSRF ini intinya adalah sebuah serangan dari website lain ke website kita. Contoh kasusnya adalah :

    ```
    <form action="https://your-application.com/user/email" method="POST">
        <input type="email" value="malicious-email@example.com">
    </form>
    ```

    Website orang lain mencoba untuk memasukkan data, tapi bukan lewat website kita melainkan dari website mereka, dan ini tentu saja menjadi berbahaya. Dan laravel sudah menangani ini dengan menambahkan `@csrf` di dalam form kita.

  - `@error`

    Fungsi ini akan digunakan untuk menangkap error yang di dapatkan saat kita akan menyimpan datanya, contoh kasusnya yaitu :

    ```
    @error('name')
        <div id="validationServerUsernameFeedback" class="invalid-feedback">
            {{ $message }}
        </div>
    @enderror
    ```

    Disitu `@error` mengangkap error yang dihasilkan oleh `name`. Untuk `$message` adalah variable untuk menampilakan errornya. Error bisa terjadi karena user tidak memassukan sesuai aturan yang di tetapkan. Aturan dari `name` sendiri adalah :

    ```
    'name' => 'required|max:255'
    ```

    Kita bisa lihat code diatas pada controller register. Jadi jika user tidak mengisikan data pada input name atau mengisikan name yang karakternya memiliki panjang melebihi 255 karakter, maka error tadi akan di tangkap oleh `@error`.

  - `old()`

    Fungsi `old()` disini digunakan untuk menangkap value yang telah di kirimkan. Kasus disini adalah jika saat kita melakukan register terus kita submit maka otomatis value yang tadi kita tuliskan pada form akan otomatis hilang, nah disini `old()` digunakan supaya saat kita memasukkan value yang salah maka value yang lama akan di tangkap oleh `old()` sehingga kita tidak mengulang semu.

- **Modifikasi Tampilan Form**

  Buat file `style.css` pada folder public :

  ```
  .form-signin .form-floating:focus-within {
    z-index: 2;
  }

  .form-signin input[type="email"] {
      margin-bottom: -1px;
      border-bottom-right-radius: 0;
      border-bottom-left-radius: 0;
  }

  .form-signin input[type="password"] {
      margin-bottom: 10px;
      border-top-left-radius: 0;
      border-top-right-radius: 0;
  }

  .form-registration input {
      border-radius: 0;
      margin-bottom: -1px;
  }
  ```

- **Menghubungkan `style.css`**

  Masuk ke view `main.blade.php`, tambahkan tag `link` seperti code di bawah :

  ```
  <link rel="stylesheet" href="style.css">
  ```
