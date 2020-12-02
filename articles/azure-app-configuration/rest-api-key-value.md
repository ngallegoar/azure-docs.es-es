---
title: 'API REST de Azure App Configuration: clave-valor'
description: Páginas de referencia para trabajar con pares clave-valor mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: f89b3f2fa4805eeb2fd9f9d511c8f228b98139ac
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2020
ms.locfileid: "95241036"
---
# <a name="key-values"></a>Valores clave

Un par clave-valor es un recurso identificado por una combinación única de `key` + `label`. `label` es opcional. Para hacer referencia de manera explícita a un par clave-valor sin etiqueta, use "\0" (se codifica para URL como ``%00``). Revise los detalles para cada operación.

Este artículo se aplica a la API versión 1.0.

## <a name="operations"></a>Operations

- Obtener
- Enumerar varios
- Set
- Eliminar

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-rest-api-prereqs.md)]

## <a name="syntax"></a>Sintaxis

```json
{
  "etag": [string],
  "key": [string],
  "label": [string, optional],
  "content_type": [string, optional],
  "value": [string],
  "last_modified": [datetime ISO 8601],
  "locked": [boolean],
  "tags": [object with string properties, optional]
}
```

## <a name="get-key-value"></a>Obtención del par clave-valor

Obligatorio: ``{key}``, ``{api-version}``  
Opcional: ``label`` (si se omite, implica un par clave-valor sin etiqueta).

```http
GET /kv/{key}?label={label}&api-version={api-version}
```

**Respuestas:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8;
Last-Modified: Tue, 05 Dec 2017 02:41:26 GMT
ETag: "4f6dd610dd5e4deebc7fbaef685fb903"
```

```json
{
  "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
  "key": "{key}",
  "label": "{label}",
  "content_type": null,
  "value": "example value",
  "last_modified": "2017-12-05T02:41:26+00:00",
  "locked": "false",
  "tags": {
    "t1": "value1",
    "t2": "value2"
  }
}
```

Si la clave no existe, se devuelve la respuesta siguiente:

```http
HTTP/1.1 404 Not Found
```

## <a name="get-conditionally"></a>Obtención (de manera condicional)

Para mejorar el almacenamiento en caché del cliente, use los encabezados de solicitud `If-Match` o `If-None-Match`. El argumento `etag` forma parte de la representación de la clave. Para obtener más información, consulte las [secciones 14.24 y 14.26](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

La siguiente solicitud recupera el par clave-valor solo si la representación actual no coincide con el argumento `etag` especificado:

```http
GET /kv/{key}?api-version={api-version} HTTP/1.1
Accept: application/vnd.microsoft.appconfig.kv+json;
If-None-Match: "{etag}"
```

**Respuestas:**

```http
HTTP/1.1 304 NotModified
```

or

```http
HTTP/1.1 200 OK
```

## <a name="list-key-values"></a>Enumeración de pares clave-valor

Opcional: ``key`` (si no se especifica, sugiere cualquier clave). Opcional: ``label`` (si no se especifica, sugiere cualquier etiqueta).

```http
GET /kv?label=*&api-version={api-version} HTTP/1.1
```

**Respuesta:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kvset+json; charset=utf-8
```

Para obtener más opciones, consulte la sección "Filtrado" más adelante en este artículo.

## <a name="pagination"></a>Paginación

El resultado se pagina si el número de elementos devueltos supera el límite de respuesta. Siga los encabezados de respuesta `Link` opcionales y use `rel="next"` para la navegación.
Como alternativa, el contenido proporciona un vínculo Siguiente en forma de propiedad `@nextLink`. El URI vinculado incluye el argumento `api-version`.

```http
GET /kv?api-version={api-version} HTTP/1.1
```

**Respuesta:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kvs+json; charset=utf-8
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

Se admite una combinación de filtrado de `key` y `label`.
Use los parámetros de cadena de consulta opcionales `key` y `label`.

```http
GET /kv?key={key}&label={label}&api-version={api-version}
```

### <a name="supported-filters"></a>Filtros admitidos

|Filtro de clave|Efecto|
|--|--|
|se omite `key` o `key=*`|Coincide con **cualquier** clave|
|`key=abc`|Coincide con una clave de nombre **abc**|
|`key=abc*`|Coincide con los nombres de claves que empiezan por **abc**|
|`key=abc,xyz`|Coincide con los nombres de claves **abc** o **xyz** (limitado a 5 archivos CSV)|

|Filtro de etiqueta|Efecto|
|--|--|
|se omite `label` o `label=*`|Coincide con **cualquier** etiqueta|
|`label=%00`|Coincide con pares clave-valor sin etiqueta|
|`label=prod`|Coincide con la etiqueta **prod**|
|`label=prod*`|Coincide con las etiquetas que empiezan por **prod**|
|`label=prod,test`|Coincide con las etiquetas **prod** o **test** (limitado a 5 archivos CSV)|

**_Caracteres reservados_* _

`_`, `\`, `,`

Si un carácter reservado forma parte del valor, se debe escapar mediante `\{Reserved Character}`. Los caracteres no reservados también se pueden escapar.

***Validación del filtro** _

En el caso de un error de validación del filtro, la respuesta es HTTP `400` con detalles del error:

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

_ *Ejemplos**

- All

    ```http
    GET /kv?api-version={api-version}
    ```

- El nombre de clave comienza por **abc** e incluye todas las etiquetas

    ```http
    GET /kv?key=abc*&label=*&api-version={api-version}
    ```

- El nombre de clave comienza por **abc** y la etiqueta es igual a **v1** o **v2**

    ```http
    GET /kv?key=abc*&label=v1,v2&api-version={api-version}
    ```

## <a name="request-specific-fields"></a>Solicitud de campos específicos

Use el parámetro de cadena de consulta opcional `$select` y proporcione una lista separada por comas de campos solicitados. Si se omite el parámetro `$select`, la respuesta contiene el conjunto predeterminado.

```http
GET /kv?$select=key,value&api-version={api-version} HTTP/1.1
```

## <a name="time-based-access"></a>Acceso basado en el tiempo

Obtenga una representación del resultado tal como era en un momento anterior. Para obtener más información, consulte la sección [2.1.1](https://tools.ietf.org/html/rfc7089#section-2.1). Todavía se admite la paginación tal y como se definió anteriormente en este artículo.

```http
GET /kv?api-version={api-version} HTTP/1.1
Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT
```

**Respuesta:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kvset+json"
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

## <a name="set-key"></a>Establecimiento de clave

- Obligatorio: ``{key}``
- Opcional: ``label`` (si no se especifica o, el valor es label=%00, implica un par clave-valor sin etiqueta).

```http
PUT /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
Content-Type: application/vnd.microsoft.appconfig.kv+json
```

```json
{
  "value": "example value",         // optional
  "content_type": "user defined",   // optional
  "tags": {                         // optional
    "tag1": "value1",
    "tag2": "value2",
  }
}
```

**Respuestas:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8
Last-Modified: Tue, 05 Dec 2017 02:41:26 GMT
ETag: "4f6dd610dd5e4deebc7fbaef685fb903"
```

```json
{
  "etag": "4f6dd610dd5e4deebc7fbaef685fb903",
  "key": "{key}",
  "label": "{label}",
  "content_type": "user defined",
  "value": "example value",
  "last_modified": "2017-12-05T02:41:26.4874615+00:00",
  "tags": {
    "tag1": "value1",
    "tag2": "value2",
  }
}
```

Si el elemento está bloqueado, recibirá la siguiente respuesta:

```http
HTTP/1.1 409 Conflict
Content-Type: application/problem+json; charset="utf-8"
```

```json
{
    "type": "https://azconfig.io/errors/key-locked",
    "title": "Modifing key '{key}' is not allowed",
    "name": "{key}",
    "detail": "The key is read-only. To allow modification unlock it first.",
    "status": "409"
}
```

## <a name="set-key-conditionally"></a>Establecimiento de clave (de manera condicional)

Para evitar condiciones de carrera, use los encabezados de solicitud `If-Match` o `If-None-Match`. El argumento `etag` forma parte de la representación de la clave.
Si se omiten `If-Match` o `If-None-Match`, la operación es incondicional.

La siguiente respuesta actualiza el valor solo si la representación actual coincide con el valor de `etag` especificado:

```http
PUT /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
Content-Type: application/vnd.microsoft.appconfig.kv+json
If-Match: "4f6dd610dd5e4deebc7fbaef685fb903"
```

La siguiente respuesta actualiza el valor solo si la representación actual no coincide con el valor de `etag` especificado:

```http
PUT /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
Content-Type: application/vnd.microsoft.appconfig.kv+json;
If-None-Match: "4f6dd610dd5e4deebc7fbaef685fb903"
```

La siguiente solicitud agrega el valor solo si ya existe una representación:

```http
PUT /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
Content-Type: application/vnd.microsoft.appconfig.kv+json;
If-Match: "*"
```

La siguiente solicitud agrega el valor solo si no existe una representación:

```http
PUT /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
Content-Type: application/vnd.microsoft.appconfig.kv+json
If-None-Match: "*"
```

**Respuestas**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8
...
```

or

```http
HTTP/1.1 412 PreconditionFailed
```

## <a name="delete"></a>Eliminar

- Obligatorio: `{key}`, `{api-version}`
- Opcional: `{label}` (si no se especifica o, el valor es label=%00, implica un par clave-valor sin etiqueta).

```http
DELETE /kv/{key}?label={label}&api-version={api-version} HTTP/1.1
```

**Respuesta:** Devuelve el par clave-valor eliminado, o no devuelve ningún valor si el par clave-valor no existía.

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8
...
```

or

```http
HTTP/1.1 204 No Content
```

## <a name="delete-key-conditionally"></a>Eliminación de clave (de manera condicional)

Esto es similar a la sección "Establecimiento de clave (de manera condicional)" anteriormente en este artículo.
