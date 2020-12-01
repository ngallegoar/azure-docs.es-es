---
title: 'API REST de Azure App Configuration: coherencia'
description: Páginas de referencia para garantizar la coherencia en tiempo real mediante la API REST de Azure App Configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: reference
ms.date: 08/17/2020
ms.openlocfilehash: db9553c2c9c79a6beb9c66d0cb1a1a60435b2abd
ms.sourcegitcommit: 30906a33111621bc7b9b245a9a2ab2e33310f33f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2020
ms.locfileid: "95253344"
---
# <a name="real-time-consistency"></a>Coherencia en tiempo real

Debido a la naturaleza de algunos sistemas distribuidos, es difícil aplicar de manera implícita la coherencia en tiempo real entre solicitudes. Una solución consiste en permitir la compatibilidad con protocolos mediante varios tokens de sincronización. Los tokens de sincronización son opcionales.

## <a name="initial-request"></a>Solicitud inicial

Para garantizar la coherencia en tiempo real entre las distintas instancias y solicitudes de cliente, use los encabezados de solicitud y respuesta `Sync-Token` opcionales.

Sintaxis:

```http
Sync-Token: <id>=<value>;sn=<sn>
```

|Parámetro|Descripción|
|--|--|
| `<id>` | Id. de token (opaco). |
| `<value>` | Valor de token (opaco). Admite cadenas codificadas en base64. |
| `<sn>` | Número de secuencia de token (versión). Un número mayor significa una versión más reciente del mismo token. Permite mejorar el almacenamiento en caché del cliente y la simultaneidad. El cliente puede optar por usar solo la última versión del token, ya que las versiones del token son inclusivas. Este parámetro no es obligatorio para las solicitudes. |

## <a name="response"></a>Response

El servicio proporciona un encabezado `Sync-Token` con cada respuesta.

```http
Sync-Token: jtqGc1I4=MDoyOA==;sn=28
```

## <a name="subsequent-requests"></a>Solicitudes siguientes

Todas las solicitudes posteriores tienen garantizada una respuesta coherente en tiempo real con respecto al `Sync-Token` proporcionado.

```http
Sync-Token: <id>=<value>
```

Si omite el encabezado `Sync-Token` en la solicitud, es posible que el servicio responda con los datos en caché durante un breve período de tiempo (hasta unos segundos) antes de que se configuren internamente. Este comportamiento puede producir lecturas incoherentes si ha habido cambios inmediatamente antes de la lectura.

## <a name="multiple-sync-tokens"></a>Varios tokens de sincronización

El servidor podría responder con varios tokens de sincronización para una única solicitud. Para mantener la coherencia en tiempo real de la solicitud siguiente, el cliente debe responder con todos los tokens de sincronización recibidos. Cuando hay varios valores de encabezado, se deben separar con comas.

```http
Sync-Token: <token1-id>=<value>,<token2-id>=<value>
```
