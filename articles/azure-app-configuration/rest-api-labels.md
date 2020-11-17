---
title: 'API REST de Azure App Configuration: etiquetas'
description: Páginas de referencia para trabajar con etiquetas mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: e6fcc8399c1dbc36a5b2e915c726f1c2496ee2f9
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423771"
---
# <a name="labels"></a>Etiquetas

api-version: 1.0

El recurso **Etiqueta** se define de la siguiente manera:

```json
{
      "name": [string]             // Name of the label
}
```

Admite las siguientes operaciones:

- List

Para todas las operaciones, ``name`` es un parámetro de filtro opcional. Si se omite, implica **cualquier etiqueta**.

## <a name="prerequisites"></a>Requisitos previos

- Se deben autenticar todas las solicitudes HTTP. Consulte la sección [Autenticación](./rest-api-authentication-index.md).
- Todas las solicitudes HTTP deben proporcionar parámetros `api-version` explícitos. Consulte la sección [Control de versiones](./rest-api-versioning.md).

## <a name="list-labels"></a>Enumeración de etiquetas

```http
GET /labels?api-version={api-version} HTTP/1.1
```

**Respuestas:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.labelset+json; charset=utf-8"
```

```json
{
    "items": [
        {
          "name": "{label-name}"
        },
        ...
    ],
    "@nextLink": "{relative uri}"
}
```

## <a name="pagination"></a>Paginación

El resultado se pagina si el número de elementos devueltos supera el límite de respuesta. Siga los encabezados de respuesta `Link` opcionales y use `rel="next"` para la navegación. Como alternativa, el contenido proporciona un vínculo siguiente en forma de la propiedad `@nextLink`. El siguiente vínculo contiene el parámetro `api-version`.

```http
GET /labels?api-version={api-version} HTTP/1.1
```

**Respuesta:**

```http
HTTP/1.1 OK
Content-Type: application/vnd.microsoft.appconfig.labelset+json; charset=utf-8
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

## <a name="filtering"></a>Filtrado

Se admite el filtrado por `name`.

```http
GET /labels?name={label-name}&api-version={api-version}
```

### <a name="supported-filters"></a>Filtros admitidos

|Filtro de clave|Efecto|
|--|--|
|se omite `name` o `name=*`|Coincide con **cualquier** etiqueta.|
|`name=abc`|Coincide con una etiqueta denominada **abc**.|
|`name=abc*`|Coincide con los nombres de etiqueta que empiezan por **abc**.|
|`name=abc,xyz`|Coincide con los nombres de etiqueta **abc** o **xyz** (se limita a 5 archivos CSV).|

### <a name="reserved-characters"></a>Caracteres reservados

`*`, `\`, `,`

Si un carácter reservado forma parte del valor, se debe escapar mediante `\{Reserved Character}`. Los caracteres no reservados también pueden se pueden escapar.

### <a name="filter-validation"></a>Validación del filtro

Si se produce un error de validación del filtro, la respuesta es HTTP `400` con detalles del error:

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

### <a name="examples"></a>Ejemplos

- All

    ```http
    GET /labels?api-version={api-version}
    ```

- El nombre de la etiqueta comienza con **abs**.

    ```http
    GET  /labels?name=abc*&api-version={api-version}
    ```

- El nombre de la etiqueta es **abc** o **xyz**.

    ```http
    GET /labels?name=abc,xyz&api-version={api-version}
    ```

## <a name="request-specific-fields"></a>Campos específicos de solicitud

Use el parámetro de cadena de consulta `$select` opcional y proporcione una lista separada por comas de los campos solicitados. Si se omite el parámetro `$select`, la respuesta contiene el conjunto predeterminado.

```http
GET /labels?$select=name&api-version={api-version} HTTP/1.1
```

## <a name="time-based-access"></a>Acceso basado en el tiempo

Obtenga una representación del resultado como estaba en un momento anterior. Consulte la sección [2.1.1](https://tools.ietf.org/html/rfc7089#section-2.1).

```http
GET /labels&api-version={api-version} HTTP/1.1
Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT
```

**Respuesta:**

```http
HTTP/1.1 200 OK
Content-Type: application/vnd.microsoft.appconfig.labelset+json"
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
