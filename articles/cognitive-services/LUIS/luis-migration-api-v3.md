---
title: Cambios de punto de conexión para predicciones en la versión 3 de la API
description: Se cambiaron las API v3 del punto de conexión para la predicción de consultas. Use esta guía para aprender a migrar a la versión 3 de las API de punto de conexión.
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: diberry
ms.openlocfilehash: 9a8e8cb331dd11eebaddbcbf8f603c1148415aef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79117372"
---
# <a name="prediction-endpoint-changes-for-v3"></a>Cambios de punto de conexión para predicciones para la versión 3

Se cambiaron las API v3 del punto de conexión para la predicción de consultas. Use esta guía para aprender a migrar a la versión 3 de las API de punto de conexión.

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

**Estado de disponibilidad general**: esta versión 3 de la API incluye cambios importantes en la solicitud JSON y la respuesta en comparación con la versión 2 de la API.

La versión 3 de la API proporciona las características nuevas siguientes:

* [Entidades externas](#external-entities-passed-in-at-prediction-time)
* [Listas dinámicas](#dynamic-lists-passed-in-at-prediction-time)
* [Cambios de JSON en las entidades precompiladas](#prebuilt-entity-changes)

La [solicitud](#request-changes) y [respuesta](#response-changes) del punto de conexión para predicciones recibió cambios importantes para admitir las nuevas características enumeradas anteriormente, incluido lo siguiente:

* [Cambios al objeto de respuesta](#top-level-json-changes)
* [Referencias al nombre del rol de entidad en lugar del nombre de entidad](#entity-role-name-instead-of-entity-name)
* [Propiedades para marcar las entidades en expresiones](#marking-placement-of-entities-in-utterances)

Puede consultar la [documentación de referencia](https://aka.ms/luis-api-v3) para la v3.

## <a name="v3-changes-from-preview-to-ga"></a>Cambios en la versión 3 desde la versión preliminar a la disponibilidad general

La versión 3 realizó los cambios siguientes como parte de la migración a la disponibilidad general:

* Estas entidades creadas previamente tienen respuestas JSON distintas:
    * [OrdinalV1](luis-reference-prebuilt-ordinal.md)
    * [GeographyV2](luis-reference-prebuilt-geographyv2.md)
    * [DatetimeV2](luis-reference-prebuilt-datetimev2.md)
    * Nombre de clave de unidad medible de `units` a `unit`

* Cambio de JSON del cuerpo de la solicitud:
    * de `preferExternalEntities` a `preferExternalEntities`
    * parámetro `score` opcional para entidades externas

* Cambios de JSON del cuerpo de la respuesta:
    * se quitó `normalizedQuery`

## <a name="suggested-adoption-strategy"></a>Estrategia de adopción sugerida

Si usa Bot Framework, Bing Spell Check V7 o solo quiere migrar la creación de aplicaciones de LUIS, siga usando la versión 2 del punto de conexión.

Si sabe que ninguna de las aplicaciones cliente o integraciones (Bot Framework y Bing Spell Check V7) se ven afectadas y está familiarizado con la migración de la creación de aplicaciones de LUIS y el punto de conexión para predicciones al mismo tiempo, empiece a usar la versión 3 del punto de conexión para predicciones. La versión 2 del punto de conexión para predicciones seguirá disponible y es una buena estrategia de retroceso.


## <a name="not-supported"></a>No compatible

### <a name="bing-spell-check"></a>Bing Spell Check

Esta API no es compatible en la versión 3 del punto de conexión para predicciones. Siga usando la versión 2 de la API del punto de conexión para predicciones para las correcciones ortográficas. Si necesita una corrección ortográfica mientras usa la versión 3 de la API, haga que la aplicación cliente llame a la API [Bing Spell Check](https://docs.microsoft.com/azure/cognitive-services/bing-spell-check/overview) y cambie el texto para que tenga la ortografía correcta antes de enviarlo a API LUIS.

## <a name="bot-framework-and-azure-bot-service-client-applications"></a>Aplicaciones cliente de Azure Bot Service y Bot Framework

Siga usando la versión 2 de la API del punto de conexión para predicciones hasta que aparezca la versión 4.7 de Bot Framework.

## <a name="v2-api-deprecation"></a>Desuso de la versión 2 de la API

La versión 2 de Prediction API no entrará en desuso durante un mínimo de 9 meses después del lanzamiento de la versión preliminar v3, el 8 de junio de 2020.

## <a name="endpoint-url-changes"></a>Cambios en la dirección URL del punto de conexión

### <a name="changes-by-slot-name-and-version-name"></a>Cambios por nombre de ranura y nombre de versión

Se cambió el formato para la llamada HTTP al punto de conexión v3.

Si quiere realizar una consulta por visión, primero debe [publicarla a través de la API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3b) con `"directVersionPublish":true`. Consulte el punto de conexión que hace referencia al Id. de versión en lugar del nombre de la ranura.

|VERSIÓN DE LA API PARA PREDICCIONES|METHOD|URL|
|--|--|--|
|V3|GET|https://<b>{REGION}</b>.api.cognitive.microsoft.com/luis/<b>prediction</b>/<b>v3.0</b>/apps/<b>{APP-ID}</b>/slots/<b>{SLOT-NAME}</b>/predict?query=<b>{QUERY}</b>|
|V3|POST|https://<b>{REGION}</b>.api.cognitive.microsoft.com/luis/<b>prediction</b>/<b>v3.0</b>/apps/<b>{APP-ID}</b>/slots/<b>{SLOT-NAME}</b>/predict|
|V2|GET|https://<b>{REGION}</b>.api.cognitive.microsoft.com/luis/<b>prediction</b>/<b>v3.0</b>/apps/<b>{APP-ID}</b>/versions/<b>{VERSION-ID}</b>/predict?query=<b>{QUERY}</b>|
|V2|POST|https://<b>{REGION}</b>.api.cognitive.microsoft.com/luis/<b>prediction</b>/<b>v3.0</b>/apps/<b>{APP-ID}</b>/versions/<b>{VERSION-ID}</b>/predict|

|Valores válidos para `SLOT-NAME`|
|--|
|`production`|
|`staging`|

## <a name="request-changes"></a>Solicitud de cambios

### <a name="query-string-changes"></a>Cambios en la cadena de consulta

La API v3 tiene parámetros de cadena de consulta diferentes.

|Nombre del parámetro|Tipo|Versión|Valor predeterminado|Propósito|
|--|--|--|--|--|
|`log`|boolean|v2 y v3|false|Almacena la consulta en el archivo de registro. El valor predeterminado es False.|
|`query`|string|Solo v3|No tiene valor predeterminado: es obligatorio en la solicitud GET.|**En la versión 2**, la expresión que va a predecirse se almacena en el parámetro `q`. <br><br>**En la versión 3**, la funcionalidad se pasa en el parámetro `query`.|
|`show-all-intents`|boolean|Solo v3|false|Devuelve todas las intenciones con la puntuación correspondiente en el objeto **prediction.intents**. Las intenciones se devuelven como objetos en un objeto `intents` primario. Esto habilita el acceso mediante programación sin necesidad de encontrar la intención en una matriz: `prediction.intents.give`. En la versión 2, se devuelven en una matriz. |
|`verbose`|boolean|v2 y v3|false|**En la versión 2**, cuando se establece en true, se devuelven todas las intenciones previstas. Si necesita todas las intenciones previstas, use el parámetro v3 de `show-all-intents`.<br><br>**En la versión 3**, este parámetro solo proporciona detalles de los metadatos de la entidad para la predicción de entidad.  |
|`timezoneOffset`|string|V2|-|Zona horaria aplicada a las entidades datetimeV2.|
|`datetimeReference`|string|V3|-|[Zona horaria](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) aplicada a las entidades datetimeV2. Reemplaza `timezoneOffset` de la versión 2.|


### <a name="v3-post-body"></a>Cuerpo de POST de la versión 3

```JSON
{
    "query":"your utterance here",
    "options":{
        "datetimeReference": "2019-05-05T12:00:00",
        "preferExternalEntities": true
    },
    "externalEntities":[],
    "dynamicLists":[]
}
```

|Propiedad|Tipo|Versión|Valor predeterminado|Propósito|
|--|--|--|--|--|
|`dynamicLists`|array|Solo v3|No se requiere.|Las [listas dinámicas](#dynamic-lists-passed-in-at-prediction-time) permiten extender una entidad de lista existente entrenada y publicada, ya presente en la aplicación de LUIS.|
|`externalEntities`|array|Solo v3|No se requiere.|Las [entidades externas](#external-entities-passed-in-at-prediction-time) permiten que la aplicación de LUIS identifique y etiquete las entidades durante el tiempo de ejecución, que pueden usarse como características para las entidades existentes. |
|`options.datetimeReference`|string|Solo v3|Sin valor predeterminado|Permite determinar el [desplazamiento de datetimeV2](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity). El formato de datetimeReference es [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).|
|`options.preferExternalEntities`|boolean|Solo v3|false|Especifica si la [entidad externa (con el mismo nombre que la entidad existente)](#override-existing-model-predictions) del usuario o la entidad existente en el modelo se usa para la predicción. |
|`query`|string|Solo v3|Necesario.|**En la versión 2**, la expresión que va a predecirse se almacena en el parámetro `q`. <br><br>**En la versión 3**, la funcionalidad se pasa en el parámetro `query`.|



## <a name="response-changes"></a>Cambios de respuesta

El elemento JSON de la respuesta de consulta se cambió para permitir el acceso mediante programación a los datos usados con más frecuencia.

### <a name="top-level-json-changes"></a>Cambios JSON de nivel superior



Las siguientes son las principales propiedades JSON para la v2, cuando `verbose` está establecido en true, de forma que devuelve todas las intenciones y sus puntuaciones en la propiedad `intents`:

```JSON
{
    "query":"this is your utterance you want predicted",
    "topScoringIntent":{},
    "intents":[],
    "entities":[],
    "compositeEntities":[]
}
```

Las principales propiedades JSON para la versión 3 son:

```JSON
{
    "query": "this is your utterance you want predicted",
    "prediction":{
        "topIntent": "intent-name-1",
        "intents": {},
        "entities":{}
    }
}
```

El objeto `intents` es una lista sin ordenar. No suponga que el primer elemento secundario en `intents` corresponde a `topIntent`. En su lugar, use el valor `topIntent` para buscar la puntuación:

```nodejs
const topIntentName = response.prediction.topIntent;
const score = intents[topIntentName];
```

Los cambios en el esquema JSON de la respuesta permiten lo siguiente:

* Una clara distinción entre la expresión original, `query`, y la predicción devuelta, `prediction`.
* Fácil acceso mediante programación a los datos previstos. En lugar de realizar la enumeración a través de una matriz en la versión 2, puede acceder a los valores por **nombre** en ambas intenciones y entidades. Para los roles de entidad prevista, se devuelve el nombre del rol porque es único en toda la aplicación.
* Los tipos de datos, si están establecidos, se respetan. Los valores numéricos ya no se devuelven como cadenas.
* Distinción entre la información de predicciones de máxima prioridad y los metadatos adicionales, que se devuelven en el objeto `$instance`.

### <a name="entity-response-changes"></a>Cambios en la respuesta de la entidad

#### <a name="marking-placement-of-entities-in-utterances"></a>Selección de la ubicación de las entidades en las expresiones

**En la versión 2**, una entidad se marca dentro de una expresión con `startIndex` y `endIndex`.

**En la versión 3**, la entidad se marca con `startIndex` y `entityLength`.

#### <a name="access-instance-for-entity-metadata"></a>Acceso a `$instance` para obtener metadatos de entidad

Si tiene metadatos de entidad, la cadena de consulta debe usar la marca `verbose=true` y la respuesta contiene los metadatos en el objeto `$instance`. En las respuestas JSON de las secciones siguientes se muestran ejemplos.

#### <a name="each-predicted-entity-is-represented-as-an-array"></a>Todas las entidades de predicción se representan como una matriz.

El objeto `prediction.entities.<entity-name>` contiene una matriz, dado que cada entidad se puede predecir más de una vez en la expresión.

<a name="prebuilt-entities-with-new-json"></a>

#### <a name="prebuilt-entity-changes"></a>Cambios de entidad generados previamente

El objeto de respuesta de la versión 3 incluye cambios en las entidades compiladas previamente. Revise las [entidades compiladas previamente específicas](luis-reference-prebuilt-entities.md) para más información.

#### <a name="list-entity-prediction-changes"></a>Cambios en la predicción de entidad de lista

El código JSON de una predicción de entidad la lista se cambió para que sea una matriz de matrices:

```JSON
"entities":{
    "my_list_entity":[
        ["canonical-form-1","canonical-form-2"],
        ["canonical-form-2"]
    ]
}
```
Cada matriz interior corresponde al texto dentro de la expresión. El objeto interior es una matriz porque el mismo texto puede aparecer en más de una sublista de una entidad de lista.

Cuando se realiza una asignación del objeto `entities` y el objeto `$instance`, se conserva el orden de los objetos para las predicciones de entidad de lista.

```nodejs
const item = 0; // order preserved, use same enumeration for both
const predictedCanonicalForm = entities.my_list_entity[item];
const associatedMetadata = entities.$instance.my_list_entity[item];
```

#### <a name="entity-role-name-instead-of-entity-name"></a>Nombre del rol de entidad en lugar del nombre de entidad

En la versión 2, la matriz `entities` devuelve todas las entidades previstas, cuyo el nombre de entidad es el identificador único. En la versión 3, si la entidad usa roles y la predicción es para un rol de entidad, el identificador principal es el nombre del rol. Esto es posible porque los nombres de rol de entidad deben ser únicos en toda la aplicación, incluidos otros nombres del modelo (entidad, intención).

En el siguiente ejemplo: considere la posibilidad de que una expresión incluya el texto `Yellow Bird Lane`. Este texto se prevé como el rol `Location` de una entidad de `Destination` personalizada.

|Texto de expresión|Nombre de entidad|Nombre de rol|
|--|--|--|
|`Yellow Bird Lane`|`Location`|`Destination`|

En la versión 2, la entidad se identifica mediante el _nombre de entidad_, con el rol como una propiedad del objeto:

```JSON
"entities":[
    {
        "entity": "Yellow Bird Lane",
        "type": "Location",
        "startIndex": 13,
        "endIndex": 20,
        "score": 0.786378264,
        "role": "Destination"
    }
]
```

En la versión 3, se hace referencia a la entidad mediante el _rol de entidad_ si la predicción es para el rol:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ]
}
```

En la versión 3, el mismo resultado con la marca `verbose` para devolver los metadatos de entidad:

```JSON
"entities":{
    "Destination":[
        "Yellow Bird Lane"
    ],
    "$instance":{
        "Destination": [
            {
                "role": "Destination",
                "type": "Location",
                "text": "Yellow Bird Lane",
                "startIndex": 25,
                "length":16,
                "score": 0.9837309,
                "modelTypeId": 1,
                "modelType": "Entity Extractor"
            }
        ]
    }
}
```

## <a name="external-entities-passed-in-at-prediction-time"></a>Entidades externas que se pasan al momento de la predicción

Las entidades externas permiten que la aplicación de LUIS identifique y etiquete las entidades durante el tiempo de ejecución, que pueden usarse como características para las entidades existentes. Esto le permite utilizar sus propios extractores de entidades personalizados e independientes antes de enviar consultas al punto de conexión de predicción. Puesto que este proceso se realiza en el punto de conexión de predicción de consulta, no es necesario volver a entrenar y publicar el modelo.

La aplicación cliente proporciona su propio extractor de entidades al administrar la entidad coincidente y determinar su ubicación dentro de la expresión de la entidad coincidente. A continuación, envía esa información con la solicitud.

Las entidades externas son el mecanismo para extender cualquier tipo de entidad mientras aún está en uso como señal para otros modelos, como roles, compuestos y otros.

Esto resulta útil para una entidad que tiene datos disponibles solo durante el tiempo de ejecución de la predicción de consulta. Los ejemplos de este tipo de datos son los datos en cambio constante o específicos de un usuario. Puede ampliar una entidad de contacto de LUIS con información externa de la lista de contactos de un usuario.

### <a name="entity-already-exists-in-app"></a>La entidad ya existe en la aplicación

El valor de `entityName` para la entidad externa (que se pasa en el cuerpo POST de la solicitud de punto de conexión) ya debe existir en la aplicación publicada y entrenada al momento de realizar la solicitud. El tipo de entidad no importa, ya que todos los tipos son compatibles.

### <a name="first-turn-in-conversation"></a>Primera entrega en una conversación

Imagine la primera expresión de la conversación con un bot de chat, en la que un usuario escribe la siguiente información incompleta:

`Send Hazem a new message`

La solicitud del bot de chat a LUIS puede pasar información del cuerpo de POST acerca de `Hazem`, por lo que se empareja directamente como uno de los contactos del usuario.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 5,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

La respuesta de predicción incluye esa entidad externa con todas las demás entidades previstas porque está definida en la solicitud.

### <a name="second-turn-in-conversation"></a>Segunda entrega en una conversación

La siguiente expresión del usuario para el bot de chat usa un término más vago:

`Send him a calendar reminder for the party.`

En la expresión anterior, se usa `him` como una referencia a `Hazem`. El bot de chat de conversación puede asignar dentro del cuerpo POST a `him` con el valor de entidad extraído de la primera expresión, `Hazem`.

```json
    "externalEntities": [
        {
            "entityName":"contacts",
            "startIndex": 5,
            "entityLength": 3,
            "resolution": {
                "employeeID": "05013",
                "preferredContactType": "TeamsChat"
            }
        }
    ]
```

La respuesta de predicción incluye esa entidad externa con todas las demás entidades previstas porque está definida en la solicitud.

### <a name="override-existing-model-predictions"></a>Invalidación de las predicciones del modelo existente

La propiedad de opciones `preferExternalEntities` especifica que si el usuario envía una entidad externa que se superpone con una entidad de predicción con el mismo nombre, LUIS elige la entidad que se pasó o la entidad existente en el modelo.

Por ejemplo, considere la consulta `today I'm free`. LUIS detecta que `today` es un datetimeV2 con la siguiente respuesta:

```JSON
"datetimeV2": [
    {
        "type": "date",
        "values": [
            {
                "timex": "2019-06-21",
                "value": "2019-06-21"
            }
        ]
    }
]
```

Si el usuario envía la entidad externa:

```JSON
{
    "entityName": "datetimeV2",
    "startIndex": 0,
    "entityLength": 5,
    "resolution": {
        "date": "2019-06-21"
    }
}
```

Si `preferExternalEntities` está establecida en `false`, LUIS devuelve una respuesta como si la entidad externa no se hubiera enviado.

```JSON
"datetimeV2": [
    {
        "type": "date",
        "values": [
            {
                "timex": "2019-06-21",
                "value": "2019-06-21"
            }
        ]
    }
]
```

Si `preferExternalEntities` está establecida en `true`, LUIS devuelve una respuesta que incluye lo siguiente:

```JSON
"datetimeV2": [
    {
        "date": "2019-06-21"
    }
]
```



#### <a name="resolution"></a>Solución

La propiedad _optional_ `resolution` se devuelve en la respuesta de predicción, lo que le permite pasar los metadatos asociados con la entidad externa y, después, recibirlos de vuelta en la respuesta.

La finalidad principal es extender entidades precompiladas, pero no se limita a ese tipo de entidad.

La propiedad `resolution` puede ser un número, una cadena, un objeto o una matriz:

* "Dallas"
* {"text": "value"}
* 12345
* ["a", "b", "c"]



## <a name="dynamic-lists-passed-in-at-prediction-time"></a>Listas dinámicas que se pasan al momento de la predicción

Las listas dinámicas permiten extender una entidad de lista existente entrenada y publicada, ya presente en la aplicación de LUIS.

Use esta característica cuando necesite cambiar periódicamente los valores de la entidad de lista. Esta característica le permite extender una entidad de lista ya entrenada y publicada:

* Cuando se realiza la solicitud de punto de conexión de predicción de consulta.
* Para una sola solicitud.

La entidad de lista puede estar vacía en la aplicación de LUIS, pero debe existir. La entidad de lista en la aplicación de LUIS no cambia, pero la capacidad de predicción en el punto de conexión se extiende para incluir hasta 2 listas con aproximadamente 1000 elementos.

### <a name="dynamic-list-json-request-body"></a>Cuerpo de la solicitud JSON de una lista dinámica

Envíe el siguiente cuerpo JSON para agregar una nuevo sublista con sinónimos a la lista y predecir la entidad de lista del texto `LUIS` mediante la solicitud de predicción de consulta `POST`:

```JSON
{
    "query": "Send Hazem a message to add an item to the meeting agenda about LUIS.",
    "options":{
        "timezoneOffset": "-8:00"
    },
    "dynamicLists": [
        {
            "listEntity*":"ProductList",
            "requestLists":[
                {
                    "name": "Azure Cognitive Services",
                    "canonicalForm": "Azure-Cognitive-Services",
                    "synonyms":[
                        "language understanding",
                        "luis",
                        "qna maker"
                    ]
                }
            ]
        }
    ]
}
```

La respuesta de predicción incluye esa entidad de lista con todas las demás entidades previstas porque está definida en la solicitud.

## <a name="deprecation"></a>Desuso

La API v2 no entrará en desuso durante un mínimo de 9 meses después del lanzamiento de la versión preliminar v3.

## <a name="next-steps"></a>Pasos siguientes

Use la documentación de la API v3 para actualizar las llamadas REST existentes al [punto de conexión](https://aka.ms/luis-api-v3) de LUIS.
