# Comandos interesantes

- Crear un nuevo archivo de migración (para una nueva tabla en la BBDD):

```
php artisan make:migration
```

- Crear un controlador que te genere los métodos siguiendo la convención REST de Laravel:

```
php artisan make:controller NombreController -r
```

Para +info, ejecutar el comando `php artisan route:list`

- Generar modelo, controlador y migración **en un solo comando**:

```
php artisan make:model NombreModelo -mcr
```

- Generar sistema de autenticación (inicio de sesión/registro, con sus rutas, vistas Blade, etc):

```
php artisan make:auth
```