# Autorizaciones de usuarios para realizar acciones.

+info: https://laracasts.com/series/laravel-from-scratch-2018/episodes/27

En muchas ocasiones, debemos restringir las acciones del usuario para que solo pueda hacerlas sobre sus cosas (por ejemplo, un usuario **solo** puede ejecutar ciertas acciones sobre sus proyectos). Laravel ofrece las llamadas **policies** para resolver esto.

Para llevar a cabo este ejemplo, debemos hacer lo siguiente:

1. Ejecutar el siguiente comando para crear la __policy__:

`php artisan make:policy ProjectPolicy --model=Project`

2. Abrir el archivo (`ProjectPolicy.php` según este ejemplo), generado en `app/Policies/`. Se mostrarán todas las acciones que puede realizar el usuario (`view`, `create`, etc). En la acción que queramos ya podremos poner las restricciones que queramos. Por ejemplo, **que un usuario sólo pueda ver sus projectos**:

```php
// ...

/*
  Ponemos la interrogación en User $user para cuando 
  NO haya usuario logeado
*/ 
public function view(?User $user, Project $project)
{
    return $project->owner_id == $user->id;
}
// ...
```

3. Añadir la __policy__ a nuestro proveedor de autenticación, para ello hacemos esto:

    - Abrir el archivo `app/Providers/AuthServiceProvider.php`

    - Añadir nuestra __policy__ al array de `$policies` existente:
    ```php
    // ...
    protected $policies = [
        'App\Project' => 'App\Policies\ProjectPolicy',
    ];
    // ...
    ```

4. Añadimos la autorización en el método del controlador que queramos (en este caso para que el usuario __vea__ sus projectos):

```php
// ...
public function show(Project $project)
{
    $this->authorize('view', $project);
    return view('projects.show', compact('project'));
}
// ...
```

## Autorizaciones en las vistas:

Para mostrar u ocultar código HTML en las vistas Blade, podemos usar lo siguiente:

```blade
@can('update', $project)
    <a href="#">Update</a>
@endcan
```