# Líneas de código interesantes para plantillas Blade


- Llamar a método PATCH/DELETE etc. en un formulario:

```
@method('DELETE')
```

- Comprobar errores en un formulario:

    - En el PHP, validamos los campos recibidos del form. 
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
   - **BONUS**: Comprobar si un campo específico de un form. tiene errores:
   ```blade
   {{$errors->has('title') ? 'has-error' : ''}}
   ```

- Incluir plantillas Blade:
    - Si tenemos este archivo, llamado `errors.blade.php`:
    ```blade
    @if($errors->any())
        <div class="errores">
            <ul>
                @foreach($errors->all() as $error)
                    <li>{{$error}}</li>
                @endforeach
            </ul>
        </div>
    @endif
    ```   
    - Lo llamamos así en otra plantilla  
    ```blade
    @include('errors')
    ```

- Diferenciar si un usuario está logeado en la web:
    - `@guest` es cuando el usuario no está logeado (funciona como `@if`, `@else` y `@endif`)
    - `route('login')` genera una ruta con el nombre pasado
    - `Auth::user()->name` muestra datos del usuario logeado (el nombre en este caso)
    ```blade
    @guest
        <a href="{{route('login')}}">Iniciar sesión</a>
    @else
        <p>Hola, {{Auth::user()->name}}</p>
    @endguest
    ```

- Enlazar un archivo CSS o JS:

```blade
<link rel="stylesheet" href="{{asset('css/app.css')}}">
<script src="{{asset('js/app.js')}}"></script>
```

- Hay veces que al poner `@section` y `@endsection` se producen espacios/saltos de línea:

![https://i.imgur.com/uo0Ro1h.png](https://i.imgur.com/uo0Ro1h.png)

![https://i.imgur.com/4plYmi5.png](https://i.imgur.com/4plYmi5.png)

Para solucionar esto, le pasamos a `@section` como 2do parámetro el texto que queramos:

```blade
@section('body_class', 'index')
```

Con esto ya se solucionan los espacios en blanco que se producían antes:

![https://i.imgur.com/eiHEvrk.png](https://i.imgur.com/eiHEvrk.png)

- Convertir variable a JSON:

```blade
<script>
    var projects = @json($projects);
</script>
```
