---
title: Inicio rápido de la biblioteca cliente para Python de Bing Web Search
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/05/2020
ms.author: aahi
ms.openlocfilehash: b6ab34019207ccbeec5448b848e501e063707d16
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81384965"
---
La biblioteca cliente de Bing Web Search facilita la integración de Bing Web Search en la aplicación de Python. En esta guía de inicio rápido, aprenderá a enviar una solicitud, recibir una respuesta JSON, y filtrar y analizar los resultados.

¿Desea ver el código ahora mismo? En GitHub se encuentran disponibles ejemplos de [bibliotecas cliente de Bing Search para Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/tree/master/samples/search).


## <a name="prerequisites"></a>Prerequisites
El SDK de Bing Web Search es compatible con Python 2.7, 3.3, 3.4, 3.5 y 3.6. Se recomienda usar un entorno virtual para esta guía de inicio rápido.

* Python 2.7, 3.3, 3.4, 3.5 o 3.6
* [virtualenv](https://docs.python.org/3/tutorial/venv.html) para Python 2.7
* [venv](https://pypi.python.org/pypi/virtualenv) para Python 3.x

[!INCLUDE [bing-web-search-quickstart-signup](~/includes/bing-web-search-quickstart-signup.md)]

## <a name="create-and-configure-your-virtual-environment"></a>Creación y configuración del entorno virtual

Las instrucciones para instalar y configurar un entorno virtual son diferentes para Python 2.x y Python 3.x. Siga estos pasos para crear e inicializar el entorno virtual.

### <a name="python-2x"></a>Python 2.x

Cree un entorno virtual con `virtualenv` para Python 2.7:

```console
virtualenv mytestenv
```

Active su entorno:

```console
cd mytestenv
source bin/activate
```

Instale las dependencias del SDK de Bing Web Search:

```console
python -m pip install azure-cognitiveservices-search-websearch
```

### <a name="python-3x"></a>Python 3.x

Cree un entorno virtual con `venv` para Python 3.x:

```console
python -m venv mytestenv
```

Active su entorno:

```console
mytestenv\Scripts\activate.bat
```

Instale las dependencias del SDK de Bing Web Search:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-websearch
```

## <a name="create-a-client-and-print-your-first-results"></a>Creación de un cliente e impresión de los primeros resultados

Ahora que ha configurado el entorno virtual e instalado las dependencias, vamos a crear un cliente. El cliente va a controlar solicitudes y respuestas de Bing Web Search API.

Si la respuesta contiene páginas web, imágenes, noticias o vídeos, se imprime el primer resultado para cada uno.

1. Cree un nuevo proyecto de Python con su IDE o editor favorito.

1. Copie este código de ejemplo en el proyecto. `endpoint` puede ser el punto de conexión global siguiente o el punto de conexión del [subdominio personalizado](~/articles/cognitive-services/cognitive-services-custom-subdomains.md) que se muestra en Azure Portal para el recurso:  

    ```python
    # Import required modules.
    from azure.cognitiveservices.search.websearch import WebSearchClient
    from azure.cognitiveservices.search.websearch.models import SafeSearch
    from msrest.authentication import CognitiveServicesCredentials

    # Replace with your subscription key.
    subscription_key = "YOUR_SUBSCRIPTION_KEY"

    # Instantiate the client and replace with your endpoint.
    client = WebSearchClient(endpoint="YOUR_ENDPOINT", credentials=CognitiveServicesCredentials(subscription_key))

    # Make a request. Replace Yosemite if you'd like.
    web_data = client.web.search(query="Yosemite")
    print("\r\nSearched for Query# \" Yosemite \"")

    '''
    Web pages
    If the search response contains web pages, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.web_pages, 'value'):

        print("\r\nWebpage Results#{}".format(len(web_data.web_pages.value)))

        first_web_page = web_data.web_pages.value[0]
        print("First web page name: {} ".format(first_web_page.name))
        print("First web page URL: {} ".format(first_web_page.url))

    else:
        print("Didn't find any web pages...")

    '''
    Images
    If the search response contains images, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.images, 'value'):

        print("\r\nImage Results#{}".format(len(web_data.images.value)))

        first_image = web_data.images.value[0]
        print("First Image name: {} ".format(first_image.name))
        print("First Image URL: {} ".format(first_image.url))

    else:
        print("Didn't find any images...")

    '''
    News
    If the search response contains news, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.news, 'value'):

        print("\r\nNews Results#{}".format(len(web_data.news.value)))

        first_news = web_data.news.value[0]
        print("First News name: {} ".format(first_news.name))
        print("First News URL: {} ".format(first_news.url))

    else:
        print("Didn't find any news...")

    '''
    If the search response contains videos, the first result's name and url
    are printed.
    '''
    if hasattr(web_data.videos, 'value'):

        print("\r\nVideos Results#{}".format(len(web_data.videos.value)))

        first_video = web_data.videos.value[0]
        print("First Videos name: {} ".format(first_video.name))
        print("First Videos URL: {} ".format(first_video.url))

    else:
        print("Didn't find any videos...")
    ```

1. Reemplace `SUBSCRIPTION_KEY` por una clave de suscripción válida.

1. Reemplace `YOUR_ENDPOINT` por la dirección URL del punto de conexión en el portal.

1. Ejecute el programa. Por ejemplo: `python your_program.py`.

## <a name="define-functions-and-filter-results"></a>Definición de funciones y filtro de resultados

Ahora que ha realizado su primera llamada a Bing Web Search API, revise algunas funciones. En las siguientes secciones se resalta la funcionalidad de SDK para perfeccionar las consultas y filtrar los resultados. Cada función se puede agregar al programa de Python que creó en la sección anterior.

### <a name="limit-the-number-of-results-returned-by-bing"></a>Limitación del número de resultados devueltos por Bing

Este ejemplo utiliza los parámetros `count` y `offset` para limitar el número de resultados devueltos mediante el [`search`método](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python) del SDK. `name` y `url` se imprimen para el primer resultado.

1. Agregue este código a su proyecto de Python:

   ```python
    # Declare the function.
    def web_results_with_count_and_offset(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, offset, and count using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="Best restaurants in Seattle", offset=10, count=20)
            print("\r\nSearching for \"Best restaurants in Seattle\"")

            if web_data.web_pages.value:
                '''
                If web pages are available, print the # of responses, and the first and second
                web pages returned.
                '''
                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't find any web pages...")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Ejecute el programa.

### <a name="filter-for-news-and-freshness"></a>Filtro de noticias y actualización

Este ejemplo utiliza los parámetros `response_filter` y `freshness` para filtrar los resultados de búsqueda mediante el [`search`método](/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations) del SDK. Los resultados de búsqueda devueltos se limitan a artículos de noticias y páginas que Bing ha detectado en las últimas 24 horas. `name` y `url` se imprimen para el primer resultado.

1. Agregue este código a su proyecto de Python:

    ```python
    # Declare the function.
    def web_search_with_response_filter(subscription_key):
        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))
        try:
            '''
            Set the query, response_filter, and freshness using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(query="xbox",
                response_filter=["News"],
                freshness="Day")
            print("\r\nSearching for \"xbox\" with the response filter set to \"News\" and freshness filter set to \"Day\".")

            '''
            If news articles are available, print the # of responses, and the first and second
            articles returned.
            '''
            if web_data.news.value:

                print("# of news results: {}".format(len(web_data.news.value)))

                first_web_page = web_data.news.value[0]
                print("First article name: {} ".format(first_web_page.name))
                print("First article URL: {} ".format(first_web_page.url))

                print("")

                second_web_page = web_data.news.value[1]
                print("\nSecond article name: {} ".format(second_web_page.name))
                print("Second article URL: {} ".format(second_web_page.url))

            else:
                print("Didn't find any news articles...")

        except Exception as err:
            print("Encountered exception. {}".format(err))

    # Call the function.
    web_search_with_response_filter(subscription_key)
    ```

1. Ejecute el programa.

### <a name="use-safe-search-answer-count-and-the-promote-filter"></a>Uso de búsqueda segura, recuento de respuestas y filtro de promoción

Este ejemplo utiliza los parámetros `answer_count`, `promote` y `safe_search`para filtrar los resultados de búsqueda mediante el [`search`método](https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python) del SDK. `name` y `url` se muestran para el primer resultado.

1. Agregue este código a su proyecto de Python:

    ```python
    # Declare the function.
    def web_search_with_answer_count_promote_and_safe_search(subscription_key):

        client = WebSearchAPI(CognitiveServicesCredentials(subscription_key))

        try:
            '''
            Set the query, answer_count, promote, and safe_search parameters using the SDK's search method. See:
            https://docs.microsoft.com/python/api/azure-cognitiveservices-search-websearch/azure.cognitiveservices.search.websearch.operations.weboperations?view=azure-python.
            '''
            web_data = client.web.search(
                query="Niagara Falls",
                answer_count=2,
                promote=["videos"],
                safe_search=SafeSearch.strict  # or directly "Strict"
            )
            print("\r\nSearching for \"Niagara Falls\"")

            '''
            If results are available, print the # of responses, and the first result returned.
            '''
            if web_data.web_pages.value:

                print("Webpage Results#{}".format(len(web_data.web_pages.value)))

                first_web_page = web_data.web_pages.value[0]
                print("First web page name: {} ".format(first_web_page.name))
                print("First web page URL: {} ".format(first_web_page.url))

            else:
                print("Didn't see any Web data..")

        except Exception as err:
            print("Encountered exception. {}".format(err))
    ```

1. Ejecute el programa.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este proyecto, asegúrese de quitar la clave de suscripción desde el código del programa y desactivar el entorno virtual.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ejemplos del SDK de Cognitive Services para Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)

## <a name="see-also"></a>Consulte también

* [Referencia del SDK de Azure Python](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/websearch)
