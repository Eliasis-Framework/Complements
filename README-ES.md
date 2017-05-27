# Módulos para Eliasis PHP Framework

[English version](README-ES.md)

---

- [Módulos disponibles](#módulos-disponibles)
- [Desarrollados con Eliasis](#desarrollados-con-eliasis)
- [Crear módulo](#crear-módulo)
- [Contribuir](#contribuir)
- [Copyright](#copyright)

---

## Módulos disponibles

| Módulo | Descripción |
| --- | --- |
| [fork-me-github](https://github.com/Eliasis-Framework/fork-me-github) | Agrega tira 'Fork me on GitHub' en la parte superior de la página. |

## Desarrollados con Eliasis

| Módulo | Descripción | Tipo
| --- | --- | --- |
| [copy-movie-grifus](https://github.com/Josantonius/Copy-Movie-Grifus.git) | Añade un nuevo botón en las páginas de películas del theme Grifus con el que podrás copiar la información completa de la película con un solo click. | [Plugin WordPress](https://github.com/Josantonius/Extensions-For-Grifus.git)
| [custom-images-grifus](https://github.com/Josantonius/Custom-Images-Grifus.git) | Guardas imágenes externas de IMDB en tu sitio de WordPress. Reemplaza imágenes externas de IMDB y las guarda en tu sitio de WordPress. | [Plugin WordPress](https://github.com/Josantonius/Extensions-For-Grifus.git)
| [custom-rating-grifus](https://github.com/Josantonius/Custom-Rating-Grifus.git) | Añade un nuevo botón en las páginas de películas del theme Grifus con el que podrás copiar la información completa de la película con un solo click. | [Plugin WordPress](https://github.com/Josantonius/Extensions-For-Grifus.git)

## Crear módulo

La estructura y configuración para crear un módulo básico para [Eliasis PHP Framework](https://github.com/eliasis-framework/eliasis) es la siguiente:

**Estructura**

**Crea un directorio con el nombre del módulo. Para este ejemplo nuestro módulo se llamará `my-custom-module`**:

    $ mkdir my-custom-module

**Accede al directorio creado**:

    $ cd my-custom-module/

**Crea los directorios del módulo**:

    $ mkdir assets/ assets/css config/ src/ src/Controller/ src/template/

### Contenido

**Crea el archivo de configuración del módulo**:

	$ nano my-custom-module.php 

```php
<?php
/**
 * Estados iniciales:
 * 
 * active      → El módulo se iniciará como activado.
 * inactive    → El módulo se iniciará como desactivado.
 * installed   → El módulo se iniciará como instalado.
 * uninstalled → El módulo se iniciará como desinstalado.
 */

return [
    
    'id'          => 'MyCustomModule',
    'name'        => 'My Custom Module',
    'version'     => '1.0.0',
    'description' => 'Custom module for Eliasis PHP Framework',
    'state'       => 'active',
    'category'    => 'extensions'
    'uri'         => 'https://github.com/Eliasis-Framework/my-custom-module',
    'author'      => 'Josantonius',
    'author-uri'  => 'https://josantonius.com/',
    'license'     => 'MIT',
];
```

**Crear archivo para añadir urls**:

    $ nano config/add-urls.php 

```php
<?php

use Eliasis\App\App,
    Eliasis\Module\Module;

$url = App::MODULES_URL() . Module::MyCustomModule('folder');

return [

    'url' => [

        'css'  => $url . 'public' . App::DS . 'css'      . App::DS,
        'view' => $url . 'src'    . App::DS . 'template' . App::DS . 'view' . App::DS,
    ],
];
```

**Crear archivos para añadir namespaces**:

    $ nano config/set-namespaces.php 

```php
<?php

$namespace = App::MyCustomApp('namespace', 'modules');

$module = 'MyCustomModule';

return [

    'namespace' => [

        'controller' => $namespace . $module . '\\Controller\\'
    ],
];

```

**Añade las rutas y/o los hooks de tu módulo**:

    $ nano config/set-hooks.php 

```php
<?php

use Eliasis\Module\Module;

$namespace = Module::MyCustomModule('namespace', 'controller');

/**
 * module-loa     → Hook opcional → Se ejecuta en cada carga del módulo.
 * activation     → Hook opcional → Se ejecuta cuando el módulo es activado.
 * deactivation   → Hook opcional → Se ejecuta cuando el módulo es desactivado.
 * installation   → Hook opcional → Se ejecuta cuando el módulo es instalado.
 * uninstallation → Hook opcional → Se ejecuta cuando el módulo es desinstalado.
 */

return [

    'hooks' => [
        'css'            => $namespace . 'Launcher\\Launcher@css',
        'after-body'     => $namespace . 'Launcher\\Launcher@render',
        'module-loa'     => $namespace . 'Launcher\\Launcher@init',
        'activation'     => $namespace . 'Launcher\\Launcher@activation',
        'deactivation'   => $namespace . 'Launcher\\Launcher@deactivation',
        'installation'   => $namespace . 'Launcher\\Launcher@installation',
        'uninstallation' => $namespace . 'Launcher\\Launcher@uninstallation',
    ],
];
```

    $ nano config/routes.php 

```php
<?php

use Eliasis\Module\Module;

$namespace = Module::MyCustomModule('namespace', 'controller');

return [

    'routes' => [

        'example' => $namespace . 'MyCustomModule' . '@example',
    ],
];
```

**Crea el archivo para el controlador principal del módulo**:

    $ nano src/Controller/Launcher/Launcher.php 

```php
<?php

namespace App\Modules\MyCustomModule\Controller\Launcher;

use Josantonius\Asset\Asset,
    Eliasis\Controller\Controller;

class Launcher extends Controller {

    public function init() {

        // Acciones para cuando se carga el módulo.
    }

    public function activation() {

        // Acciones para cuando se activa el módulo.
    }

    public function deactivation() {

        // Acciones para cuando se desactiva el módulo.
    }

    public function installation() {

        // Acciones para cuando se instala el módulo.
    }

    public function uninstallation() {

        // Acciones para cuando se desintala el módulo.
    }

    public function css() {

        Asset::css(Module::MyCustomModule('url', 'css') . 'style.css');
    }

    public function render() {

        $path = Module::MyCustomModule('url', 'view');

        self::$view->renderizate($path . 'hello');
    }
}
```

**Crea el archivo para los estilos**:

	$ nano assets/css/style.css

```css
.color {
	color: red;
}
```

**Crea el archivo para la vista**:

	$ nano src/template/view/hello.php

```html
<p class="color">Hello world</p>
```

**Finalmente crea el archivo de configuración para poder instalar tu plugin desde Composer**:

	$ nano composer.json

```json
{
    "name":        "eliasis-framework/my-custom-module",
    "version":     "1.0.0",
    "description": "Custom module for Eliasis PHP Framework",
    "type": "eliasis-module",
    "license": "MIT",
    "require": {
        "composer/installers": "*",
        "Josantonius/Asset": "*",
        "Eliasis-Framework/Module": "*"
    },
    "autoload": {
        "psr-4": {
            "App\\Modules\\MyCustomModule\\" : "/src"
        }
    }
}

```

### Contribuir
1. Comprobar si hay incidencias abiertas o abrir una nueva para iniciar una discusión en torno a un fallo o función.
1. Bifurca la rama del repositorio en GitHub para iniciar la operación de ajuste.
1. Escribe una o más pruebas para la nueva característica o expón el error.
1. Haz cambios en el código para implementar la característica o reparar el fallo.
1. Envía pull request para fusionar los cambios y que sean publicados.

Esto está pensado para proyectos grandes y de larga duración.

### Copyright

2017 Josantonius, [josantonius.com](https://josantonius.com/)

Si te ha resultado útil, házmelo saber :wink:

Puedes contactarme en [Twitter](https://twitter.com/Josantonius) o a través de mi [correo electrónico](mailto:hello@josantonius.com).