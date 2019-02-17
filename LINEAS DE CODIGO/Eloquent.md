# Líneas de código para modelos Eloquent

- Si no se encuentra un registro en la BBDD, lanzar un error 404:

```php
$project = Project::findOrFail($id);
```

- Obtener el objeto/registro en la BBDD que se ha creado:

```php
$validatedAttributes = request()->validate([
    'title' => ['required', 'min:3'],
    'description' => ['required', 'min:3']
]);
$project = Project::create($validatedAttributes);
```

- Actualización **masiva** de campos, cumpliéndose antes una condición:

```php
User::where('age', '<', 18)->update(['under_18' => 1]);
```

## Ampliar modelos de Eloquent con funciones personalizadas en su clase

Hay ocasiones en las que debemos mejorar la lectura del código, para tenerlo lo más simple posible. Por ejemplo, para mejorar el primer punto (`Obtener registros según un campo determinado`) podemos hacer lo siguiente:

**NOTA**: Para seguir este ejemplo, debemos tener las relaciones ya asociadas, en este caso sería así (archivo `database/migrations/<archivo de migracion>.php`):

```php
class CreateProjectsTable extends Migration
{
    // ...
    public function up()
    {
        Schema::create('projects', function (Blueprint $table) {
            $table->increments('id');
            $table->unsignedInteger('owner_id');
            $table->string('title');
            $table->text('description');
            $table->timestamps();

            $table->foreign('owner_id')->references('id')->on('users')->onDelete('cascade');
        });
    }
}
```

Y tener ya ejecutada en la BBDD esta migración.

Una vez realizado esto, ya podemos seguir estos pasos:

1. Abrir nuestro modelo (en este caso User -ubicado en `app/User.php`-) y añadir la siguiente función (para obtener los proyectos del usuario):

```php
class User extends Authenticatable
{
    // ...

    public function projects()
    {
        // con 'owner_id' sobreescribimos la foreign key
        // si no lo ponemos, en este caso en la tabla 'projects' buscaría el 
        // campo 'user_id', el cual no existe

        // --- ESTE 2do PARÁMETRO ES OPCIONAL ---

        return $this->hasMany(Project::class, 'owner_id');
    }
}
```

2. En nuestro controlador, ya podremos llamar a esta función que nos devuelve los proyectos del usuario logeado:

```php
// ...
$projects = auth()->user()->projects;
```

También puede ocurrir que de un proyecto queramos conseguir su propietario, en este caso abrimos el modelo Eloquent (`app/Project.php`) y creamos la siguiente función (al igual que antes, en el archivo de migración **deben** estar las relaciones ya asociadas):

```php
class Project extends Model
{
    // ...
    public function owner()
    {
        return $this->belongsTo(User::class);
    }
}
```

+info de la diferencia de `belongsTo` y `hasOne` [aquí](https://stackoverflow.com/questions/30058949/should-i-use-belongsto-or-hasone-in-laravel)

## Métodos de Eloquent útiles

**NOTA**: Las colecciones Eloquent pueden tratarse como un array. Por ejemplo, para acceder al primer registro de la colección que devuelve el método `all()`, podemos hacer lo siguiente:

```php
$users = \App\Users::all();
dd($users[0]);
```

A continuación se listan algunos métodos Eloquent bastante útiles que tener en cuenta:

- Obtener **todos** los registros:

```php
$projects = Project::all();
```

- Obtener registros según un campo determinado (OJO: estos ejemplos devuelven una **colección** Eloquent, +info de las colecciones abajo):

```php
$projects = Project::where('owner_id', auth()->id())->get();
// ...
$user = User::where('email', 'johndoe@example.com')->get();
```

- Obtener el **primer** registro:

```php
$firstProject = Project::first();
```

- Obtener el **primer** registro que contenga un campo determinado:

```php
$user = User::where('email', 'johndoe@example.com')->first();
```

- Obtener el **último** registro:

```php
$lastProject = Project::latest()->first();
```

- Obtener un registro **según un ID determinado**:

```php
$projectId3 = Project::find(3);
```

- Obtener los resultados de un campo determinado (**DEVUELVE UNA COLECCIÓN**):

```php
$userEmails = User::pluck('email');
```

- Convertir una colección Eloquent a un array:

```php
$projectsArray = Project::all()->toArray();
```

- Consulta LIKE:

```php
Project::where('title', 'LIKE', '%new%')->get();
```

- Consulta IN:

```php
$users = User::whereIn('id', array(1, 2, 3))->get();
```

- Obtener campos específicos:

```php
Project::select('title','description')->where('id', 1)->get();
```

- Devolver un JSON automáticamente de una colección Eloquent:

```php
// ...
$projects = Project::all();
return $projects;
```

- Iterar sobre cada resultado de una colección Eloquent:

```php
// ...
$users = User::all();
$userNames = $users->map(function($user) {
    return $user->name; 
});
// devuelve una colección con los nombres de los usuarios, en este caso:

/* Illuminate\Support\Collection {#3239
    all: [
       "JohnDoe",
       "JaneDoe",
    ],
} */
```

- Filtrar los resultados de una colección (obtener los usuarios con un ID mayor o igual que 3, en este caso):

```php
// ...
$users = User::all();
$filteredUsers = $users->filter(function($user) { 
    return $user->id >= 3; 
});
// devuelve una colección con los usuarios que cumplan este requisito
```

Otra forma más rápida de filtrar por un **campo** sería la siguiente:

```php
// ...
$users = User::all();
$usersEmailVerified = $users->filter->email_verified_at;
// devuelve una colección con los usuarios que cumplan este requisito
```

**NOTA**: Esto también puede refactorizarse añadiendo una nueva función al modelo Eloquent (archivo `app/User.php`):

```php
// ...
public function isVerified()
{
    return $this->email_verified_at;
}

// ...
$users = User::all();
$usersEmailVerified = $users->filter->isVerified();
```

- Convertir un array a una colección Eloquent (esto puede ser útil para iterar sobre cada ítem con las funciones `map()`, `filter()` de arriba):

```php
$collection = collect(['foo', 'bar']);
$uppercaseItems = $collection->map(function($item) { 
    return strtoupper($item);  
});
```

- Devolver la suma de un campo específico:

```php
// ...
$users = User::all();
$idSum = $users->sum('id');
```