# Guía de desarrollo de apps en horbito

Bienvenido a la guia para el desarrollo para horbito. A continuación se explicarán todos los puntos necesarios para realizar una aplicación para la plataforma.

El objetivo de esta guía está centrado únicamente en el desarrollo en la parte de cliente (navegador web) y con lenguajes HTML, CSS y JavaScript. Las funcionalidades dependientes de desarrollo sobre servidor quedan excluidas de este documento.

Actualmente si se quiere programar una aplicación se nos debe solicitar la creación de la misma para que pueda iniciarse el desarrollo.

## Acceso a los ficheros de la app

A partir de este momento usted podrá comenzar a subir codigo a la aplicación a traves de un cliente de SFTP (File Transfer Protocol over SSH). Recomendamos usar cualquiera de estos clientes:

* FileZilla (Windows, Mac y Linux)
* Cyberduck (Windows y Mac)
* Transmit (Mac)

Es posible que otros clientes sean compatibles con nuestro servicio de SFTP pero no garantizamos que la funcionalidad sea plena.

Para conectarse al servidor de SFTP use los siguientes datos de conexión:

* **Protocolo**: SFTP
* **Servidor**: `sftp.horbito.com`
* **Usuario**: Su nombre de usuario de horbito
* **Contraseña**: Su contraseña de horbito
* **Puerto**: `22`

Al acceder encontrará la carpeta `/apps` donde se listan todas las apps de las que es desarrollador.

## Estructura de los ficheros

Inicialmente la aplicación que viene como ejemplo es Feedback, cuya estructura de ficheros es muy sencilla y fácil de entender, ideal para empezar un proyecto nuevo.

Dentro de la carpeta de aplicación podremos encontrarnos con varios archivos:

```
info.json
main.html

language/en.json
language/es.json

static/launchpad.png
static/launchpad@2x.png
static/taskbar.png
static/taskbar@2x.png
static/script.js
static/style.css
```

* `info.json` es el fichero donde se define la configuración de la aplicación, sus ventanas, widgets... Lo explicamos con más detenimiento más adelante en esta guía.
* Los ficheros `.html` contienen la estructura de las ventanas y widgets.
* El directorio `language` contiene los archivos de idioma. Se explica más adelante.
* El directorio `static` contiene las imagenes, scripts, ficheros de CSS... En definitiva todos los ficheros que deban ser accesibles desde el navegador vía URL. Si desea mantener archivos privados (documentación, backups y similares) puede mantenerlos fuera del alcance del usuario siempre y cuando estén fuera de esta carpeta.

Los ficheros dentro de la app deben distribuirse de manera explicada para poder operar correctamente.

### `info.json`

El fichero `info.json` es el centro de configuración de la aplicación. Explicaremos a continuación que función tiene cada parte fraccionando el código del mismo.

#### `name`
Indica cual es el nombre de la aplicación. Puede ser un `String` para que la app se llame igual en cualquier idioma o un objeto (como en el ejemplo) donde se especifica una traducción. El atributo `main` es el nombre de la app por defecto en caso de que no exista una traducción especificada para un idioma.

```js
{

  "name" : {
    "main" : "Feedback",
    "en"   : "Feedback",
    "es"   : "Sugerencias"
  },
```

#### Versionado
Indica la versión de la app, su nivel de estabilidad y que versión del API debe usar. Actualmente estos 3 valores pueden ser ignorados dado que no se le da un uso.

```js
  "version" : "1.0",
  "stage"   : "beta",
  "api"     : "a100d2",
```

#### Idiomas
Indica cual es el idioma por defecto que se usará en caso de que no exista una traducción específica para el idioma del usuario. En el ejemplo puede ignorarse el atributo `list` dado que no se le da uso.

```js
  "language" : {
    "default" : "en-en",
    "list"    : [ "en-en" ]
  },
```

#### Ventanas y widgets
La declaración de que ventanas y widgets tiene la aplicación está sujeta a los dispositivos que queramos soportar. En este ejemplo la app solo es usable en `desktop`. En caso de querer desarrollar una app para horbito mobile, por favor, contacte previamente con el equipo de desarrollo.

Centrándonos en el ejemplo, existen 3 secciones:

##### Configuración general
Aquí se especifica cual es la ventana que se abre por defecto al lanzarse la app (en este caso la llamada `main`), los scripts que se ejecutan en segundo plano aunque la app no esté abierta (aquí llamados como `service`) y los estilos y scripts que comparten (si es deseado) todas las ventanas y widgets de la app.

```js
  "device" : {

    "desktop" : {

      "primary" : "main",
      "service" : [],
      "common"  : {
        "css"    : [],
        "script" : [],
        "start"  : []
      },
```

##### Ventanas
También llamadas como Vistas. Cada ventana tiene un nombre (elegible por el desarrollador). En nuestro ejemplo la ventana principal `main` está definida por un HTML, un CSS y un script. Igualmente se pueden configurar ciertas restricciones de la ventana como su capacidad de ser redimensionada o los tamaños que puede tener.

```js
      "view" : {

        "main" : {

          "template" : "main.html",

          "maxizable" : false,
          "resizable" : false,

          "width"  : {
            "default" : 565,
            "min"     : 0,
            "max"     : 0
          },

          "height" : {
            "default" : 500,
            "min"     : 0,
            "max"     : 0
          },

          "css"    : ["style.css"],
          "script" : ["script.js"]

        }

      }

    }

  },
```

##### Widgets
Si bien en el `info.json` de ejemplo no viene un widget definido, aquí tenemos un ejemplo de declaración de widget (debe declararse al mismo nivel que el atributo `view`).

```js
"widget" : {

    "register" : {

      "template" : "register.html",
      "css"      : ["register.css"],
      "script"   : ["register.js"],

      "alwaysInFront" : true

    }

  }
```

La estructura es mucho más simple que la de una ventana. Solo hay un atributo que permite especificar si se desea mostrar siempre sobre el resto de la interfaz (por ejemplos para situaciones donde el widget es urgente que sea atendido).

Actualmente un widget no se "autoregistra" en la cuenta de un usuario por lo que no se muestra automáticamente. Una vez declarado en el `info.json` debe contactarse con el equipo de desarrollo para que habilitemos el widget en la cuenta del usuario. Somos conscientes de lo poco funcional que es este caso, estamos trabajando para que esto sea un paso automático.

##### Permisos
En ocasiones para poder acceder desde JavaScript a alguna de nuestras APIs se requiere solicitar el permiso en `info.json`. En este caso se activa la opción de poder enviar mensajes al equipo de soporte a través de Feedback API.

```js
  "permission" : {
    "feedback" : true
  }

}
```

### Idiomas

### Estilos

### Scripts

### Imágenes

## App API

El App API tiene los siguientes métodos:

### `api.app.blurAllViews()`
### `api.app.close()`
### `api.app.createView()`

* [undefined] {mixed} - params Parameters for the new view, can be anything. Views can receive parameters from the system or other apps to communicate with one another.
* [{}] {object} - options Sets several options of the view.","If the argument is a string it will be considered the view type.","If the argument is an object it can have any of these attributes:

> * {bool} animation: Create the view with the initial animation. If it is not defined if will use the info.json's animation configuration. If no configuration is found in info.json, the default value is `true`.
> * {bool} focus: Give focus to the new view. Defaults to `false`.
> * {int} height: Height in pixels of the view. It will use info.json's configuration if not explictly defined.
> * {int} left: Horizontal position in pixels of the view, starting from the left of the desktop (hole Inevio environment except the taskbar). If it's not defined, horizontal position is calculated to center the view.
> * {int} maxHeight: Maximum height in pixels of the view. If it is not defined it will use the default view's maximum height in the info.json's configuration.
> * {int} maxWidth: Maximum width in pixels of the view. If it is not defined it will use the default view's maximum width in the info.json's configuration.
> * {int} minHeight: Minimum height in pixels of the view. If it is not defined it will use the default view's minimum height in the info.json's configuration.
> * {int} minWidth: Minimum width in pixels of the view. If it is not defined it will use the default view's minimum width in the info.json's configuration.
> * {int} top: Vertical position in pixels of the view, starting from the left of the desktop. If it's not defined, vertical position is calculated to center the view.
> * {string} type: Type for the view. If the type does not exist it will be redefined with the primary view type.
> * {int} width: Width in pixels of the view. If it is not defined it will use the default view's width in the info.json's configuration.

```js
api.app.createView( params, nameOfTheView )
```

### `api.app.createWidget()`
### `api.app.getAll()`
### `api.app.getBadge()`
### `api.app.getViews()`
### `api.app.getWidgets()`
### `api.app.maximizeView()`
### `api.app.minimizeView()`
### `api.app.openApp()`
### `api.app.removeStorage()`
### `api.app.removeView()`
```js
api.app.removeView( $(this) )
```

### `api.app.restoreView()`
### `api.app.setBadge()`
### `api.app.unmaximizeView()`
### `api.app.viewToFront()`

## WQL API

## Prácticas recomendadas

#### CSS

Los estilos serán procesados con LESS antes de ser servidos. Si desea mantener las instrucciones `calc` debe escaparlas:

```css
.estilo{
  width: calc(100% - 41px); // Sin escapar, será procesada y convertida a un valor en píxeles fijo
  width: ~'calc(100% - 41px)'; // Escapada, se enviará al navegador sin ser modificada
}
```

Igualmente se recomienda no hacer uso de IDs de CSS (selector `#`) y tampoco en el HTML dada la posibilidad de existir varias ventanas de la misma app en el escritorio. Si bien no debería ser un problema en algunos navegadores el comportamiento puede ser impredecible.

#### JavaScript

Las ventanas de las aplicaciones funcionan en un entorno aislado del resto de aplicaciones para no generar interferencias.

Dentro de la ventana se tiene acceso a una versión de jQuery que funciona únicamente bajo esa ventana, es decir, no se corre el riesgo de manipular otras apps.

La variable `this` en el script hace referencia al elemento del DOM de la ventana.

Algunas variables como `window` y `document` están deshabilitadas por seguridad. Si se necesita tener acceso a alguna (por ejemplo, para dar soporte a scripts de terceros que se usa) añada al principio de su script estas dos líneas.

```js
var window = $(this).parents().slice(-1)[ 0 ].parentNode.defaultView
var document = window.document
```
