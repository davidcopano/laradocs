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

3. Lo siguiente será crear un Listener. Este será el encargado de manejar el evento. Para crear un Listener para este caso ejecutamos lo siguiente:

`php artisan make:listener SendProjectCreatedNotification --event=ProjectCreated`

Se creará un archivo (en este caso `SendProjectCreatedNotification.php`) en el directorio `app/Listeners`.

4. Con el Listener ya creado, ya podemos abrir el archivo (ubicado en `app/Listeners/SendProjectCreatedNotification.php`) y hacer lo que queramos cuando se produzca el evento. En este caso, enviaremos un email al usuario que ha creado un proyecto:

```php
<?php

namespace App\Listeners;

use App\Events\ProjectCreated;
use App\Mail\ProjectCreated as ProjectCreatedMail;
use Illuminate\Support\Facades\Mail;

class SendProjectCreatedNotification
{
    /**
     * Create the event listener.
     *
     * @return void
     */
    public function __construct()
    {
        //
    }

    /**
     * Handle the event.
     *
     * @param  ProjectCreated  $event
     * @return void
     */
    public function handle(ProjectCreated $event)
    {
        Mail::to($event->project->owner->email)->send(
            new ProjectCreatedMail($event->project)
        );
    }
}
```