---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 07/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: db866da43310f5407ce4daae1cade2c7512b91ea
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369289"
---
La biblioteca cliente de predicción de Language Understanding (LUIS) para Python puede usarse con los siguientes fines:

* Obtener predicciones por espacio
* Obtener predicción por versión

[Documentación de referencia](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis) | [Prediction runtime Package (PyPi)](https://pypi.org/project/azure-cognitiveservices-language-luis/) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py)

## <a name="prerequisites"></a>Prerrequisitos

* Cuenta de portal de Language Understanding (LUIS): [cree una de forma gratuita](https://www.luis.ai).
* [Python 3.x](https://www.python.org/)
* Un identificador de aplicación de LUIS: use el identificador de aplicación de IoT público de `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. La consulta de usuario que se usa en el código del inicio rápido es específica de esa aplicación.

## <a name="setting-up"></a>Instalación

### <a name="get-your-language-understanding-luis-runtime-key"></a>Obtención de la clave de entorno de ejecución de Language Understanding (LUIS)

Para obtener una [clave de entorno de ejecución](../luis-how-to-azure-subscription.md), cree un recurso de entorno de ejecución de LUIS. Conserve la clave y punto de conexión de la clave para el siguiente paso.

### <a name="create-a-new-python-file"></a>Creación de un archivo de Python

Cree un nuevo archivo de Python en el IDE o en el editor que prefiera y llámelo `prediction_quickstart.py`.

### <a name="install-the-sdk"></a>Instalación del SDK

Desde el directorio de la aplicación, instale la biblioteca cliente del runtime de predicción de Language Understanding (LUIS) para Python con el siguiente comando:

```python
python -m pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>Modelo de objetos

El cliente del entorno de ejecución de predicción de Language Understanding (LUIS) es un objeto [LUISRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python) que se autentica en Azure, que contiene la clave del recurso.

Una vez creado el cliente, úselo para acceder a la funcionalidad, entre la que se incluye:

* Predicción por [almacenamiento provisional o espacio de producción](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)
* Predicción por [versión](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)

## <a name="code-examples"></a>Ejemplos de código

Estos fragmentos de código muestran cómo realizar las siguientes operaciones con la biblioteca cliente del runtime de predicción de Language Understanding (LUIS) para Python:

* [Predicción por espacio](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Adición de las dependencias

En el directorio del proyecto, abra el archivo `prediction_quickstart.py` en el entorno de desarrollo integrado o en el editor que prefiera. Agregue las siguientes dependencias:

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=Dependencies)]

## <a name="authenticate-the-client"></a>Autenticar el cliente

1. Cree variables para su propia información obligatoria de LUIS: la clave de predicción y el punto de conexión.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=AuthorizationVariables)]

1. Cree una variable para el identificador de la aplicación establecida en la aplicación IoT pública, **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** . Cree una variable para establecer la ranura publicada `production`.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=OtherVariables)]


1. Cree un objeto de credenciales con la clave y úselo con el punto de conexión para crear un objeto [LUISRuntimeClientConfiguration](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python).

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=Client)]

## <a name="get-prediction-from-runtime"></a>Obtención de la predicción a partir del entorno de ejecución

Agregue el método siguiente para crear la solicitud al entorno de ejecución de predicción.

La expresión de usuario forma parte del objeto [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python).

El método **[get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** necesita varios parámetros como, por ejemplo, el identificador de la aplicación, el nombre de la ranura y el objeto de la solicitud de predicción para completar la solicitud. Las demás opciones, como verbose, muestran todas las intenciones y el registro es opcional. La solicitud devuelve un objeto [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python).

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=predict)]

## <a name="main-code-for-the-prediction"></a>Código principal de la predicción

Use el método Main siguiente para vincular las variables y los métodos para obtener la predicción.

```python
predict(luisAppID, luisSlotName)
```
## <a name="run-the-application"></a>Ejecución de la aplicación

Ejecute la aplicación con el comando `python prediction_quickstart.py` desde el directorio de la aplicación.

```console
python prediction_quickstart.py
```

La consola del inicio rápido muestra la salida:

```console
Top intent: HomeAutomation.TurnOn
Sentiment: None
Intents:
        "HomeAutomation.TurnOn"
Entities: {'HomeAutomation.Operation': ['on']}
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con las predicciones, limpie el trabajo de este inicio rápido mediante la eliminación del archivo y sus subdirectorios.
