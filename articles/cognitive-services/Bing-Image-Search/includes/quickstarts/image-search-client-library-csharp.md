---
title: Inicio rápido de la biblioteca cliente para C# de Bing Image Search
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/21/2020
ms.author: aahi
ms.openlocfilehash: 247aea49d60c2d953d8b6dff37d22188a5442a1f
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625296"
---
Use este inicio rápido para realizar la primera búsqueda de imágenes mediante la biblioteca cliente de Bing Image Search. 

La biblioteca de búsqueda de cliente es un contenedor de la API REST y contiene las mismas características. 

Va a crear una aplicación de C# que envía una consulta de búsqueda de imágenes, analiza la respuesta de JSON y muestra la dirección URL de la primera imagen devuelta.

El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) con anotaciones y control de errores adicionales.

## <a name="prerequisites"></a>Prerrequisitos

* Si usa Windows, cualquier edición de [Visual Studio 2017 o posterior](https://visualstudio.microsoft.com/vs/whatsnew/).
* Si utiliza macOS o Linux, [VS Code](https://code.visualstudio.com) con [.NET Core instalado](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/install).
* [Una suscripción de Azure gratuita](https://azure.microsoft.com/free/dotnet)

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](~/includes/cognitive-services-bing-image-search-signup-requirements.md)]

Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-a-console-project"></a>Creación de un proyecto de consola

En primer lugar, cree una nueva aplicación de consola de C#.

## <a name="create-a-console-project"></a>Creación de un proyecto de consola

# <a name="visual-studio"></a>[Visual Studio](#tab/visualstudio)

1. Cree una solución de consola denominada *BingImageSearch* en Visual Studio.
    
1. Agregue el [paquete NuGet de búsqueda de imagen cognitiva](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch).
    1. En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto.
    1. Seleccione **Administrar paquetes NuGet**.
    1. Busque y seleccione *Microsoft.Azure.CognitiveServices.Search.ImageSearch* y, a continuación, instale el paquete.
    
# <a name="vs-code"></a>[Código de VS](#tab/vscode)

1. Abra la ventana de terminal en VS Code.
1. Cree un nuevo proyecto de consola denominado *BingImageSearch*. Para ello, escriba el código siguiente en la ventana de terminal:
    
    ```bash
    dotnet new console -n BingImageSearch
    ```
1. Abra la carpeta *BingImageSearch* en VS Code.
1. Agregue el [paquete NuGet de búsqueda de imagen cognitiva](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch). Para ello, escriba el código siguiente en la ventana de terminal:

    ```bash
    dotnet add package Microsoft.Azure.CognitiveServices.Search.ImageSearch
    ```

---

## <a name="initialize-the-application"></a>Inicialización de la aplicación


1. Reemplace todas las instrucciones `using` en *Program.cs* por el código siguiente:

    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
    using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
    ```

1. En el método `Main` del proyecto, cree variables para la clave de suscripción válida, los resultados de imágenes que Bing devolverá y un término de búsqueda. Luego, use la clave para crear instancias del cliente de búsqueda de imagen.

    ```csharp
    static async Task Main(string[] args)
    {
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
    }
    ```
    
## <a name="send-a-search-query-using-the-client"></a>Envío de una consulta de búsqueda con el cliente
    
Todavía en el método `Main`, use el cliente para buscar con el texto de una consulta:
    
```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = await client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Análisis y visualización del primer resultado de la imagen

Analice los resultados de la imagen devueltos en la respuesta. 

Si la respuesta contiene resultados de búsqueda, almacene el primer resultado e imprima algunos de sus detalles.

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.WriteLine("Press any key to exit ...");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Image Search](../../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Consulte también

* [¿Qué es Bing Image Search?](../../overview.md)  
* [Prueba de una demostración interactiva en línea](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Ejemplos de .NET para el SDK de Azure Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Documentación de Azure Cognitive Services](../../../index.yml)
* [Referencia de Bing Image Search API](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)