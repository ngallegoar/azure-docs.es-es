---
title: 'API REST de Azure App Configuration: limitación'
description: Páginas de referencia para comprender la limitación cuando se usa la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: 26464138b7739bb476d9d0dd1078194473ecd23e
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423817"
---
# <a name="throttling"></a>Limitación de peticiones

Los almacenes de configuración tienen límites en las solicitudes que pueden atender. Las solicitudes que superen la cuota asignada para un almacén de configuración recibirán una respuesta HTTP 429 (Demasiadas solicitudes).

La limitación se divide en diferentes directivas de cuota:

- **Solicitudes totales**: número total de solicitudes
- **Ancho de banda total**: datos de salida en bytes
- **Almacenamiento**: tamaño total de almacenamiento de datos de usuario en bytes

## <a name="handling-throttled-responses"></a>Control de las respuestas limitadas

Cuando se alcanza el límite de frecuencia de una cuota determinada, el servidor responderá a las solicitudes posteriores de ese tipo con un código de estado _429_. La respuesta _429_ contendrá un encabezado _retry-after-ms_ que proporciona al cliente un tiempo de espera sugerido (en milisegundos) para permitir la reposición de la cuota de solicitudes.

```http
HTTP/1.1 429 (Too Many Requests)
retry-after-ms: 10
Content-Type: application/problem+json; charset=utf-8
```

```json
{
  "type": "https://azconfig.io/errors/too-many-requests",
  "title": "Resource utilization has surpassed the assigned quota",
  "policy": "Total Requests",
  "status": 429
}
```

En el ejemplo anterior, el cliente ha superado su cuota permitida y se recomienda que se ralentice y espere 10 milisegundos antes de intentar realizar más solicitudes. Los clientes deben considerar también el retroceso progresivo.

## <a name="other-retry"></a>Otro reintento

El servicio puede identificar situaciones distintas de la limitación que necesitan un reintento del cliente (por ejemplo: 503 Servicio no disponible). En todos estos casos, se proporcionará el encabezado de respuesta `retry-after-ms`. Para aumentar la solidez, se recomienda que el cliente siga el intervalo sugerido y realice un reintento.

```http
HTTP/1.1 503 Service Unavailable
retry-after-ms: 787
```
