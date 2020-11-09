---
title: 'Inicio rápido: Obtención de conclusiones de imágenes mediante la API REST y Python: Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Aprenda a cargar una imagen con Bing Visual Search API y Python y, a continuación, obtener conclusiones sobre esta.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 05/22/2020
ms.author: scottwhi
ms.custom: devx-track-python
ms.openlocfilehash: 712f86eeaa49c1afe281ad5ede7a6cf2cc0ada4b
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93074987"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-python"></a>Inicio rápido: Obtención de conclusiones de imágenes mediante la API REST Bing Visual Search y Python

> [!WARNING]
> Las Bing Search API se mueven de Cognitive Services a Bing Search Services. A partir del  **30 de octubre de 2020** , las nuevas instancias de Bing Search deben aprovisionarse siguiendo el proceso documentado [aquí](https://aka.ms/cogsvcs/bingmove).
> El aprovisionamiento de las Bing Search API mediante Cognitive Services será posible durante los próximos tres años o hasta que finalice el Contrato Enterprise, lo que antes suceda.
> Puede encontrar instrucciones sobre la migración en [Bing Search Services](https://aka.ms/cogsvcs/bingmigration).

Use este inicio rápido para realizar la primera llamada a la API Bing Visual Search. Esta aplicación de Python carga una imagen en la API y muestra la información que se devuelve. Aunque esta aplicación está escrita en Python, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

## <a name="prerequisites"></a>Prerrequisitos

* [Python 3.x](https://www.python.org/)

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>Inicialización de la aplicación

1. Cree un archivo de Python en el IDE o editor que prefiera y agregue las siguientes instrucciones `import`:

    ```python
    import requests, json
    ```

2. Cree variables para la clave de suscripción, el punto de conexión y la ruta de acceso a la imagen que está cargando. Para el valor de `BASE_URI` puede usar el punto de conexión global en el código siguiente, o el punto de conexión del [subdominio personalizado](../../../cognitive-services/cognitive-services-custom-subdomains.md) que se muestra en Azure Portal para el recurso.

    ```python

    BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'
    SUBSCRIPTION_KEY = 'your-subscription-key'
    imagePath = 'your-image-path'
    ```
    
3. Al cargar una imagen local, los datos del formulario deben incluir el encabezado `Content-Disposition`. Establezca su parámetro `name` en "image", y el parámetro `filename` en el nombre de archivo de la imagen. El contenido del formulario incluye los datos binarios de la imagen. El tamaño de imagen máximo que puede cargar es de 1 MB.
    
    ```
    --boundary_1234-abcd
    Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
    
    ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...
    
    --boundary_1234-abcd--
    ```

4. Cree un objeto de diccionario para almacenar la información de encabezado de la solicitud. Enlace la clave de suscripción a la cadena `Ocp-Apim-Subscription-Key`.

    ```python
    HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}
    ```

5. Cree otro diccionario para contener la imagen, que se abrirá y cargará cuando envíe la solicitud.

    ```python
    file = {'image' : ('myfile', open(imagePath, 'rb'))}
    ```

## <a name="parse-the-json-response"></a>Procese la respuesta JSON.

Cree un método denominado `print_json()` para aceptar la respuesta de la API, e imprima el código JSON.

```python
def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))
```

## <a name="send-the-request"></a>Envío de la solicitud

Use `requests.post()` para enviar una solicitud a Bing Visual Search API. Incluya la cadena para la información de archivo, encabezado y punto de conexión. Imprima `response.json()` con `print_json()`.

```python
try:
    response = requests.post(BASE_URI, headers=HEADERS, files=file)
    response.raise_for_status()
    print_json(response.json())
    
except Exception as ex:
    raise ex
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de página única de Visual Search](../tutorial-bing-visual-search-single-page-app.md)
