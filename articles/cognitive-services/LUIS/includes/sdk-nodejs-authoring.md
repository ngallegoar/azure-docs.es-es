---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 05/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 6e240a0c5d5d77489c92862238c2e5041bdeabe3
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84171363"
---
La biblioteca cliente de creación de Language Understanding (LUIS) para Node.js puede usarse con los siguientes fines:

* Crear una aplicación.
* Agregar intenciones, entidades y expresiones de ejemplo.
* Agregar características, como una lista de frases.
* Entrenar y publicar una aplicación.
* Eliminar la aplicación

[Documentación de referencia](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/?view=azure-node-latest) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-authoring) | [Paquete de creación (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-authoring), [Paquete de runtime (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/luis_authoring_quickstart.js)

## <a name="prerequisites"></a>Prerrequisitos

* Recurso de creación de Language Understanding: [Crear uno en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)

## <a name="setting-up"></a>Instalación

### <a name="get-your-language-understanding-luis-starter-key"></a>Obtención de la clave de inicio de Language Understanding (LUIS)

Para obtener la [clave de inicio](../luis-how-to-azure-subscription.md#starter-key), cree un recurso de creación de LUIS. Conserve la clave y punto de conexión de la clave para el siguiente paso.

### <a name="create-an-environment-variable"></a>Creación de una variable de entorno

Con la clave y la región de la clave, cree dos variables de entorno para la autenticación:

* `LUIS_AUTHORING_KEY`: la clave de recurso para autenticar las solicitudes.
* `LUIS_AUTHORING_ENDPOINT`: el punto de conexión asociado a la clave.

Siga las instrucciones adecuadas para su sistema operativo.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx LUIS_AUTHORING_KEY <replace-with-your-luis-authoring-key
setx LUIS_AUTHORING_ENDPOINT <replace-with-your-luis-authoring-endpoint>
```

Después de agregar la variable de entorno, reinicie la ventana de la consola.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export LUIS_AUTHORING_KEY=<replace-with-your-luis-authoring-key>
export LUIS_AUTHORING_ENDPOINT=<replace-with-your-luis-authoring-endpoint>
```

Después de agregar la variable de entorno, ejecute `source ~/.bashrc` desde la ventana de consola para que los cambios surtan efecto.

#### <a name="macos"></a>[macOS](#tab/unix)

Edite `.bash_profile` y agregue la variable de entorno:

```bash
export LUIS_AUTHORING_KEY=<replace-with-your-luis-authoring-key>
export LUIS_AUTHORING_ENDPOINT=<replace-with-your-luis-authoring-endpoint>
```

Después de agregar la variable de entorno, ejecute `source .bash_profile` desde la ventana de consola para que los cambios surtan efecto.
***

### <a name="install-the-npm-library-for-luis-authoring"></a>Instalación de la biblioteca NPM para la creación de LUIS

Dentro del directorio de aplicaciones, instale las dependencias con el siguiente comando:

```console
npm install @azure/cognitiveservices-luis-authoring @azure/ms-rest-js
```

## <a name="object-model"></a>Modelo de objetos

El cliente de creación de Language Understanding (LUIS) es un objeto [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient?view=azure-node-latest) que se autentica en Azure, que contiene la clave de creación.

Una vez creado el cliente, úselo para acceder a la funcionalidad, entre la que se incluye:

* Aplicaciones: [agregar](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#add-applicationcreateobject--msrest-requestoptionsbase-), [eliminar](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#deletemethod-string--models-appsdeletemethodoptionalparams-), [publicar](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#publish-string--applicationpublishobject--msrest-requestoptionsbase-)
* Expresiones de ejemplo: [adición por lote](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#batch-string--string--examplelabelobject----msrest-requestoptionsbase-), [eliminación por identificador](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#deletemethod-string--string--number--msrest-requestoptionsbase-)
* Características: administración de [listas de frases](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/features?view=azure-node-latest#addphraselist-string--string--phraselistcreateobject--msrest-requestoptionsbase-)
* Modelo: administración de [intenciones](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) y entidades
* Patrón: administración de [patrones](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/pattern?view=azure-node-latest#addpattern-string--string--patternrulecreateobject--msrest-requestoptionsbase-)
* Entrenamiento: [entrenamiento](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#trainversion-string--string--msrest-requestoptionsbase-) de la aplicación y sondeo del [estado del entrenamiento](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#getstatus-string--string--msrest-requestoptionsbase-)
* [Versiones](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/versions?view=azure-node-latest): administración con clonación, exportación y eliminación


## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes acciones con la biblioteca cliente de creación de Language Understanding (LUIS) para Node.js:

* [Creación de una aplicación](#create-a-luis-app)
* [Adición de entidades](#create-entities-for-the-app)
* [Adición de intenciones](#create-intent-for-the-app)
* [Add example utterances](#add-example-utterance-to-intent) (Adición de expresiones de ejemplo)
* [Entrenamiento de la aplicación](#train-the-app)
* [Publicación de la aplicación](#publish-a-language-understanding-app)
* [Eliminación de la aplicación](#delete-a-language-understanding-app)
* [Enumeración de las aplicaciones](#list-language-understanding-apps)

## <a name="create-a-new-nodejs-application"></a>Creación de una aplicación Node.js

Cree un nuevo archivo de texto en el IDE o en el editor que prefiera y llámelo `luis_authoring_quickstart.js`. Después agregue las siguientes dependencias.

[!code-javascript[Create a new application in your preferred editor or IDE.](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=Dependencies)]

Cree variables para el punto de conexión y la clave de Azure del recurso. Si ha creado la variable de entorno después de haber iniciado la aplicación, deberá cerrar y volver a abrir el editor, el IDE o el shell que lo ejecuta para acceder a la variable.

[!code-javascript[Create variables for your resource's Azure endpoint and key.](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=Variables)]

## <a name="authenticate-the-client"></a>Autenticar el cliente

Cree un objeto [CognitiveServicesCredentials](https://docs.microsoft.com/javascript/api/@azure/ms-rest-js/apikeycredentials?view=azure-node-latest) con la clave y úselo con el punto de conexión para crear un objeto [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/luisauthoringclient?view=azure-node-latest).

[!code-javascript[Create LUIS client object](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringCreateClient)]

## <a name="create-a-luis-app"></a>Creación de una aplicación de LUIS

1. Cree una aplicación de LUIS que incluya el modelo de procesamiento de lenguaje natural que contiene intenciones, entidades y expresiones de ejemplo.

1. Cree el método [add](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest) de un objeto [AppsOperation](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest) para crear la aplicación. El nombre y la referencia cultural del idioma son propiedades necesarias.

    [!code-javascript[Create LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringCreateApplication&highlight=9-11)]


## <a name="create-intent-for-the-app"></a>Creación de una intención para la aplicación
El objeto principal en el modelo de una aplicación LUIS es la intención. Esta se alinea con una agrupación de _intenciones_ de expresiones de usuario. Un usuario puede formular una pregunta o realizar una declaración buscando una respuesta _intencionada_ concreta de un bot (u otra aplicación cliente). Ejemplos de intenciones pueden ser la reserva de un vuelo, preguntar sobre el tiempo en una ciudad de destino o pedir la información de contacto del servicio de atención al cliente.

Use el método [model.add_intent](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addintent-string--string--modelcreateobject--msrest-requestoptionsbase-) con el nombre de la intención única y, después, pase el identificador de la aplicación, el identificador de la versión y el nuevo nombre de la intención.

[!code-javascript[Create intent](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringAddIntents&highlight=2-6)]

## <a name="create-entities-for-the-app"></a>Creación de entidades para la aplicación

Si bien las entidades no son necesarias, se encuentran en la mayoría de las aplicaciones. La entidad extrae información de la expresión del usuario, necesaria para cumplir la intención del usuario. Hay varios tipos de entidades [precompiladas](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest#addcustomprebuiltentity-string--string--prebuiltdomainmodelcreateobject--msrest-requestoptionsbase-) y personalizadas, cada una con sus propios modelos de objeto de transformación de datos.  Las entidades precompiladas comunes que se pueden agregar a una aplicación incluyen [number](../luis-reference-prebuilt-number.md), [datetimeV2](../luis-reference-prebuilt-datetimev2.md), [geographyV2](../luis-reference-prebuilt-geographyv2.md) y [ordinal](../luis-reference-prebuilt-ordinal.md).

Este método **add_entities** crea una entidad simple `Location` con dos roles, una entidad simple `Class`, una entidad compuesta `Flight` y agrega varias entidades precompiladas.

Es importante saber que las entidades no se marcan con una intención. Pueden aplicarse, y así suelen hacerlo, a muchas intenciones. Solo las expresiones de usuario de ejemplo se marcan para una única intención específica.

Los métodos de creación de entidades forman parte de la clase [Model](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/model?view=azure-node-latest). Cada tipo de entidad tiene su propio modelo de objeto de transformación de datos (DTO).

[!code-javascript[Create entities for the app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringAddEntities&highlight=2-6)]

## <a name="add-example-utterance-to-intent"></a>Adición de expresiones de ejemplo a la intención

Con el fin de determinar la intención de una expresión y extraer las entidades, la aplicación necesita ejemplos de expresiones. Los ejemplos deben tener como objetivo una única intención específica y deben marcar todas las entidades personalizadas. No es necesario marcar las entidades precompiladas.

Para agregar expresiones de ejemplo, cree una lista de objetos [ExampleLabelObject](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examplelabelobject?view=azure-node-latest), un objeto para cada expresión de ejemplo. Cada ejemplo debe marcar todas las entidades con un diccionario de pares nombre-valor de nombre de entidad y valor de entidad. El valor de la entidad debe reflejar exactamente lo que aparece en el texto de la expresión de ejemplo.

Llame a [examples.batch](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/examples?view=azure-node-latest#batch-string--string--examplelabelobject----msrest-requestoptionsbase-) con el identificador de la aplicación, el identificador de la versión y la lista de ejemplos. La llamada responde con una lista de resultados. Debe comprobar el resultado de cada ejemplo, para asegurarse de que se ha agregado correctamente al modelo.

[!code-javascript[Add example utterance to intent](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringBatchAddUtterancesForIntent&highlight=52-56)]

## <a name="train-the-app"></a>Entrenamiento de la aplicación

Una vez creado el modelo, debe entrenarse la aplicación de LUIS para esta versión del modelo. Un modelo entrenado puede usarse en un [contenedor](../luis-container-howto.md) o bien [publicarse](../luis-how-to-publish-app.md) en los espacios de ensayo o del producto.

El método [train.trainVersion](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#trainversion-string--string--msrest-requestoptionsbase-) necesita el identificador de la aplicación y el identificador de la versión.

Un modelo muy pequeño, como el que se muestra en este inicio rápido, se entrenará muy rápidamente. En el caso de las aplicaciones en el nivel de producción, el entrenamiento de la aplicación debe incluir una llamada de sondeo al método [get_status](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/train?view=azure-node-latest#getstatus-string--string--msrest-requestoptionsbase-) para determinar si el entrenamiento se ha realizado correctamente. La respuesta es una lista de objetos [ModelTrainingInfo](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/modeltraininginfo?view=azure-node-latest) con un estado independiente para cada objeto. Para que el entrenamiento se considere completo, todos los objetos deben ser correctos.

[!code-javascript[Train LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringTrainVersion&highlight=2-5)]

El entrenamiento de todos los modelos lleva tiempo. Use operationResult para comprobar el estado del entrenamiento.

[!code-javascript[Get training status](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringWaitForOperation&highlight=11-14)]

## <a name="publish-a-language-understanding-app"></a>Publicación de una aplicación de Language Understanding

Publique la aplicación de LUIS con el método [app.publish](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#publish-string--applicationpublishobject--msrest-requestoptionsbase-). Así se publica la versión entrenada actual en el espacio especificado en el punto de conexión. La aplicación cliente usa este punto de conexión para enviar expresiones del usuario y con ellas predecir la intención y extraer entidades.

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringPublishVersion&highlight=2-5)]

## <a name="delete-a-language-understanding-app"></a>Eliminación de una aplicación de Language Understanding

Elimine la aplicación de LUIS con el método [app.deleteMethod](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-authoring/apps?view=azure-node-latest#deletemethod-string--models-appsdeletemethodoptionalparams-). Esto elimina la aplicación actual.

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringDeleteApp&highlight=2)]

## <a name="list-language-understanding-apps"></a>Enumeración de las aplicaciones de Language Understanding

Obtenga una lista de las aplicaciones asociadas a la clave de Language Understanding

[!code-javascript[Publish LUIS client app](~/cognitive-services-quickstart-code/javascript/LUIS/node-sdk-authoring-prediction/luis_authoring_quickstart.js?name=AuthoringListApps)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `node luis_authoring_quickstart.js` en el archivo de inicio rápido.

```console
node luis_authoring_quickstart.js
```

La salida de la línea de comandos de la aplicación es:

```console
Created LUIS app with ID e137a439-b3e0-4e16-a7a8-a9746e0715f7
Entity Destination created.
Entity Class created.
Entity Flight created.
Intent FindFlights added.
Created 3 entity labels.
Created 3 entity labels.
Created 3 entity labels.
Example utterances added.
Waiting for train operation to finish...
Current model status: ["Queued"]
Current model status: ["InProgress"]
Current model status: ["InProgress"]
Current model status: ["InProgress"]
Current model status: ["Success"]
Application published. Endpoint URL: https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/e137a439-b3e0-4e16-a7a8-a9746e0715f7
Application with ID e137a439-b3e0-4e16-a7a8-a9746e0715f7 deleted. Operation result: Operation Successful
```
