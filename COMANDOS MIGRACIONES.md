# Comandos para las migraciones

- Borrar **TODAS las tablas** y ejecutar todas las migraciones:

`php artisan migrate:fresh`

- Deshacer **TODAS las migraciones** y **VOLVER** a ejecutarlas:

`php artisan migrate:refresh`

- Deshacer la **ÚLTIMA** migración:

`php artisan migrate:rollback`

- Deshacer **TODAS** las migraciones:

`php artisan migrate:reset`