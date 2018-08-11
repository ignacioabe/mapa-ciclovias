# Santiago pedaleable

## Qué es?

Es un mapa que resalta la infraestructura para ciclistas por sobre el resto de la información.

Se centra en las ciclovías, las estaciones de bicicletas públicas y los estacionamientos de bicicletas. Como complemento aparecen las tiendas y talleres de bicicletas.

Toda la información a representar se obtiene de OpenStreetMap.  

## Instrucciones de uso

Flujo de trabajo:

- Datos se obtienen con un script de overpass api

### Para las ciclovías:

Pueden ir etiquetadas en las calles:

`highway=() + cycleway=lane` (sin segregación física)
`highway=() + cycleway=track` (con segregación física)

O dibujadas de modo autónomo:

`highway=cycleway`

Se pueden descargar estos tres tipos con el siguiente script de overpass turbo

```
[out:xml][timeout:25];

(
  // parte de la consulta para ciclovías etiquetadas en la calle
  way["cycleway"]({{bbox}});
  // parte de la consulta para ciclovías con el lado marcado
  // cycleway:left o cycleway:right
  way[~"^cycleway:.*$"~"."]({{bbox}});
  // parte de la consulta para ciclovías independientes
  way["highway"="cycleway"]({{bbox}});
  // parte de la consulta para relaciones de rutas pedaleables (que no hay en santiago realmente, excepto quizás el mapocho)
  relation["route"="bicycle"]({{bbox}});
);

// print results
out meta;
>;
out meta qt;
```

### Para los puntos de interés

`amenity=bicycle_rental` (estaciones de bicicletas públicas)
`amenity=bicycle_parking` (estacionamientos de bicicleta)
`shop=bicycle` (talleres y/o tiendas de bicicleta)

Por simpleza, se descargan tres archivos de puntos separados

eof
