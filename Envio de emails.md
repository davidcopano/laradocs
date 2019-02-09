# Envío de emails con Laravel

**NOTA**: Debemos tener configurados los parámetros de email (en el archivo `.env` son los parámetros `MAIL_DRIVER`, `MAIL_HOST`, etc) para que se envíen los email correctamente.

Para enviar emails siguiendo la convención de Laravel, debemos hacer lo siguiente:

1. Ejecutamos el siguiente comando, esto creará la clase de Mail que le pongamos y nos creará una vista Blade en el directorio que especifiquemos (`mail` en este caso) para poder poner el contenido del correo:

`php artisan make:mail ProjectCreated --markdown="mail.project-created"`

2. Si queremos pasarle parámetros al email (por ejemplo del proyecto que se ha creado), debemos hacer lo siguiente en nuestra clase de email creada (ubicada en `app/Mail/ProjectCreated.php` para este caso):

```php
class ProjectCreated extends Mailable
{
    // ...

    public $project;

    public function __construct($project)
    {
        $this->project = $project;
    }
}
```

3. Editamos nuestra vista Blade creada antes para poder mostrar, en este caso, el proyecto creado:

```blade
@component('mail::message')
# New Project: {{$project->title}}

The body of your message.

@component('mail::button', ['url' => url('/projects/' . $project->id)])
View Project
@endcomponent

Thanks,<br>
{{ config('app.name') }}
@endcomponent
```

4. Una vez editada la vista Blade generada antes, enviamos el mail con Laravel de la siguiente forma:

```php
$project = Project::create($validatedAttributes);

\Mail::to('johndoe@example.com')->send(
    new ProjectCreated($project)
);
```