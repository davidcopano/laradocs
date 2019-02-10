# Líneas de código para modelos Eloquent

- Obtener registros según un campo determinado:

```php
$projects = Project::where('owner_id', auth()->id())->get();
```

- Obtener **todos** los registros:

```php
$projects = Project::all();
```

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

- Obtener el último registro de la BBDD basado en un modelo:

```php
$project = Project::latest()->first();
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
        return $this->hasOne(User::class);
    }
}
```