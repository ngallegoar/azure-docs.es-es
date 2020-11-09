---
title: Ejemplos del SDK de Bing Web Search
titleSuffix: Azure Cognitive Services
description: Use el SDK de Bing Web Search para agregar funcionalidades de búsqueda a una aplicación de Python, Node.js, C# o Java.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: sample
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: b31335df7c1dc48f82699aa3676561721474071d
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93076221"
---
# <a name="bing-web-search-sdk-samples"></a>Ejemplos del SDK de Bing Web Search

> [!WARNING]
> Las Bing Search API se mueven de Cognitive Services a Bing Search Services. A partir del  **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de las Bing Search API mediante Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que antes suceda.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

El SDK de Bing Web Search está disponible en Python, Node.js, C# y Java. En GitHub se proporcionan ejemplos de código, los requisitos previos y las instrucciones de compilación. Se cubren los siguientes escenarios:

* Se consulta una sola palabra e imprime el nombre y la dirección URL del primer resultado de páginas web, imágenes, artículo de noticias y vídeos.
* Se consulta una frase, comprueba el número de resultados e imprime el nombre y la dirección URL del primer resultado.
* Se consulta un término de búsqueda con filtros de respuesta establecidos en `news` y se imprimen los detalles de los resultados de noticias.
* Se consulta un término de búsqueda con los parámetros `answerCount` y `promote` y, después, se imprimen los detalles de los resultados.

## <a name="sdks-and-libraries"></a>SDK y bibliotecas

Utilice estos vínculos para acceder al SDK del idioma que desee.

* Introducción a [ejemplos de Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)
  * Consulte también [bibliotecas de Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-search-websearch) para las definiciones y dependencias.
* Introducción a [ejemplos de Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
  * Consulte también [Web Search de Node.js](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/cognitiveServicesWebSearch).
* Introducción a [ejemplos de .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
  * [Paquete NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.WebSearch/1.2.0)
  * Consulte también [bibliotecas de .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Search.BingWebSearch) para las definiciones y dependencias.
* Introducción a [ejemplos de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples)
  * Consulte también [bibliotecas de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master/Search/BingWebSearch) para las definiciones y dependencias.
