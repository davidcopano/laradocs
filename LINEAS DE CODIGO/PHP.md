# Líneas de código interesantes para PHP

- Generar automaticamente las rutas siguiendo la convención REST de Laravel:

```
Route::resource('projects', 'ProjectsController');
```

Para ver las rutas generadas, ejecutar el comando `php artisan route:list`


- Si no se encuentra un registro en la BBDD, lanzar un error 404:

```
$project = Project::findOrFail($id);
```

- Validar campos de un formulario:

```
// devuelve un array asociativo con los campos del form. ya validados
$validatedAttributes = request()->validate([
    'title' => ['required', 'min:3'],
    'description' => ['required', 'min:3']
]);
Project::create($validatedAttributes);
```


- Volver a la URL anterior (en vez de usar return `redirect('/url')`):

```
return back();
```

- Comprobar errores en un formulario:

    - Validamos los campos recibidos del form. 
   ```php
   request()->validate([
      'description' => 'required'
   ]);
    ```

   - Mostramos los errores que hubieran en el form.
   ```blade
   @if($errors->any())
       <ul>
           @foreach($errors->all() as $error)
               <li>{{$error}}</li>
           @endforeach
       </ul>
    @endif
   ```
   - **BONUS**: Comprobar si un campo especifico de un form. tiene errores:
   ```blade
   {{$errors->has('title') ? 'has-error' : ''}}
   ```