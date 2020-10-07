---
title: Inicio rápido de la biblioteca cliente para Python de Bing News Search
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/12/2020
ms.author: aahi
ms.openlocfilehash: c1bd0d86a3fd9d19d67d84b9b05955421373e01e
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "79503513"
---
Este inicio rápido para empezar a buscar noticias con la biblioteca cliente de Bing News Search para Python. Aunque Bing News Search tiene una API REST compatible con la mayoría de los lenguajes de programación, la biblioteca cliente proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/news_search_samples.py).

## <a name="prerequisites"></a>Prerrequisitos

* [Python](https://www.python.org/) 2.x o 3.x

Se recomienda usar un [entorno virtual](https://docs.python.org/3/tutorial/venv.html) para el desarrollo de Python. Puede instalar e inicializar el entorno virtual con el [módulo venv](https://pypi.python.org/pypi/virtualenv). Debe instalar un módulo virtualenv para Python 2.7. Puede crear un entorno virtual con:

```console
python -m venv mytestenv
```

Puede instalar las dependencias de la biblioteca cliente de Bing News Search con este comando:
    
```console
python -m pip install azure-cognitiveservices-search-newssearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un archivo de Python en su IDE o editor favorito e importe las bibliotecas siguientes. Cree una variable para la clave de suscripción y el término de búsqueda.

    ```python
    from azure.cognitiveservices.search.newssearch import NewsSearchClient
    from msrest.authentication import CognitiveServicesCredentials
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    search_term = "Quantum Computing"
    ```

## <a name="initialize-the-client-and-send-a-request"></a>Inicialización del cliente y envío de una solicitud

1. Creación de una instancia de `CognitiveServicesCredentials`.
    
    ```python
    client = NewsSearchClient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

2. Envíe una consulta de búsqueda a News Search API y almacene la respuesta.

    ```python
    news_result = client.news.search(query=search_term, market="en-us", count=10)
    ```

## <a name="parse-the-response"></a>Análisis de la respuesta

Si se encuentran resultados de la búsqueda, imprima el primer resultado de la página web:

```python
if news_result.value:
    first_news_result = news_result.value[0]
    print("Total estimated matches value: {}".format(
        news_result.total_estimated_matches))
    print("News result count: {}".format(len(news_result.value)))
    print("First news name: {}".format(first_news_result.name))
    print("First news url: {}".format(first_news_result.url))
    print("First news description: {}".format(first_news_result.description))
    print("First published time: {}".format(first_news_result.date_published))
    print("First news provider: {}".format(first_news_result.provider[0].name))
else:
    print("Didn't see any news result data..")
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](../../tutorial-bing-news-search-single-page-app.md)
