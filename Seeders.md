# Seeders

Los **seeders** son la mejor forma para cargar datos de prueba para nuestras tablas en Laravel. Podremos cargar todo tipo de datos para nuestras tablas.

Para realizar esto, hacemos lo siguiente:

1. Generar un nuevo **seed** con este comando (en nuestro caso será para usuarios de prueba):

`php artisan make:seeder UserSeeder`

Al ejecutar este comando, se generará un archivo `UserSeeder.php` dentro del directorio `database/seeds`.

2. Abrimos el archivo `UserSeeder.php` y dentro del método `run()` añadimos nuestro código para añadir registros de prueba:

```php
// ...
public function run()
{
    // desactivamos la revisión de claves foráneas
    DB::statement('SET FOREIGN_KEY_CHECKS = 0;');

    // vaciamos todos los registros de la tabla
    DB::table('users')->truncate();

    // reactivamos la revisión de claves foráneas
    DB::statement('SET FOREIGN_KEY_CHECKS = 1;');
    
    // añadimos nuestros datos de prueba
    for($i = 0; $i < 10; $i++) {
        DB::table('users')->insert([
            'name' => 'nombre-seed-' . $i,
            'email' => 'email-seed-' . $i . '@example.com',
            'password' => Hash::make('123456')
        ]);
    }
}
```

3. Queda registrar nuestro seeder. Los seeders se registran en la clase `DatabaseSeeder` dentro de `database/seeds/DatabaseSeeder.php`. Dentro del método `run()` llamamos al método `call()` pasando como argumento el nombre de la clase de nuestro seeder:

```php
class DatabaseSeeder extends Seeder
{
    // ...
    public function run()
    {
        $this->call(UserSeeder::class);
    }
}
```

4. Ejecutamos nuestro seeder creado con el siguiente comando:

`php artisan db:seed`

En caso de que tengamos múltiples seeders, podemos pasar la opción `--class`, la cual permite ejecutar solamente el seeder pasado como argumento:

`php artisan db:seed --class=UserSeeder`

## Usando Model Factories para generar datos de prueba

Ir rellenando manualmente registros de prueba en la BBDD puede ser engorroso.

Podemos utilizar las Model Factories de Laravel para generar rápidamente grandes cantidades de registros en la BBDD. 

Una vez que hayamos definido nuestras Factories, podemos utilizar la función `factory()` para insertar registros en la BBDD:

```php
public function run()
{
    factory(App\User::class, 50)->create();
}
```

[+info de las Factories aquí](https://github.com/davidcopano/laradocs/blob/master/Model%20Factories.md)