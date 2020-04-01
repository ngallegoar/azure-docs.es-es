---
title: Realización de análisis de opiniones con Text Analytics API de REST
titleSuffix: Azure Cognitive Services
description: En este artículo se muestra cómo detectar opiniones en texto con la API de REST de Azure Cognitive Services Text Analytics.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 03/09/2020
ms.author: aahi
ms.openlocfilehash: b3c112876bfd2578e6ebaa95c6902aa9b8f832d9
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "79203464"
---
# <a name="how-to-detect-sentiment-using-the-text-analytics-api"></a>Procedimientos: Detección de opiniones mediante la API Text Analytics

La característica Análisis de sentimiento de la API Text Analytics evalúa el texto y devuelve las etiquetas y puntuaciones de opinión de cada oración, lo que resulta útil para detectar opiniones positivas y negativas tanto en redes sociales como en reseñas de los clientes o en foros de discusión, entre otros. El servicio proporciona los modelos de AI que usa la API, el usuario solo tiene que enviar contenido para su análisis.

> [!TIP]
> Text Analytics proporciona también una imagen de contenedor de Docker basada en Linux para la detección del lenguaje, por lo que puede [instalar y ejecutar el contenedor de Text Analytics](text-analytics-how-to-install-containers.md) cerca de los datos.

Análisis de sentimiento admite una amplia gama de idiomas, y hay más en la versión preliminar. Para más información, consulte [Lenguajes admitidos](../text-analytics-supported-languages.md).

## <a name="concepts"></a>Conceptos

La API Text Analytics utiliza un algoritmo de clasificación de aprendizaje automático para generar una puntuación de las opiniones entre 0 y 1. Las puntuaciones cercanas a 1 indican una opinión positiva y las puntuaciones cercanas a 0 indican una opinión negativa. El análisis de sentimiento se realiza en todo el documento, en lugar de en entidades individuales del texto, lo que significa que las puntuaciones de opinión se devuelven a nivel de documento o de oración. 

El modelo que se usa se entrena previamente con un amplio corpus de texto y asociaciones de opinión. Usa una combinación de técnicas para el análisis, lo que incluye el procesamiento de texto, el análisis de las funciones de sintaxis, la ubicación de las palabras y las asociaciones de palabras. Para más información acerca del algoritmo, consulte la [introducción a Text Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/). Actualmente, no puede proporcionar sus propios datos de entrenamiento. 

Se observa una tendencia de mejora de la puntuación cuando los documentos contienen menos frases, en lugar de un bloque de texto grande. Durante la fase de evaluación de la objetividad, el modelo determina si un documento completo es objetivo o contiene opiniones. Un documento que es mayoritariamente objetivo no pasa a la fase de detección de opiniones, lo que genera una puntuación de 0,5 y detiene el procesamiento. En el caso de los documentos que continúan en la canalización, la siguiente fase genera una puntuación por encima o por debajo de 0,50. Dicha puntuación depende del grado de opinión detectado en el documento.

## <a name="sentiment-analysis-versions-and-features"></a>Versiones y características de Análisis de sentimiento

La API Text Analytics ofrece dos versiones de Análisis de sentimiento, la versión 2 y la versión 3. La versión 3 de Análisis de sentimiento (versión preliminar pública) aporta importantes mejoras a la precisión y al detalle de la puntuación y categorización del texto de la API.

> [!NOTE]
> * El formato de las solicitudes de la versión 3 de Análisis de sentimiento y los [límites de datos](../overview.md#data-limits) son los mismos que en la versión anterior.
> * La versión 3 de Análisis de sentimiento está disponible en las siguientes regiones: `Australia East`, `Central Canada`, `Central US`, `East Asia`, `East US`, `East US 2`, `North Europe`, `Southeast Asia`, `South Central US`, `UK South`, `West Europe` y `West US 2`.

| Característica                                   | Análisis de sentimiento, versión 2 | Análisis de sentimiento, versión 3 |
|-------------------------------------------|-----------------------|-----------------------|
| Métodos para solicitudes individuales y por lotes    | X                     | X                     |
| Puntuaciones de opinión de todo el documento  | X                     | X                     |
| Puntuaciones de opinión de oraciones individuales |                       | X                     |
| Etiquetado de opiniones                        |                       | X                     |
| Control de versiones de los modelos                   |                       | X                     |

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

### <a name="sentiment-scoring"></a>Puntuación de opinión

La versión 3 de Análisis de sentimiento clasifica el texto con etiquetas de opinión (que se describen a continuación). Las puntuaciones devueltas representan la confianza del modelo de que el texto es positivo, negativo o neutro. Cuanto más alto sea el valor, mayor será la confianza. 

### <a name="sentiment-labeling"></a>Etiquetado de opiniones

La versión 3 de Análisis de sentimiento puede devolver las puntuaciones y etiquetas tanto a nivel de documento como a nivel de frase. Las puntuaciones y las etiquetas son `positive`, `negative`, y `neutral`. A nivel de documento, la `mixed` etiqueta de opinión también se puede devolver sin puntuación alguna. La opinión del documento se determina a continuación:

| Opiniones de frases                                                                            | Etiqueta de documento devuelta |
|-----------------------------------------------------------------------------------------------|-------------------------|
| Hay al menos una oración `positive` en el documento. El resto de las oraciones son `neutral`. | `positive`              |
| Hay al menos una oración `negative` en el documento. El resto de las oraciones son `neutral`. | `negative`              |
| Hay al menos una oración `negative` y al menos una oración `positive` en el documento.    | `mixed`                 |
| Todas las oraciones del documento son `neutral`.                                                  | `neutral`               |

### <a name="model-versioning"></a>Control de versiones de los modelos

> [!NOTE]
> El control de versiones del modelo para el análisis de opiniones está disponible a partir de la versión `v3.0-preview.1`.

[!INCLUDE [v3-model-versioning](../includes/model-versioning.md)]

### <a name="example-c-code"></a>Ejemplo de código C#

Puede encontrar un ejemplo de aplicación C# que llama a esta versión dl Análisis de sentimiento en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/TextAnalyticsSentiment.cs).


#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

### <a name="sentiment-scoring"></a>Puntuación de opinión

El analizador de sentimiento clasifica el texto como predominantemente positivo o negativo. Asigna una puntuación en el rengo que oscila entre 0 y 1. Los valores próximos a 0,5 son neutros o indeterminados. Una puntuación de 0,5 indica neutralidad. Cuando el sentimiento de una cadena no se puede analizar o no tiene sentimiento, la puntuación es siempre 0,5. Por ejemplo, si se pasa una cadena de español con un código de idioma inglés, la puntuación es 0,5.

---

## <a name="sending-a-rest-api-request"></a>Envío de una solicitud de API REST 

### <a name="preparation"></a>Preparación

Análisis de sentimiento genera un resultado de mayor calidad cuando se le proporcionan fragmentos de texto más pequeños para trabajar. Esto es opuesto a la extracción de frases clave, que funciona mejor en bloques de texto más grandes. Para obtener los mejores resultados de ambas operaciones, considere la posibilidad de reestructurar las entradas en consecuencia.

Debe tener documentos JSON en este formato: identificador, texto e idioma.

El tamaño del documento debe ser inferior a 5120 caracteres por documento. Puede tener hasta 1000 elementos (identificadores) por colección. La colección se envía en el cuerpo de la solicitud.

## <a name="structure-the-request"></a>Estructurar la solicitud

Cree una solicitud POST. Puede [usar Postman](text-analytics-how-to-call-api.md) o la **consola de pruebas de API** en los siguientes vínculos de referencia para estructurar y enviar rápidamente una. 

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

[Referencia de Análisis de sentimiento, versión 3](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-Preview-1/operations/Sentiment)

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

[Referencia de Análisis de sentimiento, versión 2](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9)

---

Establezca el punto de conexión HTTPS para el análisis de sentimiento, para lo que puede usar un recurso de Text Analytics en Azure o un [contenedor de Text Analytics](text-analytics-how-to-install-containers.md) con instancias. Debe incluir la dirección URL correcta para la versión que desea usar. Por ejemplo:

> [!NOTE]
> Puede encontrar la clave y el punto de conexión del recurso de Text Analytics en Azure Portal. Están en la página **Inicio rápido** del recurso, en **Administración de recursos**. 

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.0-preview.1/sentiment`

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v2.1/sentiment`

---

Establezca un encabezado de la solicitud para incluir la clave de la API Text Analytics. En el cuerpo de la solicitud, proporcione la colección de documentos JSON que preparó para este análisis.

### <a name="example-sentiment-analysis-request"></a>Solicitud de ejemplo de Análisis de sentimiento 

En el siguiente ejemplo se presenta contenido que se podría enviar para el análisis de opiniones. El formato de la solicitud es el mismo para las dos versiones de la API.
    
```json
{
    "documents": [
    {
        "language": "en",
        "id": "1",
        "text": "Hello world. This is some input text that I love."
    },
    {
        "language": "en",
        "id": "2",
        "text": "It's incredibly sunny outside! I'm so happy."
    }
    ],
}
```

### <a name="post-the-request"></a>Publicar la solicitud

El análisis se realiza tras la recepción de la solicitud. Para obtener información sobre el tamaño y el número de solicitudes que puede enviar por minuto y segundo, consulte la sección de [límites de datos](../overview.md#data-limits) de la introducción.

La API Text Analytics no tiene estado. No se almacenan datos en la cuenta y los resultados se devuelven inmediatamente en la respuesta.


### <a name="view-the-results"></a>View the results

El analizador de sentimiento clasifica el texto como predominantemente positivo o negativo. Asigna una puntuación en el rengo que oscila entre 0 y 1. Los valores próximos a 0,5 son neutros o indeterminados. Una puntuación de 0,5 indica neutralidad. Cuando el sentimiento de una cadena no se puede analizar o no tiene sentimiento, la puntuación es siempre 0,5. Por ejemplo, si se pasa una cadena de español con un código de idioma inglés, la puntuación es 0,5.

La salida se devuelve inmediatamente. Puede transmitir los resultados a una aplicación que acepte JSON o guardar la salida en un archivo en el sistema local. Después, importe el resultado en una aplicación que pueda usar para ordenar los datos, realizar búsquedas en ellos y manipularlos. Debido a la compatibilidad con varios idiomas y con Emojis, la respuesta puede contener desplazamientos de texto. Consulte [cómo procesar desplazamientos](../concepts/text-offsets.md) para más información.

#### <a name="version-30-preview"></a>[Versión 3.0 (versión preliminar)](#tab/version-3)

### <a name="sentiment-analysis-v3-example-response"></a>Respuesta de ejemplo de la versión 3 de Análisis de sentimiento

Las respuestas de la versión 3 de Análisis de sentimiento contienen puntuaciones y etiquetas de opinión de cada oración y documento analizados. `documentScores` no se devuelve si la etiqueta de opinión del documento es `mixed`.

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.98570585250854492,
                "neutral": 0.0001625834556762,
                "negative": 0.0141316400840878
            },
            "sentences": [
                {
                    "sentiment": "neutral",
                    "sentenceScores": {
                        "positive": 0.0785155147314072,
                        "neutral": 0.89702343940734863,
                        "negative": 0.0244610067456961
                    },
                    "offset": 0,
                    "length": 12
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.98570585250854492,
                        "neutral": 0.0001625834556762,
                        "negative": 0.0141316400840878
                    },
                    "offset": 13,
                    "length": 36
                }
            ]
        },
        {
            "id": "2",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.89198976755142212,
                "neutral": 0.103382371366024,
                "negative": 0.0046278294175863
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.78401315212249756,
                        "neutral": 0.2067587077617645,
                        "negative": 0.0092281140387058
                    },
                    "offset": 0,
                    "length": 30
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.99996638298034668,
                        "neutral": 0.0000060341349126,
                        "negative": 0.0000275444017461
                    },
                    "offset": 31,
                    "length": 13
                }
            ]
        }
    ],
    "errors": []
}
```

#### <a name="version-21"></a>[Versión 2.1](#tab/version-2)

### <a name="sentiment-analysis-v2-example-response"></a>Respuesta de ejemplo de la versión 2 de Análisis de sentimiento

Las respuestas de la versión 2 de Análisis de sentimiento contienen puntuaciones de opinión de cada documento enviado.

```json
{
  "documents": [{
    "id": "1",
    "score": 0.98690706491470337
  }, {
    "id": "2",
    "score": 0.95202046632766724
  }],
  "errors": []
}
```

---

## <a name="summary"></a>Resumen

En este artículo, ha aprendido los conceptos y el flujo de trabajo del análisis de sentimiento mediante la API Text Analytics. En resumen:

+ Análisis de sentimiento está disponible para determinados idiomas en dos versiones.
+ Los documentos JSON del cuerpo de la solicitud incluyen un identificador, un texto y el código de idioma.
+ La solicitud POST se realiza a un punto de conexión `/sentiment`, mediante una [clave de acceso y un punto de conexión](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource) personalizados, que sean válidos para la suscripción.
+ La salida de la respuesta, que consta de una puntuación de las opiniones de cada identificador de documento, se puede transmitir en secuencias a cualquier aplicación que acepte JSON. Por ejemplo, Excel y Power BI.

## <a name="see-also"></a>Consulte también

* [Información general de Text Analytics](../overview.md)
* [Uso de la biblioteca cliente de Text Analytics](../quickstarts/text-analytics-sdk.md)
* [Novedades](../whats-new.md)
