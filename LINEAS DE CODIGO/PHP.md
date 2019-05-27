# Líneas de código interesantes para PHP

- Generar automaticamente las rutas siguiendo la convención REST de Laravel:

```php
Route::resource('projects', 'ProjectsController');
```

Para ver las rutas generadas, ejecutar el comando `php artisan route:list`


- Si no se encuentra un registro en la BBDD, lanzar un error 404:

```php
$project = Project::findOrFail($id);
```

- Validar campos de un formulario:

```php
// devuelve un array asociativo con los campos del form. ya validados
$validatedAttributes = request()->validate([
    'title' => ['required', 'min:3'],
    'description' => ['required', 'min:3']
]);
```

- Volver a la URL anterior (en vez de usar return `redirect('/url')`):

```php
return back();
```

- Comprobar errores en un formulario:

    - Validamos los campos recibidos del form. (devuelve los atributos que se han validado);
   ```php
   $attributes = request()->validate([
      'description' => 'required'
   ]);
    ```

   - Mostramos los errores que hubieran en el form.:
   ```blade
   @if($errors->any())
       <ul>
           @foreach($errors->all() as $error)
               <li>{{$error}}</li>
           @endforeach
       </ul>
    @endif
   ```

    - Mostrar error de campo específico de formulario (`@error('nombre_del_input_del_form')`):
    ```blade
    @error('description')
        <div class="alert alert-danger alert-dismissible fade show mt-3">
            <button type="button" class="close" data-dismiss="alert"></button>
            {{$message}}
        </div>
    @enderror
    ```

   - **BONUS**: Comprobar si un campo especifico de un form. tiene errores:
   ```blade
   {{$errors->has('title') ? 'has-error' : ''}}
   ```

- Hacer un `var_dump()` de una variable, e inmediatamente un `die()`:

```php
dd('hola');
```

- Obtener un parámetro de configuración del archivo `.env`:

    - Declaramos la clave/valor que deseemos en el archivo `.env`, por ejemplo:
    ```
    TWITTER_APIKEY=souryd12oi323io
    ```
    - Obtenemos el valor de esta clave con la función `env()`, ejemplo:
    ```
    $twitterApiKey = env('TWITTER_APIKEY');
    ```
    - **Ejemplo completo** con varios archivos:
        - `config/services.php`
        ```php
        return [
            // ...
            'twitter' => [
                'apiKey' => env('TWITTER_APIKEY')
            ]
        ];
        ```
        - `app/Services/Twitter.php`
        ```php
        <?php

        namespace App\Services;

        class Twitter
        {
            protected $apiKey;

            public function __construct($apiKey)
            {
                $this->apiKey = $apiKey;
            }
        }
        ```
        - `app/Providers/AppServiceProvider.php` (utilizamos la función `config()` para obtener el valor del parámetro de `config/services.php`)
        ```php
        use App\Services\Twitter;

        // ...
        
        public function register()
        {
            $this->app->singleton(Twitter::class, function () {
                return new Twitter(config('services.twitter.apiKey'));
            });
        }
        ```
        
        - **NOTA**: Con el ejemplo de arriba completo ya podremos utilizar **la clase Twitter ya instanciada** en nuestro controlador:
        
        ```php
        use App\Services\Twitter;
        // ...
        public function index(Twitter $twitter)
        {
            dd($twitter);
        }
        ```

- Lanzar error 403:

    ```php
    abort(403);

    // también se puede poner de la siguiente forma:
    abort_if($condicion, 403, 'Mensaje de error (esto es OPCIONAL)');
    ```

- Devolver un JSON automáticamente de una colección Eloquent:

```php
// ...
$projects = Project::all();
return $projects;
```

- Generar una contraseña encriptada:

```php
$password = 'JohnDoe';
$hashedPassword = Hash::make($password);
echo $hashedPassword; // $2y$10$jSAr/RwmjhwioDlJErOk9OQEO7huLz9O6Iuf/udyGbHPiTNuB3Iuy
```

- Subir un archivo, devolviendo el nombre único con el que se ha subido:

```php
// le pasamos a store() la carpeta donde se subirá el archivo
$path = $request->file('avatar')->store('avatars');
```

- Sobreescribir método de mostrar formulario de inicio de sesión/login (útil para poner una lógica personalizada antes de mostrar el formulario):

    - En nuestro archivo `LoginController.php` (ubicado en `app/Http/Controllers/Auth`), añadimos la siguiente función

```php
// ...
class LoginController extends Controller 
{
    // ...
    public function showLoginForm()
    {
        return view('auth.login', ['foo' => 'bar1111']);
    }
}
```