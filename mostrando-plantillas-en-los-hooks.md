---
description: >-
  Lo lógico es crear plantillas smarty para mostrarlos en los hooks en vez de
  devolver cadenas de texto simples
---

# Mostrando plantillas en los hooks

## Registramos el hook del footer

Igual que lo hicimos en la página anterior:

```php
public function install() {
    if (!parent::install() ||
        !Configuration::updateValue('CWEJEMPLO_URL','http://example.com') ||
        !$this->registerHook('displayProductAdditionalInfo') ||
        !$this->registerHook('displayFooter') ||
        !$this->registerHook('ejemploCustomHook'))
        return false;
    return true;
}
```

Creamos el método correspondiente para la salida de ese hook:

```php
public function hookDisplayFooter($params) {
    return '';
}
```

## Creación de plantillas

En la siguiente carpeta creamos la plantilla smarty correspondiente:

```text
/modules/cwejemplo/views/templates/hook/displayFooter.tpl
```

Podemos darle un contenido con variables que se rellenarán en la sección siguiente:

```php
<a href="{urlvalue}">Enlace :)</a>
```

## Devolviendo la plantilla

Cambiamos el método para devolver la plantilla:

```php
public function hookDisplayFooter($params) {
    $urlvalue = Configuration::get('CWEJEMPLO_URL');
    $this->context->smarty->assign('urlvalue',$urlvalue);
    return $this->display(__FILE__,'displayFooter.tpl');
}
```

{% hint style="danger" %}
¡Ojo! si metes un formulario en un hook que esté dentro de otro formulario te dará problemas
{% endhint %}



