---
title: ¿Qué es Bing Web Search API?
titleSuffix: Azure Cognitive Services
description: API Bing Web Search es un servicio RESTful que proporciona respuestas instantáneas a las consultas de búsqueda en web. Configure los resultados para incluir páginas web, imágenes, vídeos, noticias, etc. Los resultados se proporcionan como JSON y según la pertinencia de la búsqueda y las suscripciones de Bing Web Search.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: overview
ms.date: 03/31/2020
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 4cbf79abaf915e7f6e055f5bcb0d8f09de026ad5
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2020
ms.locfileid: "85604869"
---
# <a name="what-is-the-bing-web-search-api"></a>¿Qué es Bing Web Search API?

Bing Web Search API es un servicio RESTful que proporciona respuestas instantáneas a las consultas de los usuarios. Los resultados de búsqueda se configuran fácilmente para incluir páginas web, imágenes, vídeos, noticias, traducciones y mucho más. Bing Web Search proporciona los resultados como JSON según la pertinencia de la búsqueda y las suscripciones de Bing Web Search.

Esta API es óptima para aplicaciones que necesitan acceso a todo el contenido que es relevante para la consulta de búsqueda de un usuario. Si va a compilar una aplicación que requiere solo un tipo de resultado específico, considere el uso de [Bing Image Search API](../Bing-Image-Search/overview.md), [Bing Video Search API](../Bing-Video-Search/search-the-web.md) o [Bing News Search API](../Bing-News-Search/search-the-web.md). Consulte [Cognitive Services APIs](https://docs.microsoft.com/azure/cognitive-services) para obtener una lista completa de Bing Search APIs.

¿Quiere ver cómo funciona? Pruebe nuestra [demostración de Bing Web Search API](https://azure.microsoft.com/services/cognitive-services/bing-web-search-api/).

## <a name="features"></a>Características  

Bing Web Search solo no proporciona acceso a las respuestas instantáneas. También ofrece características y funcionalidad adicionales que le permiten personalizar los resultados de búsqueda de sus usuarios.

| Característica | Descripción |
|---------|-------------|
| [Sugerencia de términos de búsqueda en tiempo real](../bing-autosuggest/get-suggested-search-terms.md) | Mejore su experiencia con las aplicaciones mediante Bing Autosuggest API para mostrar términos de búsqueda sugeridos a medida que se escriben. |
| [Filtrado y restricción de resultados por tipo de contenido](filter-answers.md) | Personalice y refine los resultados de búsqueda con filtros y parámetros de consulta para páginas web, imágenes, vídeos, búsqueda segura y más. |
| [Resaltado de referencias de caracteres Unicode](hit-highlighting.md) | Con el resaltado de referencias, puede identificar y quitar los caracteres Unicode no deseados de los resultados de búsqueda antes de mostrarlos a los usuarios. |
| [Localización de resultados de búsqueda por país, región o mercado](supported-countries-markets.md) | Bing Web Search admite más de 30 países o regiones. Use esta característica para refinar los resultados de búsqueda para un país, región o mercado específicos. |
| [Análisis de métricas de búsqueda con Bing Statistics](bing-web-stats.md) | Bing Statistics es una suscripción de pago que proporciona análisis sobre volumen de llamadas, cadenas de consultas principales, distribución geográfica y mucho más. |

## <a name="workflow"></a>Flujo de trabajo

Bing Web Search API es fácil de llamar desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar respuestas JSON. El servicio es accesible mediante la [API REST](quickstarts/python.md) o las [bibliotecas cliente de Bing Web Search](./quickstarts/client-libraries.md).

1. [Cree un recurso de Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para Bing Search API. Si no tiene una suscripción a Azure, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/cognitive-services/).  
2. Envíe una [solicitud a Bing Web Search API](quickstarts/python.md).
3. Analice la respuesta JSON.

## <a name="next-steps"></a>Pasos siguientes

* Use nuestra [guía de inicio rápido de Python](./quickstarts/client-libraries.md?pivots=programming-language-python) para realizar la primera llamada a Bing Web Search API.  
* [Compile una aplicación web de una sola página](tutorial-bing-web-search-single-page-app.md).
* Revise la documentación de [referencia de Web Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference).  
* Más información sobre [cómo usar y mostrar los requisitos](UseAndDisplayRequirements.md) de Bing Web Search.  
