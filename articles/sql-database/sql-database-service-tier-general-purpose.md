---
title: Nivel de servicio de uso general
description: Obtenga información sobre el nivel de servicio de uso general de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/07/2019
ms.openlocfilehash: 7c57755ae63f8af5a2a4faa4764bc6a9597e8c2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79228520"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>Nivel de servicio Uso general: Azure SQL Database

> [!NOTE]
> El nivel de servicio de uso general del modelo de compra basado en núcleo virtual se denomina el nivel de servicio estándar en el modelo de compra basado en DTU. Para ver una comparación entre el modelo de compra basado en núcleo virtual y el modelo de compra basado en DTU, consulte [Modelos de compra y recursos de Azure SQL Database](sql-database-purchase-models.md).

Azure SQL Database se basa en la arquitectura del motor de base de datos de SQL Server adaptada al entorno en la nube, con el fin de garantizar una disponibilidad del 99,99 % incluso en los casos de error de la infraestructura. Hay tres niveles de servicio que se usan en Azure SQL Database, cada uno con diferentes modelos de arquitectura. Estos niveles de servicio son:

- Uso general
- Crítico para la empresa
- Hiperescala

El modelo de arquitectura para el nivel de servicio de uso general se basa en la separación del proceso y el almacenamiento. Este modelo de arquitectura se basa en la alta disponibilidad y confiabilidad de Azure Blob Storage que replica de forma transparente los archivos de base de datos y garantiza que no se pierdan los datos si se produce algún error en la infraestructura subyacente.

En la siguiente imagen se muestran cuatro nodos en el modelo de arquitectura estándar con las capas separadas de proceso y almacenamiento.

![Separación de proceso y almacenamiento](media/sql-database-managed-instance/general-purpose-service-tier.png)

En el modelo de arquitectura para el nivel de servicio de uso general, hay dos capas:

- Una capa de proceso sin estado que ejecuta el proceso `sqlservr.exe` y que solo contiene datos en caché y transitorios (por ejemplo, caché de planes, grupo de búferes o grupo de almacenes de columnas). Este nodo sin estado de SQL Server lo opera Azure Service Fabric, que inicializa el proceso, controla el estado del nodo y realiza la conmutación por error en otro lugar si es necesario.
- Una capa de datos con estado con archivos de base de datos (.mdf o .ldf) que se almacenan en Azure Blob Storage. Azure Blob Storage garantiza que no se pierdan los datos de ningún registro que se encuentre en un archivo de base de datos. Azure Storage tiene redundancia o disponibilidad de datos integrada que garantiza la conservación de los registros de una página o archivo de registro aunque se bloquee el proceso de SQL Server.

Siempre que se actualice un sistema operativo o un motor de base de dato, si se produce un error en la infraestructura subyacente o, si se detecta algún problema crítico en el proceso de SQL Server, Azure Service Fabric moverá el proceso sin estado de SQL Server a otro nodo de proceso sin estado. Hay un conjunto de nodos libres que esperan ejecutar un nuevo servicio de proceso si se produce una conmutación por error del nodo principal, a fin de minimizar el tiempo de conmutación por error. Esto no afecta a los datos de la capa de Azure Storage y los archivos de registro o de datos se anexan al proceso de SQL Server inicializado recientemente. Este proceso garantiza una disponibilidad del 99,99 %, pero podría afectar al rendimiento de grandes cargas de trabajo que se ejecutan debido al tiempo de transición y al hecho de que el nuevo nodo de SQL Server se inicia con la caché inactiva.

## <a name="when-to-choose-this-service-tier"></a>¿Cuándo elegir este nivel de servicio?

El nivel de servicio Uso general es un nivel de servicio predeterminado e Azure SQL Database diseñado para la mayoría de las cargas de trabajo genéricas. Si necesita un motor de base de datos totalmente administrado con un Acuerdo de Nivel de Servicio de un 99,99 % de latencia de almacenamiento de entre 5 y 10 ms que coincida con IaaS de Azure SQL en la mayoría de los casos, el nivel de uso general es la opción más conveniente.

## <a name="next-steps"></a>Pasos siguientes

- Encontrar características de recursos (número de núcleos, E/S, memoria) del nivel estándar o de uso general en la [instancia administrada](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), la base de datos Single en el [modelo de núcleos virtuales](sql-database-vcore-resource-limits-single-databases.md#general-purpose---provisioned-compute---gen4) o el [modelo DTU](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes), o el grupo elástico en el [modelo de núcleos virtuales](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose---provisioned-compute---gen4) y el [modelo DTU](sql-database-dtu-resource-limits-elastic-pools.md#standard-elastic-pool-limits).
- Obtenga información sobre los niveles [Crítico para la empresa](sql-database-service-tier-business-critical.md) e [Hiperescala](sql-database-service-tier-hyperscale.md).
- Obtenga información sobre [Service Fabric](../service-fabric/service-fabric-overview.md).
- Para conocer más opciones de alta disponibilidad y recuperación ante desastres, consulte [Continuidad del negocio](sql-database-business-continuity.md).
