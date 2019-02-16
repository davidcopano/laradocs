# Frontend

Por defecto, Laravel trae una serie de utilidades para los estilos/scripts del Frontend. En el archivo `package.json` podemos lo que trae.

Para instalar las dependencias del fichero `package.json`, ejecutamos el siguiente comando:

**NOTA**: Antes de ejecutar, debemos asegurarnos que en el archivo `package.json` las dependencias `vue` y `vue-template-compiler` deben tener **LA MISMA VERSIÓN** para que funcione correctamente.

`npm install`

Entre estas dependencias, encontramos **laravel-mix**. Esta utilidad nos facilita compilar todas las dependencias .js y css en un archivo, respectivamente. Podemos encontrar la configuración de esta herramienta en el archivo `webpack.mix.js`:

```javascript
const mix = require('laravel-mix');

// ...

mix.js('resources/js/app.js', 'public/js')
   .sass('resources/sass/app.scss', 'public/css');
```

Si abrimos el archivo `resources/js/app.js`, podemos ver por defecto carga Bootstrap (en el archivo `resources/js/bootstrap.js`) y Vue. Para Vue, podemos hacer que nos cargue TODOS los componentes `.vue` que creemos en la carpeta `resources/js/components/`:

```javascript
// ...

// DESCOMENTAR ESTO DE ABAJO PARA QUE COMPILE Y PONGA 
// EN UN ÚNICO ARCHIVO .js NUESTROS COMPONENTES .vue

// const files = require.context('./', true, /\.vue$/i)
// files.keys().map(key => Vue.component(key.split('/').pop().split('.')[0], files(key).default))

Vue.component('example-component', require('./components/ExampleComponent.vue').default);
```

Una vez instaladas las dependencias con el comando `npm install`, podemos ejecutar una serie de comandos para que nos compile el .js y los .scss en función cómo queramos compilarlo:

- `npm run dev`: Compila los .js y .scss en modo de desarrollo
- `npm run watch`: Lo mismo que lo anterior, pero compila automáticamente cuando cambiamos el contenido de un archivo .js o .scss
- `npm run prod`: Compila los .js y .scss en modo de producción, esto minificará nuestros archivos para que pesen menos los archivos resultantes.