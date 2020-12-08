---
title: Llamada a la API Text Analytics
titleSuffix: Azure Cognitive Services
description: En este artículo se explica cómo se puede llamar a la API REST y a Postman de Text Analytics de Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 11/19/2020
ms.author: aahi
ms.openlocfilehash: 2977946b2e1f37aa356ee075d2caac237170df0f
ms.sourcegitcommit: 9889a3983b88222c30275fd0cfe60807976fd65b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "95993326"
---
# <a name="how-to-call-the-text-analytics-rest-api"></a>Cómo llamar a la API REST de Text Analytics

En este artículo, se usan la API REST de Text Analytics y [Postman](https://www.postman.com/downloads/) para mostrar los conceptos clave. La API proporciona varios puntos de conexión sincrónicos y asincrónicos para usar las características del servicio. 

## <a name="using-the-api-asynchronously"></a>Uso de la API de forma asincrónica

A partir de v3.1-preview.3, la API Text Analytics proporciona dos puntos de conexión asincrónicos: 

* El punto de conexión `/analyze` para Text Analytics permite analizar el mismo conjunto de documentos de texto con varias características de análisis de texto en una llamada API. Anteriormente, para usar varias características sería preciso realizar llamadas API independientes para cada operación. Tenga en cuenta esta funcionalidad cuando necesite analizar grandes conjuntos de documentos con más de una característica de Text Analytics.

* El punto de conexión `/health` para Text Analytics fo Health, que puede extraer y etiquetar información médica relevante de documentos clínicos.  

En la tabla siguiente puede ver qué características se pueden usar de forma asincrónica. Tenga en cuenta que desde el punto de conexión `/analyze` solo se puede llamar a unas pocas características. 

| Característica | Sincrónica | Asincrónica |
|--|--|--|
| Detección de idiomas | ✔ |  |
| análisis de opiniones | ✔ |  |
| Minería de opiniones | ✔ |  |
| Extracción de la frase clave | ✔ | ✔* |
| Reconocimiento de entidades con nombre (se incluyen PII y PHI) | ✔ | ✔* |
| Text Analytics for Health (contenedor) | ✔ |  |
| Text Analytics for Health (API) |  | ✔  |

`*`: se le llama de forma asincrónica a través del punto de conexión `/analyze` .


[!INCLUDE [text-analytics-api-references](../includes/text-analytics-api-references.md)]

[!INCLUDE [v3 region availability](../includes/v3-region-availability.md)]

## <a name="prerequisites"></a>Prerrequisitos


> [!NOTE]
> Necesitará un recurso de Text Analytics mediante un [plan de tarifa](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/) Estándar (S) si desea utilizar los puntos de conexión `/analyze` o `/health`.

1.  En primer lugar, vaya a [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) y cree un recurso de Text Analytics, en caso de que no tenga ninguno. Elija el plan de tarifa Estándar (S) si desea utilizar los puntos de conexión `/analyze` o `/health`.

2.  Seleccione la región que desee que use su punto de conexión.

3.  Cree el recurso de Text Analytics y vaya a la "hoja de claves y puntos de conexión" a la izquierda de la página. Copie la clave que va a usar más adelante cuando llame a las API. La agregará más adelante como valor en el encabezado `Ocp-Apim-Subscription-Key` .


<a name="json-schema"></a>

## <a name="api-request-format"></a>Formato de solicitud de API

#### <a name="synchronous"></a>[Sincrónica](#tab/synchronous)

El formato de las solicitudes de API es el mismo para todas las operaciones sincrónicas. Los documentos se envían en un objeto JSON como texto no estructurado sin formato. XML no se admite. El esquema JSON consta de los elementos que se describen a continuación.

| Elemento | Valores válidos | ¿Necesario? | Uso |
|---------|--------------|-----------|-------|
|`id` |El tipo de datos es una cadena, pero en la práctica, los identificadores de documento tienden a ser números enteros. | Obligatorio | El sistema usa los identificadores que proporcione para estructurar la salida. Se generan códigos de idioma, frases clave y puntuaciones de opinión para cada identificador de la solicitud.|
|`text` | Texto sin formato no estructurado, hasta 5120 caracteres. | Obligatorio | Para la detección de idioma, el texto se puede expresar en cualquier idioma. Para el análisis de opiniones, la extracción de frases clave y la identificación de entidades, el texto debe estar en un [idioma admitido](../language-support.md). |
|`language` | Código [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) de dos caracteres para un [idioma admitido](../language-support.md) | Varía | Análisis de opiniones, extracción de frases clave, detección de idioma y vinculación de entidades; opcional para la detección de idioma. No hay ningún error si se excluye, pero sin él se debilita el análisis. El código de idioma se debe corresponder al `text` que proporcione. |

A continuación verá un ejemplo de una solicitud de API para los puntos de conexión de Text Analytics sincrónicos. 

```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Sample text to be sent to the text analytics api."
    }
  ]
}
```

#### <a name="analyze"></a>[Analizar](#tab/analyze)

> [!NOTE]
> La versión preliminar más reciente de la biblioteca cliente de Text Analytics permite llamar a operaciones asincrónicas de Analyze mediante un objeto de cliente. Puede encontrar ejemplos en GitHub:
* [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics)
* [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/)
* [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)

El punto de conexión `/analyze` le permite elegir cuál de las características compatibles de Text Analytics desea usar en una llamada API individual. Este punto de conexión actualmente admite:

* la extracción de frases clave 
* Reconocimiento de entidades con nombre (se incluyen PII y PHI)

| Elemento | Valores válidos | ¿Necesario? | Uso |
|---------|--------------|-----------|-------|
|`displayName` | String | Opcional | Se usa como nombre para mostrar para el identificador único en el trabajo.|
|`analysisInput` | Incluye el campo `documents` siguiente | Obligatorio | Contiene la información de los documentos que desea enviar. |
|`documents` | Incluye los campos `id` y `text` siguientes | Obligatorio | Contiene información de cada documento que se envía y el texto sin formato del documento. |
|`id` | String | Obligatorio | Los identificadores que proporcione se usan para estructurar la salida. |
|`text` | Texto sin formato no estructurado, hasta 125 000 caracteres. | Obligatorio | Debe estar en inglés, que es el único idioma que se admite actualmente. |
|`tasks` | Incluye las siguientes características de Text Analytics: `entityRecognitionTasks`, `keyPhraseExtractionTasks` o `entityRecognitionPiiTasks`. | Obligatorio | Una o varias de las características Text Analytics que desea usar. Tenga en cuenta que `entityRecognitionPiiTasks` tiene un parámetro `domain` opcional que se puede establecer en `pii` o `phi`. Si no se especifica, el valor predeterminado del sistema es `pii`. |
|`parameters` | Incluye los campos `model-version` y `stringIndexType` siguientes | Obligatorio | Este campo está incluido en las tareas de características anteriores que elija. Contienen información no solo sobre la versión del modelo que desea utilizar, sino también sobre el tipo de índice. |
|`model-version` | String | Obligatorio | Especifique la versión del modelo que desea usar que se va a llamar.  |
|`stringIndexType` | String | Obligatorio | Especifique el descodificador de texto que se ajuste a su entorno de programación.  Los tipos que se admiten son: `textElement_v8` (predeterminado), `unicodeCodePoint` y `utf16CodeUnit`. Para más información, consulte el [artículo sobre desplazamientos de texto](../concepts/text-offsets.md#offsets-in-api-version-31-preview).  |
|`domain` | String | Opcional | Solo se aplica como parámetro a la tarea `entityRecognitionPiiTasks` y se puede establecer en `pii` o `phi`. Si no se especifica, su valor predeterminado es `pii`.  |

```json
{
    "displayName": "My Job",
    "analysisInput": {
        "documents": [
            {
                "id": "doc1",
                "text": "It's incredibly sunny outside! I'm so happy"
            },
            {
                "id": "doc2",
                "text": "Pike place market is my favorite Seattle attraction."
            }
        ]
    },
    "tasks": {
        "entityRecognitionTasks": [
            {
                "parameters": {
                    "model-version": "latest",
                    "stringIndexType": "TextElements_v8"
                }
            }
        ],
        "keyPhraseExtractionTasks": [{
            "parameters": {
                "model-version": "latest"
            }
        }],
        "entityRecognitionPiiTasks": [{
            "parameters": {
                "model-version": "latest"
            }
        }]
    }
}

```

#### <a name="text-analytics-for-health"></a>[Text Analytics for Health](#tab/health)

El formato de las solicitudes de API a la API hospedada de Text Analytics for Health es el mismo que el de su contenedor. Los documentos se envían en un objeto JSON como texto no estructurado sin formato. XML no se admite. El esquema JSON consta de los elementos que se describen a continuación.  Rellene y envíe el [formulario de solicitud de Cognitive Services](https://aka.ms/csgate) para solicitar acceso a la versión preliminar pública de Text Analytics for Health. El uso de Text Analytics for Health no se facturará. 

| Elemento | Valores válidos | ¿Necesario? | Uso |
|---------|--------------|-----------|-------|
|`id` |El tipo de datos es una cadena, pero en la práctica, los identificadores de documento tienden a ser números enteros. | Obligatorio | El sistema usa los identificadores que proporcione para estructurar la salida. |
|`text` | Texto sin formato no estructurado, hasta 5120 caracteres. | Obligatorio | Tenga en cuenta que en la actualidad solo se admite texto en inglés. |
|`language` | Código [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) de dos caracteres para un [idioma admitido](../language-support.md) | Obligatorio | Actualmente solo se admite `en`. |

A continuación verá un ejemplo de una solicitud de API para los puntos de conexión de Text Analytics for Health. 

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Subject was administered 100mg remdesivir intravenously over a period of 120 min"
    }
  ]
}
```

---

>[!TIP]
> Consulte el artículo [Límites de datos y velocidad de la API Text Analytics](../concepts/data-limits.md) para obtener información sobre los límites de tamaño y velocidad para enviar datos a la API Text Analytics.


## <a name="set-up-a-request"></a>Configuración de una solicitud 

En Postman (o cualquier otra herramienta para pruebas de API web), agregue el punto de conexión de la característica que desea usar. Use la tabla siguiente para buscar el formato de punto de conexión adecuado y reemplace `<your-text-analytics-resource>` por el punto de conexión del recurso. Por ejemplo:

`https://my-resource.cognitiveservices.azure.com/text/analytics/v3.0/languages`

#### <a name="synchronous"></a>[Sincrónica](#tab/synchronous)

| Característica | Tipo de solicitud | Puntos de conexión de recursos |
|--|--|--|
| Detección de idiomas | POST | `<your-text-analytics-resource>/text/analytics/v3.0/languages` |
| análisis de opiniones | POST | `<your-text-analytics-resource>/text/analytics/v3.0/sentiment` |
| Minería de opiniones | POST | `<your-text-analytics-resource>/text/analytics/v3.0/sentiment?opinionMining=true` |
| Extracción de la frase clave | POST | `<your-text-analytics-resource>/text/analytics/v3.0/keyPhrases` |
| Reconocimiento de entidades con nombre: general | POST | `<your-text-analytics-resource>/text/analytics/v3.0/entities/recognition/general` |
| Reconocimiento de entidades con nombre: PII | POST | `<your-text-analytics-resource>/text/analytics/v3.0/entities/recognition/pii` |
| Reconocimiento de entidades con nombre: PHI | POST |  `<your-text-analytics-resource>/text/analytics/v3.0/entities/recognition/pii?domain=phi` |

#### <a name="analyze"></a>[Analizar](#tab/analyze)

| Característica | Tipo de solicitud | Puntos de conexión de recursos |
|--|--|--|
| Envío de trabajo de análisis | POST | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/analyze` |
| Obtención del estado y los resultados del análisis | GET | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/analyze/jobs/<Operation-Location>` |

#### <a name="text-analytics-for-health"></a>[Text Analytics for Health](#tab/health)

| Característica | Tipo de solicitud | Puntos de conexión de recursos |
|--|--|--|
| Envío del trabajo de Text Analytics for Health  | POST | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/entities/health/jobs` |
| Obtención del estado y resultados del trabajo | GET | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/entities/health/jobs/<Operation-Location>` |
| Cancelar trabajo | DELETE | `https://<your-text-analytics-resource>/text/analytics/v3.1-preview.3/entities/health/jobs/<Operation-Location>` |

--- 

Una vez que tenga el punto de conexión, en Postman (o en otra herramienta para pruebas de API web):

1. Elija el tipo de solicitud de la característica que desea usar.
2. Pegue el punto de conexión de la operación adecuada que desee de la tabla anterior.
3. Establezca los tres encabezados de solicitud:

   + `Ocp-Apim-Subscription-Key`: la clave de acceso, se obtiene en Azure Portal
   + `Content-Type`: application/json
   + `Accept`: application/json

    Si utiliza Postman, la solicitud debe ser similar a la captura de pantalla siguiente, siempre que el punto de conexión sea `/keyPhrases`.
    
    ![Captura de pantalla de solicitud con el punto de conexión y los encabezados](../media/postman-request-keyphrase-1.png)
    
4. Elija **raw** (sin formato) como formato en **Body** (Cuerpo).
    
    ![Captura de pantalla de la solicitud con la configuración del cuerpo](../media/postman-request-body-raw.png)

5. Pegue algunos documentos JSON con un formato válido. Use los ejemplos de la sección **Formato de solicitud de API** anterior y para más información y ejemplos, vea los temas siguientes:

      + [Detección de idioma](text-analytics-how-to-language-detection.md)
      + [Extracción de frases clave](text-analytics-how-to-keyword-extraction.md)
      + [Análisis de opiniones](text-analytics-how-to-sentiment-analysis.md)
      + [Reconocimiento de entidades](text-analytics-how-to-entity-linking.md)

## <a name="send-the-request"></a>Envío de la solicitud

Envíe la solicitud de API. Si realizó la llamada a un punto de conexión sincrónico, la respuesta se mostrará inmediatamente, en forma de un documento JSON individual, con un elemento para cada identificador de documento proporcionado en la solicitud.

Si realizó la llamada a los puntos de conexión `/analyze` o `/health` asincrónicos, compruebe que recibió un código de respuesta 202. Tendrá que obtener la respuesta para ver los resultados:

1. En la respuesta de la API, busque `Operation-Location` en el encabezado, que identifica el trabajo que envió a la API. 
2. Cree una solicitud GET para el punto de conexión que usó. En la [tabla anterior](#set-up-a-request) encontrará el formato del punto de conexión.Consulte la [documentación de referencia de la API](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/AnalyzeStatus). Por ejemplo:

    `https://my-resource.cognitiveservices.azure.com/text/analytics/v3.1-preview.3/analyze/jobs/<Operation-Location>`

3. Agregue `Operation-Location` a la solicitud.

4. La respuesta será un solo documento JSON, con un elemento para cada identificador de documento proporcionado en la solicitud.

## <a name="example-api-responses"></a>Respuestas de ejemplo de la API
 
# <a name="synchronous"></a>[Sincrónica](#tab/synchronous)

Las respuestas sincrónicas del punto de conexión variarán en función del punto de conexión que se use. Para ver algunas respuestas de ejemplo, consulte los siguientes artículos.

+ [Detección de idioma](text-analytics-how-to-language-detection.md#step-3-view-the-results)
+ [Extracción de frases clave](text-analytics-how-to-keyword-extraction.md#step-3-view-results)
+ [Análisis de opiniones](text-analytics-how-to-sentiment-analysis.md#view-the-results)
+ [Reconocimiento de entidades](text-analytics-how-to-entity-linking.md#view-results)

# <a name="analyze"></a>[Analizar](#tab/analyze)

Si se ha realizado correctamente, la solicitud GET al punto de conexión `/analyze` devolverá un objeto que contiene las tareas asignadas. Por ejemplo, `keyPhraseExtractionTasks`. Estas tareas contienen el objeto de respuesta de la característica de Text Analytics adecuada. Para más información, consulta los siguientes artículos.

+ [Extracción de frases clave](text-analytics-how-to-keyword-extraction.md#step-3-view-results)
+ [Reconocimiento de entidades](text-analytics-how-to-entity-linking.md#view-results)


```json
{
  "displayName": "My Analyze Job",
  "jobId": "dbec96a8-ea22-4ad1-8c99-280b211eb59e_637408224000000000",
  "lastUpdateDateTime": "2020-11-13T04:01:14Z",
  "createdDateTime": "2020-11-13T04:01:13Z",
  "expirationDateTime": "2020-11-14T04:01:13Z",
  "status": "running",
  "errors": [],
  "tasks": {
      "details": {
          "name": "My Analyze Job",
          "lastUpdateDateTime": "2020-11-13T04:01:14Z"
      },
      "completed": 1,
      "failed": 0,
      "inProgress": 2,
      "total": 3,
      "keyPhraseExtractionTasks": [
          {
              "name": "My Analyze Job",
              "lastUpdateDateTime": "2020-11-13T04:01:14.3763516Z",
              "results": {
                  "inTerminalState": true,
                  "documents": [
                      {
                          "id": "doc1",
                          "keyPhrases": [
                              "sunny outside"
                          ],
                          "warnings": []
                      },
                      {
                          "id": "doc2",
                          "keyPhrases": [
                              "favorite Seattle attraction",
                              "Pike place market"
                          ],
                          "warnings": []
                      }
                  ],
                  "errors": [],
                  "modelVersion": "2020-07-01"
              }
          }
      ]
  }
}
```

# <a name="text-analytics-for-health"></a>[Text Analytics for Health](#tab/health)

En el artículo siguiente encontrará más información de la respuesta de API asincrónica de Text Analytics for Health:

+ [Text Analytics for Health](text-analytics-for-health.md#hosted-asynchronous-web-api-response)


--- 

## <a name="see-also"></a>Consulte también

* [Información general de Text Analytics](../overview.md)
* [Preguntas más frecuentes](../text-analytics-resource-faq.md)</br>
* [Página del producto de Text Analytics](//go.microsoft.com/fwlink/?LinkID=759712)
* [Uso de la biblioteca cliente de Text Analytics](../quickstarts/text-analytics-sdk.md)
* [Novedades](../whats-new.md)
