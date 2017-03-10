# Eliasis PHP Framework modules

[English version](README-ES.md)

---

- [Módulos disponibles](#módulos-disponibles)
- [Crear módulo](#crear-módulo)
- [Contribuir](#contribuir)
- [Copyright](#copyright)

---

### Módulos disponibles

| Módulo | Descripción |
| --- | --- |
| [fork-me-github](https://github.com/Eliasis-Framework/fork-me-github) | Agrega tira 'Fork me on GitHub' en la parte superior de la página. |

### Crear módulo

La estructura y configuración para crear un módulo básico para [Eliasis PHP Framework](https://github.com/eliasis-framework/eliasis) es la siguiente:

**Estructura**

· Crea un directorio con el nombre del módulo. Para este ejemplo nuestro módulo se llamará `my-custom-module`:

    $ mkdir my-custom-module

· Accede al directorio creado:

    $ cd my-custom-module/

· Crea los directorios del módulo:

/assets/ 			[opcional]
/assets/js  		[opcional]
/assets/css 		[opcional]

/config/ 			[opcional]

/src/  				[requerido]
/src/Model/  		[opcional]
/src/template/		[opcional]
/src/template/view/ [opcional]
/src/Controller/ 	[opcional]

    $ mkdir assets/ assets/css assets/js config/ src/ src/Controller/ src/Model/  src/template/

**Contenido**

· Crea el archivo de configuración del módulo.

	$ nano my-custom-module.php 

```php
<?php

return [

    'name'        => 'MyCustomModule',
    'version'     => '1.0.0',
    'description' => 'Custom module for Eliasis PHP Framework',
    'uri'         => 'https://github.com/Eliasis-Framework/my-custom-module',
    'author'      => 'Josantonius',
    'author-uri'  => 'https://josantonius.com/',
    'license'     => 'MIT',

];
```

· Añade las rutas o los hooks de tu módulo.

	$ nano src/Hooks.php 

```php
<?php

namespace App\Modules\MyCustomModule;

use Eliasis\Hook\Hook,
    Eliasis\Module\Module;

class Hooks {

    public static function add() {

        $controller = Module::MyCustomModule('getNamespace', 'controller');

        $hooks = [

            'css' => $controller . 'MyCustomModule' . '@css',

        ];

        Hook::addHook($hooks);
    }
}
```

	$ nano src/Routes.php 

```php
<?php

namespace App\Modules\MyCustomModule;

use Eliasis\Route\Route,
    Eliasis\Module\Module;

class Routes {

    public static function add() {

        $controller = Module::MyCustomModule('getNamespace', 'controller');

    	$routes = [

            '/' => $controller . 'MyCustomModule' . '@render',

        ];

        Route::addRoute($routes);
    }
}
```

· Crea el archivo para el controlador principal del módulo.

	$ nano src/Controller/MyCustomModule.php 

```php
<?php

namespace App\Modules\ForkMeGitHub\Controller;

use Josantonius\Asset\Asset,
    Eliasis\Module\Module,
    Eliasis\Controller\Controller;

class ForkMeGitHub extends Controller {

    public function css() {

        Asset::css(Module::ForkMeGitHub('getUrl', 'css') . 'style.css');
    }

    public function render() {

        $path = Module::ForkMeGitHub('getPath', 'view');

        self::$view->renderizate($path . 'hello');
    }
}
```

· Crea el archivo para los estilos.

	$ nano assets/css/style.css

```css
.color {
	color: red;
}
```

· Crea el archivo para la vista.

	$ nano src/template/view/hello.php

```html
<p class="color">Hello world</p>
```

· Finalmente crea el archivo de configuración para poder instalar tu plugin desde Composer.

	$ nano composer.json

```json
{
    "name":        "eliasis-framework/my-custom-module",
    "version":     "1.0.0",
    "description": "Custom module for Eliasis PHP Framework",
    "type": "eliasis-module",
    "license": "MIT",
    "require": {
        "eliasis-framework/installers": "*"
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

### Copyright

2017 Josantonius, [josantonius.com](https://josantonius.com/)

Si te ha resultado útil... ¡házmelo saber! Sígueme en [Twitter](https://twitter.com/Josantonius).