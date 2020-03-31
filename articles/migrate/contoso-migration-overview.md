---
title: Serie de migración de Contoso | Microsoft Docs
description: Se proporciona una introducción a la estrategia y los escenarios de migración que Contoso usa para migrar su centro de datos local a Azure.
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: raynew
ms.openlocfilehash: d37bee589eb7ee2e6e30c8dcea2531dd1f063481
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78939192"
---
# <a name="contoso-migration-series"></a>Serie de migración de Contoso


Hay una serie de artículos en los que mostramos cómo la organización ficticia Contoso migra una infraestructura local a la nube de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/). 

La serie incluye información y escenarios que ilustran cómo configurar una infraestructura de migración y ejecutar los diferentes tipos de migraciones. La complejidad de los escenarios va en aumento. En los artículos se muestra cómo la empresa Contoso completa su misión de migración, y también se proporcionan consejos de lectura general e instrucciones específicas a lo largo de la serie.

## <a name="migration-articles"></a>Artículos sobre la migración

En la tabla siguiente se resumen los artículos de la serie.  

- A cada escenario de migración lo impulsan objetivos empresariales ligeramente distintos que determinan la estrategia de migración.
- Para cada escenario de implementación, se proporciona información sobre los impulsores y objetivos empresariales, una arquitectura propuesta, pasos para realizar la migración y una recomendación para la limpieza y los pasos siguientes una vez completada la migración.

**Artículo** | **Detalles** 
--- | --- 
[Artículo 1: Información general](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Información general de la serie de artículos, la estrategia de migración de Contoso y las aplicaciones de ejemplo que se usan en esta serie. 
[Artículo 2: implementar una infraestructura de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso prepara la infraestructura local y la infraestructura de Azure para la migración. Se usa la misma infraestructura en todos los artículos de la serie sobre migración. 
[Artículo 3: Valoración de los recursos locales para migrarse a Azure](https://docs.microsoft.com/azure/cloud-adoption-framework/migrate/azure-migration-guide/assess?tabs=Tools)  | Contoso ejecuta una valoración de su aplicación local SmartHotel360 que se ejecuta en VMware. Contoso evalúa las máquinas virtuales de la aplicación mediante el servicio Azure Migrate, y la base de datos SQL Server de la aplicación con Data Migration Assistant.
[Artículo 4: rehospedaje de una aplicación en una máquina virtual de Azure e Instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso ejecuta una migración mediante lift-and-shift a Azure para su aplicación SmartHotel360 local. Contoso migra la máquina virtual de front-end de la aplicación mediante [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migra la base de datos de la aplicación a una instancia administrada de Azure SQL Database mediante [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview).
[Artículo 5: Rehospedaje de una aplicación en máquinas virtuales de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso migra sus máquinas virtuales de la aplicación SmartHotel360 a máquinas virtuales de Azure mediante el servicio Site Recovery. 
[Artículo 6: Rehospedaje de una aplicación en máquinas virtuales de Azure y en el grupo de disponibilidad Always On de SQL Server](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) |Contoso migra la aplicación SmartHotel360. Contoso usa Site Recovery para migrar las máquinas virtuales de la aplicación. Usa Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server que está protegido por un grupo de disponibilidad Always On. 
[Artículo 7: Rehospedaje de una aplicación de Linux en máquinas virtuales de Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso realiza una migración mediante "lift-and-shift" de su aplicación osTicket de Linux a máquinas virtuales de Azure mediante el servicio Site Recovery.
[Artículo 8: Rehospedaje de una aplicación de Linux en máquinas virtuales de Azure y Azure Database for MySQL](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso migra su aplicación osTicket de Linux a máquinas virtuales de Azure mediante Site Recovery. Migra la base de datos de la aplicación a Azure Database for MySQL con MySQL Workbench. 
[Artículo 9: refactorizar una aplicación local a una aplicación web de Azure y una base de datos de Azure SQL](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso migra su aplicación SmartHotel360 a una aplicación web de Azure, y migra la base de datos de la aplicación a una instancia de Azure SQL Server con Database Migration Assistant.     
[Artículo 10: Refactorización de una aplicación de Linux en una aplicación web de Azure y en Azure Database for MySQL](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso migra su aplicación osTicket de Linux a una aplicación web de Azure en varias regiones de Azure con Azure Traffic Manager, integrado con GitHub para la entrega continua. Contoso migra la base de datos de la aplicación a una instancia de Azure Database for MySQL. 
[Artículo 11: Refactorización de Team Foundation Server en Azure DevOps Services](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso migra su implementación local de Team Foundation Server a Azure DevOps Services en Azure.
[Artículo 12: rediseñar la arquitectura de una aplicación local en un contenedor de Azure y Azure SQL Database](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rearchitect-container-sql) | Contoso migra su aplicación SmartHotel a Azure. A continuación, rediseña el nivel de aplicación web como un contenedor de Windows que se ejecuta en Azure Service Fabric, y la base de datos con Azure SQL Database.
[Artículo 13: Recompilación de una aplicación en Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso recompila su aplicación SmartHotel mediante una serie de funcionalidades y servicios de Azure, como Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services y Azure Cosmos DB.
[Artículo 14: Escalado de una migración en Azure](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Después de probar combinaciones de migración, Contoso se prepara para escalar una migración completa a Azure.

## <a name="next-steps"></a>Pasos siguientes

- [Más información](https://docs.microsoft.com/azure/architecture/cloud-adoption/migrate/) sobre la migración a la nube.
- Obtenga información sobre las estrategias de migración para otros escenarios (pares de origen/destino) en la [Guía de Database Migration](https://datamigration.microsoft.com/).
