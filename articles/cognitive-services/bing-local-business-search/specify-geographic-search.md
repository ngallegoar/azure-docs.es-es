---
title: Uso de límites geográficos para filtrar los resultados de Bing Local Business Search API
titleSuffix: Azure Cognitive Services
description: Use este artículo para obtener información sobre cómo filtrar los resultados de la búsqueda de Bing Local Business Search API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: rosh
ms.openlocfilehash: 210de0ef2cfbaec676528b4788bebecaa34136e4
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93095091"
---
# <a name="use-geographic-boundaries-to-filter-results-from-the-bing-local-business-search-api"></a>Uso de límites geográficos para filtrar los resultados de Bing Local Business Search API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Bing Local Business Search API le permite establecer límites en la zona geográfica específica que le gustaría buscar mediante los parámetros de consulta `localCircularView` o `localMapView`. Asegúrese de usar solo un parámetro en las consultas. 

Si un término de búsqueda contiene una ubicación geográfica explícita, Bing Local Business API la usará automáticamente para establecer límites para los resultados de la búsqueda. Por ejemplo, si es el término de búsqueda es `sailing in San Diego`, `San Diego` se usará como la ubicación y cualquier otra ubicación especificada en los parámetros de consulta o en los encabezados de usuario se ignorará. 

Si no se detecta una ubicación geográfica en el término de búsqueda y no se especifica ninguna ubicación geográfica mediante los parámetros de consulta, Bing Local Business Search API intentará determinar la ubicación a partir de los encabezados `X-Search-ClientIP` o `X-Search-Location` de la solicitud. Si no se especifica ningún encabezado, la API determinará la ubicación a partir de la dirección IP del cliente de la solicitud o las coordenadas GPS en dispositivos móviles.

## <a name="localcircularview"></a>localCircularView

El parámetro `localCircularView` crea un área geográfica circular en torno a un conjunto de coordenadas de latitud y longitud definida por un radio. Cuando se usa este parámetro, las respuestas de Bing Local Business Search API solo incluirán las ubicaciones dentro de este círculo, a diferencia del parámetro `localMapView`, que puede incluir ubicaciones ligeramente fuera del área de búsqueda.

Para especificar un área de búsqueda geográfica circular, elija una latitud y una longitud para que actúen como el centro del círculo y un radio en metros. Este parámetro se puede anexar a una cadena de consulta, por ejemplo: `q=Restaurants&localCircularView=47.6421,-122.13715,5000`.

Consulta completa:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localCircularView=47.6421,-122.13715,5000&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="localmapview"></a>localMapView

El parámetro `localMapView` especifica un área geográfica rectangular para la búsqueda, con dos conjuntos de coordenadas para especificar sus esquinas sudeste y noroeste. Cuando se usa este parámetro, las respuestas de Bing Local Business Search API pueden incluir ubicaciones dentro y ligeramente fuera del área especificada, a diferencia del parámetro `localCircularView`, que solo incluye ubicaciones dentro del área de búsqueda.

Para especificar un área de búsqueda rectangular, elija dos conjuntos de coordenadas de latitud y longitud para que actúen como las esquinas sudeste y noroeste del límite. Asegúrese de definir las coordenadas sudeste en primer lugar, como en el ejemplo siguiente: `localMapView=47.619987,-122.181671,47.6421,-122.13715`.

Consulta completa:

```
https://api.cognitive.microsoft.com/bing/v7.0/localbusinesses/search?q=restaurant&localMapView=47.619987,-122.181671,47.6421,-122.13715&appid=0123456789ABCDEF&mkt=en-us&form=monitr
```

## <a name="next-steps"></a>Pasos siguientes
- [Guía de inicio rápido de Local Business Search para Java](quickstarts/local-search-java-quickstart.md)
- [Guía de inicio rápido de Local Business Search para C#](quickstarts/local-quickstart.md)
- [Guía de inicio rápido de Local Business Search para Node](quickstarts/local-search-node-quickstart.md)
- [Guía de inicio rápido de Local Business Search para Python](quickstarts/local-search-python-quickstart.md)
