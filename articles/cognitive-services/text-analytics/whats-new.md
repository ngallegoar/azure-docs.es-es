---
title: Novedades de Text Analytics API
titleSuffix: Text Analytics - Azure Cognitive Services
description: En este artículo se proporciona información sobre las versiones y características nuevas de Text Analytics de Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: 162e60ac8d33dc5d1951a58b0a9643b668608d7b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "77188804"
---
# <a name="whats-new-in-the-text-analytics-api"></a>Novedades de Text Analytics API

Text Analytics API se actualiza constantemente. Para mantenerse al día con los avances recientes, en este artículo se proporciona información sobre las nuevas versiones y características.

## <a name="february-2020"></a>Febrero de 2020

### <a name="sdk-support-for-text-analytics-api-v3-public-preview"></a>Compatibilidad del SDK con la versión preliminar pública de Text Analytics API v3

Como parte de la [versión unificada de Azure SDK](https://techcommunity.microsoft.com/t5/azure-sdk/january-2020-unified-azure-sdk-release/ba-p/1097290), el SDK de Text Analytics API v3 ahora está disponible como versión preliminar pública para los siguientes lenguajes de programación:
   * [C#](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-csharp)
   * [Python](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-python)
   * [JavaScript (Node.js)](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-javascript)
   * [Java](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3&pivots=programming-language-java)

> [!div class="nextstepaction"]
> [Más información sobre el SDK de Text Analytics API v3](https://docs.microsoft.com/azure/cognitive-services/text-analytics/quickstarts/text-analytics-sdk?tabs=version-3)

### <a name="named-entity-recognition-v3-public-preview"></a>Versión preliminar pública de reconocimiento de entidades con nombre v3

Los tipos de entidad adicionales ahora están disponibles en el servicio de versión preliminar pública de Reconocimiento de entidades con nombre (NER) v3 a medida que se expande la detección de entidades de información general y personal detectada en el texto. Esta actualización presenta [versión de modelo](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) `2020-02-01`, que incluye:

* Reconocimiento de los siguientes tipos de entidad generales (solo inglés):
    * PersonType
    * Producto
    * Evento
    * Entidad geopolítica (GPE) como subtipo de Ubicación
    * Habilidad

* Reconocimiento de los tipos de entidad de información personal siguientes (solo inglés):
    * Person
    * Organización
    * Edad como subtipo de Cantidad
    * Fecha como subtipo de DateTime
    * Email 
    * Número de teléfono (solo EE. UU.)
    * URL
    * Dirección IP

> [!div class="nextstepaction"]
> [Más información sobre el Reconocimiento de entidades con nombre v3](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features)

### <a name="october-2019"></a>Octubre de 2019

#### <a name="named-entity-recognition-ner"></a>Reconocimiento de entidades con nombre (NER)

* Un [punto de conexión nuevo](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionPii) para reconocer los tipos de entidad de información personal (solo en inglés).

* Puntos de conexión independientes para el [reconocimiento de entidades](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral) y la [vinculación de entidades](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesLinking).

* [Versión de modelo](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) `2019-10-01`, que incluye:
    * Detección expandida y categorización de entidades detectadas en el texto. 
    * Reconocimiento de los siguientes tipos de entidad nuevos:
        * Número de teléfono
        * Dirección IP

La vinculación de entidades admite el inglés y el español. La compatibilidad de idiomas de NER varía según el tipo de entidad.

#### <a name="sentiment-analysis-v3-public-preview"></a>Versión preliminar pública de la versión 3 de Análisis de sentimiento

* Un [nuevo punto de conexión](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment) para analizar la opinión.
* [Versión de modelo](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features) `2019-10-01`, que incluye:

    * Importantes mejoras respecto a la precisión y el detalle de la categorización del texto y la puntuación de la API.
    * Etiquetado automático para los distintos sentimientos en el texto.
    * Análisis de sentimiento y salida a nivel de documento y oración. 

Admite inglés (`en`), japonés (`ja`), chino simplificado (`zh-Hans`), chino tradicional (`zh-Hant`), francés (`fr`), italiano (`it`), español (`es`), neerlandés (`nl`), portugués (`pt`) y alemán (`de`) y está disponible en las siguientes regiones: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` y `West US 2`. 

> [!div class="nextstepaction"]
> [Más información sobre Análisis de sentimiento v3](how-tos/text-analytics-how-to-sentiment-analysis.md#sentiment-analysis-versions-and-features)

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es Text Analytics API?](overview.md)  
* [Ejemplos de escenarios de usuario](text-analytics-user-scenarios.md)
* [Análisis de opiniones](how-tos/text-analytics-how-to-sentiment-analysis.md)
* [Detección de idioma](how-tos/text-analytics-how-to-language-detection.md)
* [Reconocimiento de entidades](how-tos/text-analytics-how-to-entity-linking.md)
* [Extracción de frases clave](how-tos/text-analytics-how-to-keyword-extraction.md)
