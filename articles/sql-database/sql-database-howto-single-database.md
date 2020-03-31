---
title: Cómo configurar una base de datos única
description: 'Aprenda cómo configurar y administrar Azure SQL Database: base de datos única.'
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
ms.date: 01/14/2020
ms.openlocfilehash: 4283e1a2c92b7fe738fb57a8103cea1deb0015a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76027720"
---
# <a name="how-to-use-a-single-database-in-azure-sql-database"></a>Cómo usar una base de datos única en Azure SQL Database

En esta sección puede encontrar diferentes guías, scripts y explicaciones que pueden ayudarle a administrar y configurar una base de datos única en Azure SQL Database.

## <a name="migrate"></a>Migrar

- [Migración a SQL Database](sql-database-single-database-migrate.md): obtenga información sobre el proceso de migración y las herramientas que se recomiendan para la migración a una instancia administrada.
- Aprenda cómo [administrar SQL Database después de la migración](sql-database-manage-after-migration.md).

## <a name="configure-features"></a>Configuración de características

- [Configuración de la replicación transaccional](replication-to-sql-database.md) para replicar la fecha entre bases de datos.
- [Configuración de la detección de amenazas](sql-database-threat-detection.md) para permitir que Azure SQL Database identifique las actividades sospechosas como inyección de código SQL o acceso desde ubicaciones sospechosas.
- [Configuración del enmascaramiento de datos dinámicos](sql-database-dynamic-data-masking-get-started-portal.md) para proteger los datos confidenciales.
- [Configuración de la retención de copias de seguridad](sql-database-long-term-backup-retention-configure.md) para que una base de datos mantenga las copias de seguridad en Azure Blob Storage. Como alternativa, dispone del enfoque de [Configuración de la retención de copias de seguridad mediante el almacén de Azure (en desuso)](sql-database-long-term-backup-retention-configure-vault.md).
- [Configuración de la replicación geográfica](sql-database-geo-replication-portal.md) para mantener una réplica de la base de datos en otra región.
- [Configuración de la seguridad de las replicaciones geográficas](sql-database-geo-replication-security-config.md).

## <a name="monitor-and-tune-your-database"></a>Supervisión y ajuste de base de datos

- [Habilitación del ajuste automático](sql-database-automatic-tuning-enable.md) para permitir que Azure SQL Database optimice el rendimiento de la carga de trabajo.
- [Habilitación de las notificaciones por correo electrónico para el ajuste automático](sql-database-automatic-tuning-email-notifications.md) para obtener información sobre las recomendaciones de optimización.
- [Aplicación de recomendaciones de rendimiento](sql-database-advisor-portal.md) para optimizar la base de datos.
- [Creación de alertas](sql-database-insights-alerts-portal.md) para recibir notificaciones de Azure SQL Database.
- [Solución de problemas de conectividad](troubleshoot-connectivity-issues-microsoft-azure-sql-database.md) si observa algunos problemas de conectividad entre las aplicaciones y la base de datos. También puede consultar [Uso de Resource Health para problemas de conectividad](sql-database-resource-health.md).
- [Administración del espacio de archivo](sql-database-file-space-management.md) para supervisar el uso de almacenamiento en la base de datos.

## <a name="query-distributed-data"></a>Consulta de datos distribuidos

- [Consulta de datos particionados verticalmente](sql-database-elastic-query-getting-started-vertical.md) en varias bases de datos.
- [Informes de capa de datos escalada horizontalmente](sql-database-elastic-query-horizontal-partitioning.md).
- [Consulta de tablas con esquemas diferentes](sql-database-elastic-query-vertical-partitioning.md).

## <a name="elastic-database-jobs"></a>Trabajos de Elastic Database

- [Creación y administración](elastic-jobs-powershell.md) de trabajos de Elastic Database mediante PowerShell.
- [Creación y administración](elastic-jobs-tsql.md) de trabajos de Elastic Database mediante Transact-SQL.
- [Migración de trabajos elásticos antiguos](elastic-jobs-migrate.md).

## <a name="database-sharding"></a>Partición de base de datos

- [Actualización de biblioteca de cliente de base de datos elástica](sql-database-elastic-scale-upgrade-client-library.md).
- [Creación de una aplicación con particiones](sql-database-elastic-scale-get-started.md).
- [Consulta de datos particionados horizontalmente](sql-database-elastic-query-getting-started.md).
- Ejecución de [consultas en varias particiones](sql-database-elastic-scale-multishard-querying.md).
- [Traslado de datos con particiones](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
- [Configuración de seguridad](sql-database-elastic-scale-split-merge-security-configuration.md) en particiones de base de datos.
- [Incorporación de una partición](sql-database-elastic-scale-add-a-shard.md) al conjunto actual de particiones de base de datos.
- [Solución de problemas de asignación de particiones](sql-database-elastic-database-recovery-manager.md).
- [Migración de bases de datos con particiones](sql-database-elastic-convert-to-use-elastic-tools.md).
- [Creación de contadores](sql-database-elastic-database-perf-counters.md).
- [Uso de Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) para consultar los datos con particiones.
- [Uso de la plataforma Dapper](sql-database-elastic-scale-working-with-dapper.md) para consultar los datos con particiones.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las [guías de procedimientos para una instancia administrada](sql-database-howto-managed-instance.md).
