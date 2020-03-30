---
title: Obtención de intención con la llamada a REST en Python
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: 23ac98f91c989c9bedb6b91e6a7ce26dc164ac5a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76987810"
---
## <a name="prerequisites"></a>Prerequisites

* [Python 3.6](https://www.python.org/downloads/) o versiones posteriores.
* [Visual Studio Code](https://code.visualstudio.com/)
* Identificador de la aplicación pública: `df67dcdb-c37d-46af-88e1-8b97951ca1c2`

## <a name="create-luis-runtime-key-for-predictions"></a>Creación de una clave de tiempo de ejecución de LUIS para predicciones

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Haga clic en [Crear **Language Understanding**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne).
1. Establezca todas las opciones de configuración necesarias para la clave de **tiempo de ejecución**:

    |Configuración|Value|
    |--|--|
    |Nombre|Nombre que quiera (2-64 caracteres).|
    |Subscription|Seleccione una suscripción adecuada.|
    |Location|Seleccione cualquier ubicación disponible cercana.|
    |Nivel de precios|`F0`: el plan de tarifa mínimo.|
    |Grupo de recursos|Seleccione un grupo de recursos disponible.|

1. Haga clic en **Crear** y espere a que el recurso se cree. Una vez creado, vaya a la página de recursos.
1. Recopile el elemento `endpoint` configurado y una `key`.

## <a name="get-intent-from-the-prediction-endpoint"></a>Obtención de la intención desde el punto de conexión de predicción

Use Python para consultar el [punto de conexión de predicción](https://aka.ms/luis-apim-v3-prediction) y obtener un resultado de predicción.

1. Copie este fragmento de código en un archivo llamado `predict.py`:

    ```python
    ########### Python 3.6 #############
    import requests

    try:

        key = 'YOUR-KEY' # your Runtime key
        endpoint = 'YOUR-ENDPOINT' # such as 'your-resource-name.api.cognitive.microsoft.com'
        appId = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
        utterance = 'turn on all lights'

        headers = {
        }

        params ={
            'query': utterance,
            'timezoneOffset': '0',
            'verbose': 'true',
            'show-all-intents': 'true',
            'spellCheck': 'false',
            'staging': 'false',
            'subscription-key': key
        }

        r = requests.get(f'https://{endpoint}/luis/prediction/v3.0/apps/{appId}/slots/production/predict',headers=headers, params=params)
        print(r.json())

    except Exception as e:
        print(f'{e}')
    ```

1. Reemplace los valores `YOUR-KEY` y `YOUR-ENDPOINT` por su propia clave y punto de conexión del **tiempo de ejecución** de la predicción.

    |Information|Propósito|
    |--|--|
    |`YOUR-KEY`|La clave de **tiempo de ejecución** de predicción de 32 caracteres.|
    |`YOUR-ENDPOINT`| El punto de conexión de la dirección URL de predicción. Por ejemplo, `replace-with-your-resource-name.api.cognitive.microsoft.com`.|

1. Instale la dependencia `requests`. Se utiliza para realizar solicitudes HTTP:

    ```console
    pip install requests
    ```

1. Ejecute el script con este comando de la consola:

    ```console
    python predict.py
    ```

1. Revise la respuesta de la predicción, que se devuelve como JSON:

    ```console
    {'query': 'turn on all lights', 'prediction': {'topIntent': 'HomeAutomation.TurnOn', 'intents': {'HomeAutomation.TurnOn': {'score': 0.5375382}, 'None': {'score': 0.08687421}, 'HomeAutomation.TurnOff': {'score': 0.0207554}}, 'entities': {'HomeAutomation.Operation': ['on'], '$instance': {'HomeAutomation.Operation': [{'type': 'HomeAutomation.Operation', 'text': 'on', 'startIndex': 5, 'length': 2, 'score': 0.724984169, 'modelTypeId': -1, 'modelType': 'Unknown', 'recognitionSources': ['model']}]}}}}
    ```

    A continuación se incluye la respuesta JSON con formato para mejorar la legibilidad:

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
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

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de expresiones y entrenamiento](../get-started-get-model-rest-apis.md)