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
ms.date: 02/10/2020
ms.author: aahi
ms.openlocfilehash: 243086ddaae47eba20eea6877fe6d7f8f9889290
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79203498"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics"></a>Uso del reconocimiento de entidades con nombre en Text Analytics

Text Analytics API permite tomar el texto no estructurado y devuelve una lista de entidades desambiguadas con vínculos a más información en la web. La API admite el reconocimiento de entidades con nombre (NER) y la vinculación de entidades.

### <a name="entity-linking"></a>Entity Linking

La vinculación de entidad es la capacidad de identificar y desambiguar la identidad de una entidad que se encuentra en el texto (por ejemplo, determinar si una aparición de la palabra `Mars` hace referencia al planeta o al dios romano de la guerra). Este proceso requiere la presencia de una base de conocimiento en un lenguaje adecuado para vincular entidades reconocidas en el texto. La vinculación de entidades utiliza [Wikipedia](https://www.wikipedia.org/) como base de conocimiento.


### <a name="named-entity-recognition-ner"></a>Reconocimiento de entidades con nombre (NER)

El reconocimiento de entidades con nombre (NER) es la capacidad de identificar diferentes entidades en el texto y clasificarlas en clases o tipos predefinidos, como: persona, ubicación, evento, producto y organización.  

A partir de la versión 3, esta característica de la API Text Analytics también puede identificar tipos de información personal y confidencial, por ejemplo: número de teléfono, número del seguro social, dirección de correo electrónico y número de cuenta bancaria.  La identificación de estas entidades puede ayudar a clasificar documentos confidenciales y a indicar información personal.

## <a name="named-entity-recognition-versions-and-features"></a>Características y versiones del reconocimiento de entidades con nombre

Text Analytics API ofrece dos versiones de reconocimiento de entidades con nombre, la versión 2 y la versión 3. La versión 3 (versión preliminar pública) proporciona mayor detalle en las entidades que se pueden detectar y clasificar.

| Característica                                                         | NER v2 | NER v3 |
|-----------------------------------------------------------------|--------|--------|
| Métodos para solicitudes individuales y por lotes                          | X      | X      |
| Reconocimiento de entidades básicas en varias categorías              | X      | X      |
| Clasificación expandida para entidades reconocidas                 |        | X      |
| Separe los puntos de conexión independientes para enviar las solicitudes de vinculación de entidad y de NER. |        | X      |
| Control de versiones de los modelos                                                |        | X      |

Consulte [Compatibilidad de idioma](../language-support.md#sentiment-analysis-key-phrase-extraction-and-named-entity-recognition) para obtener más información.


#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

### <a name="entity-types"></a>Tipos de entidades

El reconocimiento de entidades con nombre v3 proporciona detección expandida en varios tipos. Actualmente, NER v3 puede reconocer las siguientes categorías de entidades:

* General
* Información personal 

Para obtener una lista detallada de las entidades y los lenguajes compatibles, consulte el artículo sobre los [tipos de entidades que se admiten en NER v3](../named-entity-types.md).

### <a name="request-endpoints"></a>Puntos de conexión de solicitud

El reconocimiento de entidades con nombre v3 usa puntos de conexión independientes para las solicitudes de NER y de vinculación de entidad. Use un formato de dirección URL a continuación en función de la solicitud:

NER
* Entidades generales: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Información personal - `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Vinculación de entidad
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

### <a name="model-versioning"></a>Control de versiones de los modelos

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

### <a name="entity-types"></a>Tipos de entidades

> [!NOTE]
> La versión 2 del reconocimiento de entidades con nombre (NER) solo admite las entidades siguientes. NER v3 se encuentra en versión preliminar pública y expande en gran medida el número y la profundidad de las entidades que se reconocen en el texto.   

| Tipo  | SubType | Ejemplo |
|:-----------   |:------------- |:---------|
| Person        | N/D\*         | "Jeff", "Bill Gates"     |
| Location      | N/D\*         | "Redmond, Washington", "París"  |
| Organización  | N/D\*         | "Microsoft"   |
| Cantidad      | Number        | "6", "seis"     |
| Cantidad      | Porcentaje    | "50 %", "cincuenta por ciento"|
| Cantidad      | Ordinal       | "2.º", "segundo"     |
| Cantidad      | Age           | "90 días","30 años"    |
| Cantidad      | Moneda      | "10,99 $"     |
| Cantidad      | Dimensión     | "10 millas", "40 cm"     |
| Cantidad      | Temperatura   | "32 grados"    |
| DateTime      | N/D\*         | "6:30 p. m., 4 de febrero de 2012"      |
| DateTime      | Date          | "2 de mayo de 2017", "02/05/2017"   |
| DateTime      | Time          | "8 a. m.", "8:00"  |
| DateTime      | DateRange     | "Del 2 de mayo al 5 de mayo"    |
| DateTime      | TimeRange     | "De las 6 p. m. a las 7 p. m."     |
| DateTime      | Duration      | "1 minuto y 45 segundos"   |
| DateTime      | Set           | "Todos los martes"     |
| URL           | N/D\*         | "https:\//www.bing.com"    |
| Email         | N/D\*         | "support@contoso.com" |
| Número de teléfono en EE. UU.  | N/D\*         | (Solo números de teléfono de EE. UU.) "(312) 555-0176" |
| Dirección IP    | N/D\*         | "10.0.0.100" |

\* En función de las entidades de entrada y extraídas, algunas entidades pueden omitir el valor `SubType`.  Todos los tipos de entidad admitidos enumerados solo están disponibles para los idiomas inglés, chino simplificado, francés, alemán y español.

### <a name="request-endpoints"></a>Puntos de conexión de solicitud

El reconocimiento de entidades con nombre v2 usa un punto de conexión único para las solicitudes de NER y de vinculación de entidad:

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

---

## <a name="sending-a-rest-api-request"></a>Envío de una solicitud de API REST

### <a name="preparation"></a>Preparación

Debe tener documentos JSON en este formato: identificador, texto, idioma.

Cada documento tiene un tamaño inferior a 5120 caracteres. Puede tener hasta 1000 elementos (identificadores) por colección. La colección se envía en el cuerpo de la solicitud.

### <a name="structure-the-request"></a>Estructurar la solicitud

Cree una solicitud POST. Puede [usar Postman](text-analytics-how-to-call-api.md) o la **consola de pruebas de API** en los siguientes vínculos de referencia para estructurar y enviar rápidamente una. 

> [!NOTE]
> Puede encontrar la clave y el punto de conexión del recurso de Text Analytics en Azure Portal. Están en la página **Inicio rápido** del recurso, en **Administración de recursos**. 

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

[Referencia del reconocimiento de entidades con nombre (NER) v3](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/EntitiesRecognitionGeneral)

La versión 3 usa puntos de conexión independientes para las solicitudes de NER y de vinculación de entidad. Use un formato de dirección URL a continuación en función de la solicitud:

NER
* Entidades generales: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/general`

* Entidades de información personal: `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/recognition/pii`

Vinculación de entidad
* `https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/entities/linking`

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

[Referencia del reconocimiento de entidades con nombre (NER) v2](https://eastus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/5ac4251d5b4ccd1554da7634)

La versión 2 usa el siguiente punto de conexión para las solicitudes de NER y de vinculación de entidad: 

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/entities`

---

Establezca un encabezado de la solicitud para incluir la clave de la API Text Analytics. En el cuerpo de la solicitud, proporcione los documentos JSON que tiene preparados.

### <a name="example-ner-request"></a>Solicitud de NER de ejemplo 

En el siguiente ejemplo se presenta contenido que podría a la API. El formato de la solicitud es el mismo para las dos versiones de la API.

```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "I had a wonderful trip to Seattle last week."
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

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

### <a name="example-v3-responses"></a>Respuestas de la versión 3 de ejemplo

La versión 3 proporciona puntos de conexión independientes para la vinculación de entidad y para NER. A continuación se muestran las respuestas para ambas operaciones. 

#### <a name="example-ner-response"></a>Respuesta de NER de ejemplo

```json
{
    "documents": [{
    "id": "1",
    "entities": [{
        "text": "Seattle",
        "type": "Location",
        "offset": 26,
        "length": 7,
        "score": 0.80624294281005859
    }, {
        "text": "last week",
        "type": "DateTime",
        "subtype": "DateRange",
        "offset": 34,
        "length": 9,
        "score": 0.8
    }]
    }],
    "errors": [],
    "modelVersion": "2019-10-01"
}
```

#### <a name="example-entity-linking-response"></a>Respuesta de vinculación de entidad de ejemplo

```json
{
  "documents": [{
    "id": "1",
    "entities": [{
      "name": "Seattle",
      "matches": [{
        "text": "Seattle",
        "offset": 26,
        "length": 7,
        "score": 0.15046201222847677
      }],
      "language": "en",
      "id": "Seattle",
      "url": "https://en.wikipedia.org/wiki/Seattle",
      "dataSource": "Wikipedia"
    }]
  }],
  "errors": [],
  "modelVersion": "2019-10-01"
}
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

### <a name="example-ner-v2-response"></a>Respuesta de NER v2 de ejemplo
```json
{
  "documents": [{
    "id": "1",
    "entities": [{
      "name": "Seattle",
      "matches": [{
        "wikipediaScore": 0.15046201222847677,
        "entityTypeScore": 0.80624294281005859,
        "text": "Seattle",
        "offset": 26,
        "length": 7
      }],
      "wikipediaLanguage": "en",
      "wikipediaId": "Seattle",
      "wikipediaUrl": "https://en.wikipedia.org/wiki/Seattle",
      "bingId": "5fbba6b8-85e1-4d41-9444-d9055436e473",
      "type": "Location"
    }, {
      "name": "last week",
      "matches": [{
        "entityTypeScore": 0.8,
        "text": "last week",
        "offset": 34,
        "length": 9
      }],
      "type": "DateTime",
      "subType": "DateRange"
    }]
  }],
  "errors": []
}
```

---

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo de vinculación de entidad mediante Text Analytics de Cognitive Services. En resumen:

* El reconocimiento de entidades con nombre está disponible para determinados idiomas en dos versiones.
* Los documentos JSON del cuerpo de la solicitud incluyen un identificador, un texto y el código de idioma.
* Las solicitudes POST se envían a uno o varios puntos de conexión, con una [clave de acceso y un punto de conexión](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) personalizados, que son válidos para la suscripción.
* La salida de respuesta, que consta de entidades vinculadas (incluidas las puntuaciones de confianza, los desplazamientos y los vínculos web de cada id. de documento) se puede usar en cualquier aplicación.

## <a name="next-steps"></a>Pasos siguientes

* [Información general de Text Analytics](../overview.md)
* [Uso de la biblioteca cliente de Text Analytics](../quickstarts/text-analytics-sdk.md)
* [Novedades](../whats-new.md)
