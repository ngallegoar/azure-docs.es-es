---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: 9c15e4217c5331346c5a95329bae7e2a4f0e0841
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732074"
---
La biblioteca cliente de entorno de ejecución de Language Understanding (LUIS) para Node.js puede usarse con los siguientes fines:

* Predicción por ranura
* Predicción por versión

[Documentación de referencia](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/?view=azure-node-latest) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/cognitiveservices/cognitiveservices-luis-runtime) | [Paquete de entorno de ejecución (NPM)](https://www.npmjs.com/package/@azure/cognitiveservices-luis-runtime) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/javascript/LUIS/luis_prediction.js)

## <a name="prerequisites"></a>Prerrequisitos

* Recurso de entorno de ejecución de Language Understanding: [Crear uno en Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne)
* [Node.js](https://nodejs.org)
* Un identificador de aplicación de LUIS: use el identificador de aplicación de IoT público de `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. La consulta de usuario que se usa en el código del inicio rápido es específica de esa aplicación.

## <a name="setting-up"></a>Instalación

### <a name="get-your-language-understanding-luis-runtime-key"></a>Obtención de la clave de entorno de ejecución de Language Understanding (LUIS)

Para obtener una [clave de entorno de ejecución](../luis-how-to-azure-subscription.md), cree un recurso de entorno de ejecución de LUIS. Conserve la clave y punto de conexión de la clave para el siguiente paso.

[!INCLUDE [Set up environment variables for prediction quickstart](sdk-prediction-environment-variables.md)]

### <a name="create-a-new-javascript-nodejs-file"></a>Creación de un archivo de JavaScript (Node.js)

Cree un archivo de JavaScript en el entorno de desarrollo integrado (IDE) o editor que prefiera y llámelo `luis_prediction.js`.

### <a name="install-the-npm-library-for-the-luis-runtime"></a>Instalación de la biblioteca NPM para el entorno de ejecución de LUIS

Dentro del directorio de aplicaciones, instale las dependencias con el siguiente comando:

```console
npm install @azure/cognitiveservices-luis-runtime @azure/ms-rest-js
```

## <a name="object-model"></a>Modelo de objetos

El cliente de creación de Language Understanding (LUIS) es un objeto [LUISAuthoringClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest) que se autentica en Azure, que contiene la clave de creación.

Una vez creado el cliente, úselo para acceder a la funcionalidad, entre la que se incluye:

* [Predicción](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-) por ranura `staging` o `production`
* [Predicción por versión](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getversionprediction-string--string--predictionrequest--models-predictiongetversionpredictionoptionalparams-)

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes acciones con la biblioteca cliente de entorno de ejecución de predicción de Language Understanding (LUIS):

* [Predicción por espacio](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Adición de las dependencias

En el directorio del proyecto, abra el archivo `luis_prediction.js` en el entorno de desarrollo integrado o en el editor que prefiera. Agregue las siguientes dependencias:

[!code-javascript [Dependencies](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Dependencies)]

## <a name="authenticate-the-client"></a>Autenticar el cliente

1. Cree variables para su propia información obligatoria de LUIS:

    Agregue variables para administrar una clave de predicción extraída de una variable de entorno denominada `LUIS_RUNTIME_KEY`. Si ha creado la variable de entorno una vez iniciada la aplicación, el editor, IDE o shell que se esté ejecutando se deberá cerrar y volver a cargar para acceder a la variable. Los métodos se crearán más adelante.

    Cree una variable que contenga el nombre del recurso `LUIS_RUNTIME_ENDPOINT`.

    [!code-javascript [Azure resource variables](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Variables)]

1. Cree una variable para el identificador de la aplicación como una variable de entorno denominada `LUIS_APP_ID`. Establezca la variable de entorno en la aplicación de IoT pública, **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** . Cree una variable para establecer la ranura publicada `production`.

    [!code-javascript [LUIS app variables](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=OtherVariables)]


1. Cree un objeto ApiKeyServiceClientCredentials con la clave y úselo con el punto de conexión para crear un objeto [LUIS.LUISRuntimeClient](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/luisruntimeclient?view=azure-node-latest).

    [!code-javascript [LUIS Runtime client is required to access predictions for LUIS apps](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=AuthoringCreateClient)]

## <a name="get-prediction-from-runtime"></a>Obtención de la predicción a partir del entorno de ejecución

Agregue el método siguiente para crear la solicitud al entorno de ejecución de predicción.

La expresión del usuario forma parte del objeto [predictionRequest](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionrequest?view=azure-node-latest).

El método **[luisRuntimeClient.prediction.getSlotPrediction](https://docs.microsoft.com/javascript/api/@azure/cognitiveservices-luis-runtime/predictionoperations?view=azure-node-latest#getslotprediction-string--string--predictionrequest--models-predictiongetslotpredictionoptionalparams-)** necesita varios parámetros como, por ejemplo, el identificador de la aplicación, el nombre de la ranura y el objeto de la solicitud de predicción para completar la solicitud. Las demás opciones, como verbose, muestran todas las intenciones y el registro es opcional.

[!code-javascript [LUIS prediction request and response in Node.js NPM SDK](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=predict)]

## <a name="main-code-for-the-prediction"></a>Código principal de la predicción

Use el método Main siguiente para vincular las variables y los métodos para obtener la predicción.

[!code-javascript [Main method and main call](~/cognitive-services-quickstart-code/javascript/LUIS/luis_prediction.js?name=Main)]

## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `node luis_prediction.js` desde el directorio de la aplicación.

```console
node luis_prediction.js
```

El resultado de la predicción devuelve un objeto JSON:

```console
{
   "query":"turn on all lights",
   "prediction":{
      "topIntent":"HomeAutomation.TurnOn",
      "intents":{
         "HomeAutomation.TurnOn":{
            "score":0.5375382
         },
         "None":{
            "score":0.08687421
         },
         "HomeAutomation.TurnOff":{
            "score":0.0207554
         }
      },
      "entities":{
         "HomeAutomation.Operation":[
            "on"
         ],
         "$instance":{
            "HomeAutomation.Operation":[
               {
                  "type":"HomeAutomation.Operation",
                  "text":"on",
                  "startIndex":5,
                  "length":2,
                  "score":0.724984169,
                  "modelTypeId":-1,
                  "modelType":"Unknown",
                  "recognitionSources":[
                     "model"
                  ]
               }
            ]
         }
      }
   }
}
```


## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con las predicciones, limpie el trabajo de este inicio rápido mediante la eliminación del archivo y sus subdirectorios.
