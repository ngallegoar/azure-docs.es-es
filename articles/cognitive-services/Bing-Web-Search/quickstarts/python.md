---
title: 'Inicio rápido: Realizar una búsqueda con Python - Bing Web Search API'
titleSuffix: Azure Cognitive Services
description: Use esta guía de inicio rápido para enviar solicitudes a la API de REST Bing Web Search mediante Python y reciba una respuesta JSON.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: c1af1142faca76cc58b6b3ca9a7106bc0433ea18
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74976373"
---
# <a name="quickstart-use-python-to-call-the-bing-web-search-api"></a>Inicio rápido: Uso de Python para llamar a Bing Web Search API  

Use este inicio rápido para realizar la primera llamada a Bing Web Search API y recibir una respuesta JSON. Esta aplicación de Python envía una solicitud de búsqueda a la API y muestra la respuesta. Si bien esta aplicación está escrita en Python, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

En este ejemplo se ejecuta como una instancia de Jupyter Notebook en [MyBinder](https://mybinder.org). Seleccione el distintivo launch Binder:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingWebSearchAPI.ipynb)

## <a name="prerequisites"></a>Prerequisites

* [Python 2.x o 3.x](https://www.python.org/)

[!INCLUDE [bing-web-search-quickstart-signup](../../../../includes/bing-web-search-quickstart-signup.md)]

## <a name="define-variables"></a>Definición de variables

Reemplace el valor `subscription_key` por una clave de suscripción válida de su cuenta de Azure.

```python
subscription_key = "YOUR_ACCESS_KEY"
assert subscription_key
```

Declare el punto de conexión de Bing Web Search API. Puede usar el punto de conexión global siguiente o el punto de conexión del [subdominio personalizado](../../../cognitive-services/cognitive-services-custom-subdomains.md) que se muestra en Azure Portal para el recurso.

```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/search"
```

No dude en personalizar la consulta de búsqueda reemplazando el valor de `search_term`.

```python
search_term = "Azure Cognitive Services"
```

## <a name="make-a-request"></a>Realización de una solicitud

Este bloque usa la biblioteca `requests` para llamar a Bing Web Search API y devolver los resultados como objeto JSON. La clave de API se pasa en el diccionario `headers` y el término de búsqueda y los parámetros de consulta se pasan en el diccionario `params`. Consulte la documentación de [Bing Web Search API v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference) para obtener una lista completa de opciones y parámetros.

```python
import requests

headers = {"Ocp-Apim-Subscription-Key": subscription_key}
params = {"q": search_term, "textDecorations": True, "textFormat": "HTML"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

## <a name="format-and-display-the-response"></a>Formato y presentación de la respuesta

El objeto `search_results` incluye los resultados de la búsqueda, junto con metadatos como consultas y páginas relacionadas. Este código utiliza la biblioteca `IPython.display` para dar formato y mostrar la respuesta en el explorador.

```python
from IPython.display import HTML

rows = "\n".join(["""<tr>
                       <td><a href=\"{0}\">{1}</a></td>
                       <td>{2}</td>
                     </tr>""".format(v["url"], v["name"], v["snippet"])
                  for v in search_results["webPages"]["value"]])
HTML("<table>{0}</table>".format(rows))
```

## <a name="sample-code-on-github"></a>Código de ejemplo en GitHub

Si desea ejecutar este código localmente, el [ejemplo completo está disponible en GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingWebSearchv7.py).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de aplicación de una sola página de Bing Web Search](../tutorial-bing-web-search-single-page-app.md)

[!INCLUDE [bing-web-search-quickstart-see-also](../../../../includes/bing-web-search-quickstart-see-also.md)]
