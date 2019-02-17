# Mensajes Flash

Los mensajes flash son útiles para mostrar información al usuario **una única vez**, normalmente cuando ha rellenado un formulario de contacto o completado alguna acción.

En Laravel, para poder crear mensajes flash, hacemos lo siguiente:

```php
// ...
session()->flash('message', 'Project created successfully');
```

Para poder mostrar este mensaje en la vista Blade, hacemos lo siguiente:

```blade
@if(session('message'))
    {{session('message')}}
@endif
```