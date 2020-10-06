---
title: ¿Qué es Bing Entity Search API?
titleSuffix: Azure Cognitive Services
description: Conozca más información acerca de Bing Entity Search API y cómo extraer y buscar entidades y lugares a partir de consultas de búsqueda.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: e0402b1695e1d5f5c9f29d128f4cd405f219e724
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90532501"
---
# <a name="what-is-bing-entity-search-api"></a>¿Qué es Bing Entity Search API?

Bing Entity Search API envía una consulta de búsqueda a Bing y obtiene resultados que incluyen entidades y lugares. Los resultados de lugares incluyen restaurantes, hoteles u otros negocios locales. Bing devuelve lugares si en la consulta se especifica el nombre de la empresa local o si se solicita un tipo de negocio (por ejemplo, restaurantes cercanos). Bing devuelve entidades si en la consulta se especifican personas, lugares o incluso cosas famosas (puntos turísticos, estados, países o regiones, etc.).

|Característica  |Descripción  |
|---------|---------|
|[Sugerencias de búsqueda en tiempo real](concepts/search-for-entities.md#suggest-search-terms-with-the-bing-autosuggest-api)     | Proporciona sugerencias de búsqueda que pueden mostrarse como una lista desplegable a medida que los usuarios escriben.       | 
| [Desambiguación de entidades](concepts/search-for-entities.md#the-bing-entity-search-api-response)  | Obtenga varias entidades para las consultas con varios significados posibles. |
| [Búsqueda de lugares](concepts/search-for-entities.md#find-places) | Busca y devuelve información sobre entidades y negocios locales  |

## <a name="workflow"></a>Flujo de trabajo

Bing Entity Search API es un servicio web RESTful, lo que significa que es fácil llamarlo desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar código JSON. Puede usar el servicio con la API REST o el SDK.

1. Cree una [cuenta de API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con acceso a Bing Search APIs. Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/free/cognitive-services/) gratuita.
2. Envíe una solicitud a la API con una consulta de búsqueda válida.
3. Analice el mensaje JSON devuelto para procesar la respuesta de API.

## <a name="next-steps"></a>Pasos siguientes

* Pruebe la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) de Bing Entity Search API. 
* Para empezar a trabajar rápidamente con la primera solicitud, pruebe con este [inicio rápido](quickstarts/csharp.md).
* Sección de referencia de [Bing Entity Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
* En los [requisitos de uso y visualización de Bing](./use-display-requirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.
* Visite la [página central de Bing Search API](../bing-web-search/search-the-web.md) para explorar las otras API disponibles.