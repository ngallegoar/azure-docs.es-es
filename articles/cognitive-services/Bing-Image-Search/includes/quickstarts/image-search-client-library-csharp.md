---
title: Inicio rápido de la biblioteca cliente para C# de Bing Image Search
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/04/2020
ms.author: aahi
ms.openlocfilehash: 9c3bae9d2ad388409c40a8e8c89bcdd52f536cdb
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "85805625"
---
Use este inicio rápido para buscar su primera imagen mediante la biblioteca cliente de Bing Image Search, un contenedor de la API y que contiene las mismas características. Esta sencilla aplicación de C# envía una consulta de búsqueda de imagen, analiza la respuesta de JSON y muestra la dirección URL de la primera imagen devuelta.

El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) con anotaciones y control de errores adicionales.

## <a name="prerequisites"></a>Requisitos previos
* Cualquier edición de [Visual Studio 2017 o versiones posteriores](https://visualstudio.microsoft.com/vs/whatsnew/).
* El [paquete NuGet de búsqueda de imagen cognitiva](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/).

Para instalar la biblioteca cliente de Bing Image Search en Visual Studio, use la opción **Administrar paquetes NuGet** del **Explorador de soluciones**.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](~/includes/cognitive-services-bing-image-search-signup-requirements.md)]

Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

En primer lugar, cree una aplicación de consola en C# mediante Visual Studio. Agregue los siguientes paquetes al proyecto.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
```

En el método principal del proyecto, cree variables para la clave de suscripción válida, los resultados de imágenes que Bing devolverá y un término de búsqueda. Luego, use la clave para crear instancias del cliente de búsqueda de imagen.

```csharp
//IMPORTANT: replace this variable with your Cognitive Services subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";

//initialize the client
//NOTE: If you're using version 1.2.0 or below for the Bing Image Search client library, 
// use ImageSearchAPI() instead of ImageSearchClient() to initialize your search client.

var client = new ImageSearchClient(new ApiKeyServiceClientCredentials(subscriptionKey));
```

## <a name="send-a-search-query-using-the-client"></a>Envío de una consulta de búsqueda con el cliente

Utilice al cliente para buscar con un texto de consulta:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Análisis y visualización del primer resultado de la imagen

Analice los resultados de la imagen devueltos en la respuesta.
Si la respuesta contiene resultados de la búsqueda, almacene el primer resultado e imprima sus detalles, como la dirección URL de una miniatura, la dirección URL original, además del número total de imágenes devueltas.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Image Search](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Consulte también

* [¿Qué es Bing Image Search?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Prueba de una demostración interactiva en línea](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ejemplos de .NET para el SDK de Azure Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Documentación de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Referencia de Bing Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
