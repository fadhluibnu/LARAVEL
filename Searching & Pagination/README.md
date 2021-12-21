## Mari muali membuat

- **Buka `PostController.php`**

  Pastekan kode di bawah :

  ```
  public function index()
  {
      $title = '';
      if (request('author')) {
          $author = User::firstWhere('username', request('author'));
          $title .= ' by ' . $author->name;
      }
      if (request('category')) {
          $category = Category::firstWhere('slug', request('category'));
          $title .= ' in ' . $category->name;
      }
      return view('posts', [
          'title' => 'All Post' . $title,
          'active' => 'posts',
          'posts' => Post::latest()->filter(request(['search', 'category', 'author']))->paginate(7)->withQueryString()
      ]);
  }
  ```

  `paginate(7)` disitu digunakan untuk menambahkan suatu fitur pagination, code tersebut artinya menampilakan postingan maxsimal 7 postingan

  `withQueryString()` disutu digunakan untuk menangkap semua url yang ada pada aplikasi kita dan akan digabungkan dengan url pagination

- **Buka `Post.php`**

  Buka file tersebut dan pastekan kode di bawah :

  ```
  public function scopeFilter($query, array $filters)
  {
      $query->when($filters['search'] ?? false, function ($query, $search) {
          return $query->where(function ($query) use ($search) {
              $query->where('title', 'like', '%' . $search . '%')
                  ->orWhere('body', 'like', '%' . $search . '%');
          });
      });

      $query->when($filters['category'] ?? false, function ($query, $category) {
          return $query->whereHas('category', function ($query) use ($category) {
              $query->where('slug', $category);
          });
      });

      // menggunakan erro funnction
      $query->when(
          $filters['author'] ?? false,
          fn ($query, $author) =>
          $query->whereHas('author', fn ($query) =>
          $query->where('username', $author))
      );
  }
  ```

- **Buka File `posts.blade.php`**

  Pastekan ganti semua code dengan code ini :

  ```
  @extends('layouts.main')

  @section('container')

          <h1 class="mb-3 text-center">{{ $title }}</h1>

          <div class="row mb-3 justify-content-center">
              <div class="col-md-6">
                  <form action="/posts">
                      @if (request('category'))
                          <input type="hidden" name="category" value="{{ request('category') }}">
                      @endif
                      @if (request('author'))
                          <input type="hidden" name="author" value="{{ request('author') }}">
                      @endif
                      <div class="input-group mb-3">
                          <input type="text" class="form-control" placeholder="Search blogs.." name="search"
                              value="{{ request('search') }}">
                          <button class="btn btn-primary" type="submit">Search</button>
                      </div>
                  </form>
              </div>
          </div>


          {{-- mengecek apakah ada postingan atau tidak --}}

          @if ($posts->count())
              <div class="card mb-3 text-center">
                  <img src="https://source.unsplash.com/1200x400?{{ $posts[0]->category->name }}" class="card-img-top"
                      alt="{{ $posts[0]->slug }}">
                  <div class="card-body">
                      <h3 class="card-title"><a href="/posts/{{ $posts[0]->slug }}"
                              class="text-decoration-none text-dark">{{ $posts[0]->title }}</a></h3>
                      <p>
                          <small> By <a
                                  href="/posts?author={{ $posts[0]->author->username }}{{ request('category') == true ? '&category=' . request('category') : '' }}{{ request('search') == true ? '&search=' . request('search') : '' }}"
                                  class="text-decoration-none">{{ $posts[0]->author->name }}</a> in <a
                                  href="/posts?category={{ $posts[0]->category->slug }}{{ request('author') == true ? '&author=' . request('author') : '' }}{{ request('search') == true ? '&search=' . request('search') : '' }}"
                                  class="text-decoration-none">{{ $posts[0]->category->name }}</a> |
                              {{ $posts[0]->created_at->diffForHumans() }}
                          </small>
                      </p>
                      <p class="card-text">{{ $posts[0]->excerpt }}</p>
                      <a href="/posts/{{ $posts[0]->slug }}" class="text-decoration-none btn btn-primary">Read more</a>
                  </div>
              </div>

              <div class="container mb-3">
                  <div class="row">
                      @foreach ($posts->skip(1) as $post)
                          <div class="col-md-4">
                              <div class="card">
                                  <div class="position-absolute bg-primary px-3 py-2 " style="border-bottom-right-radius: 5px;"><a
                                          href="/posts?category={{ $post->category->slug }}{{ request('author') == true ? '&author=' . request('author') : '' }}{{ request('search') == true ? '&search=' . request('search') : '' }}"
                                          class="text-white text-decoration-none">{{ $post->category->name }}</a></div>
                                  <img src="https://source.unsplash.com/500x400?{{ $post->category->name }}"
                                      class="card-img-top" alt="{{ $post->slug }}">
                                  <div class="card-body">
                                      <h5 class="card-title">{{ $post->title }}</h5>
                                      <p>
                                          <small> By <a
                                                  href="/posts?author={{ $post->author->username }}{{ request('category') == true ? '&category=' . request('category') : '' }}{{ request('search') == true ? '&search=' . request('search') : '' }}"
                                                  class="text-decoration-none">{{ $post->author->name }}</a> |
                                              {{ $post->created_at->diffForHumans() }}
                                          </small>
                                      </p>
                                      <p class="card-text">{{ $post->excerpt }}</p>
                                      <a href="/posts/{{ $post->slug }}" class="btn btn-primary">Read More</a>
                                  </div>
                              </div>
                          </div>
                      @endforeach
                  </div>
              </div>
          @else
              <p class="text-center fs-4">Not post found</p>

          @endif
          <div class="d-flex justify-content-center">
              {{ $posts->links() }}
          </div>

  @endsection

  ```

- **Masuk Ke View `post.blade.php`**

  Ganti semua code menjadi :

  ```
  @extends('layouts.main')
    @section('container')

        <div class="container">
            <div class="row justify-content-center">
                <div class="col-md-8">
                    <h2 class="mb-3">{{ $post->title }}</h2>
                    <p>By <a href="/posts?author={{ $post->author->username }}"
                            class="text-decoration-none">{{ $post->author->name }}</a>
                        in
                        <a href="/posts?category={{ $post->category->slug }}"
                            class="text-decoration-none">{{ $post->category->name }}</a>
                    </p>
                    <img src="https://source.unsplash.com/1200x400?{{ $post->category->name }}" class="img-fluid"
                        alt="{{ $post->category->name }}">

                    <article class="my-3 fs-6">
                        {!! $post->body !!}
                    </article>
                    <a href="/posts">Back To Posts</a>

                </div>
            </div>
        </div>

    @endsection

  ```

- **Masuk Ke `navbar.blade.php`**

  Ganti semua code menjadi :

  ```
  <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
      <div class="container">
          <a class="navbar-brand" href="#">WPU Blog</a>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav"
              aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
              <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarNav">
              <ul class="navbar-nav">
                  <li class="nav-item">
                      <a class="nav-link {{ $active === 'home' ? 'active' : '' }}" href="/">Home</a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link {{ $active === 'about' ? 'active' : '' }}" href="/about">About</a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link {{ $active === 'posts' ? 'active' : '' }}" href="/posts">Blog</a>
                  </li>
                  <li class="nav-item">
                      <a class="nav-link {{ $active === 'category' ? 'active' : '' }}" href="/categories">Category</a>
                  </li>
              </ul>
          </div>
      </div>
  </nav>

  ```

- **Masuk Ke View `categories.blade.php`**

  Ganti semua code menjadi :

  ```
  @extends('layouts.main')

  @section('container')

      <h1 class="mb-5">Post Categories</h1>

      <div class="container">
          <div class="row">
              @foreach ($categories as $category)
                  <div class="col-md-4">
                      <a href="/posts?category={{ $category->slug }}">
                          <div class="card bg-dark text-white">
                              <img src="https://source.unsplash.com/1200x400?{{ $category->name }}" class="card-img"
                                  alt="{{ $category->name }}">
                              <div class="card-img-overlay d-flex align-items-center">
                                  <h5 class="card-title text-center flex-fill p-2 rounded bg-primary">{{ $category->name }}
                                  </h5>
                              </div>
                          </div>
                      </a>
                  </div>
              @endforeach
          </div>
      </div>
  @endsection
  ```

- **Masuk Ke File `AppServiceProvider.php`**

  Cari method `boot()`, tambahkan code di bawah :

  ```
  Paginator::useBootstrap();
  ```

  ini digunakan agar framework yang dipasang oleh laravel menjadi bootstrap, framework default yang digunakan oleh laravel yaitu Telwind
