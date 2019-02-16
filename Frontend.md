# Frontend

Por defecto, Laravel trae una serie de utilidades para los estilos/scripts del Frontend. En el archivo `package.json` podemos lo que trae.

Para instalar las dependencias del fichero `package.json`, ejecutamos el siguiente comando:

`npm install`

Entre estas dependencias, encontramos **laravel-mix**. Esta utilidad nos facilita compilar todas las dependencias .js y css en un archivo, respectivamente. Podemos encontrar la configuración de esta herramienta en el archivo `webpack.mix.js`:

```javascript
const mix = require('laravel-mix');

// ...

mix.js('resources/js/app.js', 'public/js')
   .sass('resources/sass/app.scss', 'public/css');
```

Si abrimos el archivo `resources/js/app.js`, podemos ver por defecto carga Bootstrap (en el archivo `resources/js/bootstrap.js`) y Vue. Para Vue, podemos hacer que nos cargue TODOS los componentes `.vue` que creemos en la carpeta `resources/js/components/`.