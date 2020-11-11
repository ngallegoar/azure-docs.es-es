---
title: Uso de Bing Spell Check API
titleSuffix: Azure Cognitive Services
description: Conozca los modos de Bing Spell Check, la configuración y otra información relacionada con la API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: aahi
ms.openlocfilehash: a02d5217cb051516e11d17730f31869618a2cfb0
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94369399"
---
# <a name="using-the-bing-spell-check-api"></a>Uso de Bing Spell Check API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

En este artículo puede aprender sobre el uso de Bing Spell Check API para realizar la revisión ortográfica y gramatical contextual. Aunque la mayoría de los correctores ortográficos dependen de conjuntos de reglas basadas en diccionario, el corrector ortográfico de Bing aprovecha el aprendizaje automático y la traducción automática estadística para proporcionar correcciones precisas y contextuales. 

## <a name="spell-check-modes"></a>Modos de comprobación ortográfica

La API admite dos modos de corrección, `Proof` y `Spell`.  Vea los ejemplos [aquí](https://azure.microsoft.com/services/cognitive-services/spell-check/).

### <a name="proof---for-documents"></a>Proof: para documentos 

El modo predeterminado es `Proof`. El modo de corrección ortográfica `Proof` es el que proporciona las comprobaciones más completas, agrega la capitalización, la puntuación básica y otras características que ayudan en la creación de documentos. Pero está disponible solo en los mercados de en-US (inglés de Estados Unidos), es-ES (español) y pt-BR (portugués) (Nota: en el caso de español y portugués solo en versión beta). Para los restantes mercados, establezca el parámetro de consulta de modo en Ortografía. 

> [!NOTE]
> Si el texto de la consulta tiene más de 4096 caracteres, se truncará a 4096 y luego se procesará. 

### <a name="spell----for-web-searchesqueries"></a>Spell: consultas y búsquedas web

`Spell` es más agresivo para devolver mejores resultados de búsqueda. El modo `Spell` busca la mayoría de los errores ortográficos, pero no encuentra algunos de los errores gramaticales que `Proof` detecta, por ejemplo, las palabras repetidas o los errores de mayúsculas y minúsculas.

> [!NOTE]
> * La longitud máxima de consulta admitida es inferior. Si la consulta supera la longitud máxima, ni la consulta ni sus resultados se modificarán.
>    * 130 caracteres para los códigos de idioma siguientes: en, de, es, fr, pl, pt, sv, ru, nl, nb, tr-tr, it, zh y ko. 
>    * 65 caracteres para los demás.
> * El modo de corrección ortográfica no admite caracteres de corchetes (`[` y `]`) en las consultas y puede causar resultados incoherentes. Se recomienda eliminarlos de las consultas cuando se usa el modo de corrección ortográfica.

## <a name="market-setting"></a>Configuración del mercado

Un [código de mercado](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference#market-codes) se debe especificar con el parámetro de consulta `mkt` en la solicitud. De lo contrario, la API usará un mercado predeterminado en función de la dirección IP de la solicitud.


## <a name="http-post-and-get-support"></a>Compatibilidad con HTTP POST y GET

La API admite de HTTP POST o HTTP GET. Lo que se usa depende de la longitud del texto que se va a revisar. Si todas las cadenas tienen menos de 1500 caracteres, usaría una operación GET. Pero si desea admitir cadenas de hasta 10 000 caracteres, usaría POST. La cadena de texto puede incluir cualquier carácter UTF-8 válido.

El ejemplo siguiente muestra una solicitud POST para comprobar la ortografía y la gramática de una cadena de texto. El ejemplo incluye el parámetro de consulta [mode](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference#mode) por razones de integridad (se podría se han omitido, ya que el valor predeterminado de `mode` es Proof). El parámetro de consulta [text](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference#text) contiene la cadena que deben revisar.
  
```  
POST https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?mode=proof&mkt=en-us HTTP/1.1  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 47  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
 
text=when+its+your+turn+turn,+john,+come+runing  
``` 

Si utiliza HTTP GET, incluiría el parámetro de consulta `text` en la cadena de consulta de la dirección URL
  
A continuación se muestra la respuesta a la solicitud anterior. La respuesta contiene un objeto [SpellCheck](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference#spellcheck). 
  
```json
{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  
  
El campo [flaggedTokens](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference#flaggedtokens) enumera los errores de ortografía y gramática que la API encontró en la cadena [text](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference#text). El campo `token` contiene la palabra que se va a reemplazar. Usaría el desplazamiento de base cero en el campo `offset` para buscar el token en la cadena `text`. Luego, reemplazaría la palabra de dicha ubicación por la palabra del campo `suggestion`. 

Si el campo `type` es RepeatedToken, puede reemplazar el token por `suggestion`, pero es probable que también necesite quitar el espacio final.

## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Bing Spell Check API?](../overview.md)
- [Referencia de Bing Spell Check API v7](/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)