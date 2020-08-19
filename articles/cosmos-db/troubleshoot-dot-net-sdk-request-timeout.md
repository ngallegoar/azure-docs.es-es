---
title: Solución de Azure Cosmos DB HTTP 408 o problemas de tiempo de espera de solicitud con el SDK de .NET
description: Cómo diagnosticar y corregir la excepción de tiempo de espera de solicitud del SDK de .NET
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 09442e01fa160d3851169a51230fa4cbef7e0980
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2020
ms.locfileid: "88118576"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-net-sdk-request-timeout"></a>Diagnóstico y solución de problemas de tiempo de espera de solicitud del SDK de .NET de Azure Cosmos DB
El error HTTP 408 se produce si el SDK no puede completar la solicitud antes de que se produzca el límite de tiempo de espera.

## <a name="customizing-the-timeout-on-the-azure-cosmos-net-sdk"></a>Personalización del tiempo de espera en el SDK de.NET para Azure Cosmos

El SDK tiene dos alternativas distintas a los tiempos de espera de control, cada una con un ámbito diferente.

### <a name="requesttimeout"></a>RequestTimeout

La configuración de `CosmosClientOptions.RequestTimeout` (o `ConnectionPolicy.RequestTimeout` para SDK V2) le permite establecer un tiempo de espera que afecta a cada solicitud de red individual.  Una operación iniciada por un usuario puede abarcar varias solicitudes de red (por ejemplo, podría haber una limitación) y esta configuración se aplicaría para cada solicitud de red en el reintento. Este no es un tiempo de espera de la solicitud de operación de un extremo a otro.

### <a name="cancellationtoken"></a>CancellationToken

Todas las operaciones asincrónicas del SDK tienen un parámetro CancellationToken opcional. Este parámetro [CancellationToken](https://docs.microsoft.com/dotnet/standard/threading/how-to-listen-for-cancellation-requests-by-polling) se utiliza en toda la operación y en todas las solicitudes de red. Entre las solicitudes de red, se puede comprobar el parámetro CancellationToken y se cancela una operación si el token relacionado ha expirado. CancellationToken se debe usar para definir un tiempo de espera aproximado estimado en el ámbito de la operación.

> [!NOTE]
> CancellationToken es un mecanismo en el que la biblioteca comprobará la cancelación cuando [no cause un estado no válido](https://devblogs.microsoft.com/premier-developer/recommended-patterns-for-cancellationtoken/). Es posible que la operación no se cancele exactamente cuando finalice el tiempo definido en la cancelación, sino que, una vez transcurrido dicho tiempo, se cancelará cuando sea seguro hacerlo.

## <a name="troubleshooting-steps"></a>Pasos para solucionar problemas
La lista siguiente contiene las causas y las soluciones conocidas para las excepciones de tiempo de espera de solicitud.

### <a name="1-high-cpu-utilization-most-common-case"></a>1. Uso elevado de la CPU (caso más común)
Para una latencia óptima, se recomienda que el uso de la CPU sea aproximadamente del 40 %. Se recomienda usar 10 segundos como intervalo para supervisar el uso de la CPU máximo (no el promedio). Los picos de CPU son más habituales con consultas entre particiones en las que se pueden realizar varias conexiones para una sola consulta.

#### <a name="solution"></a>Solución:
La aplicación cliente que usa el SDK se debe escalar verticalmente u horizontalmente.

### <a name="2-socket--port-availability-might-be-low"></a>2. La disponibilidad de puertos/sockets puede ser reducida
Cuando se ejecutan en Azure, los clientes que usan el SDK de .NET pueden alcanzar el agotamiento de puertos SNAT (PAT) de Azure.

#### <a name="solution-1"></a>Solución 1:
Si utiliza máquinas virtuales de Azure, siga la [guía de agotamiento de puertos SNAT](troubleshoot-dot-net-sdk.md#snat).

#### <a name="solution-2"></a>Solución 2:
Si utiliza Azure App Service, siga la [guía de solución de problemas de errores de conexión](../app-service/troubleshoot-intermittent-outbound-connection-errors.md#cause) y [use el diagnóstico de App Service](https://azure.github.io/AppService/2018/03/01/Deep-Dive-into-TCP-Connections-in-App-Service-Diagnostics.html).

#### <a name="solution-3"></a>Solución 3:
Si utiliza Azure Functions, asegúrese de seguir la [recomendación de Azure Functions](../azure-functions/manage-connections.md#static-clients) de mantener los clientes singleton/estáticos para todos los servicios implicados (incluido Cosmos DB) y compruebe los [límites del servicio ](../azure-functions/functions-scale.md#service-limits) según el tipo y el tamaño del hospedaje de la aplicación de funciones.

#### <a name="solution-4"></a>Solución 4:
Si usa un Proxy HTTP, asegúrese de que pueda admitir el número de conexiones configuradas en el SDK de `ConnectionPolicy`.
En caso contrario, se encontrará con problemas de conexión.

### <a name="3-creating-multiple-client-instances"></a>3. Crear varias instancias de cliente
La creación de varias instancias de cliente podría provocar problemas de tiempo de espera y la contención de la conexión.

#### <a name="solution"></a>Solución:
Siga los [consejos de rendimiento](performance-tips-dotnet-sdk-v3-sql.md#sdk-usage) y use una sola instancia de CosmosClient en un proceso completo.

### <a name="4-hot-partition-key"></a>4. Clave de partición activa
Azure Cosmos DB distribuye el rendimiento general aprovisionado de forma uniforme entre las particiones físicas. Cuando hay una partición activa, una o más claves de partición lógicas de una partición física consumen todos las RU/s de la partición física, mientras que las RU/s de otras particiones físicas no se usan. Como síntoma, el total de RU/s consumido será inferior al total de RU/s aprovisionadas en la base de datos o el contenedor, pero seguirá viendo la limitación (429s) en las solicitudes de la clave de la partición lógica activa. Use la [métrica de consumo normalizado de RU](monitor-normalized-request-units.md) para ver si la carga de trabajo está detectando una partición activa. 

#### <a name="solution"></a>Solución:
Elija una buena clave de partición que distribuya uniformemente el volumen y el almacenamiento de solicitudes. Obtenga más información acerca de [cómo cambiar la clave de partición](https://devblogs.microsoft.com/cosmosdb/how-to-change-your-partition-key/).

### <a name="5-high-degree-of-concurrency"></a>5. Alto grado de simultaneidad
La aplicación tiene un alto nivel de simultaneidad, lo que puede provocar contención en el canal.

#### <a name="solution"></a>Solución:
La aplicación cliente que usa el SDK se debe escalar verticalmente u horizontalmente.

### <a name="6-large-requests-andor-responses"></a>6. Solicitudes y/o respuestas de gran tamaño
Un gran número de solicitudes o respuestas puede provocar un bloqueo de encabezado de línea en el canal y agravar la contención, incluso con un nivel relativamente bajo de simultaneidad.

#### <a name="solution"></a>Solución:
La aplicación cliente que usa el SDK se debe escalar verticalmente u horizontalmente.

### <a name="7-failure-rate-is-within-cosmos-db-sla"></a>7. La tasa de errores está dentro del acuerdo de nivel de servicio de Cosmos DB
La aplicación debe ser capaz de controlar los errores transitorios y volver a intentarlo cuando sea necesario. Las excepciones 408 no se reintentan porque, en las rutas de acceso de creación, no es posible saber si el servicio creó el elemento o si no lo hizo. Si se vuelve a enviar el mismo elemento para crearlo, se producirá una excepción de conflicto. La lógica empresarial de las aplicaciones de usuario podría tener una lógica personalizada para controlar los conflictos, lo que produciría errores por la ambigüedad de un elemento existente frente al conflicto de un reintento de creación.

### <a name="8-failure-rate-is-violating-the-cosmos-db-sla"></a>8. La tasa de errores infringe el acuerdo de nivel de servicio de Cosmos DB
Póngase en contacto con el soporte técnico de Azure.

## <a name="next-steps"></a>Pasos siguientes
* [Diagnóstico y solución de problemas](troubleshoot-dot-net-sdk.md) al usar el SDK de .NET de Azure Cosmos DB
* Más información sobre las directrices de rendimiento para [.NET V3](performance-tips-dotnet-sdk-v3-sql.md) y [.NET V2](performance-tips.md)
