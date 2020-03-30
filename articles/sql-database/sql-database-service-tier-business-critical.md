---
title: Nivel de servicio Crítico para la empresa
description: Obtenga información sobre el nivel de servicio Crítico para la empresa de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: fc328c34c1543a75fdc885087d44b28e24c0850a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79232456"
---
# <a name="business-critical-tier---azure-sql-database"></a>Nivel de servicio Crítico para la empresa: Azure SQL Database

> [!NOTE]
> El nivel Crítico para la empresa se denomina Premium en el modelo de compra basado en la unidad de transacción de base de datos. Para ver una comparación entre el modelo de compra basado en núcleo virtual y el modelo de compra basado en DTU, consulte [Modelos de compra y recursos de Azure SQL Database](sql-database-purchase-models.md).

Azure SQL Database se basa en la arquitectura del motor de base de datos de SQL Server que se ajusta al entorno en la nube, con el fin de garantizar una disponibilidad del 99,99 % incluso en los casos de error de la infraestructura. Hay tres modelos de arquitectura que se usan en Azure SQL Database:
- De uso general/Estándar 
- Crítico para la empresa/Premium
- Hiperescala

El modelo de nivel de servicio Crítico para la empresa/Premium se basa en un clúster de procesos del motor de base de datos. Este modelo de arquitectura se basa en el hecho de que siempre hay un cuórum de nodos de motor de base de datos disponibles, y tiene un impacto mínimo en el rendimiento de su carga de trabajo, incluso durante las actividades de mantenimiento.

Azure actualiza y revisa los sistemas operativos subyacentes, los controladores y el Motor de base de datos de SQL Server de forma transparente con el mínimo tiempo de inactividad para los usuarios finales. 

La disponibilidad Premium está habilitada en los niveles Premium y Crítico para la empresa, y se ha diseñado para cargas de trabajo intensivas que no pueden tolerar ningún impacto en el rendimiento debido a operaciones de mantenimiento continuado.

En el modelo Premium, Azure SQL Database integra el proceso y el almacenamiento en un único nodo. La alta disponibilidad en este modelo de arquitectura se logra mediante la replicación del proceso (proceso del motor de base de datos de SQL Server) y el almacenamiento (SSD conectado localmente) implementados en un clúster de cuatro nodos, con una tecnología parecida a la de los [Grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) de SQL Server.

![Clúster de nodos del motor de base de datos](media/sql-database-managed-instance/business-critical-service-tier.png)

El proceso del motor de base de datos SQL y los archivos mdf o ldf subyacentes se colocan en el mismo nodo con el almacenamiento SSD conectado localmente, lo que proporciona baja latencia para la carga de trabajo. La alta disponibilidad se implementa mediante tecnología parecida a la de los [grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) de SQL Server. Cada base de datos es un clúster de nodos de base de datos con una base de datos principal, a la que se puede acceder para la carga de trabajo de cliente, y a tres procesos secundarios que contienen copias de los datos. El nodo principal inserta constantemente los cambios a los nodos secundarios para garantizar que los datos estén disponibles en réplicas secundarias si el nodo principal se bloquea por cualquier motivo. El Motor de base de datos de SQL Server controla la conmutación por error: una réplica secundaria se convierte en el nodo principal y se crea una nueva réplica secundaria para garantizar que hay suficientes nodos en el clúster. La carga de trabajo se redirige automáticamente al nuevo nodo principal.

Además, el clúster Crítico para la empresa incorpora la funcionalidad [Escalado horizontal de lectura](sql-database-read-scale-out.md) que proporciona un nodo de solo lectura integrado gratuito que se puede usar para ejecutar consultas de solo lectura (por ejemplo, informes) que no deberían afectar al rendimiento de la carga de trabajo principal.

## <a name="when-to-choose-this-service-tier"></a>¿Cuándo elegir este nivel de servicio?

El nivel de servicio Crítico para la empresa está diseñado para las aplicaciones que requieren respuestas de baja latencia desde el almacenamiento SSD subyacente (1 o 2 ms como promedio), recuperación rápida si se produce un error en la infraestructura subyacente o necesidad de descargar informes, análisis y consultas de solo lectura en la versión gratuita de la réplica secundaria legible de la base de datos principal.

A continuación, se indican las principales razones por las que debe elegir el nivel de servicio Crítico para la empresa en lugar del nivel De uso general:
-   Requisitos de latencia baja de E/S: la carga de trabajo que necesita la respuesta rápida de la capa de almacenamiento (promedio de 1 a 2 milisegundos) debe usar el nivel Crítico para la empresa. 
-   Comunicación frecuente entre la aplicación y la base de datos. La aplicación que no puede aprovechar el almacenamiento en caché de nivel de aplicación o el [procesamiento por lotes de solicitudes](sql-database-use-batching-to-improve-performance.md) y necesita enviar muchas consultas SQL que se deben procesar rápidamente son buenos candidatos para el nivel Crítico para la empresa.
-   Gran número de actualizaciones: las operaciones de inserción, actualización y eliminación modifican las páginas de datos en memoria (página desfasada) que deben guardarse en archivos de datos con la operación `CHECKPOINT`. Un posible bloqueo del proceso del motor de base de datos o una conmutación por error de la base de datos con un gran número de páginas desfasadas podría aumentar el tiempo de recuperación en el nivel De uso general. Use el nivel Crítico para la empresa si tiene una carga de trabajo que produce muchos cambios en memoria. 
-   Transacciones de larga duración que modifican los datos. Las transacciones que se abren durante más tiempo impiden el truncamiento del archivo de registro que podría aumentar el tamaño del registro y el número de [archivos de registro virtuales (VLF)](https://docs.microsoft.com/sql/relational-databases/sql-server-transaction-log-architecture-and-management-guide#physical_arch). Un gran número de VLF puede ralentizar la recuperación de la base de datos después de la conmutación por error.
-   Carga de trabajo con informes y consultas analíticas que se pueden redirigir a la réplica secundaria gratuita de solo lectura.
- Mayor resistencia y recuperación más rápida de los errores. En caso de error del sistema, se deshabilitará la base de datos en la instancia principal y una de las réplicas secundarias se convertirá inmediatamente en la nueva base de datos principal de lectura y escritura lista para procesar las consultas. El motor de base de datos no necesita analizar ni rehacer las transacciones del archivo de registro, así como tampoco cargar todos los datos en el búfer de memoria.
- Protección avanzada de datos dañados: el nivel Crítico para la empresa aprovecha las réplicas de bases de datos en segundo plano para fines de continuidad empresarial, por lo que el servicio también aprovecha la reparación automática de páginas, que es la misma tecnología que se usa para los [grupos de disponibilidad y la creación de reflejo](https://docs.microsoft.com/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring) de la base de datos de SQL Server. En caso de que una réplica no pueda leer una página debido a un problema de integridad de datos, se recuperará una copia nueva de la página de otra réplica, reemplazando la página ilegible sin pérdida de datos ni tiempo de inactividad del cliente. Esta funcionalidad es aplicable en el nivel De uso general si la base de datos tiene una réplica geográfica secundaria.
- Mayor disponibilidad: un nivel Crítico para la empresa en la configuración de AZ múltiple garantiza una disponibilidad del 99,995 %, en comparación con el 99,99 % del nivel De uso general.
- Recuperación geográfica rápida: el nivel Crítico para la empresa configurado con replicación geográfica tiene un objetivo de punto de recuperación (RPO) garantizado de cinco segundos y un objetivo de tiempo de recuperación (RTO) de 30 segundos para el 100 % de las horas implementadas.

## <a name="next-steps"></a>Pasos siguientes

- Encontrar características de recursos (número de núcleos, E/S, memoria) del nivel crítico para la empresa en la [instancia administrada](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), la base de datos única en el [modelo de núcleos virtuales](sql-database-vcore-resource-limits-single-databases.md#business-critical---provisioned-compute---gen4) o el [modelo DTU](sql-database-dtu-resource-limits-single-databases.md#premium-service-tier), o en el grupo elástico en el [modelo de núcleos virtuales](sql-database-vcore-resource-limits-elastic-pools.md#business-critical---provisioned-compute---gen4) y el [modelo DTU](sql-database-dtu-resource-limits-elastic-pools.md#premium-elastic-pool-limits).
- Obtenga información sobre los niveles [De uso general](sql-database-service-tier-general-purpose.md) e [Hiperescala](sql-database-service-tier-hyperscale.md).
- Obtenga información sobre [Service Fabric](../service-fabric/service-fabric-overview.md).
- Para conocer más opciones de alta disponibilidad y recuperación ante desastres, consulte [Continuidad del negocio](sql-database-business-continuity.md).
