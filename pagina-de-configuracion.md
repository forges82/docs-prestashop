---
description: >-
  Para que el administrador tenga la posibilidad de cambiar la funcionalidad del
  módulo.
---

# Página de configuración

## Templates

Vamos a crear una plantilla en smarty para la página de configuración que creamos en el apartado anterior.

```text
/modules/cwejemplo/views/templates/hook/configure.tpl
```

Debemos recordar el nombre de la plantilla, porque la vamos a llamar. Le damos un contenido cualquiera, por ejemplo una simple línea de texto:

```php
Estoy en el configure.tpl.
```

Podemos añadir lo que queremos: formularios, mensajes, etc... Podemos aprovechar además que **Bootstrap** está incluido en el core desde PrestaShop 1.6. Pero para ello deberemos hacer algo antes que veremos en la siguiente sección en el constructor de la clase principal.

Podríamos añadir un formulario en ese archivo. Como vemos asignamos al campo de texto el valor por defecto de una variable **smarty** que rellenaremos en el módulo:

```php
{if $save}
    <div class="bootstrap">
        <div class="module_confirmation conf confirm alert alert-success">
            <button type="button" class="close" data-dismiss="alert">x</button>
            Url guardada correctamente
        </div>
    </div>
{/if}

<form action="" method="post">
    <div class="form-group">
        <label for="exampleUrl">URL</label>
        <input type="text" value="{$urlvalue}" class="form-control" name="exampleUrl" id="exampleUrl" aria-describedby="urlHelp" placeholder="Introduzca url">
        <small id="urlHelp" class="form-text text-muted">Introduzca una URL</small>
    </div>
    <button type="submit" name="submitCwEjemplo" class="btn btn-primary">Enviar</button>
</form>
```

## GetContent

Como vimos en [Primer módulo](https://jorgepuente.gitbook.io/prestashop/~/edit/drafts/-LLifQAdZSHROZNyTODW/pagina-de-configuracion) creamos un método en la clase principal del módulo. Ahora vamos a cambiar ese método que está en ese archivo: 

```text
/modules/cwejemplo/cwejemplo.php
```

Por esto:

```php
<?php

if (!defined('_PS_VERSION_')) {
    exit;
}

class CwEjemplo extends Module {

    public function __construct() {
        $this->name = 'cwejemplo';
        $this->tab = 'front_office_features';
        $this->version = '1.0.0';
        $this->author = 'Jorge Puente';
        $this->bootstrap = true;
        $this->ps_versions_compliancy = array(
            'min' => '1.6',
            'max' => _PS_VERSION_
        );
        $this->displayName = 'Módulo de ejemplo';
        $this->description = 'Módulo de ejemplo para probar';
        
        parent::__construct();
    }

    public function getContent() {
        return $this->dispay(__FILE__,'configure.tpl');
    }
}
```

Lo que nos va a hacer es devolvernos el contenido de **configure.tpl**. Además vemos que hemos añadido una variable en el constructor que nos va a permitir usar Bootstrap en las plantillas smarty.

### Procesar formulario

Para ello usamos el mismo método **getContent** añadiéndole una condición que se procesará solo si se ha pulsado el botón del nombre especificado:

```php
public function getContent() {
    if(Tools::isSubmit('submitCwEjemplo')) {
        $myurl = Tools::getValue('exampleUrl');
        Configuration::updateValue('CWEJEMPLO_URL',$myurl);
    }
    return $this->dispay(__FILE__,'configure.tpl');
}
```

Las operaciones realizadas son:

1. Recoger en una variable el valor del campo del formulario.
2. Guardarlo en la base de datos en la tabla **ps\_configuration**.

### Mostrar el valor actual en el formulario

Tendremos que recuperar ese valor de la base de datos y almacenarlo en una variable para pasársela a smarty:

```php
public function getContent() {

    $this->smarty->assign('save',false);
    
    if(Tools::isSubmit('submitCwEjemplo')) {
        $myurl = Tools::getValue('exampleUrl');
        Configuration::updateValue('CWEJEMPLO_URL',$myurl);
        $this->smarty->assign('save',true);
    }
    $urlvalue = Configuration::get('CWEJEMPLO_URL');
    $this->smarty->assign('urlvalue',$urlvalue);
    return $this->dispay(__FILE__,'configure.tpl');
}
```

Como vemos le pasamos a un método del objeto **smarty** la asignación de la variable actual al nombre de la variable que tendrá en la plantilla.

También le pasamos una variable que le mostrará un mensaje si se ha enviado el formulario.

