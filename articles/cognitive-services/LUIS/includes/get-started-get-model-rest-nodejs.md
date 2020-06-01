---
title: Obtención del modelo con la llamada de REST en C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/18/2020
ms.author: diberry
ms.openlocfilehash: 19f72dbb62fc2084bf0c9609fb3782e083c911af
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655492"
---
## <a name="prerequisites"></a>Prerrequisitos

* Azure Language Understanding: creación de una clave de 32 caracteres y de la dirección URL del punto de conexión de creación. Cree con [Azure Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) o con la [CLI de Azure](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli).
* Importe la aplicación de [pizza](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json) desde el repositorio de GitHub `Azure-Samples/cognitive-services-sample-data-files`.
* El identificador de la aplicación de LUIS para la aplicación de pizza importada. El identificador de aplicación se muestra en el panel de la aplicación.
* El identificador de versión dentro de la aplicación que recibe las expresiones.
* Lenguaje de programación [Node.js](https://nodejs.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="create-the-nodejs-project"></a>Creación del proyecto Node.js

1. Cree una nueva carpeta para almacenar el proyecto de Node. js, como `node-model-with-rest`.

1. Abra un nuevo símbolo del sistema, vaya a la carpeta que ha creado y ejecute el siguiente comando:

    ```console
    npm init
    ```

    Presione Entrar en cada aviso para aceptar los valores predeterminados.

1. Introduzca el siguiente comando para instalar el módulo request-promise:

    ```console
    npm install --save request-promise
    ```

## <a name="change-model-programmatically"></a>Cambio de modelo mediante programación

1. Cree un nuevo archivo llamado `model.js`. Agregue el siguiente código:

    ```javascript
    var request = require('request-promise');

    //////////
    // Values to modify.

    // YOUR-APP-ID: The App ID GUID found on the www.luis.ai Application Settings page.
    const LUIS_appId = "YOUR-APP-ID";

    // YOUR-AUTHORING-KEY: Your LUIS authoring key, 32 character value.
    const LUIS_authoringKey = "YOUR-AUTHORING-KEY";

    // YOUR-AUTHORING-ENDPOINT: Replace this with your authoring key endpoint.
    // For example, "https://your-resource-name.api.cognitive.microsoft.com/"
    const LUIS_endpoint = "YOUR-AUTHORING-ENDPOINT";

    // NOTE: Replace this your version number. The Pizza app uses a version number of "0.1".
    const LUIS_versionId = "0.1";
    //////////

    const addUtterancesURI = `${LUIS_endpoint}luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/examples`;
    const addTrainURI = `${LUIS_endpoint}luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/train`;

    const utterances = [
        {
            'text': 'order a pizza',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 6,
                    'endCharIndex': 12
                }
            ]
        },
        {
            'text': 'order a large pepperoni pizza',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 6,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'FullPizzaWithModifiers',
                    'startCharIndex': 6,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'PizzaType',
                    'startCharIndex': 14,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'Size',
                    'startCharIndex': 8,
                    'endCharIndex': 12
                }
            ]
        },
        {
            'text': 'I want two large pepperoni pizzas on thin crust',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 7,
                    'endCharIndex': 46
                },
                {
                    'entityName': 'FullPizzaWithModifiers',
                    'startCharIndex': 7,
                    'endCharIndex': 46
                },
                {
                    'entityName': 'PizzaType',
                    'startCharIndex': 17,
                    'endCharIndex': 32
                },
                {
                    'entityName': 'Size',
                    'startCharIndex': 11,
                    'endCharIndex': 15
                },
                {
                    'entityName': 'Quantity',
                    'startCharIndex': 7,
                    'endCharIndex': 9
                },
                {
                    'entityName': 'Crust',
                    'startCharIndex': 37,
                    'endCharIndex': 46
                }
            ]
        }
    ];

    // Main function.
    const main = async() =>{

        await addUtterances(utterances);
        await train("POST");
        await train("GET");

    }

    // Adds the utterances to the model.
    const addUtterances = async (utterances) => {

        const options = {
            uri: addUtterancesURI,
            method: 'POST',
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: utterances
        };

        const response = await request(options)
        console.log("addUtterance:\n" + JSON.stringify(response, null, 2));
    }

    // With verb === "POST", sends a training request.
    // With verb === "GET", obtains the training status.
    const train = async (verb) => {

        const options = {
            uri: addTrainURI,
            method: verb,
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: null // The body can be empty for a training request
        };

        const response = await request(options)
        console.log("train " + verb + ":\n" + JSON.stringify(response, null, 2));
    }

    // MAIN
    main().then(() => console.log("done")).catch((err)=> console.log(err));
    ```

1. Reemplace los valores a partir de `YOUR-` por sus propios valores.

    |Information|Propósito|
    |--|--|
    |`YOUR-APP-ID`| El identificador de la aplicación de LUIS. |
    |`YOUR-AUTHORING-KEY`|La clave de creación de 32 caracteres.|
    |`YOUR-AUTHORING-ENDPOINT`| El punto de conexión de la dirección URL de creación. Por ejemplo, `https://replace-with-your-resource-name.api.cognitive.microsoft.com/`. El nombre del recurso se establece al crear el recurso.|

    Las claves y recursos asignados son visibles en el portal de LUIS, en la sección Administrar de la página de **recursos de Azure**. El identificador de la aplicación está disponible en la misma sección Administrar, en la página **Configuración de la aplicación**.

1. En el símbolo del sistema, escriba el siguiente comando para ejecutar el proyecto:

    ```console
    node model.js
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine la carpeta del proyecto del sistema de archivos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para una aplicación](../luis-concept-best-practices.md)