# Eventos

Los eventos son útiles para **realizar** una acción cuando se ha **producido** otra acción. Para crear un evento hacemos lo siguiente:

1. Ejecutar el siguiente comando para crear la estructura del evento:

`php artisan make:event ProjectCreated`

2. Se habrá creado un archivo (en este caso `ProjectCreated.php`) en la ruta `app/Events`. La estructura con la que se crea en principio es compleja. Se puede reducir a este archivo:

```php
<?php

namespace App\Events;

use Illuminate\Queue\SerializesModels;
use Illuminate\Foundation\Events\Dispatchable;

class ProjectCreated
{
    use Dispatchable, SerializesModels;

    /**
     * Create a new event instance.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }
}
```