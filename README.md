# Eliasis PHP Framework modules

[Spanish version](README-ES.md)

---

- [Available modules](#available-modules)
- [Create module](#create-module)
- [Contribute](#contribute)
- [Copyright](#copyright)

---

## Available modules

| Module | Description|
| --- | --- |
| [fork-me-github](https://github.com/Eliasis-Framework/fork-me-github) | Add a strip 'Fork me on GitHub' at the top of the page. |

## Create module

The structure and configuration to create a basic module for [Eliasis PHP Framework](https://github.com/eliasis-framework/eliasis) is the next:

### Skeleton

**Create a directory with the module name. For this example our module will be called `my-custom-module`**:

    $ mkdir my-custom-module

**Access the created directory**:

    $ cd my-custom-module/

**Create module directories**:

    $ mkdir assets/ assets/css config/ src/ src/Controller/ src/template/

### Content

**Create module configuration file**:

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

**Add the routes or hooks of your module**:

	$ nano config/hooks.php 

```php
<?php

use Eliasis\Module\Module;

$controller = Module::getNamespace('controller');

return [

    'hooks' => [
        'css'        => $controller . 'MyCustomModule' . '@css',
        'after-body' => $controller . 'MyCustomModule' . '@render',
    ],
];
```

	$ nano config/routes.php 

```php
<?php

use Eliasis\Module\Module;

$controller = Module::getNamespace('controller');

return [

    'routes' => [

        'example' => $controller . 'MyCustomModule' . '@example',
    ],
];
```

**Create the main file of the module**:

	$ nano src/Controller/MyCustomModule.php 

```php
<?php

namespace App\Modules\MyCustomModule\Controller;

use Josantonius\Asset\Asset,
    Eliasis\Module\Module,
    Eliasis\Controller\Controller;

class MyCustomModule extends Controller {

    public function css() {

        Asset::css(Module::MyCustomModule('getUrl', 'css') . 'style.css');
    }

    public function render() {

        $path = Module::MyCustomModule('getPath', 'view');

        self::$view->renderizate($path . 'hello');
    }
}
```

**Create the file for styles**:

	$ nano assets/css/style.css

```css
.color {
	color: red;
}
```

**Create the view file**:

	$ nano src/template/view/hello.php

```html
<p class="color">Hello world</p>
```

**Finally create the configuration file to be able to install your plugin from Composer**:

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
        "Josantonius/Asset": "*"
    },
    "autoload": {
        "psr-4": {
            "App\\Modules\\MyCustomModule\\" : "/src"
        }
    }
}

```

## Contribute
1. Check for open issues or open a new issue to start a discussion around a bug or feature.
1. Fork the repository on GitHub to start making your changes.
1. Write one or more tests for the new feature or that expose the bug.
1. Make code changes to implement the feature or fix the bug.
1. Send a pull request to get your changes merged and published.

This is intended for large and long-lived objects.

## Copyright

2017 Josantonius, [josantonius.com](https://josantonius.com/)

If you found this release useful please let the author know! Follow on [Twitter](https://twitter.com/Josantonius).