# Laravel
Laravel is a PHP framework

## Docs
- [How to setup Vagrant with Laravel Homestead Tutorial](https://www.youtube.com/watch?v=rs2Hzx4qBm8)
- [Découvrez le framework PHP Laravel](https://openclassrooms.com/fr/courses/3613341-decouvrez-le-framework-php-laravel/3616434-installation-et-organisation) (french)

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
