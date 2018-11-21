# Vistas o plantillas

Utiliza el motor de plantillas [Smarty](http://www.smarty.net/) para generar sus vistas.

## Vistas de theme

Las vistas se almacenan en archivos _.tpl_, y son usados a través de PS:

* _Vistas front office_: los archivos pertenecen al theme activo, que está en la carpeta `/themes`
* _Vistas back office_: los archivos pertenecen al theme activo que está en `/admin-dev/themes`

## Vistas de módulo

Los módulos pueden añadir sus propias plantillas para adaptarlas como partes de la interfaz:

* _Vistas front office_:  `/modules/bankwire/views/templates/front/payment_execution.tpl`
* _Vistas back office_: `/modules/blocklayered/views/templates/admin/view.tpl`

## Sobrescribiendo un archivo de vista

Como no hay herencia, no hay forma de sobrescribir una vista. Para cambiar una vista, debes sobrescribir el archivo de plantilla y colocarlo en la carpeta de tu módulo en la misma ruta.

Por ejemplo, si quieres sobrescribir `/admin-dev/themes/default/template/controllers/orders/helpers/view/view.tpl` copia el archivo y pégalo en `/override/controllers/admin/templates/orders/helpers/view/view.tpl`

Cuando añadas un archivo sobrescrito de forma manual, no te olvides de eliminar el archivo `/cache/class_index.php` para que PS tenga los cambios en cuenta.

