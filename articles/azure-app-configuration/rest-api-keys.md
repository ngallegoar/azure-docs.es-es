---
title: 'API REST de Azure App Configuration: claves'
description: Páginas de referencia para trabajar con claves mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: f44ecdf571791d54a78d25dde514d57053b59160
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423837"
---
# <a name="keys"></a>Claves

api-version: 1.0

La siguiente sintaxis representa un recurso de clave:

```http
{
    "name": [string]             // Name of the key
}
```

## <a name="operations"></a>Operations

Los recursos clave admiten la operación siguiente:

- List

Para todas las operaciones, `name` es un parámetro de filtro opcional. Si se omite, indica *cualquier* clave.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="list-keys"></a>Enumera las claves

```http
GET /keys?api-version={api-version} HTTP/1.1
```

**Respuestas:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.keyset+json; charset=utf-8"
```

```json
{
    "items": [
        {
          "name": "{key-name}"
        },
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="pagination"></a>Paginación

El resultado se pagina si el número de elementos devueltos supera el límite de respuesta. Siga los encabezados de respuesta `Link` opcionales y use `rel="next"` para la navegación. Como alternativa, el contenido proporciona un vínculo Siguiente en forma de la propiedad `@nextLink`. El siguiente vínculo contiene el parámetro `api-version`.

```http
GET /keys?api-version={api-version} HTTP/1.1
```

**Respuesta:**

```http
HTTP/1.1 OK
Content-Type: application/vnd.microsoft.appconfig.keyset+json; charset=utf-8
Link: <{relative uri}>; rel="next"
```

```json
{
    "items": [
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="filtering"></a>Filtrado

Se admite el filtrado por ```name```.

```http
GET /keys?name={key-name}&api-version={api-version}
```

Se admiten los siguientes filtros:

|Filtro de clave|Efecto|
|--|--|
|se omite `name` o `name=*`|Coincide con **cualquier** clave|
|`name=abc`|Coincide con una clave denominada **abc**|
|`name=abc*`|Coincide con los nombres de clave que empiezan por **abc**|
|`name=abc,xyz`|Coincide con los nombres de clave **abc** o **xyz** (se limita a 5 archivos .CSV)|

No se permiten los caracteres siguientes: `*`, `\` y `,`

Si un carácter reservado forma parte del valor, se debe escapar mediante `\{Reserved Character}`. Los caracteres no reservados también se pueden escapar.

## <a name="filter-validation"></a>Validación del filtro

En el caso de un error de validación del filtro, la respuesta es HTTP `400` con detalles del error:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid request parameter 'name'",
  "name": "name",
  "detail": "name(2): Invalid character",
  "status": 400
}
```

## <a name="examples"></a>Ejemplos

- All

    ```http
    GET /keys?api-version={api-version}
    ```

- El nombre de la clave comienza con **abc**.

    ```http
    GET  /keys?name=abc*&api-version={api-version}
    ```

- El nombre de la clave es **abc** o **xyz**.

    ```http
    GET /keys?name=abc,xyz&api-version={api-version}
    ```

## <a name="request-specific-fields"></a>Solicitud de campos específicos

Use el parámetro de cadena de consulta opcional `$select` y proporcione una lista separada por comas de campos solicitados. Si se omite el parámetro `$select`, la respuesta contiene el conjunto predeterminado.

```http
GET /keys?$select=name&api-version={api-version} HTTP/1.1
```

## <a name="time-based-access"></a>Acceso basado en el tiempo

Obtenga una representación del resultado tal como era en un momento anterior. Consulte la sección [2.1.1](https://tools.ietf.org/html/rfc7089#section-2.1)

```http
GET /keys&api-version={api-version} HTTP/1.1
Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT
```

**Respuesta:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.keyset+json"
Memento-Datetime: Sat, 12 May 2018 02:10:00 GMT
Link: <relative uri>; rel="original"
```

```json
{
    "items": [
        ....
    ]
}
```
