# Comandos interesantes

- Crear un controlador que te genere los métodos siguiendo la convención REST de Laravel:

```
php artisan make:controller NombreController -r
```

Para +info, ejecutar el comando `php artisan route:list`

- Generar modelo, junto con una nueva migración y [factory](https://laravel.com/docs/5.7/seeding#using-model-factories) para el mismo:

```
php artisan make:model NombreModelo -m -f
```