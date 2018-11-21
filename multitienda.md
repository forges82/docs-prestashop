# Multitienda

## El objeto Shop

Hemos visto antes esto:

```text
if (Shop::isFeatureActive()) {
  Shop::setContext(Shop::CONTEXT_ALL);
}
```

Este objeto nos permite gestionar la multitienda. Sus dos métodos principales son:

* _Shop::isFeatureActive\(\)_: simplemente comprueba si hay multitienda o no, y si al menos hay dos tiendas activadas actualmente.
* _Shop::setContext\(Shop::CONTEXT\_ALL\)_: esto cambia el contexto para aplicar los cambios a todas las tiendas en lugar de solo a una.

