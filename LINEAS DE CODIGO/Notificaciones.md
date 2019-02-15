# Notificaciones

Las notificaciones de Laravel pueden ser útiles para avisar, por ejemplo, cuando su renovación de suscripción no ha sido procesado correctamente. 

Si nos fijamos en el modelo Eloquent llamado User (en `app/User.php`), hay un __trait__ llamado `Notifiable`:

```php
class User extends Authenticatable
{
    use Notifiable;
    // ...
}
```

