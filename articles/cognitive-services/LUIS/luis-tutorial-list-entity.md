---
title: 'Entidades de coincidencia de texto extracto: LUIS'
titleSuffix: Azure Cognitive Services
description: Información sobre cómo agregar una entidad de lista para ayudar a LUIS a etiquetar variaciones de una palabra o frase.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: diberry
ms.openlocfilehash: f3c99856eaffc454754618a1eac34630b985a77e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73499472"
---
# <a name="use-a-list-entity-to-increase-entity-detection"></a>Use una entidad de lista para aumentar la detección de entidades 
En este artículo se muestra el uso de una [entidad de lista](luis-concept-entity-types.md) para aumentar la detección de entidades. No es necesario etiquetar las entidades de lista ya que son una coincidencia exacta de términos.  

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]

En este artículo aprenderá a:

> [!div class="checklist"]
> * Crear una entidad de lista 
> * Agregar valores y sinónimos normalizados
> * Validar una identificación de entidad mejorada

## <a name="prerequisites"></a>Prerequisites

> [!div class="checklist"]
> * Versión de [Node.js](https://nodejs.org) más reciente.
> * [Aplicación de LUIS HomeAutomation](luis-get-started-create-app.md). Si no ha creado la aplicación Automatización de dispositivos del hogar, cree una aplicación nueva y agregue el dominio creado previamente **HomeAutomation**. Entrene y publique la aplicación. 
> * [AuthoringKey](luis-concept-keys.md#authoring-key), [EndpointKey](luis-concept-keys.md#endpoint-key) (si se consulta muchas veces), id. de aplicación, id. de versión y [region](luis-reference-regions.md) para la aplicación LUIS.

> [!Tip]
> Si aún no tiene una suscripción, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).

Todo el código de este artículo está disponible en el [repositorio Azure-Samples de GitHub](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-list-entity). 

## <a name="use-homeautomation-app"></a>Usar la aplicación HomeAutomation
La aplicación HomeAutomation le ofrece el control de dispositivos como luces, sistemas de entretenimiento y del entorno, como la calefacción y la refrigeración. Estos sistemas tienen nombres diferentes que pueden incluir nombres de fabricantes, alias, acrónimos y jerga. 

Un sistema que tiene muchos nombres en las diferentes culturas y demografías es el termostato. Un termostato puede controlar los sistemas de calefacción y refrigeración de una casa o un edificio.

Lo idóneo es que las siguientes expresiones se resuelvan en la entidad creada previamente **HomeAutomation.Device**:

|#|expresión|Entidad identificada|score|
|--|--|--|--|
|1|turn on the ac|HomeAutomation.Device: "ac"|0.8748562|
|2|turn up the heat|HomeAutomation.Device: "heat"|0.784990132|
|3|make it colder|||

Las dos primeras expresiones se asignan a distintos dispositivos. La tercera expresión, "make it colder", no se asigna a un dispositivo, sino que solicita un resultado. LUIS no sabe que el término, "colder", significa que el dispositivo solicitado es el termostato. Lo idóneo es que LUIS resuelva todas estas expresiones en el mismo dispositivo. 

## <a name="use-a-list-entity"></a>Usar una entidad de lista
La entidad HomeAutomation.Device es muy útil para un pequeño número de dispositivos o con pocas variaciones de los nombres. Para un edificio de oficinas o campus, los nombres de dispositivo aumentan más allá de la utilidad de la entidad HomeAutomation.Device. 

Una **entidad de lista** es una buena elección para este escenario porque el conjunto de términos para un dispositivo de un edificio o campus es un conjunto conocido, incluso si es un conjunto de gran tamaño. Mediante el uso de una entidad de lista, LUIS puede recibir cualquier valor posible del conjunto para termostato y resolverlo hasta un único dispositivo "thermostat". 

En este artículo se va a crear una lista de entidades con termostato. Los nombres alternativos para un termostato en este artículo son: 

|Nombres alternativos para termostato|
|--|
| ac |
| a/c|
| a-c|
|heater|
|hot|
|hotter|
|cold|
|colder|

Si LUIS debe determinar un nombre alternativo nuevo con frecuencia, entonces la mejor respuesta es una [lista de frases](luis-concept-feature.md#how-to-use-phrase-lists).

## <a name="create-a-list-entity"></a>Crear una entidad de lista
Cree un archivo Node.js y copie el siguiente código en él. Cambie los valores de authoringKey, appId, versionId y region.

   [!code-javascript[Create DevicesList List Entity](~/samples-luis/documentation-samples/tutorial-list-entity/add-entity-list.js "Create DevicesList List Entity")]

Utilice el siguiente comando para instalar las dependencias NPM y ejecute el código para crear la entidad de lista:

```console
npm install && node add-entity-list.js
```

El resultado de la ejecución es el identificador de la entidad de lista:

```console
026e92b3-4834-484f-8608-6114a83b03a6
```

## <a name="train-the-model"></a>Entrenamiento del modelo
Entrene LUIS para que la nueva lista afecte a los resultados de la consulta. El entrenamiento es un proceso de dos partes, el propio entrenamiento y, a continuación, la comprobación del estado si ha finalizado el entrenamiento. El entrenamiento de una aplicación con muchos modelos puede tardar. El siguiente código entrena la aplicación y luego espera hasta que el entrenamiento es correcto. El código usa una estrategia de espera y reintento para evitar el error 429 "Demasiadas solicitudes". 

Cree un archivo Node.js y copie el siguiente código en él. Cambie los valores de authoringKey, appId, versionId y region.

   [!code-javascript[Train LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/train.js "Train LUIS")]

Use el comando siguiente para ejecutar el código para entrenar la aplicación:

```console
node train.js
```

La salida de la ejecución es el estado de cada iteración del entrenamiento en los modelos LUIS. La siguiente ejecución solo ha requerido una comprobación del entrenamiento:

```console
1 trained = true
[ { modelId: '2c549f95-867a-4189-9c35-44b95c78b70f',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '5530e900-571d-40ec-9c78-63e66b50c7d4',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '519faa39-ae1a-4d98-965c-abff6f743fe6',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9671a485-36a9-46d5-aacd-b16d05115415',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '9ef7d891-54ab-48bf-8112-c34dcd75d5e2',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } },
  { modelId: '8e16a660-8781-4abf-bf3d-f296ebe1bf2d',
    details: { statusId: 2, status: 'UpToDate', exampleCount: 45 } } ]

```
## <a name="publish-the-model"></a>Publicar el modelo
Al publicarlo, la entidad de lista estará disponible desde el punto de conexión.

Cree un archivo Node.js y copie el siguiente código en él. Cambie los valores de endpointKey, appId y region. Se puede usar authoringKey si no tiene previsto llamar a este archivo más allá del límite de cuota.

   [!code-javascript[Publish LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/publish.js "Publish LUIS")]

Use el comando siguiente para ejecutar el código para consultar la aplicación:

```console
node publish.js
```

La salida siguiente incluye la dirección URL del punto de conexión para las consultas. Los resultados JSON reales incluirán el appID real. 

```json
{ 
  versionId: null,
  isStaging: false,
  endpointUrl: 'https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>',
  region: null,
  assignedEndpointKey: null,
  endpointRegion: 'westus',
  publishedDateTime: '2018-01-29T22:17:38Z' }
}
```

## <a name="query-the-app"></a>Consultar la aplicación 
Consulte la aplicación del punto de conexión para probar que la entidad de lista ayuda a LUIS a determinar el tipo de dispositivo.

Cree un archivo Node.js y copie el siguiente código en él. Cambie los valores de endpointKey, appId y region. Se puede usar authoringKey si no tiene previsto llamar a este archivo más allá del límite de cuota.

   [!code-javascript[Query LUIS](~/samples-luis/documentation-samples/tutorial-list-entity/query.js "Query LUIS")]

Use el comando siguiente para ejecutar el código y consultar la aplicación:

```console
node train.js
```

La salida es los resultados de la consulta. Dado que el código agregó el par nombre/valor **verbose** a la cadena de consulta, la salida incluye todas las intenciones y sus puntuaciones:

```json
{
  "query": "turn up the heat",
  "topScoringIntent": {
    "intent": "HomeAutomation.TurnOn",
    "score": 0.139018849
  },
  "intents": [
    {
      "intent": "HomeAutomation.TurnOn",
      "score": 0.139018849
    },
    {
      "intent": "None",
      "score": 0.120624863
    },
    {
      "intent": "HomeAutomation.TurnOff",
      "score": 0.06746891
    }
  ],
  "entities": [
    {
      "entity": "heat",
      "type": "HomeAutomation.Device",
      "startIndex": 12,
      "endIndex": 15,
      "score": 0.784990132
    },
    {
      "entity": "heat",
      "type": "DevicesList",
      "startIndex": 12,
      "endIndex": 15,
      "resolution": {
        "values": [
          "Thermostat"
        ]
      }
    }
  ]
}
```

El dispositivo específico **Thermostat** se identifica con una consulta orientada a resultados de "turn up the heat". Puesto que la entidad HomeAutomation.Device original aún está en la aplicación, también puede ver sus resultados. 

Pruebe las otras dos expresiones para ver que también se devuelven como termostato. 

|#|expresión|Entidad|type|value|
|--|--|--|--|--|
|1|turn on the ac| ac | DevicesList | Thermostat|
|2|turn up the heat|heat| DevicesList |Thermostat|
|3|make it colder|colder|DevicesList|Thermostat|

## <a name="next-steps"></a>Pasos siguientes

Puede crear otra entidad de lista para expandir las ubicaciones de dispositivo a las salas, plantas o edificios. 
