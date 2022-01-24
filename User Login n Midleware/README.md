## User Login

#### [Authentication](https://laravel.com/docs/8.x/authentication#authenticating-users)

Laravel menyediakan plugin yag menangani masalah authentication yang bernama laravel breese. Jika ingin tidak menggunakan Laravel Breeze kita bisa menggunakan `Facades`. Dengan menggunakan ini kita di permudah dalam mengelola session dan lain lain.

Istilah :

- `::attampt()` untuk mengecek apakah login yang di lakukan user benar atau tidak.

### Mari Mulai

- **Buka View Login**

  Pastekan kode di bawah :

  ```
  @extends('layouts.main')
  @section('container')
        <div class="row justify-content-center">
            <div class="col-md-5">

                @if (session()->has('success'))
                    <div class="alert alert-success alert-dismissible fade show" role="alert">
                        {{ session('success') }}
                        <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                    </div>
                @endif
                @if (session()->has('loginError'))
                    <div class="alert alert-danger alert-dismissible fade show" role="alert">
                        {{ session('loginError') }}
                        <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
                    </div>
                @endif

                <main class="form-signin">
                    <h1 class="h3 mb-3 fw-normal">Please sign in</h1>
                    <form action="/login" method="POST">
                        @csrf
                        <div class="form-floating">
                            <input type="email" class="form-control @error('email') is-invalid @enderror" name="email" id="email" placeholder="name@example.com" autofocus required value="{{ old('email') }}">
                            <label for="email">Email address</label>
                            @error('email')
                                {{ $message }}
                            @enderror
                        </div>
                        <div class="form-floating">
                            <input type="password" class="form-control" name="password" id="password" placeholder="Password" required>
                            <label for="password">Password</label>
                        </div>
                        <button class="w-100 btn btn-lg btn-primary" type="submit">Login</button>
                    </form>
                    <small class="d-block text-center mt-3">
                        Not registered? <a href="/register" class="text-decoration-none">Register Now</a>
                    </small>
                </main>
            </div>
        </div>
  @endsection
  ```

- **Buka Route web.php File**

  Tambhakan code di bawah :

  ```
  Route::post('/login', [LoginController::class, 'authenticate']);
  ```

- **Buka Login Controller**

  Tambahkan code di bawah :

  ```
    public function authenticate(Request $request)
    {
        $credential = $request->validate([
            'email' => 'required|email:dns',
            'password' => 'required'
        ]);

        if (Auth::attempt($credential)) {
            $request->session()->regenerate();

            return redirect()->intended('/dashboard');
        }

        return back()->with('loginError', 'Login failed!');
    }
  ```

  Tambahkan code di bawah di paling atas :

  ```
  use Illuminate\Support\Facades\Auth;
  ```

  ##### Penjelasan :

  ```
    if (Auth::attempt($credential)) {
        $request->session()->regenerate();

        return redirect()->intended('/dashboard');
    }
  ```

  - Kode di atas akan melakukan pengecekan setiap ada yang login, jika email dan passwordnya sama maka akan di redirect ke halaman tertentu.
  - Di kode tersebut juga terdapat session regenerate, ini digunakan untuk menghundarkan dari teknik hacking yang bernama `session fixation`

  ```
  return back()->with('loginError', 'Login failed!');
  ```

  Kode tersebut digunakan untuk mengembalikan ke halaman login, jika login gagal. dan `with()` digunakan untuk menampilkan flash message. Jika kita lihat lagi di file view login ada code :

  ```
  @if (session()->has('loginError'))
    <div class="alert alert-danger alert-dismissible fade show" role="alert">
        {{ session('loginError') }}
        <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
    </div>
  @endif
  ```

  Ini digunakan untuk menampilakan pesan yang dikirim dari `with()` yaitu `loginError`.

## [Midleware](https://laravel.com/docs/8.x/middleware#introduction)

Midleware menyediakan sebuah mekanisme untuk melakukan inspeksi dan filter HTTP request. Laravel melakukan verifikasi apakah user aplikasi kita sudah ter-authentication apa belum, jika belum maka midleware akan redirect ke halaman login dll. Tapi kalo sudah ter-authentication maka laravel memperbolehkan kita untuk lanjut mengakses dashboard

Instilah midleware laravel :

- `guest` = untuk pengguna yang belum ter-auntetikasi
- `auth` = untuk pengguna yang sudah ter-auntetikasi

### Mari Coba

- **Buka Route web.php**

  Tambah route login menjadi :

  ```
  Route::get('/login', [LoginController::class, 'index'])->middleware('guest');
  ```

  Ini artinya method index yang ada pada `class LoginController` hanya bisa di akses oleh seorang guest atau yang belum login, maka yang sudah login tidak bisa mengakses halaman login lagi

- **Buka File Provider**

  Kita bisa menggunakan `ctrl+p` lalu ketikkan `RouteServiceProvider.php`.

  Ubah :

  ```
  public const HOME = '/home';
  ```

  Menjadi :

  ```
  public const HOME = '/';
  ```

  Ini berfungsi untuk redirect ke route `/`. Dan ini terjadi saat seorang user yang sudah login/ter-authentikasi mencoba masuk ke halaman login

- **Mengubah Tombol Login**

  Nah karena tombol user sudah login, maka tombol login sudah tidak berfungsi kan. Sekarang kita bisa ubah tombol loginya. Mari kita ubah

  istilah :

  `@auth` adalah untuk manampilkan sebuah element setelah user ter-authentikasi oleh aplikasi

  `@guest` adalah kebalikan dari `@auth` yaitu menampilkan sebuah element jika user beluk ter-authentikasi oleh aplikasi

  - **Buka File Navbar**
    Ubah bagian tombol login menjadi :

    ```
    @auth
    <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button"
            data-bs-toggle="dropdown" aria-expanded="false">
            Welcome back, {{ auth()->user()->name }}
        </a>
        <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
            <li><a class="dropdown-item" href="/dashboard"><i class="bi bi-layout-text-sidebar-reverse"></i>
                    My Dashboard</a></li>
            <li>
                <hr class="dropdown-divider">
            </li>
            <li>
                <form action="/logout" method="POST">
                    @csrf
                    <button type="submit" class="dropdown-item">
                        <i class="bi bi-box-arrow-in-right"></i> Logout
                    </button>
                </form>
            </li>
        </ul>
    </li>
    @else
    <li class="nav-item">
        <a href="/login" class="nav-link {{ $active === 'login' ? 'active' : '' }} "><i
                class="bi bi-box-arrow-in-right"></i> Login</a>
    </li>
    @endauth
    ```

    Penjelasan :

    Jadi saat user sudah ter-authentikasi maka akan menjalankan `@auth` sehingga mengeksekusi kode :

    ```
    <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button"
            data-bs-toggle="dropdown" aria-expanded="false">
            Welcome back, {{ auth()->user()->name }}
        </a>
        <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
            <li><a class="dropdown-item" href="/dashboard"><i class="bi bi-layout-text-sidebar-reverse"></i>
                    My Dashboard</a></li>
            <li>
                <hr class="dropdown-divider">
            </li>
            <li>
                <form action="/logout" method="POST">
                    @csrf
                    <button type="submit" class="dropdown-item">
                        <i class="bi bi-box-arrow-in-right"></i> Logout
                    </button>
                </form>
            </li>
        </ul>
    </li>
    ```

    Dan jika belum ter-authentikasi maka akan menjalankan `@else` dan mengesekusi kode :

    ```
    <li class="nav-item">
        <a href="/login" class="nav-link {{ $active === 'login' ? 'active' : '' }} "><i
                class="bi bi-box-arrow-in-right"></i> Login</a>
    </li>
    ```

- **Membuat Logout**

  - **Buka Route web.php**

    Tambahkan kode berikut :

    ```
    Route::post('/logout', [LoginController::class, 'logout']);
    ```

    ini artinya jika ada route `/logout` maka akan mengeksekusi `LoginController` dengan nama logout

  - **Buka LoginController**

    Tambahkan code berikut :

    ```
    public function logout(Request $request)
    {

        Auth::logout();

        $request->session()->invalidate();

        $request->session()->regenerateToken();

        return redirect('/');
    }
    ```

    Untuk lebih jelasnya [klik disini](https://laravel.com/docs/8.x/authentication#logging-out)

- **Menambahkan Route Dashboard**

  Tambahkan code berikut :

  ```
  Route::get('/dashboard', DashboardPostController::class)->middleware('auth');
  ```

  Dan jika kita jalankan, akan memunculkan sebuah error `route login not defined`, itu karena tidak punya route yang namanya login

  - **Cara Memperbaiki**

    Masuk ke folder `app\http\midleware\Authenticate.php` code nya yaitu :

    ```
    protected function redirectTo($request)
    {
        if (! $request->expectsJson()) {
            return route('login');
        }
    }
    ```

    itu artinya jika ada sebuah user yang ingin memasuki halaman tapi belum login maka akan di redirect ke login, sementara kita tidak mempunyai route itu. Maka solusinya kita kasihkan nama pada route kita.

    Ubah route `/login` dari

    ```
    Route::get('/login', [LoginController::class, 'index'])->middleware('guest');
    ```

    Menjadi

    ```
    Route::get('/login', [LoginController::class, 'index'])->name('login')->middleware('guest');
    ```
