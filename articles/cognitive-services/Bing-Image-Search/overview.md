---
title: ¿Qué es Bing Image Search API?
titleSuffix: Azure Cognitive Services
description: Bing Image Search API le permite usar funcionalidades cognitivas de búsqueda de imágenes de Bing en la aplicación. Envíe consultas de búsqueda de usuarios con la API para obtener y mostrar imágenes relacionadas y de alta calidad similares a Bing Images.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: overview
ms.date: 12/18/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 1cdcf6a7aeee6618177440aaef6f488a31870b49
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93087849"
---
# <a name="what-is-the-bing-image-search-api"></a>¿Qué es Bing Image Search API?

> [!WARNING]
> Las Bing Search API se mueven de Cognitive Services a Bing Search Services. A partir del  **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de las Bing Search API con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Bing Image Search API le permite usar funcionalidades de búsqueda de imágenes de Bing en la aplicación. Envíe consultas de búsqueda de usuarios a la API para obtener imágenes de alta calidad similares a [bing.com/images](https://www.bing.com/images).

Aunque Bing Image Search API proporciona resultados de búsqueda solo de imágenes, puede combinar o utilizar las otras [Bing Search API](../bing-web-search/bing-api-comparison.md) disponibles para encontrar muchos tipos de contenido en la web.

## <a name="bing-image-search-features"></a>Características de Bing Image Search

| Característica                                                                                                                                                                                 | Descripción                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Sugerencia de términos de búsqueda en tiempo real](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) | Mejore su experiencia con las aplicaciones mediante [Bing Autosuggest API](../bing-autosuggest/get-suggested-search-terms.md) para mostrar términos de búsqueda sugeridos a medida que se escriben. |
| [Filtrado y restricción de resultados de imagen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images)                       | Filtre las imágenes que devuelve Bing mediante la edición de parámetros de consulta.                                                                                                       |
| [Recorte, cambio de tamaño y vista de miniaturas](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/resize-and-crop-thumbnails)                                                | Edite y muestre vistas previas en miniatura de las imágenes que devuelve Bing Image Search.                                                                                      |
| [Dinamización y expansión de las consultas de búsqueda de usuarios](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries)               | Expanda las funcionalidades de búsqueda con la inclusión y visualización de términos de búsqueda sugeridos por Bing en las consultas.                                                                    |
| [Obtención de imágenes populares](trending-images.md)                                                                     | Personalice una búsqueda de imágenes de tendencias de todo el mundo.                                                                                                          |

## <a name="workflow"></a>Flujo de trabajo

Bing Image Search API es un servicio web RESTful, lo que significa que es fácil llamar desde cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar código JSON. Puede usar el servicio con la [API REST](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp?) o el [SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

1. Cree una [cuenta de API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con acceso a Bing Search APIs. Si no tiene una suscripción de Azure, puede [crear una cuenta](https://azure.microsoft.com/free/cognitive-services/) gratuita.
2. Envíe una solicitud a la API con una [consulta de búsqueda](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) válida.
3. Analice el mensaje JSON devuelto para procesar la respuesta de API.

## <a name="next-steps"></a>Pasos siguientes

Primero, pruebe la [demostración interactiva](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) de Bing Image Search API.
Esta demostración muestra cómo puede personalizar rápidamente una consulta de búsqueda y explorar la Web en busca de imágenes.

Para empezar rápidamente con la primera solicitud de API, puede conocer cómo:

* [Enviar consultas de búsqueda a Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp) mediante la API REST o
* [Solicitar y filtrar](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) las imágenes que devuelve Bing mediante el SDK.

## <a name="see-also"></a>Consulte también

* [Detalles de precios](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/) de Bing Search API. 

* La sección de referencia de [Bing Image Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference) contiene información sobre los puntos de conexión de la API, encabezados, respuestas de API y parámetros de consulta.

* En los [requisitos de uso y visualización de Bing](./useanddisplayrequirements.md) se especifican usos aceptables del contenido y la información adquirida mediante las API de búsqueda de Bing.

* En el artículo [Obtención de imágenes desde la web con Bing Image Search API](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images) se describe cómo buscar y obtener imágenes de la web.

* En el artículo [Envío y trabajo con consultas de búsqueda](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) se describe cómo realizar, personalizar y dinamizar consultas de búsqueda.

* Visite la [página central de Bing Search API](../bing-web-search/search-the-web.md) para explorar las otras API disponibles.
