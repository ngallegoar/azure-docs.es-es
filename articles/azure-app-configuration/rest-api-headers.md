---
title: 'API REST de Azure App Configuration: encabezados'
description: Páginas de referencia para los encabezados utilizados con la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 5e1f92c68004d0197391ab72df775913c0940fec
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423838"
---
# <a name="headers"></a>Encabezados

En este artículo se proporcionan vínculos a páginas de referencia para los encabezados usados con la API REST de Azure App Configuration.

## <a name="request-headers"></a>Encabezados de solicitud

En la tabla siguiente se describen los encabezados de solicitud comunes que se usan en Azure App Configuration.

| Encabezado | Descripción | Ejemplo |
|--|--|--|
| **Autorización** | Se utiliza para [autenticar](./rest-api-authentication-index.md) una solicitud en el servicio. Consulte la [sección 14.8](https://tools.ietf.org/html/rfc2616#section-14.8). | `Authorization: HMAC-SHA256 Credential=<Credential>&SignedHeaders=Host;x-ms-date;x-ms-content-sha256&Signature=<Signature>` |
| **Aceptación** | Informa al servidor de qué tipo de elementos multimedia aceptará el cliente en una respuesta HTTP. Consulte la [sección 14.1](https://tools.ietf.org/html/rfc2616#section-14.1). | `Accept: application/vnd.microsoft.appconfig.kv+json;` |
| **Accept-Datetime** | Solicita al servidor que devuelva su contenido como una representación de su estado anterior. El valor de este encabezado es la fecha y hora solicitada de ese estado. Consulte [RFC 7089](https://tools.ietf.org/html/rfc7089#section-2.1.1). | `Accept-Datetime: Sat, 12 May 2018 02:10:00 GMT` |
| **Content-Type** | Contiene el tipo de elementos multimedia del contenido dentro del cuerpo de la solicitud HTTP. Consulte la [sección 14.17](https://tools.ietf.org/html/rfc2616#section-14.17). | `Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8;` |
| **Date** | Fecha y hora de emisión de la solicitud HTTP. Este encabezado se usa en la [autenticación de HMAC](./rest-api-authentication-hmac.md). Consulte la [sección 14.18](https://tools.ietf.org/html/rfc2616#section-14.18). | `Date: Fri, 11 May 2018 18:48:36 GMT` |
| **Host** | Especifica el inquilino para el que se ha emitido la solicitud. Este encabezado se usa en la [autenticación de HMAC](./rest-api-authentication-hmac.md). Consulte la [sección 14.23](https://tools.ietf.org/html/rfc2616#section-14.23). | `Host: contoso.azconfig.io` |
| **If-Match** | Se utiliza para crear una solicitud HTTP condicional. Esta solicitud solo se realiza correctamente si la ETag del recurso de destino coincide con el valor de este encabezado. El valor "*" coincide con cualquier ETag. Consulte la [sección 14.24](https://tools.ietf.org/html/rfc2616#section-14.24). | `If-Match: "4f6dd610dd5e4deebc7fbaef685fb903"` |
| **If-None-Match** | Se utiliza para crear una solicitud HTTP condicional. Esta solicitud solo se realiza correctamente si la ETag del recurso de destino no coincide con el valor de este encabezado. El valor "*" coincide con cualquier ETag. Consulte la [sección 14.26](https://tools.ietf.org/html/rfc2616#section-14.26). | `If-None-Match: "4f6dd610dd5e4deebc7fbaef685fb903"` |
| **Sync-Token** | Se utiliza para habilitar la coherencia en tiempo real durante una secuencia de solicitudes. | `Sync-Token: jtqGc1I4=MDoyOA==;sn=28` |
| **x-ms-client-request-id** | Identificador único proporcionado por el cliente que se usa para realizar el seguimiento de la operación de ida y vuelta de una solicitud. | `x-ms-client-request-id: 00000000-0000-0000-0000-000000000000` |
| **x-ms-content-sha256** | Hash SHA256 del cuerpo de la solicitud HTTP. Este encabezado se usa en la [autenticación de HMAC](./rest-api-authentication-hmac.md). | `x-ms-content-sha256: 47DEQpj8HBSa+/TImW+5JCeuQeRkm5NMpJWZG3hSuFU=` |
| **x-ms-date** | Este encabezado se puede establecer y usar en lugar del encabezado `Date` si no se puede acceder al encabezado de fecha. Este encabezado se usa en la [autenticación de HMAC](./rest-api-authentication-hmac.md). | `x-ms-date: Fri, 11 May 2018 18:48:36 GMT` |
| **x-ms-return-client-request-id** | Se usa junto con el encabezado `x-ms-client-request-id`. Si el valor de este encabezado es "true", se indicará al servidor que devuelva el valor del encabezado de solicitud `x-ms-client-request-id`. | `x-ms-return-client-request-id: true` |

## <a name="response-headers"></a>Encabezados de respuesta

El servidor puede incluir los siguientes encabezados HTTP en sus respuestas.

| Encabezado | Descripción | Ejemplo |
|--|--|--|
| **Content-Type** | Contiene el tipo de elementos multimedia del contenido dentro del cuerpo de la respuesta HTTP. Consulte la [sección 14.17](https://tools.ietf.org/html/rfc2616#section-14.17). | `Content-Type: application/vnd.microsoft.appconfig.kv+json; charset=utf-8;` |
| **ETag** | Token opaco que representa el estado de un recurso determinado. Se puede usar en operaciones condicionales. Consulte la [sección 14.19](https://tools.ietf.org/html/rfc2616#section-14.19). | `ETag: "4f6dd610dd5e4deebc7fbaef685fb903"` |
| **Last-Modified** | Describe cuándo se modificó por última vez el recurso solicitado. Con formato de [fecha HTTP](https://tools.ietf.org/html/rfc2616#section-3.3.1). Consulte la [sección 14.29](https://tools.ietf.org/html/rfc2616#section-14.29). | `Last-Modified: Tue, 05 Dec 2017 02:41:26 GMT` |
| **Vínculo** | Proporciona vínculos a los recursos relacionados con la respuesta. Este encabezado se utiliza para la paginación mediante el vínculo _next_ (siguiente). Consulte [RFC 5988](https://tools.ietf.org/html/rfc5988). | `Link: </kv?after={token}>; rel="next"` |
| **Memento-Datetime** | Indica que el contenido de una respuesta representa un estado anterior. El valor de este encabezado es la fecha y hora de ese estado. Consulte [RFC 7089](https://tools.ietf.org/html/rfc7089#section-2.1.1). | `Memento-Datetime: Sat, 12 May 2018 02:10:00 GMT` |
| **retry-after-ms** | Proporciona un período sugerido (en milisegundos) para que el cliente espere antes de reintentar una solicitud con errores. | `retry-after-ms: 10` |
| **x-ms-request-id** | Identificador único generado por el servidor que se utiliza para realizar el seguimiento de la solicitud en el servicio. | `x-ms-request-id: 00000000-0000-0000-0000-000000000000` |
| **WWW-Authenticate** | Se usa para desafiar a los clientes para la autenticación y proporcionar un motivo por el que se ha producido un error en el intento de autenticación. Consulte la [sección 14.47](https://tools.ietf.org/html/rfc2616#section-14.47). | `WWW-Authenticate: HMAC-SHA256 error="invalid_token" error_description="Invalid Signature"` |
