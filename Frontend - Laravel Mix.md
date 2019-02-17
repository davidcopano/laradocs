# Frontend

Por defecto, Laravel trae una serie de utilidades para los estilos/scripts del Frontend. En el archivo `package.json` podemos ver lo que trae.

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

Para enlazar estos archivos en nuestra vista Blade, utilizamos la función `asset`:

```blade
<link rel="stylesheet" href="{{asset('css/app.css')}}">
<script src="{{asset('js/app.js')}}"></script>
```

**NOTA**: Es recomendable __extraer__ y __versionar__ nuestros scripts y estilos para evitar problemas de caché en el navegador. Para conseguir esto, abrimos el archivo `webpack.mix.js` y ponemos lo siguiente:

```javascript
mix.js('resources/js/app.js', 'public/js')
    .sass('resources/sass/app.scss', 'public/css')
    // extrae las dependencias JS (jquery, etc) a un archivo 'vendor.js'
    // también crea un archivo 'manifest-js' propio de webpack
    .extract()

    // versiona los archivos .js y .css
    .version();
```

El problema que tiene __versionar__ estos archivos es que no podemos ponerlos a pelote en nuestra vista Blade, ya que al cambiar el contenido de un archivo .js/.scss el __hash__ cambia. Para enlazar estos archivos, ponemos lo siguiente en nuestra vista Blade:

```blade
{{-- la función mix() nos pone el archivo con el hash ya añadido --}}
<link rel="stylesheet" href="{{mix('css/app.css')}}">

{{-- ... --}}
<script src="{{mix('js/app.js')}}"></script>

{{-- DEBEMOS ENLAZAR ESTOS ARCHIVOS TAMBIÉN, SI NO, NO FUNCIONARÁ EL ARCHIVO app.js --}}
<script src="{{asset('js/manifest.js')}}"></script>
<script src="{{asset('js/vendor.js')}}"></script>
```

Para utilizar los componentes de Vue en la plantilla Blade, cogemos el `id` que se ha puesto como elemento raíz Vue en el archivo `resources/js/app.js`:

```javascript
// ...
const app = new Vue({
    el: '#app'
});
```

Y lo ponemos en el elemento que queramos en nuestra vista Blade:

```blade
<div id="app">
    ...
    <example-component></example-component>
</div>
```