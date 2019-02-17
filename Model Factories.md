# Model Factories

Las Model Factories de Laravel son útiles para generar una gran cantidad de datos de pruebas y guardarlos en la BBDD.

Para crear una Factory de un modelo, hacemos lo siguiente:

1. Usar el siguiente comando para generar la Factory (podemos pasarle el modelo que queramos con el parámetro `--model`):

`php artisan make:factory ProjectFactory --model=Project`

Esto creará un archivo llamado `ProjectFactory.php` dentro de `database/factories`.

2. Abrimos el archivo creado y rellenamos los datos que se rellenarán en la BBDD para esta tabla:

```php
// ...
$factory->define(App\Project::class, function (Faker $faker) {
    return [
        'owner_id' => 2,
        'title' => $faker->title,
        'description' => $faker->text,
    ];
});
```

3. Generar un seeder para esta Factory que hemos creado y registrarlo, [+info de los seeders aquí](https://github.com/davidcopano/laradocs/blob/master/Seeders.md)