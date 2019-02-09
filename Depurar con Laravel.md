# Depurar con Laravel

Para depurar proyectos Laravel, es recomendable utilizar el paquete **Laravel Telescope**.

Para instalar este paquete, seguimos estos pasos:

1. Instalanos el paquete con Composer: 

`composer require laravel/telescope --dev`

2. Instalamos los recursos de Telescope con este comando:

`php artisan telescope:install`

3. Ejecutamos las migraciones del paquete:

`php artisan migrate`

Ahora ya podemos mirar lo que ocurre en nuestro proyecto Laravel poniendo en el navegador la URL seguida de `telescope`. Ejemplo: `http://127.0.0.1:8000/telescope/`