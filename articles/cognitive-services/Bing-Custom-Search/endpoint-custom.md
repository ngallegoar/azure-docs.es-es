---
title: Punto de conexión de Bing Custom Search
titleSuffix: Azure Cognitive Services
description: Cree experiencias de búsqueda a medida de los temas que le interesan. Los usuarios ven resultados de la búsqueda a medida del contenido que les interesa.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 80215a22c5af3698aee6719f52e42c457220b7f9
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94367468"
---
# <a name="custom-search"></a>Búsqueda personalizada

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).
Bing Custom Search le permite crear experiencias de búsqueda a medida de los temas que le interesan. Los usuarios ven los resultados de búsqueda a la medida del contenido que les interesa en lugar de recorrer las páginas de los resultados de búsqueda que pueden contener contenido carente de interés.

## <a name="custom-search-endpoint"></a>Punto de conexión de Custom Search
Para obtener resultados mediante Bing Custom Search API, envíe una solicitud `GET` al punto de conexión siguiente. Use los encabezados y parámetros de dirección URL para definir especificaciones adicionales.

Punto de conexión: devuelve sugerencias de búsqueda como resultados JSON relacionados con la entrada del usuario definida por `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Para obtener ejemplos que describen cómo configurar los orígenes de Custom Search, consulte el [tutorial](./tutorials/custom-search-web-page.md). Para obtener más información sobre encabezados, parámetros, códigos de mercado, objetos de respuesta, errores etc., consulte la referencia [Bing Custom Search API v7](/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference).

## <a name="custom-search-response-json"></a>JSON de respuesta de Custom Search
Una solicitud de búsqueda personalizada devuelve los resultados como objetos JSON, consulte [Response objects](/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#response-objects) (Objetos de respuesta). 

## <a name="custom-autosuggest"></a>Custom Autosuggest
Custom Autosuggest API le permite enviar un término de consulta de búsqueda parcial a Bing y obtener una lista de consultas sugeridas que puede configurar. Con Custom Autosuggest, agregue las sugerencias devueltas por la API y, opcionalmente, especifique si desea incluir sugerencias generadas por Bing.

## <a name="custom-autosuggest-endpoint"></a>Punto de conexión de Custom Autosuggest
Para solicitar sugerencias de consulta personalizadas, envíe una solicitud GET a:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Para obtener información acerca de cómo definir sugerencias personalizadas, consulte [Define custom search suggestions](define-custom-suggestions.md) (Definición de sugerencias de búsqueda personalizadas).

## <a name="custom-image-search"></a>Custom Image Search
Custom Image Search API le permite enviar una consulta de búsqueda a Bing y obtener una lista de imágenes correspondientes de la instancia de Custom Search.

## <a name="custom-image-search-endpoint"></a>Punto de conexión de Custom Image Search
Para solicitar las imágenes de la instancia de Custom Search, envíe una solicitud GET a la dirección URL siguiente:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Para obtener información acerca de cómo configurar una instancia de Custom Search, consulte [Configure your custom search experience](./define-your-custom-view.md) (Configuración de la experiencia de búsqueda personalizada).

## <a name="next-steps"></a>Pasos siguientes
Las API de **Bing** admiten acciones de búsqueda que devuelven resultados según su tipo.  Todos los puntos de conexión de búsqueda devuelven resultados como objetos de respuesta JSON.  Todos los puntos de conexión admiten consultas que devuelven un idioma o una ubicación en concreto por longitud, latitud y radio de búsqueda.

Para una información completa acerca de los parámetros admitidos por cada punto de conexión, consulte las páginas de referencia de cada tipo.
Para obtener ejemplos de solicitudes básicas mediante Custom Search API, consulte los [inicios rápidos de Custom Search](/azure/cognitive-services/bing-custom-search/).