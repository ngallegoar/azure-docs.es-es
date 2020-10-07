---
title: Inicio rápido de la biblioteca cliente para JavaScript de Bing Video Search
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.custom: devx-track-js
ms.openlocfilehash: fa1ecb24649d9355e6104bff8add5430fdd64c3c
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91377385"
---
Use este artículo de inicio rápido para empezar a buscar vídeos con la biblioteca cliente de Bing Video Search para JavaScript. Aunque Bing Video Search tiene una API REST compatible con la mayoría de los lenguajes de programación, la biblioteca cliente proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/videoSearch.js). Contiene más anotaciones y características.

## <a name="prerequisites"></a>Prerrequisitos

* La última versión de [Node.js](https://nodejs.org/en/download/).
* El [SDK de Bing Video Search para JavaScript](https://www.npmjs.com/package/@azure/cognitiveservices-videosearch).
     *  Para instalarlo, ejecute `npm install @azure/cognitiveservices-videosearch`.
* La clase `CognitiveServicesCredentials` del paquete `@azure/ms-rest-azure-js` para autenticar el cliente.
     * Para instalarlo, ejecute `npm install @azure/ms-rest-azure-js`.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo JavaScript en el editor o el IDE que prefiera y añada una instrucción `require()` a la biblioteca cliente de Bing Video Search y al módulo `CognitiveServicesCredentials`. Cree una variable para la clave de suscripción. 
    
    ```javascript
    const CognitiveServicesCredentials = require('@azure/ms-rest-azure-js').CognitiveServicesCredentials;
    const VideoSearchAPIClient = require('@azure/cognitiveservices-videosearch');
    ```

2. Cree una instancia de `CognitiveServicesCredentials` con su clave. A continuación, utilícela para crear una instancia del cliente de búsqueda de vídeo.

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let client = new VideoSearchAPIClient(credentials);
    ```

## <a name="send-the-search-request"></a>Envío de la solicitud de búsqueda

1. Use `client.videosOperations.search()` para enviar una solicitud de búsqueda a Bing Video Search API. Cuando se devuelvan los resultados de búsqueda, utilice `.then()` para registrar el resultado.
    
    ```javascript
    client.videosOperations.search('Interstellar Trailer').then((result) => {
        console.log(result.value);
    }).catch((err) => {
        throw err;
    });
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Consulte también 

* [¿Qué es Bing Video Search API?](../../overview.md)
* [Ejemplos del SDK de Cognitive Services para .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)