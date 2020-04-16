---
title: Migración de SSIS con Instancia administrada de Azure SQL Database como destino de la carga de trabajo de base de datos
description: Migración de SSIS con Instancia administrada de Azure SQL Database como destino de la carga de trabajo de base de datos.
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 9/12/2019
ms.openlocfilehash: 2e35e4eb750aa2244df920111b201d886599eaf6
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81419058"
---
# <a name="ssis-migration-with-azure-sql-database-managed-instance-as-the-database-workload-destination"></a>Migración de SSIS con Instancia administrada de Azure SQL Database como destino de la carga de trabajo de base de datos

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Al migrar cargas de trabajo de base de datos de SQL Server local a Instancia administrada de Azure SQL Database, debe estar familiarizado con [Azure Data Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) (DMS ) y las [topologías de red para migraciones de Instancia administrada de Azure SQL Database con DMS](https://docs.microsoft.com/azure/dms/resource-network-topologies).

Este artículo se centra en la migración de los paquetes de SQL Server Integration Services (SSIS) almacenados en el catálogo de SSIS (SSISDB) y los trabajos del Agente SQL Server que programan las ejecuciones de paquetes de SSIS.

## <a name="migrate-ssis-catalog-ssisdb"></a>Migración del catálogo de SSIS (SSISDB)

La migración de SSISDB se puede realizar con DMS, tal como se describe en el artículo: [Migración de paquetes de SSIS a una instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance).

## <a name="ssis-jobs-to-azure-sql-database-managed-instance-agent"></a>Migración de trabajos de SSIS al agente de Instancia administrada de Azure SQL Database

Instancia administrada de Azure SQL Database tiene un programador nativo de primera clase como el Agente SQL Server local.  Puesto que aún no hay disponible una herramienta de migración para los trabajos de SSIS, se deben migrar del Agente SQL Server local al agente de Instancia administrada de Azure SQL Database mediante scripts o copia manual.

## <a name="additional-resources"></a>Recursos adicionales

- [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction)
- [Azure-SSIS Integration Runtime](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)
- [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview)
- [Topologías de red para migraciones a Instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/dms/resource-network-topologies)
- [Migración de paquetes de SSIS a una instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages-managed-instance)

## <a name="next-steps"></a>Pasos siguientes

- [Conexión a SSISDB en Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Ejecución de paquetes de SSIS implementados en Azure](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-run-packages)
