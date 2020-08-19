---
title: Solución de problemas de excepción de servicio no disponible de Azure Cosmos DB
description: Diagnóstico y corrección de excepción de servicio no disponible de Cosmos DB
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: b9c24ee3b94be86ccf9d27b928c42fc194800a3b
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87987382"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable"></a>Diagnóstico y solución de problemas de servicio no disponible de Azure Cosmos DB
El SDK no ha podido conectarse con el servicio de Azure Cosmos DB.

## <a name="troubleshooting-steps"></a>Pasos para solucionar problemas
La lista siguiente contiene las causas conocidas y las soluciones para las excepciones de servicio no disponible.

### <a name="1-the-required-ports-are-being-blocked"></a>1. Los puertos necesarios se están bloqueando
Compruebe que todos los [puertos necesarios](performance-tips-dotnet-sdk-v3-sql.md#networking) estén habilitados.

### <a name="2-client-side-transient-connectivity-issues"></a>2. Problemas de conectividad transitorios en el lado cliente
Las excepciones de servicio no disponible pueden surgir cuando hay problemas de conectividad transitorios que provocan tiempos de espera. Normalmente, el seguimiento de la pila relacionado con este escenario contendrá un objeto `TransportException`, por ejemplo:

```C#
TransportException: A client transport error occurred: The request timed out while waiting for a server response. 
(Time: xxx, activity ID: xxx, error code: ReceiveTimeout [0x0010], base error: HRESULT 0x80131500
```

Siga la [solución de problemas de tiempo de espera agotado de las solicitudes](troubleshoot-dot-net-sdk-request-timeout.md#troubleshooting-steps) para resolverlo.

### <a name="3-service-outage"></a>3. Interrupción del servicio
Compruebe el [estado de Azure](https://status.azure.com/status) para ver si hay algún problema pendiente.


## <a name="next-steps"></a>Pasos siguientes
* [Diagnóstico y solución de problemas](troubleshoot-dot-net-sdk.md) al usar el SDK de .NET de Azure Cosmos DB
* Más información sobre las directrices de rendimiento para [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) y [.NET V2](performance-tips.md)