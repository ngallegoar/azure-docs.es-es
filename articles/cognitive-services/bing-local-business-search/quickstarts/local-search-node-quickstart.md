---
title: 'Inicio rápido: Envío de una consulta a la API mediante Node.js: Bing Local Business Search'
titleSuffix: Azure Cognitive Services
description: Use esta guía de inicio rápido para empezar a enviar solicitudes a Bing Local Business Search API, que es un servicio de Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 05/12/2020
ms.author: aahi
ms.openlocfilehash: 3bb31c36e8c614a72b86f95cb7e7d1c588692f97
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873076"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-using-nodejs"></a>Inicio rápido: Envío de una consulta a Bing Local Business Search API mediante Node.js

Use este inicio rápido para aprender a enviar solicitudes a la API Bing Local Business Search, que es un servicio de Azure Cognitive Services. Aunque esta aplicación sencilla está escrita en Node.js, la API es un servicio web RESTful compatible con cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar JSON.

Esta aplicación de ejemplo obtiene los datos de respuesta local de la API para una consulta de búsqueda.

## <a name="prerequisites"></a>Prerrequisitos

* La última versión de [Node.js](https://nodejs.org/en/download/).
* La [biblioteca de solicitudes de JavaScript](https://github.com/request/request).
* Una [cuenta de API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con las API Bing Search. Para este inicio rápido, la [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) es suficiente. Guarde la clave de API que se proporciona al activar la evaluación gratuita. Para más información, consulte [Precios de Cognitive Services: API Bing Search](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).


## <a name="code-scenario"></a>Escenario de código

El código siguiente define y envía la solicitud, que se implementa en los pasos a continuación:

1. Declare las variables para especificar el punto de conexión por host y ruta de acceso.
2. Especifique la consulta y agregue el parámetro de consulta.
3. Cree una función de controlador para la respuesta.
4. Defina la función de búsqueda que crea la solicitud y agrega el encabezado `Ocp-Apim-Subscription-Key`.
5. Ejecute la función Search.


```javascript
'use strict';

let https = require('https');

// Replace the subscriptionKey string value with your valid subscription key.
let subscriptionKey = 'your-access-key';

let host = 'api.cognitive.microsoft.com/bing';
let path = '/v7.0/localbusinesses/search';

let mkt = 'en-US';
let q = 'hotel in Bellevue';

let params = '?q=' + encodeURI(q) + "&mkt=" + mkt;

let response_handler = function (response) {
    let body = '';
    response.on('data', function (d) {
        body += d;
    });
    response.on('end', function () {
        let body_ = JSON.parse(body);
        let body__ = JSON.stringify(body_, null, '  ');
        console.log(body__);
    });
    response.on('error', function (e) {
        console.log('Error: ' + e.message);
    });
};

let Search = function () {
    let request_params = {
        method: 'GET',
        hostname: host,
        path: path + params,
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey,
        }
    };

    let req = https.request(request_params, response_handler);
    req.end();
}

Search();

```

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido de Local Business Search para C#](local-quickstart.md)
* [Guía de inicio rápido de Local Business Search para Java](local-search-java-quickstart.md)
* [Guía de inicio rápido de Local Business Search para Python](local-search-python-quickstart.md)
