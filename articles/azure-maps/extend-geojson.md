---
title: Geometrías GeoJSON extendidas | Microsoft Azure Maps
description: En este artículo aprenderá cómo Microsoft Azure Maps amplía las especificaciones de GeoJSON para representar determinadas geometrías.
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 98db10f0fc7a417f39d4bb00e77af6bdea034a03
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84687504"
---
# <a name="extended-geojson-geometries"></a>Geometrías GeoJSON extendidas

Azure Maps proporciona una lista de API eficaces para buscar dentro de características geográficas o junto a ellas. Estas API se adhieren a la [especificación de GeoJSON][1] estándar de representación de características geográficas.  

La [especificación de GeoJSON][1] solo admite las geometrías siguientes:

* GeometryCollection
* LineString
* MultiLineString
* MultiPoint
* MultiPolygon
* Punto
* Polygon

Algunas API de Azure Maps aceptan geometrías que no forman parte de la [especificación de GeoJSON][1]. Por ejemplo, [Search Inside Geometry](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) API acepta círculos y polígonos.

Este artículo proporciona una explicación detallada sobre cómo Azure Maps amplía las [especificaciones de GeoJSON][1] para representar determinadas geometrías.

## <a name="circle"></a>Circle

La geometría `Circle` no es compatible con las [especificaciones de GeoJSON][1]. Usamos el objeto `GeoJSON Point Feature` para representar un círculo.

Una geometría `Circle` representada mediante el objeto `GeoJSON Feature` __debe__ contener las propiedades y coordenadas siguientes:

- Center

    El centro del círculo se representa mediante un objeto `GeoJSON Point`.

- Radio

    `radius` se representa mediante las propiedades de `GeoJSON Feature`. El valor del radio está en _metros_ y debe ser del tipo `double`.

- SubType

    La geometría del círculo también debe contener la propiedad `subType`. Esta propiedad debe formar parte de las propiedades de `GeoJSON Feature` y su valor debe ser _Circle_.

#### <a name="example"></a>Ejemplo

Así se representa un círculo con un objeto `GeoJSON Feature`. Vamos a centrar el círculo en latitud: 47,639754 y longitud: -122,126986, y asígnele un radio igual a 100 metros:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

## <a name="rectangle"></a>Rectángulo

La geometría `Rectangle` no es compatible con las [especificaciones de GeoJSON][1]. Usamos el objeto `GeoJSON Polygon Feature` para representar un rectángulo. La extensión de rectángulo se usa principalmente en el módulo de herramientas de dibujo del SDK para web.

Una geometría `Rectangle` representada mediante el objeto `GeoJSON Polygon Feature` __debe__ contener las propiedades y coordenadas siguientes:

- Esquinas

    Las esquinas del rectángulo se representan mediante las coordenadas de un objeto `GeoJSON Polygon`. Debería haber cinco coordenadas, una para cada vértice. Y una quinta coordenada igual que la primera para cerrar el anillo del polígono. Se supone que estas coordenadas se alinean y que el desarrollador puede rotarlas según lo desee.

- SubType

    La geometría del rectángulo también debe contener la propiedad `subType`. Esta propiedad debe formar parte de las propiedades de `GeoJSON Feature` y su valor debe ser _Rectangle_.

### <a name="example"></a>Ejemplo

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [[[5,25],[14,25],[14,29],[5,29],[5,25]]]
    },
    "properties": {
        "subType": "Rectangle"
    }
}

```
## <a name="next-steps"></a>Pasos siguientes

Más información sobre los datos de GeoJSON en Azure Maps:

> [!div class="nextstepaction"]
> [Formato de Geofence GeoJSON](geofence-geojson.md)

Revise el glosario de términos técnicos comunes asociados a las aplicaciones de Azure Maps y de inteligencia de ubicación:

> [!div class="nextstepaction"]
> [Glosario de Azure Maps](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
