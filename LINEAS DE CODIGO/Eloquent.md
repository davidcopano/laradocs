# Líneas de código para modelos Eloquent

- Obtener registros según un campo determinado:

```php
$projects = Project::where('owner_id', auth()->id())->get();
```

- Obtener **todos** los registros:

```php
$projects = Project::all();
```