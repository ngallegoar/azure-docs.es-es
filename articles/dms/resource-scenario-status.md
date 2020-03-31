---
title: Estado del escenario de migración de bases de datos
titleSuffix: Azure Database Migration Service
description: Obtenga información sobre el estado de los escenarios de migración que admite Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/05/2019
ms.openlocfilehash: 9652b78674d6a6b905eb049564d1b17cdc7c17b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254926"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Estado de los escenarios de migración que admite Azure Database Migration Service

Azure Database Migration Service está diseñado para admitir una serie de escenarios de migración (pares de origen/destino) para las migraciones sin conexión (de un solo uso) y en línea (sincronización continua). Azure Database Migration Service proporciona una cobertura de escenarios que se amplía constantemente. Nos encargamos de agregar nuevos escenarios de forma regular. En este artículo se identifican los escenarios de migración que actualmente son compatibles con Azure Database Migration Service y el estado (versión preliminar privada, versión preliminar pública o disponibilidad general) de cada escenario.

## <a name="offline-versus-online-migrations"></a>Migraciones sin conexión o en línea

Con Azure Database Migration Service, puede realizar una migración sin conexión o en línea. Si usa las migraciones *sin conexión*, el tiempo de inactividad de la aplicación comienza al mismo tiempo que la migración. Para limitar el tiempo de inactividad al tiempo necesario para la transición al nuevo entorno cuando finalice la migración, use una migración *en línea*. Es recomendable probar una migración sin conexión para determinar si el tiempo de inactividad es aceptable; si no fuera así, realice una migración en línea.

## <a name="migration-scenario-status"></a>Estado del escenario de migración

El estado de los escenarios de migración que admite Azure Database Migration Service varía con el tiempo. Por lo general, los escenarios se lanzan primero en **versión preliminar privada**. La participación en la versión preliminar privada requiere que los clientes envíen una propuesta a través de la [sitio de versión preliminar de DMS](https://aka.ms/dms-preview). Después de la versión preliminar privada, el estado del escenario cambia a **versión preliminar pública**. Los usuarios de Azure Database Migration Service pueden probar escenarios de migración en la versión preliminar pública directamente desde la interfaz de usuario. No es preciso registrarse.  Sin embargo, los escenarios de migración en versión preliminar pública podrían no estar disponibles en todas las regiones y podrían sufrir cambios adicionales antes del lanzamiento de la versión final. Después de la versión preliminar pública, el estado del escenario cambia a **disponibilidad general**. Disponibilidad general (GA) es el estado de la versión final, y la funcionalidad es completa y accesible para todos los usuarios.

## <a name="migration-scenario-support"></a>Compatibilidad del escenario de migración

En las siguientes tablas se muestran qué escenarios de migración se admiten cuando se usa Azure Database Migration Service.

> [!NOTE]
> Si un escenario que se indica como admitido no aparece en la interfaz de usuario, póngase en contacto con el alias para [preguntar a Azure Database Migration Service](mailto:AskAzureDatabaseMigrations@service.microsoft.com) para obtener información adicional.

> [!IMPORTANT]
> Para ver todos los escenarios compatibles actualmente con Azure Database Migration Service en la versión preliminar privada, consulte el [sitio de versión preliminar de DMS](https://aka.ms/dms-preview).

### <a name="offline-one-time-migration-support"></a>Compatibilidad de la migración sin conexión (de un solo uso)

En la siguiente tabla se muestra la compatibilidad de Azure Database Migration Service con las migraciones sin conexión.

| Destino  | Source | Soporte técnico | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | SQL de RDS |  |  |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | SQL de RDS |  |  |
|   | Oracle |  |   |
| **Máquina virtual de Azure SQL** | SQL Server | ✔ | GA |
|   | Oracle |   |   |
| **Azure Cosmos DB** | MongoDB | ✔ | GA |
| **Azure DB para MySQL** | MySQL |   |   |
|   | MySQL de RDS |   |   |
| **Azure DB para PostgreSQL** | PostgreSQL |  |
|  | PostgreSQL de RDS |   |   |

### <a name="online-continuous-sync-migration-support"></a>Compatibilidad con la migración en línea (sincronización continua)

En la siguiente tabla se muestra la compatibilidad de Azure Database Migration Service con las migraciones en línea.

| Destino  | Source | Soporte técnico | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | GA |
|   | SQL de RDS | ✔ | GA |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | GA |
|   | SQL de RDS | ✔ | GA |
|   | Oracle | ✔ | Versión preliminar privada |
| **Máquina virtual de Azure SQL** | SQL Server |   |   |
|   | Oracle  |  |  |
| **Azure Cosmos DB** | MongoDB | ✔ | GA |
| **Azure DB para MySQL** | MySQL | ✔ | GA |
|   | MySQL de RDS | ✔ | GA |
| **Azure DB para PostgreSQL** | PostgreSQL | ✔ | GA |
|   | PostgreSQL de RDS | ✔ | GA |
|   | Oracle | ✔ | Versión preliminar pública |

## <a name="next-steps"></a>Pasos siguientes

Para información general sobre Azure Database Migration Service y la disponibilidad regional, consulte el artículo [¿Qué es Azure Database Migration Service?](dms-overview.md)
