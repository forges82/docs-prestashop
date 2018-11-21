# Herramientas y trucos para el desarrollo

## Desactivar la caché

Es importante que podamos ver los cambios que hacemos en cada carga, y para ello en el backoffice ir a _Parámetros avanzados_ &gt; _Rendimiento_ y poner:

* Forzar compilación
* Desactivar caché

## Mostrar errores

Para poder visualizar los errores mientras desarrollamos:

```text
/config/defines.inc.php
```

Y activamos el parámetro correspondiente:

```php
define('_PS_MODE_DEV_', true);
```

### Funciones para el debug

Al activar ese parámetro activaremos dos funciones importantes para el desarrollo:

* La función **p** cuyo alias es **ppp\*** mostrará la variable pasada como parámetro sin romper la ejecución.
* La función **d** cuyo alias es **ddd\*** mostrará la variable pasada como parámetro rompiendo la ejecución.

## Smarty

En el siguiente archivo tenemos configuraciones relativas a **smarty**:

```text
/config/smarty.config.inc.php
```

El cacheado debería estar siempre desactivado porque es incompatible con PS.

```php
$smarty->caching = false;
```

En desarrollo deberías desactivar además la comprobación de compilación:

```php
$smarty->compile_check = false;
```

Desde _Rendimiento_ te permitirá activar _Debug console_ para elegir si mostrar o no la información de debug de Smarty.

## El Dispatcher o enrutador

Maneja las redirecciones URL, ya que PS en lugar de utilizar varios archivos siempre utiliza el index.php.

Ejemplo de URL del front office:

```text
/index.php?id_category=3&controller=category
```

Ejemplo de URL del back office:

```text
/admin-dev/index.php?controller=AdminDashboard
```

Además tiene la capacidad de sobreescribir para generar URLs amigables para pasar de esto:

```text
http://myprestashop.com/index.php?controller=category&id_category=3&id_lang=1
```

A esto:

```text
http://myprestashop.com/en/3-music-ipods
```

### Ventajas

* Es fácil añadir un controlador
* Puedes usar rutas personalizadas para cambiar tus URLs amigables
* Solo hay un punto de entrada para el Software, lo que mejora la fiabilidad de PS

El dispatcher usa tres clases abstractas:

* Controller
* FrontController \(hereda de la primera\)
* AdminController \(hereda de la primera\)

Se pueden añadir nuevas rutas sobrescribiendo el método **loadRoutes\(\)**.  
El administrador de la tienda puede cambiar la URL del controlador en la página `SEO & URLs` del back office.

