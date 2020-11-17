---
title: 'API REST de Azure App Configuration: control de versiones'
description: Páginas de referencia para el control de versiones mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 90d131cdc7c496853f2520951c95b9903d69f8fb
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423814"
---
# <a name="versioning"></a>Control de versiones

api-version: 1.0

Cada solicitud de cliente debe proporcionar la versión de API explícita como parámetro de cadena de la consulta. Por ejemplo: `https://{myconfig}.azconfig.io/kv?api-version=1.0`

`api-version` se expresa con el formato SemVer (principal.secundaria). No se admite la negociación del rango ni de la versión.

## <a name="error-response"></a>Respuesta de error

A continuación se incluye un resumen de las posibles respuestas de error que devuelve el servidor cuando no se puede encontrar una coincidencia con la versión de API solicitada.

### <a name="api-version-unspecified"></a>API Version is not specified (No se ha especificado la versión de la API)

Aparece cuando un cliente realiza una solicitud sin proporcionar una versión de la API.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "API version is not specified",
  "name": "api-version",
  "detail": "An API version is required, but was not specified.",
  "status": 400
}
```

### <a name="unsupported-api-version"></a>Unsupported API Version (Versión de API no admitida)

Aparece cuando una versión de API solicitada por el cliente no coincide con ninguna de las versiones de API admitidas por el servidor.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Unsupported API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

### <a name="invalid-api-version"></a>Invalid API Version (La versión de la API no es válida)

Aparece cuando un cliente realiza una solicitud con una versión de API, pero el valor tiene un formato incorrecto o no se puede analizar en el servidor.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8  
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid API version",
  "name": "api-version",
  "detail": "The HTTP resource that matches the request URI '{request uri}' does not support the API version '{api-version}'.",
  "status": 400
}
```

### <a name="ambiguous-api-version"></a>Ambiguous API Version (Versión de API ambigua)

Aparece cuando un cliente solicita una versión de la API que es ambigua para el servidor; por ejemplo, si incluye varios valores distintos.

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Ambiguous API version",
  "name": "api-version",
  "detail": "The following API versions were requested: {comma separated api versions}. At most, only a single API version may be specified. Please update the intended API version and retry the request.",
  "status": 400
}
```
