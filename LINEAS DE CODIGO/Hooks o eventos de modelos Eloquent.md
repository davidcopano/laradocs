# Hooks o eventos de modelos Eloquent

Los hooks o eventos de un modelo Eloquent pueden ser útiles para detectar cuando se ha **creado**, **actualizado**, etc. un registro de modelo en la BBDD. Podemos aprovechar esto para realizar ciertas acciones después de haberse ejecutado algo en el modelo. Podemos crear un evento dentro del modelo que queramos de la siguiente forma:

1. Ir al archivo del modelo (`app/Project.php` en este caso) y ponemos lo siguiente:

```php
class Project extends Model
{
    // ...

    protected static function boot()
    {
        parent::boot();

        // esto se ejecutará cada vez que se haya CREADO
        // un registro de este modelo en la BBDD
        static::created(function($project) {
            \Mail::to($project->owner->email)->send(
                new ProjectCreated($project)
            );
        });
    }
}
```

+info: https://laravel.com/docs/5.7/eloquent#events