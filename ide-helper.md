# Ide Helper

Este plugin provee autocompletado en IDEs como PHPStorm, etc.

## Instalación

Ejecuta este comando en el directorio raíz del proyecto:

`composer require --dev barryvdh/laravel-ide-helper`

## Uso

Ejecuta estos comandos para tener los siguientes autocompletados:

- Autocompletado básico:

`php artisan ide-helper:generate`

- Autocompletado para modelos:

Para este autocompletado debemos instalar lo siguiente mediante Composer:

`composer require --dev doctrine/dbal`

Una vez instalado este paquete, ya podemos generar autocompletado de modelos con el siguiente comando:

`php artisan ide-helper:models`

- Autocompletado para los métodos Fluent de Laravel:

Hay veces que el autocompletado no funciona (por ejemplo al definir una foreign key en un archivo de migración). Para tener este autocompletado hacemos lo siguiente:

1. Crear un archivo llamado `ide-helper.php` dentro de la carpeta `config` del proyecto.

2. Poner este código en el archivo que hemos creado antes:

```php
<?php
return [
  'include_fluent' => true
];
```

3. Ejecutar el siguiente comando:

`php artisan ide-helper:generate`