---
title: 'Idiomas admitidos: Text Analytics API'
titleSuffix: Azure Cognitive Services
description: 'Una lista de los idiomas naturales admitidos por Text Analytics API. En este artículo se explica qué idiomas se admiten para cada operación: análisis de opiniones, extracción de frases clave, detección de idioma y reconocimiento de entidad.'
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: aahi
ms.openlocfilehash: c5a413a4fe8d9ac9b7aac59ca78cedc6d5a7a313
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218546"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Compatibilidad de idiomas y regiones para Text Analytics API

En este artículo se explica qué idiomas se admiten para cada operación: análisis de opiniones, extracción de frases clave, detección de idioma y reconocimiento de entidad con nombre.

## <a name="language-detection"></a>Detección de idiomas

La API Text Analytics puede detectar una amplia variedad de idiomas, variantes, dialectos y algunos idiomas regionales o culturales.  La detección de idioma devuelve el "script" de un idioma. Por ejemplo, en el caso de la frase "Tengo un perro", se devolverá `en` en lugar de `en-US`. El único caso especial es el chino, donde la funcionalidad de detección de idioma devolverá `zh_CHS` o `zh_CHT` si puede determinar el script dado el texto proporcionado. En situaciones donde no se puede identificar un script específico para un documento chino, devolverá simplemente `zh`.

No se ha publicado la lista exacta de idiomas de esta característica, pero puede detectar una amplia gama de idiomas, variantes, dialectos y algunos lenguajes cultural o configuración regional. 

Si tiene contenido que se expresa en un idioma que se usa con menos frecuencia, puede probar Detección de idioma para ver si devuelve un código. La respuesta para los idiomas que no se pueden detectar es `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Análisis de sentimiento, Extracción de frases clave y Reconocimiento entidad con nombre

Para el Análisis de sentimiento, la Extracción de frases clave y el Reconocimiento entidad, la lista de idiomas admitidos es más selectiva, ya que los analizadores se perfeccionan para dar cabida a las reglas lingüísticas de idiomas adicionales. En Reconocimiento de entidades con nombre v2, es compatible con el conjunto completo de [tipos de entidad](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features) actualmente está limitada a los idiomas siguientes: 
* Inglés
* Chino simplificado
* Francés
* Alemán
* Español

Para los otros idiomas, solo se devuelven las entidades con nombre `Person`, `Location` y `Organization`.

## <a name="language-list-and-status"></a>Estado y lista de idiomas

La compatibilidad de idiomas se introduce inicialmente en la versión preliminar, pasando de forma gradual al estado de disponible de forma general (GA), independientemente de los demás y del servicio de Text Analytics en general. Los idiomas pueden mantenerse en versión preliminar, incluso mientras se realiza la transición de Text Analytics API a disponible de forma general.

> [!NOTE]
> Para obtener más información sobre la compatibilidad de idioma para la versión preliminar pública del Reconocimiento de entidades con nombre (NER) v3, consulte [tipos de entidades con nombre](named-entity-types.md).

| Idioma              | Código de lenguaje | Opinión | Frases clave | Reconocimiento de entidades con nombre | Vinculación de entidad |       Notas        |
|:----------------------|:-------------:|:---------:|:-----------:|:------------------------:|:--------------:|:------------------:|
| Árabe                |     `ar`      |           |             |           ✔ \*           |                |                    |
| Checo                 |     `cs`      |           |             |           ✔ \*           |                |                    |
| Chino simplificado    |   `zh-hans`   |  ✔ \*\*   |             |            ✔             |                | También se acepta `zh`                   |
| Chino (tradicional)   |   `zh-hant`   |  ✔ \*\*   |             |                          |                |                    |
| Danés                |     `da`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Neerlandés                 |     `nl`      |   ✔ \**   |      ✔      |           ✔ \*           |                |                    |
| Inglés               |     `en`      |   ✔ \**   |      ✔      |          ✔ \*\*          |     ✔ \**      |                    |
| Finés               |     `fi`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Francés                |     `fr`      |   ✔ \**   |      ✔      |            ✔             |                |                    |
| Alemán                |     `de`      |   ✔ \**   |      ✔      |            ✔             |                |                    |
| Griego                 |     `el`      |   ✔ \*    |             |                          |                |                    |
| Húngaro             |     `hu`      |           |             |           ✔ \*           |                |                    |
| Italiano               |     `it`      |   ✔ \**   |      ✔      |           ✔ \*           |                |                    |
| Japonés              |     `ja`      |   ✔ \**   |      ✔      |           ✔ \*           |                |                    |
| Coreano                |     `ko`      |   ✔ \*\*  |      ✔      |           ✔ \*           |                |                    |
| Noruego (Bokmål)   |     `no`      |   ✔ \*    |      ✔      |           ✔ \*           |                | También se acepta `nb`                   |
| Polaco                |     `pl`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Portugués (Portugal) |    `pt-PT`    |   ✔\**    |      ✔      |           ✔ \*           |                | También se acepta `pt` |
| Portugués (Brasil)   |    `pt-BR`    |           |      ✔      |           ✔ \*           |                |                    |
| Ruso               |     `ru`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Español               |     `es`      |   ✔\**    |      ✔      |           ✔ \*           |     ✔ \**      |                    |
| Sueco               |     `sv`      |   ✔ \*    |      ✔      |           ✔ \*           |                |                    |
| Turco               |     `tr`      |   ✔ \*    |             |           ✔ \*           |                |                    |

\* La compatibilidad con idiomas está en versión preliminar

\** También está disponible en las versiones preliminares públicas [Análisis de sentimiento v3](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis#sentiment-analysis-versions-and-features) o [Reconocimiento de entidades con nombre V3](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-versions-and-features).

## <a name="see-also"></a>Consulte también

[Página de documentación de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/)   
[Página de producto de Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
