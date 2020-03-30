---
title: Obtención del modelo con la llamada de REST en Go
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: 8d180eeffdbc41db6fa0e636daf7702faad47fcc
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368458"
---
## <a name="prerequisites"></a>Prerequisites

* Azure Language Understanding: creación de una clave de 32 caracteres y de la dirección URL del punto de conexión de creación. Cree con [Azure Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) o con la [CLI de Azure](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli).
* Importe la aplicación [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) desde el repositorio de GitHub cognitive-services-language-understanding.
* El identificador de LUIS de la aplicación TravelAgent importada. El identificador de aplicación se muestra en el panel de la aplicación.
* El identificador de versión dentro de la aplicación que recibe las expresiones. El id. predeterminado es "0.1".
* Lenguaje de programación [Go](https://golang.org/)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="change-model-programmatically"></a>Cambio de modelo mediante programación

1. Cree un nuevo archivo llamado `predict.go`. Agregue el siguiente código:

    ```go
    // dependencies
    package main
    import (
        "fmt"
        "net/http"
        "io/ioutil"
        "log"
        "strings"
    )

    // main function
    func main() {

        // NOTE: change to your app ID
        var appID = "YOUR-APP-ID"

        // NOTE: change to your authoring key
        var authoringKey = "YOUR-KEY"

        // NOTE: change to your authoring key's endpoint, for example, your-resource-name.api.cognitive.microsoft.com
        var endpoint = "YOUR-ENDPOINT"

        var version = "0.1"

        var exampleUtterances = `
        [
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
          ]
        `

        fmt.Println("add example utterances requested")
        addUtterance(authoringKey, appID, version, exampleUtterances, endpoint)

        fmt.Println("training selected")
        requestTraining(authoringKey, appID, version, endpoint)

        fmt.Println("training status selected")
        getTrainingStatus(authoringKey, appID, version, endpoint)
    }

    // get utterances from file and add to model
    func addUtterance(authoringKey string, appID string,  version string, labeledExampleUtterances string, endpoint string){

        var authoringUrl = fmt.Sprintf("https://%s/luis/authoring/v3.0-preview/apps/%s/versions/%s/examples", endpoint, appID, version)

        httpRequest("POST", authoringUrl, authoringKey, labeledExampleUtterances)
    }
    func requestTraining(authoringKey string, appID string,  version string, endpoint string){

        trainApp("POST", authoringKey, appID, version, endpoint)
    }
    func trainApp(httpVerb string, authoringKey string, appID string,  version string, endpoint string){

        var authoringUrl = fmt.Sprintf("https://%s/luis/authoring/v3.0-preview/apps/%s/versions/%s/train", endpoint, appID, version)

        httpRequest(httpVerb,authoringUrl, authoringKey, "")
    }
    func getTrainingStatus(authoringKey string, appID string, version string, endpoint string){

        trainApp("GET", authoringKey, appID, version, endpoint)
    }
    // generic HTTP request
    // includes setting header with authoring key
    func httpRequest(httpVerb string, url string, authoringKey string, body string){

        client := &http.Client{}

        request, err := http.NewRequest(httpVerb, url, strings.NewReader(body))
        request.Header.Add("Ocp-Apim-Subscription-Key", authoringKey)

        fmt.Println("body")
        fmt.Println(body)

        response, err := client.Do(request)
        if err != nil {
            log.Fatal(err)
        } else {
            defer response.Body.Close()
            contents, err := ioutil.ReadAll(response.Body)
            if err != nil {
                log.Fatal(err)
            }
            fmt.Println("   ", response.StatusCode)
            fmt.Println(string(contents))
        }
    }
    ```

1. Reemplace los valores a partir de `YOUR-` por sus propios valores.

    |Information|Propósito|
    |--|--|
    |`YOUR-KEY`|La clave de creación de 32 caracteres.|
    |`YOUR-ENDPOINT`| El punto de conexión de la dirección URL de creación. Por ejemplo, `replace-with-your-resource-name.api.cognitive.microsoft.com`. El nombre del recurso se establece al crear el recurso.|
    |`YOUR-APP-ID`| El identificador de la aplicación de LUIS. |

    Las claves y recursos asignados son visibles en el portal de LUIS, en la sección Administrar de la página de **recursos de Azure**. El identificador de la aplicación está disponible en la misma sección Administrar, en la página **Configuración de la aplicación**.

1. Con un símbolo del sistema en el mismo directorio que donde creó el archivo, escriba el siguiente comando para compilar el archivo de Go:

    ```console
    go build model.go
    ```

1. En el símbolo del sistema, escriba el siguiente texto para ejecutar la aplicación de Go desde la línea de comandos:

    ```console
    go run model.go
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para una aplicación](../luis-concept-best-practices.md)