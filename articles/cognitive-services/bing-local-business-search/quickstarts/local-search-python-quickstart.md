---
title: 'Inicio rápido: Envío de consultas a la API en Python: Bing Local Business Search'
titleSuffix: Azure Cognitive Services
description: Use este inicio rápido para empezar a usar Bing Local Business Search API en Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 05/12/2020
ms.author: aahi
ms.openlocfilehash: 3a90d5455c0664ceabf80647fc94a37ad0c716b5
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873024"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Inicio rápido: Envío de una consulta a Bing Local Business Search API en Python

Use este inicio rápido para aprender a enviar solicitudes a la API Bing Local Business Search, que es un servicio de Azure Cognitive Services. Aunque esta aplicación sencilla está escrita en Python, la API es un servicio web RESTful compatible con cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar JSON.

Esta aplicación de ejemplo obtiene los datos de respuesta local de la API para una consulta de búsqueda.

## <a name="prerequisites"></a>Prerrequisitos

* [Python](https://www.python.org/) 2.x o 3.x.
* Una [cuenta de API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con las API Bing Search. Para este inicio rápido, la [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) es suficiente. Guarde la clave de API que se proporciona al activar la evaluación gratuita. Para más información, consulte [Precios de Cognitive Services: API Bing Search](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-complete-application"></a>Ejecución de la aplicación completa

En el ejemplo siguiente se obtienen resultados localizados que se implementan en los pasos siguientes:
1. Declare las variables para especificar el punto de conexión por host y ruta de acceso.
2. Especifique el parámetro de consulta. 
3. Defina la función de búsqueda que crea la solicitud y agrega el encabezado `Ocp-Apim-Subscription-Key`.
4. Establezca el encabezado `Ocp-Apim-Subscription-Key`. 
5. Realice la conexión y envíe la solicitud.
6. Imprima los resultados JSON.

Este es el código completo de esta demostración es el siguiente:

```python
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/localbusinesses/search'

query = 'restaurant in Bellevue'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

```

## <a name="next-steps"></a>Pasos siguientes
- [Guía de inicio rápido de Local Business Search para Java](local-search-java-quickstart.md)
- [Inicio rápido de Local Business Search para C#](local-quickstart.md)
- [Inicio rápido de Local Business Search para Node](local-search-node-quickstart.md)
