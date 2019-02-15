# Notificaciones

Las notificaciones de Laravel pueden ser útiles para avisar, por ejemplo, cuando su renovación de suscripción no ha sido procesado correctamente. En este caso, la notificación puede llegar por varios canales: email, mensaje SMS, una llamada telefónica, Slack, etc.

Si nos fijamos en el modelo Eloquent llamado User (en `app/User.php`), hay un __trait__ llamado `Notifiable`:

```php
class User extends Authenticatable
{
    use Notifiable;
    // ...
}
```

Para crear una notificación de Laravel, hacemos lo siguiente:

1. Ejecutar este comando para crear la notificación (para este caso, será para avisar a un usuario de una renovación de suscripción no procesada correctamente):

`php artisan make:notification SubscriptionRenewalFailed`

2. El comando de arriba habrá creado un archivo llamado `SubscriptionRenewalFailed.php`, ubicado en `app/Notifications`. Si abrimos este archivo, podemos observar que, por defecto, se podrá avisar vía email:

```php
// ...
public function via($notifiable)
{
    return ['mail'];
}

// ...
public function toMail($notifiable)
{
    return (new MailMessage)
                ->line('The introduction to the notification.')
                ->action('Notification Action', url('/'))
                ->line('Thank you for using our application!');
}
```

Esta última función (`toMail()`), podemos editarla de las siguientes formas:

```php
// ...
public function toMail($notifiable)
{
    return (new MailMessage)
                // asunto del email
                ->subject('Renovación de suscripción fallida')

                ->line('The introduction to the notification.')

                // texto del botón de acción (es un enlace, se le 
                // pasa la URL que queramos) en el segundo parámetro
                ->action('Añadir nueva tarjeta de crédito', url('/'))
                ->line('Thank you for using our application!');
}
```

3. Hay ocasiones en las que debemos enviar una notificación, por ejemplo dentro de la página web (en el panel principal del usuario). Si es así, debemos añadir un nuevo canal de envío en el método `via($notifiable)`:

```php
// ...
public function via($notifiable)
{
    return ['mail', 'database'];
}
```

Si intentamos ejecutar el método de abajo (`->notify(...)`) nos dará el siguiente error:

![alt text](https://i.imgur.com/ftUeFdT.png)

Es porque no tenemos una tabla/modelo creado en la BBDD. Para arreglar esto, ejecutamos este comando:

`php artisan notifications:table`

Esto nos creará una migración para esta tabla de notificaciones. Para completar la creación de esta tabla ejecutamos el comando:

`php artisan migrate`

**NOTA**: Podremos acceder a las notificaciones del usuario con `$user->notifications`, o `auth()->user()->notifications`.

4. Una vez hemos editada nuestra notificación anterior, podemos enviar esta notificación de la siguiente forma:

```php
auth()->user()->notify(new SubscriptionRenewalFailed());
```