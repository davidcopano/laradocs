# Reglas de validación de campos

En el siguiente enlace se listan las diferentes reglas de validación de campos enviados: https://laravel.com/docs/5.8/validation#available-validation-rules

Podemos utilizar estas reglas de la siguiente forma:

```php
$validatedData = request()->validate([
    'email' => 'required|email',
    'subject' => 'required|max:100',
    'description' => 'required|max:255'
]);
```