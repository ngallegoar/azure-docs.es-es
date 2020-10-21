---
title: Uso del reconocimiento de entidades con Text Analytics API
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo identificar y eliminar la ambigüedad de la identidad de una entidad encontrada en el texto con la API REST de Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 10/09/2020
ms.author: aahi
ms.openlocfilehash: f37828d5561ef382f572c1fdd4917a71f8a00407
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91930176"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Uso del reconocimiento de entidades con nombre en Text Analytics

Text Analytics API permite tomar el texto no estructurado y devuelve una lista de entidades desambiguadas con vínculos a más información en la web. La API admite el reconocimiento de entidades con nombre (NER) y la vinculación de entidades.

### <a name="entity-linking"></a>Entity Linking

La vinculación de entidad es la capacidad de identificar y desambiguar la identidad de una entidad que se encuentra en el texto (por ejemplo, determinar si una aparición de la palabra "Marte" hace referencia al planeta o al dios romano de la guerra). Este proceso requiere la presencia de una base de conocimiento en un lenguaje adecuado para vincular entidades reconocidas en el texto. La vinculación de entidades utiliza [Wikipedia](https://www.wikipedia.org/) como base de conocimiento.


### <a name="named-entity-recognition-ner"></a>Reconocimiento de entidades con nombre (NER)

El reconocimiento de entidades con nombre (NER) es la capacidad de identificar diferentes entidades en el texto y clasificarlas en clases o tipos predefinidos, como: persona, ubicación, evento, producto y organización.  

## <a name="named-entity-recognition-versions-and-features"></a>Características y versiones del reconocimiento de entidades con nombre

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

| Característica                                                         | NER v3.0 | NER v3.1-versión preliminar.2 |
|-----------------------------------------------------------------|--------|----------|
| Métodos para solicitudes individuales y por lotes                          | X      | X        |
| Reconocimiento de entidades expandidas en varias categorías           | X      | X        |
| Separe los puntos de conexión independientes para enviar las solicitudes de vinculación de entidad y de NER. | X      | X        |
| Reconocimiento de entidades de información personal (`PII`) y de estado (`PHI`)        |        | X        |

Consulte [Compatibilidad de idioma](../language-support.md) para obtener más información.

### <a name="entity-types"></a>Tipos de entidades

El reconocimiento de entidades con nombre v3 proporciona detección expandida en varios tipos. Actualmente, NER v3.0 puede reconocer entidades en la [categoría de entidad general](../named-entity-types.md).

El Reconocimiento de entidades con nombre v3.1-versión preliminar.2 incluye las capacidades de detección de v3.0 y la capacidad de detectar información personal (`PII`) mediante el punto de conexión `v3.1-preview.2/entities/recognition/pii`. Se puede usar el parámetro opcional `domain=phi` para detectar información de estado confidencial (`PHI`). Vea el artículo [categorías de entidad](../named-entity-types.md) y la sección [puntos de conexión de solicitud](#request-endpoints) a continuación para obtener más información.


## <a name="sending-a-rest-api-request"></a>Envío de una solicitud de API REST

### <a name="preparation"></a>Preparación

Debe tener documentos JSON en este formato: identificador, texto, idioma.

Cada documento tiene un tamaño inferior a 5120 caracteres. Puede tener hasta 1000 elementos (identificadores) por colección. La colección se envía en el cuerpo de la solicitud.

### <a name="structure-the-request"></a>Estructurar la solicitud

Cree una solicitud POST. Puede [usar Postman](text-analytics-how-to-call-api.md) o la **consola de pruebas de API** en los siguientes vínculos de referencia para estructurar y enviar rápidamente una. 

> [!NOTE]
> Puede encontrar la clave y el punto de conexión del recurso de Text Analytics en Azure Portal. Están en la página **Inicio rápido** del recurso, en **Administración de recursos**. 


### <a name="request-endpoints"></a>Puntos de conexión de solicitud

#### <a name="version-31-preview2"></a>[Versión 3.1-versión preliminar.2](#tab/version-3-preview)

El Reconocimiento de entidades con nombre `v3.1-preview.2` usa puntos de conexión independientes para las solicitudes de NER y de vinculación de entidad. Use un formato de dirección URL a continuación en función de la solicitud:

Vinculación de entidad
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/linking`

[Referencia de la versión 3.1-versión preliminar para el reconocimiento de entidades con nombre para `Linking`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesLinking)

NER
* Entidades generales: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/recognition/general`

[Referencia de la versión 3.1-versión preliminar para el reconocimiento de entidades con nombre para `General`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesRecognitionGeneral)

* Información personal (`PII`): `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/recognition/pii`

También se puede usar el parámetro opcional `domain=phi` para detectar información de estado (`PHI`) en el texto. 

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.2/entities/recognition/pii?domain=phi`

Tenga en cuenta la adición de la propiedad `redactedText` en el JSON de respuesta que contiene el texto de entrada modificado donde las entidades PII detectadas se reemplazan por un * por cada carácter de las entidades.

[Referencia de la versión 3.1-versión preliminar para el reconocimiento de entidades con nombre para `PII`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-Preview-2/operations/EntitiesRecognitionPii)

#### <a name="version-30"></a>[Versión 3.0](#tab/version-3)

El reconocimiento de entidades con nombre v3 usa puntos de conexión independientes para las solicitudes de NER y de vinculación de entidad. Use un formato de dirección URL a continuación en función de la solicitud:

Vinculación de entidad
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/linking`

[Referencia de la versión 3.0 para el reconocimiento de entidades con nombre para `Linking`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/EntitiesRecognitionGeneral)

NER
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0/entities/recognition/general`

[Referencia de la versión 3.0 para el reconocimiento de entidades con nombre para `General`](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0/operations/EntitiesRecognitionGeneral)

---

Establezca un encabezado de la solicitud para incluir la clave de la API Text Analytics. En el cuerpo de la solicitud, proporcione los documentos JSON que tiene preparados.

### <a name="example-ner-request"></a>Solicitud de NER de ejemplo 

En el siguiente ejemplo se presenta contenido que podría a la API. El formato de la solicitud es el mismo para las dos versiones de la API.

```json
{
  "documents": [
    {
        "id": "1",
        "language": "en",
        "text": "Our tour guide took us up the Space Needle during our trip to Seattle last week."
    }
  ]
}

```

## <a name="post-the-request"></a>Publicar la solicitud

El análisis se realiza tras la recepción de la solicitud. Consulte la sección de [límites de datos](../overview.md#data-limits) de la introducción para obtener información sobre el tamaño y el número de solicitudes que puede enviar por minuto y segundo.

La API Text Analytics no tiene estado. No se almacenan datos en la cuenta y los resultados se devuelven inmediatamente en la respuesta.

## <a name="view-results"></a>Vista de resultados

Todas las solicitudes POST devuelven una respuesta con formato JSON con los identificadores y las propiedades de entidad detectadas.

La salida se devuelve inmediatamente. Puede transmitir los resultados a una aplicación que acepte JSON o guardar la salida en un archivo en el sistema local y, a continuación, importarlo en una aplicación que permita ordenar, buscar y manipular los datos. Debido a la compatibilidad con varios idiomas y con Emojis, la respuesta puede contener desplazamientos de texto. Consulte [cómo procesar desplazamientos de texto](../concepts/text-offsets.md) para más información.

### <a name="example-responses"></a>Respuestas de ejemplo

La versión 3 proporciona puntos de conexión independientes para la vinculación de entidad y para NER y PII. A continuación se muestran las respuestas para ambas operaciones. 

#### <a name="version-31-preview"></a>[Versión 3.1: versión preliminar](#tab/version-3-preview)

Ejemplo de una respuesta PII:
```json
{
  "documents": [
    {
    "redactedText": "You can even pre-order from their online menu at *************************, call ************ or send email to ***************************!",
    "id": "0",
    "entities": [
        {
        "text": "www.contososteakhouse.com",
        "category": "URL",
        "offset": 49,
        "length": 25,
        "confidenceScore": 0.8
        }, 
        {
        "text": "312-555-0176",
        "category": "Phone Number",
        "offset": 81,
        "length": 12,
        "confidenceScore": 0.8
        }, 
        {
        "text": "order@contososteakhouse.com",
        "category": "Email",
        "offset": 111,
        "length": 27,
        "confidenceScore": 0.8
        }
      ],
    "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-07-01"
}
```

Respuesta de vinculación de entidad de ejemplo:

```json
{
  "documents": [
    {
      "id": "1",
      "entities": [
        {
          "bingId": "f8dd5b08-206d-2554-6e4a-893f51f4de7e", 
          "name": "Space Needle",
          "matches": [
            {
              "text": "Space Needle",
              "offset": 30,
              "length": 12,
              "confidenceScore": 0.4
            }
          ],
          "language": "en",
          "id": "Space Needle",
          "url": "https://en.wikipedia.org/wiki/Space_Needle",
          "dataSource": "Wikipedia"
        },
        {
          "bingId": "5fbba6b8-85e1-4d41-9444-d9055436e473",
          "name": "Seattle",
          "matches": [
            {
              "text": "Seattle",
              "offset": 62,
              "length": 7,
              "confidenceScore": 0.25
            }
          ],
          "language": "en",
          "id": "Seattle",
          "url": "https://en.wikipedia.org/wiki/Seattle",
          "dataSource": "Wikipedia"
        }
      ],
      "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-02-01"
}
```


#### <a name="version-30"></a>[Versión 3.0](#tab/version-3)

Respuesta de NER general de ejemplo:
```json
{
  "documents": [
    {
      "id": "1",
      "entities": [
        {
          "text": "tour guide",
          "category": "PersonType",
          "offset": 4,
          "length": 10,
          "confidenceScore": 0.45
        },
        {
          "text": "Space Needle",
          "category": "Location",
          "offset": 30,
          "length": 12,
          "confidenceScore": 0.38
        },
        {
          "text": "trip",
          "category": "Event",
          "offset": 54,
          "length": 4,
          "confidenceScore": 0.78
        },
        {
          "text": "Seattle",
          "category": "Location",
          "subcategory": "GPE",
          "offset": 62,
          "length": 7,
          "confidenceScore": 0.78
        },
        {
          "text": "last week",
          "category": "DateTime",
          "subcategory": "DateRange",
          "offset": 70,
          "length": 9,
          "confidenceScore": 0.8
        }
      ],
      "warnings": []
    }
  ],
  "errors": [],
  "modelVersion": "2020-04-01"
}
```

---


## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo de vinculación de entidad mediante Text Analytics de Cognitive Services. En resumen:

* Los documentos JSON del cuerpo de la solicitud incluyen un identificador, un texto y el código de idioma.
* Las solicitudes POST se envían a uno o varios puntos de conexión, con una [clave de acceso y un punto de conexión](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) personalizados, que son válidos para la suscripción.
* La salida de respuesta, que consta de entidades vinculadas (incluidas las puntuaciones de confianza, los desplazamientos y los vínculos web de cada id. de documento) se puede usar en cualquier aplicación.

## <a name="next-steps"></a>Pasos siguientes

* [Información general de Text Analytics](../overview.md)
* [Uso de la biblioteca cliente de Text Analytics](../quickstarts/text-analytics-sdk.md)
* [Novedades](../whats-new.md)
