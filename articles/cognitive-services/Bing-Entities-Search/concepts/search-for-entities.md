---
title: Búsqueda de entidades con Bing Entity Search API
titleSuffix: Azure Cognitive Services
description: Use Bing Entity Search API para extraer y buscar entidades y lugares desde las consultas de búsqueda.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: 532bf806789476c1ec901c1e4ac8522451819625
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93085129"
---
# <a name="searching-for-entities-with-the-bing-entity-api"></a>Búsqueda de entidades con Bing Entity API

> [!WARNING]
> Bing Search APIs se mueve de Cognitive Services a Bing Search Services. A partir del **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de Bing Search APIs con Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que suceda primero.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

## <a name="suggest-search-terms-with-the-bing-autosuggest-api"></a>Sugerencia de términos de búsqueda con Bing Autosuggest API

Si proporciona un cuadro de búsqueda donde el usuario escribe su término de búsqueda, use [Bing Autosuggest API](../../bing-autosuggest/get-suggested-search-terms.md) para mejorar la experiencia. La API devuelve cadenas consulta sugeridas basadas en términos de búsqueda parciales a medida que el usuario escribe.

Una vez que el usuario escribe el término de búsqueda, codifique el término en formato de URL antes de establecer el parámetro de consulta [q](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#query). Por ejemplo, si el usuario escribe *Marcus Appel* , configure `q` como *Marcus+Appel* o *Marcus%20Appel*.

Si el término de búsqueda contiene un error ortográfico, la respuesta de búsqueda incluye un objeto [QueryContext ](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#querycontext). El objeto muestra la ortografía original y la ortografía corregida que Bing utilizó para la búsqueda.

```json
"queryContext": {
    "originalQuery": "hollo wrld",
    "alteredQuery": "hello world",
    "alterationOverrideQuery": "+hollo wrld",
    "adultIntent": false
}
```

## <a name="the-bing-entity-search-api-response"></a>La respuesta de Bing Entity Search API

La respuesta de la API contiene un objeto [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#searchresponse). Si Bing encuentra una entidad o lugar que sea relevante, el objeto incluye el campo `entities`, el campo `places` o ambos. De lo contrario, el objeto de respuesta no incluye ningún campo.
> [!NOTE]
> Las respuestas de las entidades admiten múltiples mercados, pero la respuesta de lugares solo admite ubicaciones comerciales en EE. UU. 

El campo `entities` es un objeto [EntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) que contiene una lista de objetos [Entity](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#entity) (consulte el campo `value`). La lista puede contener una sola entidad dominante, múltiples entidades de desambiguación o ambas. 

Una entidad dominante se devuelve cuando Bing cree que es la única entidad que satisface la solicitud (no hay ambigüedades en cuanto a qué entidad satisface la solicitud). Si es posible que varias entidades satisfagan la solicitud, la lista contiene más de una entidad de desambiguación. Por ejemplo, si la solicitud utiliza el título genérico de la franquicia de una película, la lista probablemente contenga entidades de desambiguación. Sin embargo, si la solicitud indica un título específico de esa franquicia, la lista probablemente contenga una sola entidad dominante.

Las entidades incluyen personajes famosos como cantantes, actores, atletas, modelos, etc ., lugares y monumentos como el monte Rainier o el Lincoln Memorial, y cosas como una banana, un perro caniche un libro o el título de una película. El campo [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#entitypresentationinfo) contiene sugerencias que identifican el tipo de entidad. Por ejemplo, si se trata de una persona, una película, un animal o una atracción. Para obtener una lista de tipos posibles, consulte [Tipos de entidad](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#entity-types).

```json
"entityPresentationInfo": {
    "entityScenario": "DominantEntity",
    "entityTypeHints": ["Attraction"],
    "entityTypeDisplayHint": "Mountain"
}, ...
```

A continuación, se muestra una respuesta que incluye una entidad dominante y de desambiguación.

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Mount Rainier"
    },
    "entities": {
        "value": [{
            "contractualRules": [{
                "_type": "ContractualRules/LicenseAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "license": {
                    "name": "CC-BY-SA",
                    "url": "https://creativecommons.org/licenses/by-sa/3.0/"
                },
                "licenseNotice": "Text under CC-BY-SA license"
            },
            {
                "_type": "ContractualRules/LinkAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "text": "contoso.com",
                "url": "http://contoso.com/mount_rainier"
            },
            {
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount-rainier"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier...",
            "name": "Mount Rainier",
            "url": "http://www.northwindtraders.com/",
            "image": {
                "name": "Mount Rainier",
                "thumbnailUrl": "https://www.bing.com/th?id=A4ae343983daa4...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier"
                }],
                "hostPageUrl": "http://contoso.com/commons/7/72/mount_rain...",
                "width": 110,
                "height": 110
            },
            "description": "Mount Rainier is 14,411 ft tall and the highest mountain...",
            "entityPresentationInfo": {
                "entityScenario": "DominantEntity",
                "entityTypeHints": ["Attraction"]
            },
            "bingId": "38b9431e-cf91-93be-0584-c42a3ecbfdc7"
        },
        {
            "contractualRules": [{
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount_rainier_national_park"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier%20National...",
            "name": "Mount Rainier National Park",
            "url": "http://worldwideimporters.com/",
            "image": {
                "name": "Mount Rainier National Park",
                "thumbnailUrl": "https://www.bing.com/th?id=A91bdc5a1b648a695a39...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier_national_park"
                }],
                "hostPageUrl": "http://contoso.com/en/7/7a...",
                "width": 50,
                "height": 50
            },
            "description": "Mount Rainier National Park is a United States National Park...",
            "entityPresentationInfo": {
                "entityScenario": "DisambiguationItem",
                "entityTypeHints": ["Organization"]
            },
            "bingId": "29d4b681-227a-3924-7bb1-8a54e8666b8c"
        }]
    }
}
```

La entidad incluye los campos `name`, `description` y `image`. Cuando visualiza estos campos en la experiencia de usuario, debe asignarles un atributo. El campo `contractualRules` contiene una lista de atribuciones que debe aplicar. La regla contractual identifica el campo al que se aplica la atribución. Para obtener información sobre cómo aplicar la atribución, consulte [Atribución](#data-attribution).

```json
"contractualRules": [{
    "_type": "ContractualRules/LicenseAttribution",
    "targetPropertyName": "description",
    "mustBeCloseToContent": true,
    "license": {
        "name": "CC-BY-SA",
        "url": "https://creativecommons.org/licenses/by-sa/3.0/"
    },
    "licenseNotice": "Text under CC-BY-SA license"
},
{
    "_type": "ContractualRules/LinkAttribution",
    "targetPropertyName": "description",
    "mustBeCloseToContent": true,
    "text": "contoso.com",
    "url": "http://contoso.com/wiki/Mount_Rainier"
},
{
    "_type": "ContractualRules/MediaAttribution",
    "targetPropertyName": "image",
    "mustBeCloseToContent": true,
    "url": "http://contoso.com/wiki/Mount_Rainier"
}], ...
```

Cuando visualice la información de la entidad (nombre, descripción e imagen), también debe usar la dirección URL en el campo `webSearchUrl` para vincularla a la página de resultados de búsqueda de Bing que contiene la entidad.

## <a name="find-places"></a>Búsqueda de lugares

El campo `places` es un objeto [LocalEntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference) que contiene una lista de objetos de tipo [Place](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#place) (para más información, consulte el apartado [Tipos de entidad](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#entity-types)). La lista contiene una o más entidades locales que satisfacen la solicitud.

El objeto Places incluye restaurantes, hoteles o empresas locales. El campo [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#entitypresentationinfo) contiene sugerencias que identifican el tipo de entidad local. La lista contiene una lista de sugerencias como Place, LocalBusiness, Restaurant. Cada sugerencia sucesiva de la matriz limita el tipo de la entidad. Para obtener una lista de tipos posibles, consulte [Tipos de entidad](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#entity-types).

```json
"entityPresentationInfo": {
    "entityScenario": "ListItem",
    "entityTypeHints": ["Place",
    "LocalBusiness",
    "Restaurant"]
}, ...
```
> [!NOTE]
> Las respuestas de las entidades admiten múltiples mercados, pero la respuesta de lugares solo admite ubicaciones comerciales en EE. UU. 

Las consultas de entidades con conocimiento del entorno local como *restaurantes cercanos* , requieren que la ubicación del usuario proporcione resultados precisos. Sus solicitudes siempre deben usar los encabezados X-Search-Location y X-MSEdge-ClientIP para especificar la ubicación del usuario. Si Bing cree que la consulta se beneficiaría de la ubicación del usuario, establece el campo `askUserForLocation` de [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#querycontext) en **true**. 

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Sinful Bakery and Cafe",
        "askUserForLocation": true
    },
    ...
}
```

El resultado de un lugar incluye el nombre, la dirección, el número de teléfono y la dirección URL de ese lugar en el sitio web de la entidad. Cuando visualice la información de la entidad, también debe usar la dirección URL en el campo `webSearchUrl` para vincularla a la página de resultados de búsqueda de Bing que contiene la entidad.

```json
"places": {
    "value": [{
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Sinful%20Bakery...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "http://libertysdelightfulsinfulbakeryandcafe.com/",
        "entityPresentationInfo": {
            "entityScenario": "ListItem",
            "entityTypeHints": ["Place",
            "LocalBusiness",
            "Restaurant"]
        },
        "address": {
            "addressLocality": "Seattle",
            "addressRegion": "WA",
            "postalCode": "98112",
            "addressCountry": "US",
            "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
    }]
}
```

> [!NOTE]
> Tenga en cuenta que ni usted, ni un tercero en su nombre, pueden usar, retener, guardar, almacenar en caché, compartir o distribuir datos de Entities API con el fin de probar, desarrollar, entrenar, distribuir o ponerlos a disposición cualquier servicio o característica que no sea de Microsoft.  

## <a name="data-attribution"></a>Atribución de datos

Las respuestas de Bing Entity API contienen información que es propiedad de terceros. Usted es responsable de garantizar que su uso sea apropiado; por ejemplo, debe cumplir con cualquier licencia de Creative Commons en la que se base su experiencia de usuario.

Si una respuesta o resultado incluye los campos `contractualRules`, `attributions` o `provider`, debe atribuir los datos. Si la respuesta no incluye ninguno de estos campos, no es necesario realizar la atribución. Si la respuesta incluye el campo `contractualRules` y los campos `attributions` o `provider`, debe usar las reglas contractuales para atribuir los datos.

En el siguiente ejemplo se muestra una entidad que incluye una regla contractual MediaAttribution y una imagen que incluye un campo `provider`. La regla MediaAttribution identifica la imagen como el destino de la regla, por lo que debe ignorar el campo `provider` de la imagen y, en su lugar, usar la regla MediaAttribution para proporcionar la atribución.  

```json
"value": [{
    "contractualRules": [
        ...
        {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://contoso.com/mount_rainier"
        }
    ],
    ...
    "image": {
        "name": "Mount Rainier",
        "thumbnailUrl": "https://www.bing.com/th?id=A46378861201...",
        "provider": [{
            "_type": "Organization",
            "url": "http://contoso.com/mount_rainier"
        }],
        "hostPageUrl": "http://www.graphicdesigninstitute.com/Uploaded...",
        "width": 110,
        "height": 110
    },
    ...
}]
```

Si una regla contractual incluye el campo `targetPropertyName`, la regla solo se aplica al campo de destino. De lo contrario, la regla se aplica al objeto principal que contiene el campo `contractualRules`.

En el siguiente ejemplo, la regla `LinkAttribution` incluye el campo `targetPropertyName`, por lo que la regla se aplica al campo `description`. En cuanto a las reglas que se aplican a campos específicos, debe incluir una línea inmediatamente después de los datos de destino que contienen un hipervínculo al sitio web del proveedor. Por ejemplo, para atribuir la descripción, incluya una línea inmediatamente después del texto descriptivo que contiene un hipervínculo a los datos del sitio web del proveedor; en este caso, debe crear un vínculo a contoso.com.

```json
"entities": {
    "value": [{
            ...
            "description": "Marcus Appel is a former American....",
            ...
            "contractualRules": [{
                    "_type": "ContractualRules/LinkAttribution",
                    "targetPropertyName": "description",
                    "mustBeCloseToContent": true,
                    "text": "contoso.com",
                    "url": "http://contoso.com/cr?IG=B8AD73..."
                 },
            ...
  
```

### <a name="license-attribution"></a>Atribución de licencias

Si la lista de reglas contractuales incluye una regla [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#licenseattribution), debe mostrar el aviso en la línea inmediatamente después del contenido al que se aplica la licencia. La regla `LicenseAttribution` usa el campo `targetPropertyName` para identificar la propiedad a la que se aplica la licencia.

A continuación, se muestra un ejemplo que incluye una regla `LicenseAttribution`.

![Atribución de licencias](../media/cognitive-services-bing-entities-api/licenseattribution.png)

El aviso de licencia que muestre debe incluir un hipervínculo al sitio web que contiene información sobre la licencia. Por lo general, debe convertir el nombre de la licencia en un hipervínculo. Por ejemplo, si el aviso es **Texto con licencia CC-BY-SA** y CC-BY-SA es el nombre de la licencia, debe hacer que CC-BY-SA sea un hipervínculo.

### <a name="link-and-text-attribution"></a>Atribución de vínculo y texto

Las reglas [LinkAttribution](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#linkattribution) y [TextAttribution](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#textattribution) se usan generalmente para identificar al proveedor de los datos. El campo `targetPropertyName` identifica el campo al que se aplica la regla.

Para atribuir los proveedores, incluya una línea inmediatamente después del contenido al que se aplican las atribuciones (por ejemplo, el campo de destino). La línea debe estar claramente etiquetada para indicar que los proveedores son la fuente de los datos. Por ejemplo, "Datos de: contoso.com". En cuanto a las reglas `LinkAttribution`, debe crear un hipervínculo al sitio web del proveedor.

A continuación, se muestra un ejemplo que incluye las reglas `LinkAttribution` y `TextAttribution`.

![Atribución de texto del vínculo](../media/cognitive-services-bing-entities-api/linktextattribution.png)

### <a name="media-attribution"></a>Atribución de medios

Si la entidad incluye una imagen y la visualiza, debe proporcionar un vínculo para obtener acceso al sitio web del proveedor con un simple clic. Si la entidad incluye una regla [MediaAttribution](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference#mediaattribution), use la dirección URL de la regla para crear el vínculo al cual obtener acceso haciendo clic. De lo contrario, use la dirección URL incluida en el campo `provider` de la imagen para crear el vínculo al que se puede acceder con un solo clic.

A continuación, se muestra un ejemplo que incluye el campo `provider` de una imagen y las reglas contractuales. Como el ejemplo incluye la regla contractual, debe ignorar el campo `provider` de la imagen y aplicar la regla `MediaAttribution`.

![Atribución de medios](../media/cognitive-services-bing-entities-api/mediaattribution.png)

### <a name="search-or-search-like-experience"></a>Experiencia de búsqueda o parecida a la búsqueda

Igual que sucede con Bing Web Search API, Bing Entity Search API solo puede utilizarse como resultado de una consulta o búsqueda directa del usuario, o como resultado de una acción dentro de una aplicación o experiencia que lógicamente puede interpretarse como una solicitud de búsqueda del usuario. A modo ilustrativo, los siguientes son algunos ejemplos de búsquedas admitidas o experiencias similares a búsquedas.

- El usuario escribe una consulta directamente en un cuadro de búsqueda en una aplicación.
- El usuario selecciona un texto o imagen específica y solicita "más información" o "información adicional".
- El usuario pregunta a un bot de búsqueda sobre un tema en particular.
- El usuario se detiene en un objeto o entidad particular en un escenario de tipo de búsqueda visual.

Si no está seguro de si su experiencia se puede considerar como una experiencia de búsqueda, se recomienda que lo consulte con Microsoft.

## <a name="throttling-requests"></a>Solicitudes de limitación

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Pasos siguientes

* Pruebe con este [inicio rápido](../quickstarts/csharp.md) para empezar a buscar entidades con Bing Entity Search API.
