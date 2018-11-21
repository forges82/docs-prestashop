# Arquitectura técnica

## Arquitectura

Se basa en una arquitectura de tres partes:

* **Objetos/datos** El acceso a base de datos se controla a través de los archivos de la carpeta _classes_
* **Data control** El contenido provisto por el usuario se controla por los archivos que hay en la carpeta raíz.
* **Diseño** Todos los archivos del theme están en la carpeta _theme_.

### Modelo-vista-controlador \(MVC\)

* **Modelo** representa el comportamiento de la aplicación: procesamiento de datos, interacción con la base de datos, etc... Contiene y describe los datos que han sido procesados por la aplicación. Administra los datos y garantiza su integridad.
* **Vista** Es la interfaz que utiliza el usuario. Muestra los datos que ha provisto el modelo, maneja todas las acciones del usuario y envía los eventos al controlador. No procesa nada, solamente muestra datos al usuario.
* **Controlador** Maneja la sincronización entre Modelo y Vista. Recibe los eventos del usuario y dispara las acciones a ejecutar. Si una acción necesita que los datos se cambien, el controlador preguntará al modelo para que se cambien, y notificará a la vista que los datos se han cambiado para que se actualice.

## Esquema de base de datos

Verlo en el siguiente [enlace](http://doc.prestashop.com/download/attachments/21463263/mpd16.pdf?version=1&modificationDate=1411047693000&api=v2).

## Estructura de carpetas

### Modules

Contiene todo el contenido de los módulos de nuestra instalación, incluidos los que nosotros vayamos creando. Cada módulo irá en una carpeta independiente.

### Admin

Contiene todos los ficheros relacionados con el panel de administración de nuestra tienda.

No tiene por qué llamarse así, de hecho es mejor cambiarle el nombre por seguridad, de modo que el atacante no sepa por donde acceder al backoffice.

Es importante protegerlo con un .htaccess.

### Cache

Almacena todos los archivos temporales que PrestaShop utiliza.

### Classes

Contiene todos los archivos que pertenecen la modelo de objetos de PS. Cada uno tiene una clase con sus métodos, y pueden ser tanto del back como del front office.

#### Module

#### _- Module.php_

Es una clase de la que extenderán todos nuestros módulos, permitiendo que se utilicen todos sus métodos y propiedades.

### Config

Aquí se almacena la configuración de PrestaShop. Nunca deberás tocarlos.

#### - Settings.inc.php

Se almacena toda la información referente a la base de datos.

#### - Config.inc.php

Principal archivo de configuración para PS. No deberías tocar nada aquí.

#### - Defines.inc.php

Contiene las contantes, entre ellas las localizaciones de las distintas carpetas.

### Controllers

Se almacena toda la arquitectura de PrestaShop. 

{% hint style="info" %}
Nunca se deben modificar estos ficheros, estos cambios se realizarán desde otros puntos. Si los cambiamos, al actualizar PrestaShop perderemos todos los cambios.
{% endhint %}

### Css

Almacena los estilos que no se utilizan en la parte pública. Contiene todas las hojas de estilos que no pertenecen a los themes. Suele usarse más para el back office.

### Docs

Referente a la documentación de PrestaShop, se puede eliminar. No debería estar en un entorno de producción.

### Download

Donde se guardan todos los archivos de los productos virtuales descargables.

Contiene tus productos virtuales, que pueden ser descargados por el usuario que pague por ellos. Se almacenan con su nombre en md5.

### Img

Contiene todas las imágenes por defecto de PS, iconos, etc... No pertenecen a los themes. Aquí es donde puedes encontrar las imágenes para las categorías de producto \(_/c_ subcarpeta\), y para el backoffice \(_/admin_ subcarpeta\).

#### p

Todas las imágenes de los productos.

#### admin

Todas las imágenes del administrador.

### Install

Contiene todos los archivos relacionados con la instalación. Hay que borrarlo después.

### Js

Contiene todos los JavaScripts necesarios para que PrestaShop funcione. No pertenecen a los themes, la mayoría del backoffice. Aquí encontrarás el framework jQuery.

### Localization

Contienen información local como moneda, idioma, reglas de impuestos y reglas de grupos, países y varias unidades en uso en el país elegido.

### Log

Contiene los logs generados por PS en varios procesos, como por ejemplo, la instalación.

### Mails

Contiene todo el HTML y los archivos de texto relacionados con los mails que envía PS. Cada idioma tiene una carpeta específica, donde puedes de forma manual editar su contenido. PS contiene una herramienta para editar tus mails, dentro del back office en _Localization_ &gt; _Translation_.

### Modules

Contiene los módulos de PS, cada uno en su carpeta. Antes de eliminar un módulo, primero desinstálalo desde el back office.

### Override

Aquí insertamos las modificaciones del Core de PrestaShop: anular, modificar o crear nuevas clases.

Usando una convención de nombres, es posible crear archivos que sobreescriban clases o controladores de PS. Te permite cambiar el comportamiento del core sin necesidad de alterar los archivos originales.

### Pdf

Contiene archivos de plantilla _.tpl_ para la generación de PDFs. Cambia estos archivos para cambiar la apariencia de los PDFs que PS genera.

### Themes

Contiene los themes actualmente instalados, cada uno en su carpeta.

### Tools

Librerías externas a PrestaShop, como por ejemplo **smarty,** TCPDF, Swift \(para mails\), entre otras.

### Translations

Contiene una carpeta por idioma. Sin embargo, para cambiar una traducción, debes hacerlos desde el back office y nunca editando estos archivos.

### Upload

Donde se guardan los archivos que los clientes suben para productos personalizables.

### Webservice

Contiene archivos que activan aplicaciones de terceros para acceder a PS a través de su API.

## Sobre la caché

El archivo `/cache/class_index.php` contiene el enlace entre las clases y los archivos donde son declaradas. Si hay algún problema, se puede borrar sin problemas.

La carpeta `/config/xml` contiene la lista de los módulos base:

* default\_country\_modules\_list.xml
* must\_have\_modules\_list.xml
* tab\_modules\_list.xml

Cuando el front end de la tienda no refleje tus cambios y borrar la caché del navegador no funcione, deberías intentar vaciar las siguientes carpetas:

* /cache/smarty/cache
* /cache/smarty/compile

