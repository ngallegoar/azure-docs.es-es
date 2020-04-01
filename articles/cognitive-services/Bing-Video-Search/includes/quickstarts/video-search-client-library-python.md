---
title: Inicio rápido de la biblioteca cliente para Python de Bing Video Search
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.openlocfilehash: 7a9fab8ba8bb9d21c9284cbf14bc67226d2ef9d3
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80289863"
---
Use este artículo de inicio rápido para empezar a buscar vídeos con la biblioteca cliente de Bing Video Search para Python. Aunque Bing Video Search tiene una API REST compatible con la mayoría de los lenguajes de programación, la biblioteca cliente proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) con anotaciones adicionales y características.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>Prerrequisitos

- [Python](https://www.python.org/) 2.x o 3.x
- La biblioteca cliente de Bing Video Search para Python

Se recomienda utilizar un [entorno virtual](https://docs.python.org/3/tutorial/venv.html) de Python. Puede instalar e inicializar un entorno virtual con el [módulo venv](https://pypi.python.org/pypi/virtualenv). Instale virtualenv para Python 2.7 con:

```console
python -m venv mytestenv
```

Instale la biblioteca cliente de Bing Video Search mediante:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Python en el IDE o editor que prefiera y agregue las siguientes instrucciones de importación. 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchClient
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Cree una variable para la clave de suscripción. 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    ```

## <a name="create-the-search-client"></a>Creación del cliente de búsqueda

Cree una instancia de la `CognitiveServicesCredentials` y una instancia del cliente:

```python
client = VideoSearchAPI(endpoint, CognitiveServicesCredentials(subscription_key))
```

## <a name="send-a-search-request-and-get-a-response"></a>Envío de una solicitud de búsqueda y obtención de una respuesta

1. Use `client.videos.search()` con la consulta de búsqueda para enviar una solicitud a Bing Video Search API y obtener una respuesta.

    ```python
    video_result = client.videos.search(query="SwiftKey")
    ```

2. Si la respuesta contiene resultados de búsqueda, obtenga el primero de ellos e imprima su identificador, nombre y dirección URL.

    ```python
    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Consulte también 

- [¿Qué es Bing Video Search API?](../../overview.md)
- [Ejemplos del SDK de Cognitive Services para .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
