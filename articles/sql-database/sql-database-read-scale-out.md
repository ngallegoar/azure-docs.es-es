---
title: Lectura de consultas en réplicas
description: Azure SQL Database permite equilibrar las cargas de trabajo de solo lectura mediante una funcionalidad de réplicas de solo lectura llamada "Escalado horizontal de lectura".
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
ms.date: 06/03/2019
ms.openlocfilehash: 1a1b9907cd931716949d92d948a7d541fd2d5057
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206952"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Uso de réplicas de solo lectura para equilibrar las cargas de trabajo de las consultas de solo lectura

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

En la [arquitectura de alta disponibilidad](./sql-database-high-availability.md#premium-and-business-critical-service-tier-availability), cada base de datos del nivel de servicio Premium y Crítico para la empresa se aprovisiona automáticamente con una réplica principal y varias réplicas secundarias. Las réplicas secundarias se aprovisionan con el mismo tamaño de proceso que la réplica principal. La característica **Escalado horizontal de lectura** le permite equilibrar las cargas de trabajo de solo lectura de Azure SQL Database utilizando la capacidad de una de las réplicas de solo lectura en lugar de compartir réplicas de lectura y escritura. De este modo, la carga de trabajo de solo lectura se aísla de la carga de trabajo principal de lectura y escritura y no afecta a su rendimiento. La característica está diseñada para las aplicaciones que contienen cargas de solo lectura separadas de forma lógica; por ejemplo, análisis. En los niveles de servicio Premium y Crítico para la empresa, las aplicaciones podrían obtener ventajas de rendimiento gracias a esta capacidad sin costo adicional.

La característica **Escalado horizontal de lectura** también está disponible en el nivel de servicio Hiperescala cuando se crea al menos una réplica secundaria. Se pueden usar varias réplicas secundarias si las cargas de trabajo de solo lectura requieren más recursos de los disponibles en una réplica secundaria. La arquitectura de alta disponibilidad de los niveles de servicio Básico, Estándar y De uso general no incluye réplicas. La característica **Escalado horizontal de lectura** no está disponible en estos niveles de servicio.

En el diagrama siguiente, se muestra un ejemplo con una base de datos de nivel Crítico para la empresa.

![Réplicas de solo lectura](media/sql-database-read-scale-out/business-critical-service-tier-read-scale-out.png)

La característica Escalado horizontal de lectura está habilitada de forma predeterminada en las nuevas bases de datos de nivel Premium, Crítico para la empresa e Hiperescala. En el caso de Hiperescala, se crea una réplica secundaria de forma predeterminada para las bases de datos nuevas. Si la cadena de conexión SQL está configurada con `ApplicationIntent=ReadOnly`, la aplicación se redirigirá por la puerta de enlace a una réplica de solo lectura de esa base de datos. Para más información sobre la propiedad `ApplicationIntent`, consulte [Especificar el intento de la aplicación](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Si desea asegurarse de que la aplicación se conecta a la réplica principal sin tener en cuenta el valor `ApplicationIntent` de la cadena de conexión SQL, debe deshabilitar explícitamente el escalado horizontal de lectura cuando cree la base de datos o modifique su configuración. Por ejemplo, si actualiza una base de datos de nivel Estándar o De uso General al nivel Premium, Crítico para la empresa o Hiperescala y desea asegurarse de que todas las conexiones siguen estableciéndose con la réplica principal, deshabilite el escalado horizontal de lectura. Para más información acerca de cómo deshabilitarlo, consulte [Habilitar y deshabilitar el escalado horizontal de lectura](#enable-and-disable-read-scale-out).

> [!NOTE]
> Las características Almacén de datos de consultas, Eventos extendidos y SQL Profiler no son compatibles con las réplicas de solo lectura.

## <a name="data-consistency"></a>Coherencia de datos

Una de las ventajas de las réplicas es que siempre se encuentran en estado de coherencia transaccional, pero es posible que en diferentes momentos pueda haber una pequeña latencia entre las distintas réplicas. El escalado horizontal de lectura admite la coherencia en el nivel de sesión. Esto significa que, si la sesión de solo lectura vuelve a conectarse después de un error de conexión provocado por la falta de disponibilidad de las réplicas, podría redirigirse a una réplica que no tuviera toda la información de la réplica de lectura y escritura. Del mismo modo, si una aplicación escribe datos en una sesión de lectura y escritura y los lee inmediatamente en una sesión de solo lectura, es posible que las actualizaciones más recientes no puedan verse inmediatamente en la réplica. La latencia se produce por una operación asincrónica que rehace el registro de transacciones.

> [!NOTE]
> Las latencias de replicación en la región son bajas y esta situación es poco frecuente.

## <a name="connect-to-a-read-only-replica"></a>Conexión a una réplica de solo lectura

Cuando se habilita el escalado horizontal de lectura en una base de datos, la opción `ApplicationIntent` de la cadena de conexión proporcionada por el cliente dictamina si la conexión se enruta a la réplica de lectura o a una réplica de solo lectura. En concreto, si el valor de `ApplicationIntent` es `ReadWrite` (el valor predeterminado), la conexión se dirigirá a la réplica de lectura-escritura de la base de datos. Este comportamiento es idéntico al existente. Si el valor de `ApplicationIntent` es `ReadOnly`, la conexión se enruta a una réplica de solo lectura.

Por ejemplo, la siguiente cadena de conexión conecta el cliente a una réplica de solo lectura (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan dichos corchetes):

```sql
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Cualquiera de las siguientes cadenas de conexión conecta el cliente a una réplica de lectura-escritura (los elementos entre corchetes angulares se sustituyen por los valores correctos para su entorno y se quitan dichos corchetes):

```sql
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>Verificación de que se trata de una conexión a una réplica de solo lectura

Puede comprobar si está conectado a una réplica de solo lectura mediante la ejecución de la consulta siguiente. Si está conectado a una réplica de solo lectura, se devolverá READ_ONLY.

```sql
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> En un momento dado, las sesiones de solo lectura solo pueden acceder a una de las réplicas de Always On.

## <a name="monitoring-and-troubleshooting-read-only-replica"></a>Supervisión y solución de problemas con las réplicas de solo lectura

Cuando se conecta a una réplica de solo lectura, puede acceder a las métricas de rendimiento utilizando la vista de administración dinámica `sys.dm_db_resource_stats`. Para acceder a las estadísticas del plan de consulta, use las vistas de administración dinámica `sys.dm_exec_query_stats`, `sys.dm_exec_query_plan` y `sys.dm_exec_sql_text`.

> [!NOTE]
> La vista de administración dinámica `sys.resource_stats` de la base de datos maestra lógica devuelve información sobre el almacenamiento y el uso de la CPU en la réplica principal.

## <a name="enable-and-disable-read-scale-out"></a>Habilitar y deshabilitar el escalado horizontal de lectura

La característica Escalado horizontal de lectura está habilitada de forma predeterminada en los niveles de servicio Premium, Crítico para la empresa e Hiperescala. No se puede habilitar en los niveles de servicio Básico, Estándar o De uso general. Se deshabilita automáticamente en las bases de datos del nivel Hiperescala que no están configuradas con ninguna réplica.

Puede deshabilitar y volver a habilitar la característica Escalado horizontal de lectura en las bases de datos únicas y las bases de datos de grupo elástico del nivel de servicio Premium o Crítico para la empresa; para ello, utilice los métodos siguientes.

> [!NOTE]
> La posibilidad de deshabilitar la característica Escalado horizontal de lectura se proporciona por motivos de compatibilidad con las versiones anteriores.

### <a name="azure-portal"></a>Portal de Azure

Puede administrar la configuración de escalado horizontal de lectura en la hoja **Configurar** de la base de datos.

### <a name="powershell"></a>PowerShell

> [!IMPORTANT]
> El módulo de Azure Resource Manager (RM) para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. El módulo de AzureRM continuará recibiendo correcciones de errores hasta diciembre de 2020 como mínimo.  Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos. Para obtener más información sobre la compatibilidad, vea [Presentación del nuevo módulo Az de Azure PowerShell](/powershell/azure/new-azureps-module-az).

Para administrar el escalado horizontal de lectura en Azure PowerShell se requiere la versión de Azure PowerShell de diciembre de 2016 u otra posterior. Para saber dónde encontrar la versión más reciente de PowerShell, consulte [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Puede habilitar o deshabilitar la característica Escalado horizontal de lectura de Azure PowerShell invocando el cmdlet [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) y pasando el valor deseado, `Enabled` o `Disabled`, al parámetro `-ReadScale`.

Para deshabilitar el escalado horizontal de lectura en una base de datos existente (los elementos entre corchetes angulares deben sustituirse por los valores correctos en función del entorno y sin incluir dichos corchetes):

```powershell
Set-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Disabled
```

Para deshabilitar el escalado horizontal de lectura en una nueva base de datos (los elementos entre corchetes angulares deben sustituirse por los valores correctos en función del entorno y sin incluir dichos corchetes):

```powershell
New-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Disabled -Edition Premium
```

Para volver a habilitar el escalado horizontal de lectura en una base de datos existente (los elementos entre corchetes angulares deben sustituirse por los valores correctos en función del entorno y sin incluir dichos corchetes):

```powershell
Set-AzSqlDatabase -ResourceGroupName <resourceGroupName> -ServerName <serverName> -DatabaseName <databaseName> -ReadScale Enabled
```

### <a name="rest-api"></a>API DE REST

Para crear una base de datos con el escalado horizontal de lectura deshabilitado o para cambiar la configuración de una base de datos existente, utilice el método siguiente con la propiedad `readScale` establecida en `Enabled` o `Disabled` como en la siguiente solicitud de ejemplo.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body: {
   "properties": {
      "readScale":"Disabled"
   }
}
```

Para más información, consulte [Databases - Create or Update](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).

## <a name="using-tempdb-on-read-only-replica"></a>Uso de TempDB en una réplica de solo lectura

La base de datos TempDB no se replica en las réplicas de solo lectura. Cada réplica tiene su propia versión de la base de datos TempDB, que se crea a la vez que lo hace la réplica. Esto garantiza que TempDB se puede actualizar y modificar durante la ejecución de consultas. Si la carga de trabajo de solo lectura depende del uso de objetos de TempDB, debe crear estos objetos como parte del script de la consulta.

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Uso del escalado horizontal de lectura con las bases de datos con replicación geográfica

Si usa el escalado horizontal de lectura para equilibrar las cargas de trabajo de solo lectura de una base de datos con replicación geográfica (por ejemplo, como miembro de un grupo de conmutación por error), asegúrese de que el escalado horizontal de lectura esté habilitado tanto en la base de datos principal como en las bases de datos secundarias con replicación geográfica. De este modo, se asegurará de que la experiencia de equilibrio de carga será la misma cuando la aplicación se conecte a la nueva base de datos principal después de la conmutación por error. Si se conecta a la base de datos secundaria con replicación geográfica con el escalado de lectura habilitado, las sesiones con `ApplicationIntent=ReadOnly` se enrutarán a una de las réplicas del mismo modo que se enrutan las conexiones en la base de datos principal.  Las sesiones sin `ApplicationIntent=ReadOnly` se enrutarán a la réplica principal de la secundaria con replicación geográfica, que también es de solo lectura. Dado que las bases de datos secundarias con replicación geográfica tienen un punto de conexión diferente al de la base de datos principal, históricamente no era necesario establecer `ApplicationIntent=ReadOnly` para acceder a la base de datos secundaria. Para garantizar la compatibilidad con versiones anteriores, la DMV de `sys.geo_replication_links` muestra `secondary_allow_connections=2` (se permite cualquier conexión de cliente).

> [!NOTE]
> No se permite round robin ni ningún otro enrutamiento de carga equilibrada entre las réplicas locales de la base de datos secundaria.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre la oferta de SQL Database de nivel Hiperescala, consulte el artículo sobre el [nivel de servicio Hiperescala](./sql-database-service-tier-hyperscale.md).
