---
title: Inicio rápido de la biblioteca cliente para Python de Bing Custom Search
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/27/2020
ms.author: aahi
ms.openlocfilehash: 92df45e38a926b4a791315f731e1d1cb078d5852
ms.sourcegitcommit: 22da82c32accf97a82919bf50b9901668dc55c97
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2020
ms.locfileid: "94371998"
---
Introducción a la biblioteca de cliente de Bing Custom Search para Python. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas. Bing Custom Search API le permite crear experiencias de búsqueda a medida y sin anuncios para los temas que le interesan. El código fuente de este ejemplo se puede encontrar en [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/custom_search_samples.py).

Use la biblioteca de cliente de Bing Custom Search para Python para:
* Buscar los resultados de la búsqueda en la Web, desde la instancia de Bing Custom Search.

[Documentación de referencia](/python/api/azure-cognitiveservices-search-customsearch/?view=azure-python) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-search-customsearch) | [Paquete (PyPi)](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) | [Ejemplos](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/)


## <a name="prerequisites"></a>Prerrequisitos

- Una instancia de Bing Custom Search. Consulte [Quickstart: Creación de la primera instancia de Bing Custom Search](../../quick-start.md) para obtener más información.
- Python [2.x o 3.x](https://www.python.org/) 
- [SDK de Bing Custom Search para Python](https://pypi.org/project/azure-cognitiveservices-search-customsearch/) 

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](~/includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="install-the-python-client-library"></a>Instalación de la biblioteca cliente de Python

Instale la biblioteca cliente de Bing Custom Search con el siguiente comando.

```Console
python -m pip install azure-cognitiveservices-search-customsearch
```


## <a name="create-a-new-application"></a>Creación de una aplicación

Cree un archivo de Python en el IDE o editor que prefiera y agregue las siguientes importaciones.

```python
from azure.cognitiveservices.search.customsearch import CustomSearchClient
from msrest.authentication import CognitiveServicesCredentials
```

## <a name="create-a-search-client-and-send-a-request"></a>Creación de un cliente de búsqueda y envío de una solicitud

1. Cree una variable para la clave y el punto de conexión de la suscripción.

    ```python
    subscription_key = 'your-subscription-key'
    endpoint = 'your-endpoint'
    ```

2. Cree una instancia de `CustomSearchClient` con un objeto `CognitiveServicesCredentials` mediante la clave de suscripción. 

    ```python
    client = CustomSearchClient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

3. Envíe una solicitud de búsqueda con `client.custom_instance.search()`. Anexe el término de búsqueda al parámetro `query` y establezca `custom_config` en el identificador de configuración personalizada para usar la instancia de búsqueda. Puede obtener el identificador en el [portal de Bing Custom Search](https://www.customsearch.ai/), donde debe hacer clic en la pestaña **Producción**.

    ```python
    web_data = client.custom_instance.search(query="xbox", custom_config="your-configuration-id")
    ```

## <a name="view-the-search-results"></a>Visualización de los resultados de la búsqueda

Si se encuentran algunos resultados de la búsqueda de páginas web, obtenga el primero e imprima su nombre, la dirección URL y las páginas web totales encontradas.

```python
if web_data.web_pages.value:
    first_web_result = web_data.web_pages.value[0]
    print("Web Pages result count: {}".format(len(web_data.web_pages.value)))
    print("First Web Page name: {}".format(first_web_result.name))
    print("First Web Page url: {}".format(first_web_result.url))
else:
    print("Didn't see any web data..")
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una página web de Custom Search](../../tutorials/custom-search-web-page.md)