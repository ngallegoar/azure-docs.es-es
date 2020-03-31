---
title: 'Inicio rápido: Generación de una miniatura (REST, Go)'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, generará una miniatura de una imagen mediante Computer Vision API con Go.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: deb7ef0d087bc33c9e59ecd95973bc55a5ab6521
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74961529"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-with-go"></a>Inicio rápido: Generación de una miniatura mediante la API REST Computer Vision con Go

En esta guía de inicio rápido, generará una miniatura de una imagen mediante la API REST de Computer Vision. Debe especificar el alto y el ancho, que pueden ser diferentes a la relación de aspecto de la imagen de entrada. Computer Vision usa el recorte inteligente para identificar el área de interés de forma inteligente y generar coordenadas de recorte en función de esa región.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

- Debe tener [Go](https://golang.org/dl/) instalado.
- Debe tener una clave de suscripción para Computer Vision. Puede obtener una clave de evaluación gratuita en la página [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). O bien, siga las instrucciones que se indican en [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse a Computer Vision y obtener su clave. Después, [cree variables de entorno](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) para la cadena de punto de conexión del servicio y la clave denominadas `COMPUTER_VISION_SUBSCRIPTION_KEY` y `COMPUTER_VISION_ENDPOINT`, respectivamente.

## <a name="create-and-run-the-sample"></a>Creación y ejecución del ejemplo

Para crear y ejecutar el ejemplo, siga estos pasos:

1. Copie el código siguiente en un editor de texto.
1. También puede reemplazar el valor de `imageUrl` por la dirección URL de una imagen diferente de la que desea generar una miniatura.
1. Guarde el código como un archivo con una extensión `.go`. Por ejemplo, `get-thumbnail.go`.
1. Abra una ventana de símbolo del sistema.
1. En el símbolo del sistema, ejecute el comando `go build` para compilar el paquete desde el archivo. Por ejemplo, `go build get-thumbnail.go`.
1. En el símbolo del sistema, ejecute el paquete compilado. Por ejemplo, `get-thumbnail`.

```go
package main

import (
    "encoding/json"
    "fmt"
    "io/ioutil"
    "net/http"
    "strings"
    "time"
)

func main() {
    // Add your Computer Vision subscription key and endpoint to your environment variables.
    subscriptionKey := os.Getenv("COMPUTER_VISION_SUBSCRIPTION_KEY")
    if (subscriptionKey == "") {
        log.Fatal("\n\nSet the COMPUTER_VISION_SUBSCRIPTION_KEY environment variable.\n" +
            "**Restart your shell or IDE for changes to take effect.**\n")

    endpoint := os.Getenv("COMPUTER_VISION_ENDPOINT")
    if ("" == endpoint) {
        log.Fatal("\n\nSet the COMPUTER_VISION_ENDPOINT environment variable.\n" +
            "**Restart your shell or IDE for changes to take effect.**")
    }
    const uriBase = endpoint + "vision/v2.1/generateThumbnail"
    const imageUrl =
        "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

    const params = "?width=100&height=100&smartCropping=true"
    const uri = uriBase + params
    const imageUrlEnc = "{\"url\":\"" + imageUrl + "\"}"

    reader := strings.NewReader(imageUrlEnc)

    // Create the HTTP client
    client := &http.Client{
        Timeout: time.Second * 2,
    }

    // Create the POST request, passing the image URL in the request body
    req, err := http.NewRequest("POST", uri, reader)
    if err != nil {
        panic(err)
    }

    // Add headers
    req.Header.Add("Content-Type", "application/json")
    req.Header.Add("Ocp-Apim-Subscription-Key", subscriptionKey)

    // Send the request and retrieve the response
    resp, err := client.Do(req)
    if err != nil {
        panic(err)
    }

    defer resp.Body.Close()

    // Read the response body.
    // Note, data is a byte array
    data, err := ioutil.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }

    // Parse the JSON data
    var f interface{}
    json.Unmarshal(data, &f)

    // Format and display the JSON result
    jsonFormatted, _ := json.MarshalIndent(f, "", "  ")
    fmt.Println(string(jsonFormatted))
}
```

## <a name="examine-the-response"></a>Examen de la respuesta

Una respuesta correcta contiene los datos binarios de la imagen en miniatura. Si se produce un error en la solicitud, la respuesta contiene un código de error y un mensaje para ayudar a determinar qué salió mal.

## <a name="next-steps"></a>Pasos siguientes

Explore Computer Vision API para analizar una imagen, detectar celebridades y sitios emblemáticos, crear una miniatura y extraer texto impreso y manuscrito. Para experimentar rápidamente con la versión de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Explore Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
