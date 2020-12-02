---
title: ¿Qué es Bing Local Business Search API?
titleSuffix: Azure Cognitive Services
description: Bing Local Business Search API es un servicio RESTful que permite a las aplicaciones buscar información acerca de lugares y empresas locales en función de las consultas de búsquedas.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: overview
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 7a5829623707797b98593f837d6cadf009410f31
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96487021"
---
# <a name="what-is-bing-local-business-search"></a>¿Qué es Bing Local Business Search?

> [!WARNING]
> Las Bing Search API se mueven de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020**, las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
Bing Local Business Search API es un servicio RESTful que permite a las aplicaciones buscar información acerca de empresas locales en función de las consultas de búsquedas. Por ejemplo, `q=<business-name> in Redmond, Washington` o `q=Italian restaurants near me`. 

## <a name="features"></a>Características
| Característica | Descripción |  
| -- | -- | 
| [Búsqueda de ubicaciones y empresas locales](quickstarts/local-quickstart.md) | Bing Local Business Search API obtiene resultados localizados a partir de una consulta. Los resultados incluyen la dirección URL del sitio web de la empresa y muestran texto, el número de teléfono y la ubicación geográfica, lo que incluye: las coordenadas de GPS, la ciudad y la dirección postal |  
| [Filtrado de los resultados locales por límites geográficos](specify-geographic-search.md) | Agregue las coordenadas como parámetros de búsqueda para limitar los resultados a un área geográfica determinada que se especifica mediante un área circular o un rectángulo delimitador. | 
| [Filtrado de los resultados de empresas locales por categoría](local-categories.md) | Busque resultados de empresas locales por categorías. Esta opción utiliza las coordenadas de GPS o la dirección IP inversa del autor de la llamada para devolver resultados localizados de diversas categorías de empresas.|

## <a name="workflow"></a>Flujo de trabajo
Llame a Bing Local Business Search API con cualquier lenguaje de programación que pueda realizar solicitudes HTTP y procesar respuestas JSON. Se puede acceder a este servicio con la API REST.
 
1. Cree una [cuenta de Cognitive Services API](../cognitive-services-apis-create-account.md) con acceso a Bing Search APIs. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/).   
2. Codifique como una dirección URL los términos de búsqueda del parámetro de consulta `q=""`. Por ejemplo, `q=nearby+restaurant` o `q=nearby%20restaurant`. Si es necesario, establezca también la paginación. 
3. Envíe una [solicitud a Bing Local Business Search API](quickstarts/local-quickstart.md). 
4. Procese la respuesta JSON. 

> [!NOTE]
> Actualmente, Local Business Search: 
> * Solo admite el mercado de `en-US`. 
> * No admite Bing Autosuggest. 

## <a name="next-steps"></a>Pasos siguientes
- [Consultas y respuestas](local-search-query-response.md)
- [Guía de inicio rápido de Local Business Search](quickstarts/local-quickstart.md)
- [Referencia de Local Business Search API](local-search-reference.md)
- [Requisitos de uso y visualización](../bing-web-search/use-display-requirements.md)