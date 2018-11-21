---
description: >-
  Cualquier módulo, una vez instalado en una tienda, puede interactuar con sus
  hooks. Los hooks te permiten enganchar tu código a la vista actual en el
  momento del parseado del código (por ejemplo, cuan
---

# Primer módulo

## Principales operaciones con módulos

Pueden mostrar una gran variedad de contenido \(bloques, textos...\), ejecutar algunas tareas \(actualización en bloque, importar, exportar, etc...\), interactuar con otras herramientas, y mucho más.

Pueden ser tan configurables como sea necesario; cuanto más lo sea, más fácil será de usar, y más usuarios lo utilizarán.

Una de las principales ventajas de un módulo es añadir funcionalidades a PS sin tener que modificar el core, permitiendo actualizaciones.

## Estructura de archivos

### Archivos obligatorios

* **nombre\_del\_modulo.php**: Es el archivo principal. Debe tener el mismo nombre que la carpeta del módulo.
* **config.xml**: Es el archivo de configuración de la caché. Si no existe PS lo creará cuando se instale.
* **logo.jpg o png o gif**: ****Es el icono que representa al módulo en el back office. Por ejemplo un PNG de 32x32 es buena opción.

### Carpetas opcionales

* **/views** Contiene archivos de vistas
* **/views/templates** Contiene archivos de vistas en formato .tpl.
* **/views/templates/admin** Contiene archivos de vistas que utilizan los controladores del back office
* **/views/templates/front** Contiene archivos de vistas que utilizan los controladores del front office
* **/views/templates/hook** Contiene archivos de vistas que utilizan los controladores del back office
* **/views/css** Hojas de estilo
* **/views/js** JavaScript
* **/views/img** Imágenes
* **/controllers** Controladores
* **/override** Archivos a sobrescribir
* **/translations** Traducciones
* **/themes/\[theme\_name\]/modules** Para sobrescribir archivos .tpl y de idioma
* **/upgrade** Archivos de actualización

## Naming

Vamos a darle un nombre **único** al módulo que sea **técnico** y que sea en minúsculas. Podemos ponerle como prefijo una abreviatura del proyecto actual.

## Localización

Creamos una carpeta con el nombre elegido dentro de la carpeta crear **modules**.

```text
/modules/cwejemplo
```

Los módulos además pueden ser parte de un theme si son específicos. En ese caso, estarían en la propia carpeta _/modules_ del theme, es decir con la estructura `/themes/[my-theme]/modules`

## Archivo principal

Debe llamarse igual que la carpeta y tener la extensión **.php**.

```text
/modules/cwejemplo/cwejemplo.php
```

### Por seguridad

En primer lugar hacemos una comprobación de seguridad para que únicamente se pueda ejecutar desde PrestaShop. Si no existe esta constante, paramos la ejecución:

```php
<?php

if (!defined('_PS_VERSION_')) {
    exit;
}
```

### Clase

A continuación vamos a crear una **clase** con el nombre del módulo pero con la notación Camel Case, que es poner en mayúsculas el inicio de cada palabra.

{% hint style="info" %}
El nombre de la carpeta y de la clase debe coincidir en caracteres, aunque el de la clase sea en Camel Case.
{% endhint %}

Es importante que extienda de la clase **Module \(classes/module/Module.php\)** para que herede todos sus métodos y propiedades:

```php
class CwEjemplo extends Module {
}
```

Con esto PrestaShop ya detectaría el módulo aunque no haga nada todavía.

### Método constructor

Ahora vamos a crear en la clase el método constructor, que será llamado cuando se cree una nueva instancia de la clase. En PrestaShop será el primer método al que se llamará cuando el módulo sea cargado.

```php
public function __construct() {
    $this->name = 'cwejemplo';
    $this->tab = 'front_office_features';
    $this->version = '1.0.0';
    $this->author = 'Jorge Puente';
    $this->need_instance = 0;
    $this->ps_versions_compliancy = array(
        'min' => '1.6',
        'max' => _PS_VERSION_
    );
    $this->bootstrap = true;    
    $this->displayName = $this->l('Módulo de ejemplo');
    $this->description = $this->l('Módulo de ejemplo para probar');
    
    $this->confirmUninstall = $this->l('Are you sure you want to uninstall?');
    
    parent::__construct();
}
```

Como vemos, sobrescribimos algunos valores de las propiedades de la clase **Module**:

* **name**: es el nombre técnico del módulo que debe coincidir con el de la carpeta
* **tab**: pestaña en la que queremos que aparezca nuestro módulo en el listado
* **version**: importante si vamos a darle un mantenimiento para saber en la que estamos
* **need\_instance**: si cargar la clase en el listado de módulos del back office. Tiene que estar a 1 si queremos mostrar un mensaje o aviso en ese listado.
* **author**: nombre del autor
* **ps\_versions\_compliancy**: mínimo y máximo de la versión de PrestaShop en la que el módulo funcionará
* **bootstrap**: si vamos a usar bootstrap en nuestras plantillas
* **displayName**: nombre comercial del módulo
* **description**: descripción del módulo
* **confirmUninstall**: mensaje que aparecerá al hacer click en desinstalar

Finalmente lo que hacemos es llamar al constructor del padre para que haga lo que se necesita para convertirlo en un módulo funcional, completamente instalable.

### Método de instalación

Realizará operaciones al instalar el módulo.

Devolverá _true_ si se ejecuta correctamente. Si no lo creamos se ejecutará el de la superclase, que siempre devolverá verdadero.

```php
public function install() {
    if (!parent::install())
        return false;
    return true;
}
```

Lo que hace es llamar al método install del padre, que realizará unos procesos muy importantes para el funcionamiento de nuestro módulo:

* Añadir nuestro módulo en la tabla **ps\_module**

Podemos hacer otras operaciones, por ejemplo añadir valores a la tabla **ps\_configuration**:

```php
public function install() {
    if (!parent::install() ||
        !Configuration::updateValue('CWEJEMPLO_URL','http://example.com'))
        return false;
    return true;
}
```

**Configuration** es una clase que nos va a ayudar mucho con la configuración de nuestros módulos, evitándonos tener que hacer consultas directamente. **updateValue** intenta actualizar un valor, y si no existe lo crea. Tiene otros métodos importantes para recuperar valores y eliminarlos.

#### Archivo config.xml

Verás que genera un `config.xml` en la carpeta del módulo y además añade un registro a la tabla _ps\_module_. 

Hace posible optimizar la carga del módulo en el back office.

```text
<?xml version="1.0" encoding="UTF-8" ?>
        <module>
            <name>mymodule</name>
            <displayName><![CDATA[My module]]></displayName>
            <version><![CDATA[1.0]]></version>
            <description><![CDATA[Description of my module.]]></description>
            <author><![CDATA[Firstname Lastname]]></author>
            <tab><![CDATA[front_office_features]]></tab>
            <confirmUninstall>Are you sure you want to uninstall?</confirmUninstall>
            <is_configurable>0</is_configurable>
            <need_instance>0</need_instance>
            <limited_countries></limited_countries>
        </module>
```

* _is\_configurable_ indica si tiene una página de configuración o no.
* _need\_instance_ indica si una instancia del módulo debe crearse al mostrar la lista de módulos. Es útil si el módulo tiene que ejecutar comprobaciones en la configuración de PS y mostrar un mensaje.
* _limited\_countries_ se usa para indicar los paises a los que el módulo está limitado. Por ejemplo, si el módulo está limitado a Francia y España, utiliza:

```text
<limited_countries>fr,es</limited_countries>
```

### Método de desinstalación

Realizará operaciones al desinstalar el módulo.

```php
public function uninstall() {
    if(!parent::uninstall() ||
        !Configuration::deleteByName('CWEJEMPLO_URL'))
        return false;
    return true;
}
```

Borramos el valor creado en la instalación en la tabla **ps\_configuration**.

### Página de configuración

Añadimos el método **getContent** que va a decirle a PrestaShop que tenemos contenido que mostrar, y le permitirá añadir la opción de **Configurar** al módulo creado.

```php
public function getContent() {
    return 'Hola, bienvenido!';
}
```

Con esto ya tenemos una página de configuración, que además de la cadena mostrada añadirá una serie de botones por defecto en la parte superior derecha:

* atras
* traducir
* comprobar actualizaciones
* configurar los hooks

## Icono

Logotipo para nuestro módulo. El nombre del archivo debe ser siempre **logo.png** y estar en la raíz de la carpeta de nuestro módulo.

```text
/modules/cwejemplo/logo.png
```

El tamaño debe ser siempre **32x32** medido en píxeles.

Podemos descargarlo de una librería, como por ejemplo [FatCow](http://www.fatcow.com/free-icons) que son muy parecidos a los de PrestaShop.

## Archivo para securizar el módulo

Crea el siguiente archivo:

```text
/modules/cwejemplo/index.php
```

Y añade lo siguiente:

```php
<?PHP

header("Expires: Mon, 26 Jul 1997 05:00:00 GMT");
header("Last-Modified: ".gmdate("D, d M Y H:i:s")." GMT");
header("Cache-Control: no-store, no-cache, must-revalidate");
header("Cache-Control: post-check=0, pre-check=0", false);
header("Pragma: no-cache");
header("Location: ../");
exit;
```

Esto evitará que puedan ver el contenido de la carpeta en el caso de que exista una mala configuración de apache.

