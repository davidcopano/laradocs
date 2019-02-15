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