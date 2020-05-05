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
ms.openlocfilehash: 8377f0f5d586212c94bb763598b6e7a9e391073c
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "75382725"
---
# <a name="what-is-the-bing-video-search-api"></a>¿Qué es Bing Video Search API?

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

1. Cree una [cuenta de API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con acceso a Bing Search APIs. Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) gratuita.
2. Envíe una solicitud a la API con una consulta de búsqueda válida.
3. Analice el mensaje JSON devuelto para procesar la respuesta de API.


## <a name="next-steps"></a>Pasos siguientes

La [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) de Bing Video Search API muestra cómo puede personalizar una consulta de búsqueda y buscar vídeos en Internet.

Cuando esté listo para llamar a la API, cree una [cuenta de API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) gratuita.

Utilice el [inicio rápido](csharp.md) para aprender rápidamente a realizar su primera solicitud de API.

## <a name="see-also"></a>Consulte también

* La página de referencia de [Bing Video Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference) contiene la lista de los puntos de conexión, encabezados y parámetros de consulta que se utilizan para solicitar los resultados de búsqueda.

* En los [requisitos de uso y visualización de Bing](./useanddisplayrequirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.

* Visite la [página central de Bing Search API](../bing-web-search/search-the-web.md) para explorar las otras API disponibles.