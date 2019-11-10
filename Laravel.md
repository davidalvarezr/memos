# Laravel
Laravel is a PHP framework

## Table of contents
- [Laravel](#laravel)
  * [Docs](#docs)
  * [Routes](#routes)
    + [Returning an HTML page](#returning-an-html-page)
    + [Get a parameter from a route](#get-a-parameter-from-a-route)
    + [Allow certain character in the route (regex)](#allow-certain-character-in-the-route-regex)
    + [Redirection and route naming](#redirection-and-route-naming)
    + [Use a php variable inside a view](#use-a-php-variable-inside-a-view)
    + [Templates](#templates)
  * [Controllers](#controllers)
    + [Create a controller](#create-a-controller)
    + [Call a method controller from a route](#call-a-method-controller-from-a-route)
    + [... same with a named route](#-same-with-a-named-route)
    + [Give route param to controller](#give-route-param-to-controller)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Docs
- [How to setup Vagrant with Laravel Homestead Tutorial](https://www.youtube.com/watch?v=rs2Hzx4qBm8)
- [Découvrez le framework PHP Laravel](https://openclassrooms.com/fr/courses/3613341-decouvrez-le-framework-php-laravel/3616434-installation-et-organisation) (french)
- [Laravel and Angular together](https://www.youtube.com/watch?v=97BggEw0dJI)
- [Laravel - Stateless HTTP Basic Authentication](https://laravel.com/docs/5.7/authentication#stateless-http-basic-authentication) (useful for API)

## Routes
Routes are in folder `app/routes/web.php`

### Returning an HTML page
```php
Route::get('/', function () {
    return view('welcome'); // <-- returns the view in file `resources/views/welcome.blade.php
});
```

### Get a parameter from a route
```php
Route::get('/{n}', function ($n) {
    return 'I am the page n° ' . $n . '!';
});
```

### Allow certain character in the route (regex)
```php
Route::get('{n}', function ($n) { return 'I am the page n° ' . $n . '!'; })
    ->where('n', '[1-9][0-9]*');
```
Will match any `n` which is a number not beginning by a `0`.

### Redirection and route naming
The two codes below show two ways of redirecting `/redirection` to `/`.

Basic redirection
```php
Route::get('/', function () { return view('welcome'); });

Route::get('redirection', function() {
    return redirect('/');
});
```
Redirection using route name
```php
// name `/` as `home`
Route::get('/', ['as' => 'home', function () { return view('welcome'); }]);

Route::get('redirection', function() {
    return redirect()->route('home');
});
```

### Use a php variable inside a view
Inside the code of a route:
```php
return view('article')->with('numero', $n);
// which is the same that
return view('article')->withNumero($n);
// which is the same that
return view('article', ['numero' => $n]);       // Useful when there are several variables
```
`$numero` variable will then be available inside the view `article.php` or `article.blade.php`.

In the view:
```php
// article.php
<p>It is the article n° <?php echo $numero ?></p>
```
```php
// article.blade.php (blade syntax {{}})
<p>It is the article n° {{ $numero }}</p>
```

### Templates
With tmeplates, code one template view, then fill it with other views thanks to `@extends` keyword:

The template
```blade
<!-- generic.blade.php -->

<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>@yield('title')</title>
</head>
<body>
@yield('content')
</body>
</html>
```
The view extending the template
```blade
<!-- article.blade.php -->

@extends('generic')  <!--Use the template `generic.blade.php`-->

<!--In `generic.blade.php`, replace @yield('title') by Articles-->
@section('title')
    Articles
@endsection

<!--In `generic.blade.php`, replace @yield('content') by <p>It is the article n° {{ $numero }}</p>-->
@section('content')
    <p>It is the article n° {{ $numero }}</p>
@endsection
```

## Controllers

### Create a controller
```
php artisan make:controller WelcomeController
```

### Call a method controller from a route
```php
Route::get('/', 'WelcomeController@index');
```
`WelcomeController` is the class name of the controller. `index` is the public method that is called in the controller.

### ... same with a named route
```php
Route::get('/', ['uses' => 'WelcomeController@index', 'as' => 'home']);
```

### Give route param to controller
(Nothing to do)
```php
// web.php
Route::get('article/{n}', 'ArticleController@show')->where('n', '[0-9]+');

// ArticleController.php (inside the class)
public function show($n) {
    return view('article')->with('numero', $n);
}
```

## Forms
Blade allows to create form easily
```blade
resources/views/info-form.blade.php

@extends('templates.form')      {{--Extends template `form.blade.php` in `templates/`--}}

@section('form')

    {!! Form::open(['url' => 'users']) !!}              {{--Go to this url when sent--}}
        {!! Form::label('name', 'Put your name: ') !!}  {{--label--}}
        {!! Form::text('name') !!}                      {{--text area--}}
        {!! Form::submit('Send !') !!}                  {{--submit button--}}
    {!! Form::close() !!}

@endsection
```
This will send the form to the url `/users` (**POST** by default)

## Requests
This code show to handle the previous form when it is sent
```php
// routes/web.php

Route::get('users', 'InfoController@getForm');         // Send the form to the user
Route::post('users', 'InfoController@sendFilledForm'); // Handle post with filled form


// app/Http/Controllers/InfoController.php (inside the class)

public function getForm() {
    return view('info-form');
}

public function sendFilledForm(Request $request) {
    return 'The name is ' . $request->name;
}

```
