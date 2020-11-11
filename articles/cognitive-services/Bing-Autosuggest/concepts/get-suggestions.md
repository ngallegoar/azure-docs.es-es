---
title: Sugerencia de términos de búsqueda con Bing Autosuggest API
titleSuffix: Azure Cognitive Services
description: En este artículo se describe el concepto de sugerir términos de consulta con Bing Autosuggest API y el impacto de la longitud de la consulta en la pertinencia.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: cdbbd6afeedc1c8808e02aefa268be4fe0de5f9f
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94363381"
---
# <a name="suggesting-query-terms"></a>Sugerencia de términos de consulta

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Generalmente, llamaría a Bing Autosuggest API cada vez que un usuario escribiera un carácter en el cuadro de búsqueda de la aplicación. La integridad de la cadena de consulta afecta a la relevancia de los términos de la consulta sugeridos que devuelve la API. Cuanto más completa sea la cadena de consulta, más apropiada será la lista de términos de consulta sugeridos. Por ejemplo, las sugerencias que puede devolver la API para `s` es probable que tengan menos relevancia que las consultas que devuelve para `sailing dinghies`.

## <a name="example-request"></a>Solicitud de ejemplo

En el ejemplo siguiente se muestra una solicitud que devuelve las cadenas de consulta sugeridas para *navegar*. Recuerde codificar como dirección URL el término de consulta parcial del usuario al establecer el parámetro de consulta [q](/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference#query). Por ejemplo, si el usuario escribe *sailing les* , establezca `q` en `sailing+les` o `sailing%20les`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

La respuesta siguiente contiene una lista de objetos [SearchAction](/rest/api/cognitiveservices-bingsearch/bing-autosuggest-api-v7-reference#searchaction) que contienen los términos de consulta sugeridos.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

## <a name="using-suggested-query-terms"></a>Uso de términos de consulta sugeridos

Cada sugerencia incluye un campo `displayText`, `query` y `url`. El campo `displayText` contiene la consulta sugerida que usa para rellenar la lista desplegable del cuadro de búsqueda. Debe mostrar todas las sugerencias que incluye la respuesta, y en el orden especificado.

El ejemplo siguiente muestra un cuadro de búsqueda desplegable con los términos de consulta sugeridos de Bing Autosuggest API.

![Lista del cuadro de búsqueda desplegable de AutoSuggest](../media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Si el usuario selecciona una consulta sugerida en la lista desplegable, puede usar la cadena de consulta del campo `query` para llamar a [Bing Web Search API](../../bing-web-search/overview.md) y ver los resultados por sí mismo. O bien, puede usar la dirección URL del campo `url` para enviar al usuario a la página de resultados de la búsqueda de Bing.

## <a name="next-steps"></a>Pasos siguientes

* [Qué es Bing Autosuggest API](../get-suggested-search-terms.md)