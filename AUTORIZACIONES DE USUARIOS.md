# Autorizaciones de usuarios para realizar acciones.

En muchas ocasiones, debemos restringir las acciones del usuario para que solo pueda hacerlas sobre sus cosas (por ejemplo, un usuario **solo** puede ejecutar acciones sobre sus proyectos). Laravel ofrece las llamadas **policies** para resolver esto.

Para llevar a cabo este ejmplo, debemos hacer lo siguiente:

1. Ejecutar el siguiente comando para crear la __policy__:

`php artisan make:policy ProjectPolicy --model=Project`

2. Abrir el archivo (generado en `app/Policies/`). Se mostrarán todas las acciones que puede realizar el usuario (`view`, `create`, etc). En el método que queramos

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