---
title: API del servicio de medición de Marketplace | Azure Marketplace
description: Ofertas del evento de uso para SaaS en Azure Marketplace.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/11/2019
ms.openlocfilehash: 315f36e5aed9dee0a89e1f9f504b18a6bed806e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80275754"
---
# <a name="marketplace-metering-service-apis"></a>API del servicio de medición de Marketplace

La API de eventos de uso permite emitir eventos de uso para una entidad específica que haya comprado. La solicitud de evento de uso hace referencia a la dimensión de servicios de medición que define el publicador al lanzar la oferta.

## <a name="usage-event"></a>Evento de uso

**POST**: `https://marketplaceapi.microsoft.com/api/usageEvent?api-version=<ApiVersion>`

*Parámetros de consulta*:

|            |          |
| ---------- | ---------------------- |
| `ApiVersion` | Versión de la operación que se usará para esta solicitud. La versión de API más reciente es 2018-08-31. |

*Encabezados de la solicitud*:

| Content-type       | `application/json`    |
| ------------------ | ---------------------------- |
| `x-ms-requestid`     | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| `x-ms-correlationid` | Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
| `authorization`   | [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app). Nota: Al hacer la solicitud HTTP, agregue el prefijo `Bearer` al token obtenido del vínculo al que se hace referencia. |

*Solicitud:*

```json
{
  "resourceId": "Identifier of the resource against which usage is emitted",
  "quantity": 5.0,
  "dimension": "Dimension identifier",
  "effectiveStartTime": "Time in UTC when the usage event occurred",
  "planId": "Plan associated with the purchased offer"
}
```

### <a name="responses"></a>Respuestas

Código: 200<br>
Aceptar 

```json
{
  "usageEventId": "Unique identifier associated with the usage event",
  "status": "Accepted",
  "messageTime": "Time this message was created in UTC",
  "resourceId": "Identifier of the resource against which usage is emitted",
  "quantity": 5.0,
  "dimension": "Dimension identifier",
  "effectiveStartTime": "Time in UTC when the usage event occurred",
  "planId": "Plan associated with the purchased offer"
}
```

Código: 400 <br>
La solicitud es incorrecta, faltan datos o los datos proporcionados no son válidos o han expirado.

```json
{
  "message": "One or more errors have occurred.",
  "target": "usageEventRequest",
  "details": [
    {
      "message": "The resourceId is required.",
      "target": "ResourceId",
      "code": "BadArgument"
    }
  ],
  "code": "BadArgument"
}
```

Código: 403<br>
La solicitud es incorrecta, faltan datos o los datos proporcionados no son válidos o han expirado.

```json
{
  "code": "Forbidden",
  "message": "User is not allowed authorized to call this"
}
```

Código: 409<br>
Conflicto cuando se recibe la llamada de uso para el identificador del recurso de uso y el uso real, que ya existe. La respuesta contendrá el campo `additionalInfo`, con información sobre el mensaje aceptado.

```json
{
  "code": "Conflict",
  "additionalInfo": {
    "usageEventId": "Unique identifier associated with the usage event",
    "status": "Accepted|NotProcessed|Expired",
    "messageTime": "Time this message was created in UTC",
    "resourceId": "Identifier of the resource against which usage is emitted",
    "quantity": 5.0,
    "dimension": "Dimension identifier",
    "effectiveStartTime": "Time in UTC when the usage event occurred",
    "planId": "Plan associated with the purchased offer"
  }
}
```

## <a name="batch-usage-event"></a>Evento de uso por lotes

La API de eventos de uso por lotes permite emitir a la vez eventos de uso para más de una entidad específica que haya comprado. La solicitud de evento de uso por lotes hace referencia a la dimensión de servicios de medición que define el publicador al lanzar la oferta.

>[!Note]
>Puede registrar varias ofertas de SaaS en el programa Marketplace comercial de Microsoft. Cada oferta de SaaS registrada tiene una aplicación de Azure AD única que se registra con fines de autenticación y autorización. Los eventos emitidos por lotes deben pertenecer a ofertas con la misma aplicación de Azure AD en el momento en que se registra la oferta.

**POST:** `https://marketplaceapi.microsoft.com/api/batchUsageEvent?api-version=<ApiVersion>`

*Parámetros de consulta*:

|            |     |
| ---------- | -------------------- |
| `ApiVersion` | Versión de la operación que se usará para esta solicitud. La versión de API más reciente es 2018-08-31. |

*Encabezados de la solicitud*:

| Content-type       | `application/json`       |
| ------------------ | ------ |
| `x-ms-requestid`     | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| `x-ms-correlationid` | Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| `authorization`      | [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app). Nota: Al hacer la solicitud HTTP, agregue el prefijo `Bearer` al token obtenido del vínculo al que se hace referencia.  |

*Solicitud:*
```json
{
  "request": [
    {
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer"
    },
    {
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer"
    }
  ]
}
```
### <a name="responses"></a>Respuestas

Código: 200<br>
Aceptar

```json
{
  "count": 2,
  "result": [
    {
      "usageEventId": "Unique identifier associated with the usage event",
      "status": "Accepted|Expired|Duplicate|Error|ResourceNotFound|ResourceNotAuthorized|InvalidDimension|BadArgument",
      "messageTime": "Time this message was created in UTC",
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer",
      "error": "Error object (optional)"
    },
    {
      "usageEventId": "Unique identifier associated with the usage event",
      "status": "Accepted|Expired|Duplicate|Error|ResourceNotFound|ResourceNotAuthorized|InvalidDimension|BadArgument",
      "messageTime": "Time this message was created in UTC",
      "resourceId": "Identifier of the resource against which usage is emitted",
      "quantity": 5.0,
      "dimension": "Dimension identifier",
      "effectiveStartTime": "Time in UTC when the usage event occurred",
      "planId": "Plan associated with the purchased offer",
      "error": "Error object (optional)"
    }
  ]
}
```

Descripción del código de estado al que se hace referencia en respuesta de la API `BatchUsageEvent`:

| status code  | Descripción |
| ---------- | -------------------- |
| `Accepted` | Código aceptado. |
| `Expired` | Uso expirado. |
| `Duplicate` | Se ha proporcionado un uso duplicado. |
| `Error` | Código de error. |
| `ResourceNotFound` | El recurso de uso proporcionado no es válido. |
| `ResourceNotAuthorized` | No está autorizado a proporcionar el uso de este recurso. |
| `InvalidDimension` | La dimensión para la que se ha pasado el uso no es válida para esta oferta o plan. |
| `InvalidQuantity` | La cantidad pasada es <0. |
| `BadArgument` | Falta la entrada o tiene un formato incorrecto. |

Código: 400<br>
La solicitud es incorrecta, faltan datos o los datos proporcionados no son válidos o han expirado.

```json
{
  "message": "One or more errors have occurred.",
  "target": "usageEventRequest",
  "details": [
    {
      "message": "Invalid data format.",
      "target": "usageEventRequest",
      "code": "BadArgument"
    }
  ],
  "code": "BadArgument"
}
```
Código: 403<br>
El usuario no está autorizado a hacer esta llamada.

```json
{
  "code": "Forbidden",
  "message": "User is not allowed to call this"
}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte [Facturación según uso de SaaS](./saas-metered-billing.md).
