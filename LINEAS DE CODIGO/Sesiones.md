# Sesiones

Las sesiones son útiles para almacenar información entre distintas pantallas de nuestra web. 

En Laravel, podemos **establecer** una variable de sesión así:

```php
// ...
session(['name' => 'David Copano']);
```

También lo podemos hacer así, si tenemos la variable `$request`:

```php
// ...
$request()->session()->put('name', 'David Copano');
```

Para **obtener** la variable de sesión que hemos establecido antes, hacemos lo siguiente:

```php
$name = session('name');
return $name; // David Copano
```

También lo podemos hacer así, si tenemos la variable `$request`:

```php
// ...
$request()->session()->get('name');
```

Para **borrar** una variable de sesión, lo hacemos así:

```php
session()->forget('name');
```