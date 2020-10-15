---
title: Serie de migración de Contoso | Microsoft Docs
description: Vínculos a ejemplos de escenarios de migración de Contoso para la migración a Azure.
ms.topic: conceptual
ms.date: 04/20/2020
ms.author: raynew
ms.openlocfilehash: f9f7b3b64cf91019ed4e40d5d52b859419b74836
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86107638"
---
# <a name="contoso-migration-series"></a>Serie de migración de Contoso


Hay una serie de artículos en los que mostramos cómo la organización ficticia Contoso migra su infraestructura local a la nube de [Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/). 

La serie incluye escenarios que ilustran cómo configurar una migración de infraestructura y ejecutar los diferentes tipos de migraciones. La complejidad de los escenarios va en aumento. En los artículos se muestra cómo la empresa Contoso controla la migración y se proporcionan instrucciones e indicadores generales.

## <a name="migration-articles"></a>Artículos sobre la migración

En la tabla siguiente se resumen los artículos de la serie.  

- A cada escenario de migración lo rigen requisitos empresariales ligeramente distintos que determinan la estrategia de migración.
- Para cada escenario de implementación, se proporciona información sobre las causas y objetivos empresariales, una arquitectura propuesta, pasos para realizar la migración y recomendaciones para la limpieza y los pasos siguientes una vez completada la migración.


**Artículo** | **Detalles** 
--- | --- 
[Artículo 1: Información general](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-overview) | Información general de la serie de artículos, la estrategia de migración de Contoso y las aplicaciones de ejemplo que se usan en esta serie. 
[Artículo 2: implementar una infraestructura de Azure](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-infrastructure) | Contoso prepara la infraestructura local y la de Azure para la migración. Se usa la misma infraestructura en todos los artículos de la serie. 
[Artículo 3: Valoración de los recursos locales para migrarse a Azure](/azure/cloud-adoption-framework/migrate/azure-migration-guide/assess?tabs=Tools)  | Contoso ejecuta una valoración de su aplicación local SmartHotel360 que se ejecuta en VMware. Contoso evalúa las máquinas virtuales de la aplicación mediante el servicio Azure Migrate, y la base de datos SQL Server de la aplicación con Data Migration Assistant.
[Artículo 4: Rehospedaje de una aplicación en una máquina virtual de Azure e Instancia administrada de Azure SQL](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-managed-instance) | Contoso ejecuta una migración mediante lift-and-shift a Azure para su aplicación SmartHotel360 local. Contoso migra la máquina virtual de front-end de la aplicación mediante [Azure Migrate](./migrate-services-overview.md). Contoso migra la base de datos de la aplicación a una Instancia administrada de SQL usando [Azure Database Migration Service](../dms/dms-overview.md).
[Artículo 5: Rehospedaje de una aplicación en máquinas virtuales de Azure](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm) | Contoso migra sus máquinas virtuales de la aplicación SmartHotel360 a máquinas virtuales de Azure mediante el servicio Azure Migrate. 
[Artículo 6: Rehospedaje de una aplicación en máquinas virtuales de Azure y en el grupo de disponibilidad Always On de SQL Server](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-vm-sql-ag) | Contoso migra la aplicación SmartHotel360. Contoso usa Azure Migrate para migrar las máquinas virtuales de la aplicación. Usa Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server que está protegido por un grupo de disponibilidad Always On. 
[Artículo 7: Rehospedaje de una aplicación de Linux en máquinas virtuales de Azure](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm) | Contoso realiza una migración "lift-and-shift" de su aplicación osTicket de Linux a máquinas virtuales de Azure mediante Azure Migrate.
[Artículo 8: Rehospedaje de una aplicación de Linux en máquinas virtuales de Azure y Azure Database for MySQL](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rehost-linux-vm-mysql) | Contoso migra su aplicación osTicket de Linux a máquinas virtuales de Azure mediante Azure Migrate. Después, migra la base de datos de la aplicación a Azure Database for MySQ mediante Azure Database Migration Service (incluye una opción alternativa mediante MySQL Workbench).
[Artículo 9: refactorizar una aplicación local a una aplicación web de Azure y una base de datos de Azure SQL](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql) | Contoso migra su aplicación SmartHotel360 a una aplicación web de Azure y migra la base de datos de la aplicación a Azure SQL Database por medio de Azure Database Migration Service.
[Artículo 10: Refactorización de una aplicación de Windows con los servicios de Azure App Service e Instancia administrada de SQL](/azure/cloud-adoption-framework/migrate/azure-best-practices/contoso-migration-refactor-web-app-sql-managed-instance) | Contoso migra una aplicación local basada en Windows a una aplicación web de Azure y migra la base de datos de la aplicación a una instancia administrada de Azure SQL con Azure Database Migration Service.
[Artículo 11: Refactorización de una aplicación de Linux en una aplicación web de Azure y en Azure Database for MySQL](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-refactor-linux-app-service-mysql) | Contoso migra su aplicación osTicket de Linux a una aplicación web de Azure en varias regiones de Azure mediante Azure Traffic Manager, integrado con GitHub para la entrega continua. Contoso migra la base de datos de la aplicación a una instancia de Azure Database for MySQL. 
[Artículo 12: Refactorización de Team Foundation Server en Azure DevOps Services](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-tfs-vsts) | Contoso migra su implementación local de Team Foundation Server a Azure DevOps Services en Azure.
[Artículo 13: Recompilación de una aplicación en Azure](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-rebuild) | Contoso recompila su aplicación SmartHotel mediante una serie de funcionalidades y servicios de Azure, como Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services y Azure Cosmos DB.
[Artículo 14: Escalado de una migración en Azure](/azure/architecture/cloud-adoption/migrate/azure-best-practices/contoso-migration-scale) | Después de probar combinaciones de migración, Contoso se prepara para escalar una migración completa a Azure.



## <a name="next-steps"></a>Pasos siguientes

- [Más información](/azure/architecture/cloud-adoption/migrate/) sobre la migración a la nube.
- Obtenga información sobre las estrategias de migración para otros escenarios (pares de origen/destino) en la [Guía de Database Migration](https://datamigration.microsoft.com/).
