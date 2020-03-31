---
title: 'Inicio rápido: Realizar una búsqueda de noticias mediante Python y la API de REST de Bing News Search'
titleSuffix: Azure Cognitive Services
description: Use esta guía de inicio rápido para enviar una solicitud a la API de REST de Bing News Search mediante Python y reciba una respuesta de JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 12/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 1c424c75a4df193ec412355607c68abeda0560a5
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448497"
---
# <a name="quickstart-perform-a-news-search-using-python-and-the-bing-news-search-rest-api"></a>Inicio rápido: Realizar una búsqueda de noticias mediante Python y la API de REST de Bing News Search

Use este inicio rápido para realizar la primera llamada a Bing News Search API y recibir una respuesta JSON. Esta sencilla aplicación de JavaScript envía una consulta de búsqueda a la API y procesa los resultados. Si bien esta aplicación está escrita en Python, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

Para ejecutar este ejemplo de código como Jupyter Notebook en [MyBinder](https://mybinder.org), puede hacer clic en la notificación de inicio de Binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingNewsSearchAPI.ipynb)

El código fuente del ejemplo está también disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingNewsSearchv7.py).

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. Cree un nuevo archivo de Python en el IDE o editor que prefiera e importe el módulo de solicitud. Cree variables para la clave de suscripción, un punto de conexión y un término de búsqueda. Puede usar el punto de conexión global siguiente o el punto de conexión del [subdominio personalizado](../../cognitive-services/cognitive-services-custom-subdomains.md) que se muestra en Azure Portal para el recurso.

```python
import requests

subscription_key = "your subscription key"
search_term = "Microsoft"
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/news/search"
```

### <a name="create-parameters-for-the-request"></a>Creación de parámetros para la solicitud

1. Agregue la clave de suscripción a un nuevo diccionario con `"Ocp-Apim-Subscription-Key"` como clave. Haga lo mismo para los parámetros de búsqueda.

    ```python
    headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
    params  = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
    ```

## <a name="send-a-request-and-get-a-response"></a>Envío de una solicitud y obtención de una respuesta

1. Utilice la biblioteca de solicitudes para llamar a Bing Visual Search API con su clave de suscripción y los objetos de diccionario que se crearon en el último paso.

    ```python
    response = requests.get(search_url, headers=headers, params=params)
    response.raise_for_status()
    search_results = response.json()
    ```

2. `search_results` contiene la respuesta de la API como un objeto JSON. Acceda a las descripciones de los artículos incluidos en la respuesta.
    
    ```python
    descriptions = [article["description"] for article in search_results["value"]]
    ```

## <a name="displaying-the-results"></a>Mostrar los resultados

A continuación, estas descripciones se pueden representar como una tabla con la palabra clave de búsqueda resaltada en **negrita**.

```python
from IPython.display import HTML
rows = "\n".join(["<tr><td>{0}</td></tr>".format(desc)
                  for desc in descriptions])
HTML("<table>"+rows+"</table>")
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](tutorial-bing-news-search-single-page-app.md)
