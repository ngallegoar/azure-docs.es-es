---
title: Eventos extendidos
description: Describe los eventos extendidos (XEvents) en Azure SQL Database y cómo las sesiones de eventos difieren ligeramente de las sesiones de eventos en Microsoft SQL Server.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: jrasnik
ms.date: 12/19/2018
ms.openlocfilehash: cb4eb4474ad074a3e69dc146c97b48d54343595b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79213961"
---
# <a name="extended-events-in-sql-database"></a>Eventos extendidos en SQL Database
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

En este tema se explica cómo la implementación de eventos extendidos en Azure SQL Database es ligeramente diferente en comparación con los eventos extendidos de Microsoft SQL Server.

- La versión 12 de SQL Database adquirió la característica de eventos extendidos en la segunda mitad del calendario de 2015.
- SQL Server tiene eventos extendidos desde 2008.
- El conjunto de características de eventos extendidos en SQL Database es un subconjunto sólido de las características en SQL Server.

*XEvents* es un sobrenombre informal que a veces se usa para los eventos extendidos en blogs y otras referencias informales.

Se puede encontrar información adicional sobre eventos extendidos, para Azure SQL Database y Microsoft SQL Server, en:

- [Quick Start: Extended events in SQL Server](https://msdn.microsoft.com/library/mt733217.aspx)
- [Eventos extendidos](https://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a>Prerequisites

En este tema se da por sentado que ya tiene algunos conocimientos sobre:

- [Servicio Azure SQL Database](https://azure.microsoft.com/services/sql-database/).
- [Eventos extendidos](https://msdn.microsoft.com/library/bb630282.aspx) en Microsoft SQL Server.

- La mayor parte de nuestra documentación acerca de los eventos extendidos se aplica tanto a SQL Server como a SQL Database.

Exposición anterior a los elementos siguientes es útil cuando se elige el archivo de eventos como [destino](#AzureXEventsTargets):

- [Servicio Azure Storage](https://azure.microsoft.com/services/storage/)


- PowerShell
    - [Usar Azure PowerShell con Azure Storage](../storage/common/storage-powershell-guide-full.md) proporciona información completa sobre PowerShell y el servicio Azure Storage.

## <a name="code-samples"></a>Ejemplos de código

Los temas relacionados proporcionan dos ejemplos de código:


- [Código de destino de búfer en anillo para eventos extendidos en SQL Database](sql-database-xevent-code-ring-buffer.md)
    - Breve script de Transact-SQL simple.
    - En el tema de ejemplo de código, hacemos hincapié en que, cuando haya terminado con un destino de búfer en anillo, debe liberar sus recursos mediante la ejecución de una instrucción alter-drop `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` . Más adelante, puede agregar otra instancia de búfer en anillo de `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Código de destino del archivo de evento para eventos extendidos en SQL Database](sql-database-xevent-code-event-file.md)
    - Fase 1: es PowerShell para crear un contenedor de Azure Storage.
    - Fase 2: es Transact-SQL que usa el contenedor de Azure Storage

## <a name="transact-sql-differences"></a>Diferencias de Transact-SQL


- Cuando se ejecuta el comando [CREATE EVENT SESSION](https://msdn.microsoft.com/library/bb677289.aspx) en SQL Server, se usa la cláusula **ON SERVER** . Pero en SQL Database se usa la cláusula **ON DATABASE** en su lugar.


- La cláusula **ON DATABASE** se aplica también a los comandos Transact-SQL [ALTER EVENT SESSION](https://msdn.microsoft.com/library/bb630368.aspx) y [DROP EVENT SESSION](https://msdn.microsoft.com/library/bb630257.aspx).


- Un procedimiento recomendado es incluir la opción de sesión de eventos de **STARTUP_STATE = ON** en sus instrucciones **CREATE EVENT SESSION** o **ALTER EVENT SESSION**.
    - El valor **= ON** admite un reinicio automático después de una reconfiguración de la base de datos lógica debida a una conmutación por error.

## <a name="new-catalog-views"></a>Nuevas vistas de catálogo

La característica eventos extendidos es compatible con varias [vistas de catálogo](https://msdn.microsoft.com/library/ms174365.aspx). Las vistas de catálogo le informan sobre *metadatos o definiciones* de sesiones de eventos creadas por el usuario en la base de datos actual. Las vistas no devuelven información acerca de las instancias de sesiones de eventos activas.

| Nombre de<br/>vista de catálogo | Descripción |
|:--- |:--- |
| **sys.database_event_session_actions** |Devuelve una fila por cada acción en cada evento de una sesión de eventos. |
| **sys.database_event_session_events** |Devuelve una fila por cada evento de una sesión de eventos. |
| **sys.database_event_session_fields** |Devuelve una fila por cada columna personalizable que se estableció de forma explícita en los eventos y destinos. |
| **sys.database_event_session_targets** |Devuelve una fila para cada destino de evento de una sesión de eventos. |
| **sys.database_event_sessions** |Devuelve una fila por cada sesión de eventos en la base de datos de SQL Database. |

En Microsoft SQL Server, hay vistas de catálogo similares con nombres que incluyen *.server\_* en lugar de *.database\_* . El patrón de nombre es parecido a **sys.server_event_%** .

## <a name="new-dynamic-management-views-dmvs"></a>Nuevas vistas de administración dinámica [(DMV)](https://msdn.microsoft.com/library/ms188754.aspx)

Azure SQL Database tiene [vistas de administración dinámica (DMV)](https://msdn.microsoft.com/library/bb677293.aspx) que admiten eventos extendidos. Las DMV le informan sobre las sesiones de eventos *activas* .

| Nombre de DMV | Descripción |
|:--- |:--- |
| **sys.dm_xe_database_session_event_actions** |Devuelve información sobre las acciones de la sesión de eventos. |
| **sys.dm_xe_database_session_events** |Devuelve información sobre los eventos de la sesión. |
| **sys.dm_xe_database_session_object_columns** |Muestra los valores de configuración de los objetos enlazados a una sesión. |
| **sys.dm_xe_database_session_targets** |Devuelve información acerca de los destinos de la sesión. |
| **sys.dm_xe_database_sessions** |Devuelve una fila para cada sesión de eventos del ámbito de la base de datos actual. |

En Microsoft SQL Server, las vistas de catálogo similares tienen nombres sin la parte *\_database* del nombre, como:

- **sys.dm_xe_sessions**, en vez del nombre<br/>**sys.dm_xe_database_sessions**.

### <a name="dmvs-common-to-both"></a>DMV comunes
Para eventos extendidos hay DMV adicionales que son comunes a Azure SQL Database y Microsoft SQL Server:

- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**

  <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-the-available-extended-events-actions-and-targets"></a>Búsqueda de los eventos extendidos, acciones y destinos disponibles

Puede ejecutar una sencilla instruccióon SQL **SELECT** para obtener una lista de los eventos, acciones y destinos disponibles.

```sql
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```


<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Destinos para las sesiones de eventos de SQL Database

Estos son los destinos que pueden capturar los resultados de las sesiones de eventos en SQL Database:

- [Destino de búfer de anillo](https://msdn.microsoft.com/library/ff878182.aspx) : guarda brevemente los datos en la memoria.
- [Destino del contador de eventos de](https://msdn.microsoft.com/library/ff878025.aspx) :cuenta todos los eventos que se producen durante una sesión de eventos extendidos.
- [Destino de archivo de evento](https://msdn.microsoft.com/library/ff878115.aspx) : escribe búferes completos en un contenedor de Azure Storage.

La API [Seguimiento de eventos para Windows (ETW)](https://msdn.microsoft.com/library/ms751538.aspx) no está disponible para eventos extendidos en SQL Database.

## <a name="restrictions"></a>Restricciones

Hay un par de diferencias relacionadas con la seguridad que se adaptan al entorno de nube de SQL Database:

- Los eventos extendidos se basan en el modelo de aislamiento de inquilino único. Una sesión de eventos en una base de datos no puede tener acceso a datos o eventos desde otra base de datos.
- No se puede emitir una instrucción **CREATE EVENT SESSION** en el contexto de la base de datos **maestra**.

## <a name="permission-model"></a>Nombre del permiso

Debe tener permiso de **Control** en la base de datos para emitir una instrucción **CREATE EVENT SESSION**. El propietario de la base de datos (dbo) tiene permiso de **Control** .

### <a name="storage-container-authorizations"></a>Autorizaciones de contenedor de almacenamiento

El token SAS genere para el contenedor de Azure Storage debe especificar **rwl** para los permisos. El valor **rwl** proporciona los siguientes permisos:

- Lectura
- Escritura
- List

## <a name="performance-considerations"></a>Consideraciones de rendimiento

Existen escenarios donde un uso intensivo de eventos extendidos puede acumular más memoria activa de la que sería conveniente para el buen estado de todo el sistema. Por ello, el sistema Azure SQL Database establece y ajusta de forma dinámica los límites en la cantidad de memoria activa que puede acumularse en una sesión de eventos. En el cálculo dinámico se incluyen muchos factores.

Si recibe un mensaje de error que indica que se aplicó un máximo de memoria, algunas acciones correctivas que puede tomar son:

- Ejecutar menos sesiones de eventos simultáneas.
- A través de sus instrucciones **CREATE** y **ALTER** para las sesiones de eventos, reducir la cantidad de memoria que especifica en la cláusula **MAX\_MEMORY**.

### <a name="network-latency"></a>Latencia de red

El destino del **archivo de eventos** puede experimentar latencia de red o errores al almacenar datos en los blobs de Azure Storage. Otros eventos en SQL Database podrían retrasarse mientras esperan a que se complete la comunicación de red. Este retraso puede reducir la carga de trabajo.

- Para mitigar este riesgo de rendimiento, evite establecer la opción **EVENT_RETENTION_MODE** en **NO_EVENT_LOSS** en las definiciones de la sesión de eventos.

## <a name="related-links"></a>Vínculos relacionados

- [Usar Azure PowerShell con Azure Storage](../storage/common/storage-powershell-guide-full.md)
- [Cmdlets de Azure Storage](https://docs.microsoft.com/powershell/module/Azure.Storage)
- [Usar Azure PowerShell con Azure Storage](../storage/common/storage-powershell-guide-full.md) proporciona información completa sobre PowerShell y el servicio Azure Storage.
- [Uso del almacenamiento de blobs de .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [CREATE CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/ms189522.aspx)
- [CREATE EVENT SESSION (Transact-SQL)](https://msdn.microsoft.com/library/bb677289.aspx)
- [Las publicaciones del blog de Jonathan Kehayias acerca de los eventos extendidos en Microsoft SQL Server](https://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- La página web de *actualizaciones del servicio* de Azure, restringida por parámetros a Azure SQL Database:
    - [https://azure.microsoft.com/updates/?service=sql-database](https://azure.microsoft.com/updates/?service=sql-database)


Hay otros temas de ejemplo de código para eventos extendidos disponibles en los siguientes vínculos. De todas formas, debe comprobar siempre cualquier ejemplo para ver si está destinado a Microsoft SQL Server frente a Azure SQL Database. A continuación, puede decidir si es necesario algún pequeño cambio para ejecutar el ejemplo.

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](https://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](https://msdn.microsoft.com/library/bb630355.aspx)
-->
