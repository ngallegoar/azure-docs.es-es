---
title: 'Ampliación de la aplicación en tiempo de ejecución: LUIS'
description: ''
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: c0f9d71f5d89d73d9cdce2a2f646859d8eba3adc
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538398"
---
# <a name="extend-app-at-prediction-runtime"></a>Ampliación de la aplicación en tiempo de ejecución de predicción

El esquema de la aplicación (modelos y características) se entrena y publica en el punto de conexión de predicción. Este modelo publicado se usa en el entorno de ejecución de predicción. Puede pasar información nueva, junto con la expresión del usuario, al entorno de ejecución de predicción para aumentar la predicción.

Los dos cambios de esquema del entorno de ejecución de predicción incluyen:
* [Entidades externas](#external-entities)
* [Listas dinámicas](#dynamic-lists)

<a name="external-entities-passed-in-at-prediction-time"></a>

## <a name="external-entities"></a>Entidades externas

Las entidades externas permiten que la aplicación de LUIS identifique y etiquete las entidades durante el tiempo de ejecución, que pueden usarse como características para las entidades existentes. Esto le permite utilizar sus propios extractores de entidades personalizados e independientes antes de enviar consultas al punto de conexión de predicción. Puesto que este proceso se realiza en el punto de conexión de predicción de consulta, no es necesario volver a entrenar y publicar el modelo.

La aplicación cliente proporciona su propio extractor de entidades al administrar la entidad coincidente y determinar su ubicación dentro de la expresión de la entidad coincidente. A continuación, envía esa información con la solicitud.

Las entidades externas son el mecanismo para extender cualquier tipo de entidad mientras aún está en uso como señal para otros modelos.

Esto resulta útil para una entidad que tiene datos disponibles solo durante el tiempo de ejecución de la predicción de consulta. Los ejemplos de este tipo de datos son los datos en cambio constante o específicos de un usuario. Puede ampliar una entidad de contacto de LUIS con información externa de la lista de contactos de un usuario.

Las entidades externas forman parte de la API de creación de V3. Más información sobre la [migración](luis-migration-api-v3.md) a esta versión.

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

En este turno de la conversación, la expresión usa `him` como referencia a `Hazem`. El bot de chat de conversación puede asignar dentro del cuerpo POST a `him` con el valor de entidad extraído de la primera expresión, `Hazem`.

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

<a name="dynamic-lists-passed-in-at-prediction-time"></a>

## <a name="dynamic-lists"></a>Listas dinámicas

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

## <a name="next-steps"></a>Pasos siguientes

* [Puntuación de predicción](luis-concept-prediction-score.md)
* [Cambios en la API V3 de creación](luis-migration-api-v3.md)
