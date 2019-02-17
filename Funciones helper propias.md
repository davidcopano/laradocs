# Funciones helper propias

Podemos crearnos un archivo de funciones propias de PHP para utilizarlas a lo largo de nuestro proyecto Laravel.

Para poder conseguir esto, hacemos lo siguiente:

1. Abrir el archivo `composer.json`, ir a la key `"autoload"` y añadir lo siguiente:

```json
"autoload": {
    "psr-4": {
        "App\\": "app/"
    },
    "classmap": [
        "database/seeds",
        "database/factories"
    ],
    // añadir esto de abajo
    "files": ["app/helpers.php"]
},
```

2. Crear un archivo llamado `helpers.php` en la carpeta `app`, e incluir las funciones PHP propias que queramos. En este caso ponemos la siguiente:

```php
<?php

function flash($message) {
    session()->flash('message', $message);
}
```

3. Actualizamos el archivo `autoload.php` propio de Composer con el siguiente comando (**SI NO HACEMOS ESTO, NO FUNCIONARÁ**):

`composer dump-autoload`

4. Ya podemos llamar a nuestras funciones en nuestro proyecto:

```php
Route::post('/projects', function() {
    // ...
    flash('Project created successfully');

    return redirect('/');
});
```