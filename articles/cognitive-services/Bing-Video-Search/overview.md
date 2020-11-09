---
title: ¿Qué es Bing Video Search API?
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo buscar vídeos en Internet mediante Bing Video Search API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: scottwhi
ms.openlocfilehash: 6f5da1d17722f89bee88a4b69177583e095eeac3
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93094292"
---
# <a name="what-is-the-bing-video-search-api"></a>¿Qué es Bing Video Search API?

> [!WARNING]
> Las Bing Search API se mueven de Cognitive Services a Bing Search Services. A partir del  **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de las Bing Search API mediante Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que antes suceda.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Bing Video Search API permite añadir fácilmente funcionalidades de búsqueda de vídeo a sus servicios y aplicaciones. Al enviar consultas de búsqueda de usuarios con la API, puede obtener y mostrar vídeos relacionados y de alta calidad similares a [Bing Video](https://www.bing.com/video). Utilice esta API para resultados de búsqueda que solo contienen vídeos. [Bing Web Search API](../bing-web-search/search-the-web.md) puede devolver otros tipos de contenido web, como páginas web, vídeos, noticias e imágenes.

## <a name="bing-video-search-api-features"></a>Características de Bing Video Search API

| Característica                                                                                                                                                                                 | Descripción                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Sugerencia de términos de búsqueda en tiempo real](concepts/sending-requests.md#suggest-search-terms-with-the-bing-autosuggest-api) | Mejore su experiencia con las aplicaciones mediante [Bing Autosuggest API](../bing-autosuggest/get-suggested-search-terms.md) para mostrar términos de búsqueda sugeridos a medida que se escriben. |
| [Filtrado y restricción de resultados de vídeo](concepts/get-videos.md#filtering-videos)                      | Filtre los vídeos que se devuelven mediante la edición de parámetros de consulta.                                                                                                       |
| [Recorte, cambio de tamaño y vista de miniaturas](../bing-web-search/resize-and-crop-thumbnails.md)                                                | Edite y muestre vistas previas en miniatura de los vídeos que devuelve Bing Video Search API.                                                                                      |
| [Obtención de vídeos populares](trending-videos.md) | Busque vídeos populares de todo el mundo.                                                                                                          |
| [Obtención de información de vídeos](video-insights.md) | Personalice una búsqueda de vídeos de tendencias de todo el mundo.                                                                                                          |

## <a name="workflow"></a>Flujo de trabajo

Bing Video Search API es un servicio web RESTful, por lo que resulta muy fácil llamarlo desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar código JSON. Puede usar el servicio con la [API REST](csharp.md) o el [SDK](video-search-sdk-quickstart.md).

1. Cree una [cuenta de API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con acceso a Bing Search APIs. Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/free/cognitive-services/) gratuita.
2. Envíe una solicitud a la API con una consulta de búsqueda válida.
3. Analice el mensaje JSON devuelto para procesar la respuesta de API.


## <a name="next-steps"></a>Pasos siguientes

La [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) de Bing Video Search API muestra cómo puede personalizar una consulta de búsqueda y buscar vídeos en Internet.

Utilice el [inicio rápido](csharp.md) para aprender rápidamente a realizar su primera solicitud de API.

## <a name="see-also"></a>Consulte también

* La página de referencia de [Bing Video Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference) contiene la lista de los puntos de conexión, encabezados y parámetros de consulta que se utilizan para solicitar los resultados de búsqueda.

* En los [requisitos de uso y visualización de Bing](./useanddisplayrequirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.

* Visite la [página central de Bing Search API](../bing-web-search/search-the-web.md) para explorar las otras API disponibles.