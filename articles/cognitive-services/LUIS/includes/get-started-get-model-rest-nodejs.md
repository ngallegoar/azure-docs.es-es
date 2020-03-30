---
title: Obtención del modelo con la llamada de REST en C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: bbb2ae0b10af795d71f0a78c045bec0c216ee378
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368416"
---
## <a name="prerequisites"></a>Prerequisites

* Azure Language Understanding: creación de una clave de 32 caracteres y de la dirección URL del punto de conexión de creación. Cree con [Azure Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) o con la [CLI de Azure](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli).
* Importe la aplicación [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) desde el repositorio de GitHub cognitive-services-language-understanding.
* El identificador de LUIS de la aplicación TravelAgent importada. El identificador de aplicación se muestra en el panel de la aplicación.
* El identificador de versión dentro de la aplicación que recibe las expresiones. El id. predeterminado es "0.1".
* Lenguaje de programación [Node.js](https://nodejs.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]


## <a name="change-model-programmatically"></a>Cambio de modelo mediante programación

1. Cree un nuevo archivo llamado `model.js`. Agregue el siguiente código:

    ```javascript
    var request = require('request');
    var requestpromise = require('request-promise');

    // 32 character key value
    const LUIS_authoringKey = "YOUR-KEY";

    // endpoint example: your-resource-name.api.cognitive.microsoft.com
    const LUIS_endpoint = "YOUR-ENDPOINT";
    const LUIS_appId = "YOUR-APP-ID";
    const LUIS_versionId = "0.1";
    const addUtterancesURI = `https://${LUIS_endpoint}/luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/examples`;
    const addTrainURI = `https://${LUIS_endpoint}/luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/train`;

    const utterances = [
            {
              'text': 'go to Seattle today',
              'intentName': 'BookFlight',
              'entityLabels': [
                {
                  'entityName': 'Location::LocationTo',
                  'startCharIndex': 6,
                  'endCharIndex': 12
                }
              ]
            },
            {
                'text': 'a barking dog is annoying',
                'intentName': 'None',
                'entityLabels': []
            }
          ];

    const main = async() =>{


        await addUtterance();
        await train("POST");
        await trainStatus("GET");

    }
    const addUtterance = async () => {

        const options = {
            uri: addUtterancesURI,
            method: 'POST',
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: utterances
        };

        const response = await requestpromise(options)
        console.log(response.body);
    }
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

        const response = await requestpromise(options)
        console.log(response.body);
    }

    // MAIN
    main().then(() => console.log("done")).catch((err)=> console.log(err returned));
    ```

1. Reemplace los valores a partir de `YOUR-` por sus propios valores.

    |Information|Propósito|
    |--|--|
    |`YOUR-KEY`|La clave de creación de 32 caracteres.|
    |`YOUR-ENDPOINT`| El punto de conexión de la dirección URL de creación. Por ejemplo, `replace-with-your-resource-name.api.cognitive.microsoft.com`. El nombre del recurso se establece al crear el recurso.|
    |`YOUR-APP-ID`| El identificador de la aplicación de LUIS. |

    Las claves y recursos asignados son visibles en el portal de LUIS, en la sección Administrar de la página de **recursos de Azure**. El identificador de la aplicación está disponible en la misma sección Administrar, en la página **Configuración de la aplicación**.

1. Con un símbolo del sistema en el mismo directorio que donde creó el archivo, escriba el siguiente comando para ejecutar el archivo:

    ```console
    node model.js
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para una aplicación](../luis-concept-best-practices.md)