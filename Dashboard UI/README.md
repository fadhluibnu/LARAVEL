## Dashboard UI

Sekarang kita akan membuat Design UI untuk dashboard

kita bisa menggunakan template punya bootstrap [klik disini](https://getbootstrap.com/docs/5.0/examples/)

- Download dan ekstrak
- Lalu buka folder dashboardnya
- Copy `dashboard.css` dan paste di `public/css`
- Copy `dashboard.js` dan paste di `public/js`
- Buka file index di VSCode

### Mari Kita Mulai

- **Edit Route Dashboard**

  Dari :

  ```php
  Route::get('/dashboard', DashboardPostController::class)->middleware('auth');
  ```

  Menjadi :

  ```php
  Route::get('/dashboard', function () {
    return view('dashboard.index');
  })->middleware('auth');
  ```

  Dan hapus `dasbhoard.php` (dashboard controller)

- **Buat Folder View Dashboard**

  - **Buat File `index.blade.php`**

    Masukkan code berikut :

    ```blade
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <title>WPU Blog | Dashboard</title>
        <!-- Bootstrap core CSS -->
        <link
        href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css"
        rel="stylesheet"
        integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC"
        crossorigin="anonymous"
        />
        <!-- Custom styles for this template -->
        <link href="/css/dashboard.css" rel="stylesheet" />
    </head>
    <body>
        <header
        class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow"
        >
        <a class="navbar-brand col-md-3 col-lg-2 me-0 px-3" href="#">WPU Blog</a>
        <button
            class="navbar-toggler position-absolute d-md-none collapsed"
            type="button"
            data-bs-toggle="collapse"
            data-bs-target="#sidebarMenu"
            aria-controls="sidebarMenu"
            aria-expanded="false"
            aria-label="Toggle navigation"
        >
            <span class="navbar-toggler-icon"></span>
        </button>
        <input
            class="form-control form-control-dark w-100"
            type="text"
            placeholder="Search"
            aria-label="Search"
        />
        <div class="navbar-nav">
            <div class="nav-item text-nowrap">
            <form action="/logout" method="POST">
                @csrf
                <button type="submit" class="nav-link px-3 bg-dark border-0">
                <i class="bi bi-box-arrow-in-right"></i> Logout
                </button>
            </form>
            </div>
        </div>
        </header>
        <div class="container-fluid">
        <div class="row">
            <nav
            id="sidebarMenu"
            class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse"
            >
            <div class="position-sticky pt-3">
                <ul class="nav flex-column">
                <li class="nav-item">
                    <a class="nav-link active" aria-current="page" href="#">
                    <span data-feather="home"></span>
                    Dashboard
                    </a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="#">
                    <span data-feather="file"></span>
                    My Posts
                    </a>
                </li>
                </ul>
            </div>
            </nav>
            <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
            <div
                class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
            >
                <h1 class="h2">Welcome Back</h1>
            </div>
            </main>
        </div>
        </div>
        <script
        src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM"
        crossorigin="anonymous"
        ></script>
        <script
        src="https://cdn.jsdelivr.net/npm/feather-icons@4.28.0/dist/feather.min.js"
        integrity="sha384-uO3SXW5IuS1ZpFPKugNNWqTZRRglnUJK6UAZ/gxOX80nxEkN9NcGZTftn6RzhGWE"
        crossorigin="anonymous"
        ></script>
        <script src="/js/dashboard.js"></script>
    </body>
    </html>
    ```

- **Menerapkan Partials**

  - **Buat Folder `layouts`**

    Buat folder ini di dalam folder dashboard

  - **Buat file `main.blade.php`**

    Buat file ini di dalam folder `dashboard/layouts`

    Tambahakan code berikut :

    ```blade
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <title>WPU Blog | Dashboard</title>
        <!-- Bootstrap core CSS -->
        <link
        href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css"
        rel="stylesheet"
        integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC"
        crossorigin="anonymous"
        />
        <!-- Custom styles for this template -->
        <link href="/css/dashboard.css" rel="stylesheet" />
    </head>
    <body>
        @include('dashboard.layouts.header')
        <div class="container-fluid">
        <div class="row">
            @include('dashboard.layouts.sidebar')
            <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">
            @yield('container')
            </main>
        </div>
        </div>
        <script
        src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM"
        crossorigin="anonymous"
        ></script>
        <script
        src="https://cdn.jsdelivr.net/npm/feather-icons@4.28.0/dist/feather.min.js"
        integrity="sha384-uO3SXW5IuS1ZpFPKugNNWqTZRRglnUJK6UAZ/gxOX80nxEkN9NcGZTftn6RzhGWE"
        crossorigin="anonymous"
        ></script>
        <script src="/js/dashboard.js"></script>
    </body>
    </html>
    ```

  - **Buat File `header.blade.php`**

    Buat file ini di dalam folder `dashboard/layouts`, lalu tambahkan code berikut :

    ```blade
    <header
      class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow"
    >
      <a class="navbar-brand col-md-3 col-lg-2 me-0 px-3" href="#">WPU Blog</a>
      <button
        class="navbar-toggler position-absolute d-md-none collapsed"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#sidebarMenu"
        aria-controls="sidebarMenu"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <input
        class="form-control form-control-dark w-100"
        type="text"
        placeholder="Search"
        aria-label="Search"
      />
      <div class="navbar-nav">
        <div class="nav-item text-nowrap">
          <form action="/logout" method="POST">
            @csrf
            <button type="submit" class="nav-link px-3 bg-dark border-0">
              <i class="bi bi-box-arrow-in-right"></i> Logout
            </button>
          </form>
        </div>
      </div>
    </header>
    ```

  - **Buat File `sidebar.blade.php`**

    Buat File ini di dalam `dashboard/layouts`, lalu tambahkan code berikut :

    ```blade
    <nav
    id="sidebarMenu"
    class="col-md-3 col-lg-2 d-md-block bg-light sidebar collapse"
    >
    <div class="position-sticky pt-3">
        <ul class="nav flex-column">
        <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#">
            <span data-feather="home"></span>
            Dashboard
            </a>
        </li>
        <li class="nav-item">
            <a class="nav-link" href="/dashboard/posts">
            <span data-feather="file"></span>
            My Posts
            </a>
        </li>
        </ul>
    </div>
    </nav>
    ```

  - **Buka File View Index Dashboard**

    Hapus dulu semua codenya, ganti menjadi :

    ```blade
    @extends('dashboard.layouts.main')

    @section('container')
        <div
            class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom"
        >
            <h1 class="h2">Welcome Back,  {{ auth()->user()->name }} </h1>
        </div>
    @endsection
    ```
