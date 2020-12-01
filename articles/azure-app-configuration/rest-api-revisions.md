---
title: 'API REST de Azure App Configuration: revisiones de clave y valor'
description: Páginas de referencia para trabajar con revisiones de clave y valor mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 668345da8bb89412f7b1dd36975c5bed6f229580
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2020
ms.locfileid: "95246391"
---
# <a name="key-value-revisions"></a>Revisiones de clave y valor

Una *revisión de clave y valor* define la representación histórica de un recurso de clave y valor. Las revisiones expiran después de 7 días en el caso de los almacenes de nivel Gratis, o 30 días en el de los almacenes de nivel Estándar. Las revisiones admiten la operación `List`.

En todas las operaciones, ``key`` es un parámetro opcional. Si se omite, implica cualquier clave.

En todas las operaciones, ``label`` es un parámetro opcional. Si se omite, implica cualquier etiqueta.

Este artículo se aplica a la API versión 1.0.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="list-revisions"></a>Enumeración de revisiones

```http
GET /revisions?label=*&api-version={api-version} HTTP/1.1
```

**Respuestas:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kvset+json; charset=utf-8"
Accept-Ranges: items
```

```json
{
    "items": [
        {
          "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
          "key": "{key}",
          "label": "{label}",
          "content_type": null,
          "value": "example value",
          "last_modified": "2017-12-05T02:41:26.4874615+00:00",
          "tags": []
        },
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="pagination"></a>Paginación

El resultado se pagina si el número de elementos devueltos supera el límite de respuesta. Siga el encabezado de respuesta ``Link`` opcional y use ``rel="next"`` para la navegación. Como alternativa, el contenido proporciona un vínculo siguiente en forma de la propiedad ``@nextLink``.

```http
GET /revisions?api-version={api-version} HTTP/1.1
```

**Respuesta:**

```http
HTTP/1.1 OK
Content-Type: application/vnd.microsoft.appconfig.kvs+json; charset=utf-8
Accept-Ranges: items
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

## <a name="list-subset-of-revisions"></a>Lista de subconjuntos de revisiones

Use el encabezado de solicitud `Range`. La respuesta contiene un encabezado `Content-Range`. Si el servidor no puede satisfacer el intervalo solicitado, responde con HTTP `416` (`RangeNotSatisfiable`).

```http
GET /revisions?api-version={api-version} HTTP/1.1
Range: items=0-2
```

**Respuesta**

```http
HTTP/1.1 206 Partial Content
Content-Type: application/vnd.microsoft.appconfig.revs+json; charset=utf-8
Content-Range: items 0-2/80
```

## <a name="filtering"></a>Filtrado

Se admite una combinación de filtrado de `key` y `label`.
Use los parámetros de cadena de consulta opcionales `key` y `label`.

```http
GET /revisions?key={key}&label={label}&api-version={api-version}
```

### <a name="supported-filters"></a>Filtros admitidos

|Filtro de clave|Efecto|
|--|--|
|se omite `key` o `key=*`|Coincide con **cualquier** clave|
|`key=abc`|Coincide con una clave denominada **abc**|
|`key=abc*`|Coincide con los nombres de claves que empiezan por **abc**|
|`key=*abc`|Coincide con los nombres de claves que terminan por **abc**|
|`key=*abc*`|Coincide con los nombres de claves que contienen **abc**|
|`key=abc,xyz`|Coincide con los nombres de claves **abc** o **xyz** (limitado a 5 archivos CSV)|

|Filtro de etiqueta|Efecto|
|--|--|
|se omite `label` o `label=`|Coincide con una entrada sin etiqueta|
|`label=*`|Coincide con **cualquier** etiqueta|
|`label=prod`|Coincide con la etiqueta **prod**|
|`label=prod*`|Coincide con las etiquetas que empiezan por **prod**|
|`label=*prod`|Coincide con las etiquetas que terminan por **prod**|
|`label=*prod*`|Coincide con las etiquetas que contienen **prod**|
|`label=prod,test`|Coincide con las etiquetas **prod** o **test** (limitado a 5 archivos CSV)|

### <a name="reserved-characters"></a>Caracteres reservados

Los caracteres reservados son:

`*`, `\`, `,`

Si un carácter reservado forma parte del valor, se debe escapar mediante `\{Reserved Character}`. También se puede aplicar escape a los caracteres no reservados.

### <a name="filter-validation"></a>Validación del filtro

Si se produce un error de validación del filtro, la respuesta es HTTP `400` con detalles del error:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/invalid-argument",
  "title": "Invalid request parameter '{filter}'",
  "name": "{filter}",
  "detail": "{filter}(2): Invalid character",
  "status": 400
}
```

### <a name="examples"></a>Ejemplos

- Todo:

    ```http
    GET /revisions
    ```

- Elementos donde el nombre de clave comienza por **abc**:

    ```http
    GET /revisions?key=abc*&api-version={api-version}
    ```

- Elementos donde el nombre de clave es **abc** o **xyz**, y las etiquetas contienen **prod**:

    ```http
    GET /revisions?key=abc,xyz&label=*prod*&api-version={api-version}
    ```

## <a name="request-specific-fields"></a>Campos específicos de solicitud

Use el parámetro de cadena de consulta opcional `$select` y proporcione una lista separada por comas de campos solicitados. Si se omite el parámetro `$select`, la respuesta contiene el conjunto predeterminado.

```http
GET /revisions?$select=value,label,last_modified&api-version={api-version} HTTP/1.1
```

## <a name="time-based-access"></a>Acceso basado en el tiempo

Obtenga una representación del resultado tal como era en un momento anterior. Para obtener más información, consulte [Marco HTTP para el acceso basado en tiempo a los estados de los recursos: Memento](https://tools.ietf.org/html/rfc7089#section-2.1), sección 2.1.1.

```http
GET /revisions?api-version={api-version} HTTP/1.1
Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT
```

**Respuesta:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.revs+json"
Memento-Datetime: Sat, 12 May 2018 02:10:00 GMT
Link: <{relative uri}>; rel="original"
```

```json
{
    "items": [
        ....
    ]
}
```
