---
title: 'Extracción de datos: LUIS'
description: Extraiga datos de texto de expresiones con intenciones y entidades. Conozca qué tipo de datos se pueden extraer de Language Understanding (LUIS)
author: diberry
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: 3cea4a46564210ad8c37fdeda68e24337091d0bb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82100301"
---
# <a name="extract-data-from-utterance-text-with-intents-and-entities"></a>Extracción de datos de texto de expresiones con intenciones y entidades
LUIS ofrece la capacidad de obtener información de expresiones de lenguaje natural de un usuario. La información se extrae de manera que pueda ser usada por un programa, una aplicación o un bot de chat para tomar medidas. En las secciones siguientes, obtendrá información sobre qué datos se devuelven de las intenciones y entidades con ejemplos de JSON.

Los datos más difíciles de extraer son los datos de aprendizaje automático, porque no son una coincidencia de texto exacta. El proceso de extracción de datos de [entidades](luis-concept-entity-types.md) de aprendizaje automático debe formar parte del [ciclo de creación](luis-concept-app-iteration.md) hasta que esté seguro de que recibirá los datos que espera.

## <a name="data-location-and-key-usage"></a>Ubicación de los datos y uso de la clave
LUIS proporciona los datos del [punto de conexión](luis-glossary.md#endpoint) publicado. La **solicitud HTTPS** (POST o GET) contiene la expresión, así como algunas configuraciones opcionales como los entornos de producción o de almacenamiento provisional.

#### <a name="v2-prediction-endpoint-request"></a>[Solicitud de punto de conexión de predicción de V2](#tab/V2)

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

#### <a name="v3-prediction-endpoint-request"></a>[Solicitud de punto de conexión de predicción de V3](#tab/V3)

`https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/<appID>/slots/<slot-type>/predict?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&query=book 2 tickets to paris`

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

El valor de `appID` está disponible en la página **Configuración** de la aplicación LUIS, así como parte de la dirección URL (después de `/apps/`) cuando edite esa aplicación LUIS. El valor de `subscription-key` es la clave de punto de conexión que se ha usado para consultar a la aplicación. Aunque puede usar la clave de inicio o creación gratis mientras se familiariza con LUIS, es importante que cambie la clave del punto de conexión por una clave que admita el [uso esperado de LUIS](luis-limits.md#key-limits). La unidad de `timezoneOffset` es minutos.

La **respuesta HTTPS** contiene toda la información de la intención y la entidad que LUIS puede determinar en función del modelo actual publicado de un punto de conexión de producción o de almacenamiento provisional. La dirección URL del punto de conexión se encuentra en el sitio web de [LUIS](luis-reference-regions.md), en la sección **Administrar**, en la página **Claves y puntos de conexión**.

## <a name="data-from-intents"></a>Datos de intenciones
Los datos principales son el **nombre de la intención** de puntuación superior. La respuesta del punto de conexión es:

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

```JSON
{
  "query": "when do you open next?",
  "prediction": {
    "normalizedQuery": "when do you open next?",
    "topIntent": "GetStoreInfo",
    "intents": {
        "GetStoreInfo": {
            "score": 0.984749258
        }
    }
  },
  "entities": []
}
```

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

|Objeto de datos|Tipo de datos|Ubicación de los datos|Value|
|--|--|--|--|
|Intención|String|topScoringIntent.intent|"GetStoreInfo"|

Si el bot de chat o aplicación de llamada a LUIS toma una decisión en función de más de una puntuación de intención, devuelve todas las puntuaciones de intenciones.


#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

Establezca el parámetro querystring, `verbose=true`. La respuesta del punto de conexión es:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Establezca el parámetro querystring, `show-all-intents=true`. La respuesta del punto de conexión es:

```JSON
{
    "query": "when do you open next?",
    "prediction": {
        "normalizedQuery": "when do you open next?",
        "topIntent": "GetStoreInfo",
        "intents": {
            "GetStoreInfo": {
                "score": 0.984749258
            },
            "None": {
                 "score": 0.2040639
            }
        },
        "entities": {
        }
    }
}
```

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

Las intenciones se ordenan de mayor a menor puntuación.

|Objeto de datos|Tipo de datos|Ubicación de los datos|Value|Score|
|--|--|--|--|:--|
|Intención|String|intents[0].intent|"GetStoreInfo"|0.984749258|
|Intención|String|intents[1].intent|"None"|0.0168218873|

Si agrega dominios creados previamente, el nombre de la intención indica el dominio, como `Utilties` o `Communication`, así como la intención:

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

```JSON
{
    "query": "Turn on the lights next monday at 9am",
    "prediction": {
        "normalizedQuery": "Turn on the lights next monday at 9am",
        "topIntent": "Utilities.ShowNext",
        "intents": {
            "Utilities.ShowNext": {
                "score": 0.07842206
            },
            "Communication.StartOver": {
                "score": 0.0239675418
            },
            "None": {
                "score": 0.00085447653
            }
        },
        "entities": []
    }
}
```

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

|Domain|Objeto de datos|Tipo de datos|Ubicación de los datos|Value|
|--|--|--|--|--|
|Sectores públicos|Intención|String|intents[0].intent|"<b>Utilities</b>.ShowNext"|
|Comunicación|Intención|String|intents[1].intent|<b>Communication</b>.StartOver"|
||Intención|String|intents[2].intent|"None"|


## <a name="data-from-entities"></a>Datos de entidades
La mayoría de bots de chat y aplicaciones necesitan más que el nombre de la intención. Estos datos adicionales y opcionales proceden de las entidades que se han detectado en la expresión. Cada tipo de entidad devuelve otra información sobre la coincidencia.

Una sola palabra o frase en una expresión puede coincidir con más de una entidad. En ese caso, se devuelve cada entidad coincidente con su puntuación.

Se devuelven todas las entidades de la matriz **entities** de la respuesta desde el punto de conexión:

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Name",
    "startIndex": 0,
    "endIndex": 8,
    "score": 0.473899543
  },
  {
    "entity": "3",
    "type": "builtin.number",
    "startIndex": 16,
    "endIndex": 16,
    "resolution": {
      "value": "3"
    }
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

```JSON
"entities": {
    "name":["bob jones"],
    "number": [3]
}
```
Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

## <a name="tokenized-entity-returned"></a>Entidad tokenizada devuelta

Revise la [compatibilidad de los tokens](luis-language-support.md#tokenization) en LUIS.

## <a name="simple-entity-data"></a>Datos de entidad simple

Una [entidad simple](reference-entity-simple.md) es un valor de aprendizaje automático. Puede ser una palabra o frase.

## <a name="composite-entity-data"></a>Datos de entidad compuesta

Una [entidad compuesta](reference-entity-composite.md) consta de otras entidades, como las entidades precompiladas, expresiones simples y regulares, y entidades de lista. Las entidades independientes forman una entidad completa.

## <a name="list-entity-data"></a>Datos de entidad de lista

Las [entidades de lista](reference-entity-list.md) representan un conjunto fijo y cerrado de palabras relacionadas y sus sinónimos. LUIS no detecta valores adicionales para las entidades de lista. Use la característica **Recommend** (Recomendar) para ver sugerencias de palabras nuevas en función de la lista actual. Si hay más de una entidad de lista con el mismo valor, se devolverá cada entidad en la consulta de punto de conexión.

## <a name="prebuilt-entity-data"></a>Datos de entidades creadas previamente
Las entidades [creadas previamente](luis-concept-entity-types.md) se detectan en función de una coincidencia de expresión regular mediante el proyecto de código abierto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). Las entidades creadas previamente se devuelven en la matriz de entidades y usan el nombre de tipo con el prefijo `builtin::`. El texto siguiente es una expresión de ejemplo con las entidades creadas previamente devueltas:

`Dec 5th send to +1 360-555-1212`

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
"entities": [
    {
      "entity": "dec 5th",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2017-12-05"
          },
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2018-12-05"
          }
        ]
      }
    },
    {
      "entity": "1",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 18,
      "resolution": {
        "value": "1"
      }
    },
    {
      "entity": "360",
      "type": "builtin.number",
      "startIndex": 20,
      "endIndex": 22,
      "resolution": {
        "value": "360"
      }
    },
    {
      "entity": "555",
      "type": "builtin.number",
      "startIndex": 26,
      "endIndex": 28,
      "resolution": {
        "value": "555"
      }
    },
    {
      "entity": "1212",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 35,
      "resolution": {
        "value": "1212"
      }
    },
    {
      "entity": "5th",
      "type": "builtin.ordinal",
      "startIndex": 4,
      "endIndex": 6,
      "resolution": {
        "value": "5"
      }
    },
    {
      "entity": "1 360 - 555 - 1212",
      "type": "builtin.phonenumber",
      "startIndex": 18,
      "endIndex": 35,
      "resolution": {
        "value": "1 360 - 555 - 1212"
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Sin el parámetro de cadena de consulta, `verbose=true`:

```json
"entities": {
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "XXXX-12-05",
                    "value": "2018-12-05"
                },
                {
                    "timex": "XXXX-12-05",
                    "value": "2019-12-05"
                }
            ]
        }
    ],
    "ordinal": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "ordinalV2": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "number": [
        1360,
        555,
        1212
    ],
    "phonenumber": [
        "1 360-555-1212"
    ]
}
```

Con el parámetro de cadena de consulta, `verbose=true`:

```json

"entities": {
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "XXXX-12-05",
                    "value": "2018-12-05"
                },
                {
                    "timex": "XXXX-12-05",
                    "value": "2019-12-05"
                }
            ]
        }
    ],
    "ordinal": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "ordinalV2": [
        {
            "offset": 5,
            "relativeTo": "start"
        }
    ],
    "number": [
        1360,
        555,
        1212
    ],
    "phonenumber": [
        "1 360-555-1212"
    ],
    "$instance": {
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.date",
                "text": "Dec 5th",
                "startIndex": 0,
                "length": 7,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "ordinal": [
            {
                "type": "builtin.ordinal",
                "text": "5th",
                "startIndex": 4,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2",
                "text": "5th",
                "startIndex": 4,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "number": [
            {
                "type": "builtin.number",
                "text": "1 360",
                "startIndex": 17,
                "length": 5,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.number",
                "text": "555",
                "startIndex": 23,
                "length": 3,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.number",
                "text": "1212",
                "startIndex": 27,
                "length": 4,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "phonenumber": [
            {
                "type": "builtin.phonenumber",
                "text": "1 360-555-1212",
                "startIndex": 17,
                "length": 14,
                "score": 1.0,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *
## <a name="regular-expression-entity-data"></a>Datos de entidades de expresiones regulares

Una [entidad de expresión regular](reference-entity-regular-expression.md) extrae una entidad basada en un patrón de expresión regular que se proporciona.

## <a name="extracting-names"></a>Extraer nombres
Obtener nombres de una expresión es difícil porque un nombre puede ser casi cualquier combinación de letras y palabras. En función de qué tipo de nombre vaya a extraer, tiene varias opciones. Las sugerencias siguientes no son reglas a seguir, si no más bien instrucciones.

### <a name="add-prebuilt-personname-and-geographyv2-entities"></a>Agregar las entidades PersonName y GeographyV2 creadas previamente

Las entidades [PersonName](luis-reference-prebuilt-person.md) y [GeographyV2](luis-reference-prebuilt-geographyV2.md) están disponibles en algunas [referencias culturales del idioma](luis-reference-prebuilt-entities.md).

### <a name="names-of-people"></a>Nombres de personas

Los nombres de personas pueden tener un pequeño formato en función del idioma y la referencia cultural. Use una entidad **[personName](luis-reference-prebuilt-person.md)** precompilada o una **[entidad sencilla](luis-concept-entity-types.md#simple-entity)** con [roles](luis-concept-roles.md) de nombre y apellido.

Si usa la entidad sencilla, asegúrese de proporcionar ejemplos que usen el nombre y el apellido en diferentes partes de la expresión, en expresiones de distintas longitudes y expresiones en todas las intenciones, incluida la intención None. [Revise](luis-how-to-review-endoint-utt.md) las expresiones del punto de conexión de forma regular para etiquetar los nombres que no se predijeron correctamente.

### <a name="names-of-places"></a>Nombres de lugares

Los nombres de ubicaciones se establecen y conocen, por ejemplo, ciudades, condados, estados, provincias y países o regiones. Use la entidad precompilada **[geographyV2](luis-reference-prebuilt-geographyv2.md)** para extraer información de ubicación.

### <a name="new-and-emerging-names"></a>Nombres nuevos y emergentes

Algunas aplicaciones necesitan poder encontrar nombres nuevos y emergentes, como productos o empresas. Estos tipos de nombres son el tipo más difícil de extracción de datos. Empiece con una **[entidad simple](luis-concept-entity-types.md#simple-entity)** y agregue una [lista de frases](luis-concept-feature.md). [Revise](luis-how-to-review-endoint-utt.md) las expresiones del punto de conexión de forma regular para etiquetar los nombres que no se predijeron correctamente.

## <a name="pattern-roles-data"></a>Datos de roles de patrón
Los roles son diferencias contextuales de entidades.


#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

El nombre de la entidad es `Location`, con dos roles, `Origin` y `Destination`.

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Employee",
    "startIndex": 5,
    "endIndex": 13,
    "score": 0.922820568,
    "role": ""
  },
  {
    "entity": "seattle",
    "type": "Location",
    "startIndex": 20,
    "endIndex": 26,
    "score": 0.948008537,
    "role": "Origin"
  },
  {
    "entity": "redmond",
    "type": "Location",
    "startIndex": 31,
    "endIndex": 37,
    "score": 0.7047979,
    "role": "Destination"
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

En v3, el **nombre de rol** es el nombre principal del objeto.

El nombre de la entidad es `Location`, con dos roles, `Origin` y `Destination`.

Sin el parámetro de cadena de consulta, `verbose=true`:

```json
"entities": {
    "Employee": [
        "bob jones"
    ],
    "Origin": [
        "seattle"
    ],
    "Destination": [
        "redmond"
    ]
}
```

Con el parámetro de cadena de consulta, `verbose=true`:

```json
"entities": {
    "Employee": [
        "bob jones"
    ],
    "LocationOrigin": [
        "seattle"
    ],
    "LocationDestination": [
        "redmond"
    ],
    "$instance": {
        "Employee": [
            {
                "type": "Employee",
                "text": "bob jones",
                "startIndex": 5,
                "length": 9,
                "score": 0.982873261,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Origin": [
            {
                "role": "Origin",
                "type": "Location",
                "text": "seattle",
                "startIndex": 20,
                "length": 7,
                "score": 0.9913306,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Destination": [
            {
                "role": "Destination",
                "type": "Location",
                "text": "redmond",
                "startIndex": 31,
                "length": 7,
                "score": 0.898179531,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]

}
```

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

## <a name="patternany-entity-data"></a>Datos de la entidad Pattern.any

[Pattern.any](reference-entity-pattern-any.md) es un marcador de posición de longitud variable que solo se usa en la expresión de plantilla de un patrón para marcar dónde empieza y acaba la entidad.

## <a name="sentiment-analysis"></a>análisis de opiniones
Si el análisis de sentimiento está configurado, la respuesta JSON de LUIS incluye el análisis de sentimiento. Obtenga más información sobre el análisis de sentimiento en la documentación de [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

### <a name="sentiment-data"></a>Datos de opinión
Los datos de opinión son una puntuación entre 1 y 0 que indica el valor de opinión positiva (más cercano a 1) o negativa (más cercano a 0) de los datos.

Cuando la referencia cultural es `en-us`, la respuesta es:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Para todas las demás referencias culturales, la respuesta es:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Datos de entidad de extracción de frases clave
La entidad de extracción de frases clave devuelve frases clave en la expresión, proporcionadas por [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).


#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
{
  "query": "Is there a map of places with beautiful views on a favorite trail?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.764368951
  },
  "intents": [
    ...
  ],
  "entities": [
    {
      "entity": "beautiful views",
      "type": "builtin.keyPhrase",
      "startIndex": 30,
      "endIndex": 44
    },
    {
      "entity": "map of places",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 23
    },
    {
      "entity": "favorite trail",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 64
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Sin el parámetro de cadena de consulta, `verbose=true`:

```json
"entities": {
    "keyPhrase": [
        "map of places",
        "beautiful views",
        "favorite trail"
    ]
}
```

Con el parámetro de cadena de consulta, `verbose=true`:

```json
"entities": {
    "keyPhrase": [
        "map of places",
        "beautiful views",
        "favorite trail"
    ],
    "$instance": {
        "keyPhrase": [
            {
                "type": "builtin.keyPhrase",
                "text": "map of places",
                "startIndex": 11,
                "length": 13,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "beautiful views",
                "startIndex": 30,
                "length": 15,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.keyPhrase",
                "text": "favorite trail",
                "startIndex": 51,
                "length": 14,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *


## <a name="data-matching-multiple-entities"></a>Datos que coinciden con varias entidades

LUIS devuelve todas las entidades que ha detectado la expresión. Como consecuencia, es posible que el bot de chat tenga que tomar una decisión en función de los resultados. Una expresión puede tener varias entidades en una expresión:

`book me 2 adult business tickets to paris tomorrow on air france`

El punto de conexión de LUIS puede detectar los mismos datos en diferentes entidades.

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
{
  "query": "book me 2 adult business tickets to paris tomorrow on air france",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 1.0
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 1.0
    },
    {
      "intent": "Concierge",
      "score": 0.04216196
    },
    {
      "intent": "None",
      "score": 0.03610297
    }
  ],
  "entities": [
    {
      "entity": "air france",
      "type": "Airline",
      "startIndex": 54,
      "endIndex": 63,
      "score": 0.8291798
    },
    {
      "entity": "adult",
      "type": "Category",
      "startIndex": 10,
      "endIndex": 14,
      "resolution": {
        "values": [
          "adult"
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 36,
      "endIndex": 40,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    },
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 42,
      "endIndex": 49,
      "resolution": {
        "values": [
          {
            "timex": "2018-02-21",
            "type": "date",
            "value": "2018-02-21"
          }
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 36,
      "endIndex": 40,
      "score": 0.9730773
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "business",
      "type": "Seat",
      "startIndex": 16,
      "endIndex": 23,
      "resolution": {
        "values": [
          "business"
        ]
      }
    },
    {
      "entity": "2 adult business",
      "type": "TicketSeatOrder",
      "startIndex": 8,
      "endIndex": 23,
      "score": 0.8784727
    }
  ],
  "compositeEntities": [
    {
      "parentType": "TicketSeatOrder",
      "value": "2 adult business",
      "children": [
        {
          "type": "Category",
          "value": "adult"
        },
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Seat",
          "value": "business"
        }
      ]
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Sin `verbose=true` como el parámetro de cadena de consulta.

```json
"entities": {
    "TicketsOrder": [
        {
            "number": [
                2
            ],
            "PassengerCategory": [
                "adult"
            ],
            "TravelClass": [
                "business"
            ]
        }
    ],
    "Location::LocationTo": [
        "paris"
    ],
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "2019-09-28",
                    "value": "2019-09-28"
                }
            ]
        }
    ],
    "Airline": [
        "air france"
    ]
}
```

Con `verbose=true` como el parámetro de cadena de consulta.


```json
"entities": {
    "TicketsOrder": [
        {
            "number": [
                2
            ],
            "PassengerCategory": [
                "adult"
            ],
            "TravelClass": [
                "business"
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "2",
                        "startIndex": 8,
                        "length": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "PassengerCategory": [
                    {
                        "type": "PassengerCategory",
                        "text": "adult",
                        "startIndex": 10,
                        "length": 5,
                        "score": 0.9503733,
                        "modelTypeId": 3,
                        "modelType": "Hierarchical Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "TravelClass": [
                    {
                        "type": "TravelClass",
                        "text": "business",
                        "startIndex": 16,
                        "length": 8,
                        "score": 0.950095,
                        "modelTypeId": 3,
                        "modelType": "Hierarchical Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    ],
    "Location::LocationTo": [
        "paris"
    ],
    "datetimeV2": [
        {
            "type": "date",
            "values": [
                {
                    "timex": "2019-09-28",
                    "value": "2019-09-28"
                }
            ]
        }
    ],
    "Airline": [
        "air france"
    ],
    "$instance": {
        "TicketsOrder": [
            {
                "type": "TicketsOrder",
                "text": "2 adult business",
                "startIndex": 8,
                "length": 16,
                "score": 0.942183256,
                "modelTypeId": 4,
                "modelType": "Composite Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Location::LocationTo": [
            {
                "type": "Location::LocationTo",
                "text": "paris",
                "startIndex": 36,
                "length": 5,
                "score": 0.9905354,
                "modelTypeId": 3,
                "modelType": "Hierarchical Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "datetimeV2": [
            {
                "type": "builtin.datetimeV2.date",
                "text": "tomorrow",
                "startIndex": 42,
                "length": 8,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ],
        "Airline": [
            {
                "type": "Airline",
                "text": "air france",
                "startIndex": 54,
                "length": 10,
                "score": 0.9455415,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

## <a name="data-matching-multiple-list-entities"></a>Datos que coinciden con varias entidades de lista

Si una palabra o frase coincide con más de una entidad de lista, la consulta de punto de conexión devolverá todas las entidades de lista.

Para la consulta `when is the best time to go to red rock?`, si la aplicación tiene la palabra `red` en más de una lista, LUIS reconocerá todas las entidades y devolverá una matriz de entidades como parte de la respuesta del punto de conexión JSON:

#### <a name="v2-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```



#### <a name="v3-prediction-endpoint-response"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Sin `verbose=true` en la cadena de consulta:

```JSON
{
    "query": "when is the best time to go to red rock",
    "prediction": {
        "normalizedQuery": "when is the best time to go to red rock",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "Colors": [
                [
                    "red"
                ]
            ],
            "Cities": [
                [
                    "Destinations"
                ]
            ]
        }
    }
}
```


Con `verbose=true` en la cadena de consulta:

```JSON
{
    "query": "when is the best time to go to red rock",
    "prediction": {
        "normalizedQuery": "when is the best time to go to red rock",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "Colors": [
                [
                    "red"
                ]
            ],
            "Cities": [
                [
                    "Destinations"
                ]
            ],
            "$instance": {
                "Colors": [
                    {
                        "type": "Colors",
                        "text": "red",
                        "startIndex": 31,
                        "length": 3,
                        "modelTypeId": 5,
                        "modelType": "List Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ],
                "Cities": [
                    {
                        "type": "Cities",
                        "text": "red rock",
                        "startIndex": 31,
                        "length": 8,
                        "modelTypeId": 5,
                        "modelType": "List Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    }
}
```

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

* * *

## <a name="next-steps"></a>Pasos siguientes

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.
