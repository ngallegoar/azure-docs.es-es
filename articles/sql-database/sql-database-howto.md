---
title: Configuración y administración
description: Aprenda cómo configurar y administrar Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 11/14/2019
ms.openlocfilehash: c3f7b33e4b42b08334cfb687024985c878dc3713
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79209415"
---
# <a name="how-to-use-azure-sql-database"></a>Cómo usar Azure SQL Database

En esta sección puede encontrar varias guías, scripts y explicaciones que pueden ayudarle a administrar y configurar su base de datos de Azure SQL. También puede encontrar guías de paso a paso específicas para una [base de datos única](sql-database-howto-single-database.md) y una [instancia administrada](sql-database-howto-managed-instance.md).

## <a name="load-data"></a>Cargar datos

- [Copia de una base de datos única o agrupada en Azure](sql-database-copy.md)
- [Importación de una base de datos desde un archivo BACPAC](sql-database-import.md)
- [Exportación de una base de datos a un archivo BACPAC](sql-database-export.md)
- [Carga de datos con BCP](sql-database-load-from-csv-with-bcp.md)
- [Carga de datos con ADF](../data-factory/connector-azure-sql-database.md?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Sincronización de datos

- [SQL Data Sync](sql-database-sync-data.md)
- [Agente de sincronización de datos](sql-database-data-sync-agent.md)
- [Replicación de cambios de esquema](sql-database-update-sync-schema.md)
- [Supervisión con OMS](sql-database-sync-monitor-oms.md)
- [Procedimientos recomendados para Data Sync](sql-database-best-practices-data-sync.md)
- [Solución de problemas de Data Sync](sql-database-troubleshoot-data-sync.md)

## <a name="monitoring-and-tuning"></a>Supervisión y ajuste

- [Ajuste manual](sql-database-performance-guidance.md)
- [Uso de DMV para supervisar el rendimiento](sql-database-monitoring-with-dmvs.md)
- [Uso del almacén de consultas para supervisar el rendimiento](https://docs.microsoft.com/sql/relational-databases/performance/best-practice-with-the-query-store#Insight)
- [Solución de problemas de rendimiento con Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md)
- [Uso del registro de diagnóstico de Intelligent Insights](sql-database-intelligent-insights-use-diagnostics-log.md)
- [Supervisión del espacio OLTP en memoria](sql-database-in-memory-oltp-monitoring.md)

### <a name="extended-events"></a>Eventos extendidos

- [Eventos extendidos](sql-database-xevent-db-diff-from-svr.md)
- [Almacenar eventos extendidos en el archivo de evento](sql-database-xevent-code-event-file.md)
- [Almacenar eventos extendidos en el búfer en anillo](sql-database-xevent-code-ring-buffer.md)

## <a name="configure-features"></a>Configuración de características

- [Configuración de la autenticación de Azure AD](sql-database-aad-authentication-configure.md)
- [Configuración del acceso condicional](sql-database-conditional-access.md)
- [Multi-factor Authentication para AAD](sql-database-ssms-mfa-authentication.md)
- [Configuración de Multi-factor Authentication](sql-database-ssms-mfa-authentication-configure.md)
- [Configuración temporal de la directiva de retención](sql-database-temporal-tables-retention-policy.md)
- [Configuración de TDE con BYOK](transparent-data-encryption-byok-azure-sql-configure.md)
- [Rotación de claves BYOK de TDE](transparent-data-encryption-byok-azure-sql-key-rotation.md)
- [Eliminación del protector de TDE](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md)
- [Configuración de OLTP en memoria](sql-database-in-memory-oltp-migration.md)
- [Configuración de Azure Automation](sql-database-manage-automation.md)

## <a name="develop-applications"></a>Desarrollo de aplicaciones

- [Conectividad](sql-database-libraries.md)
- [Uso de Spark Connector](sql-database-spark-connector.md)
- [Autenticación de aplicaciones](sql-database-client-id-keys.md)
- [Uso del procesamiento por lotes para mejorar el rendimiento](sql-database-use-batching-to-improve-performance.md)
- [Guía de conectividad](sql-database-connectivity-issues.md)
- [Alias DNS](dns-alias-overview.md)
- [Configuración de PowerShell con alias DNS](dns-alias-powershell.md)
- [Puertos: ADO.NET](sql-database-develop-direct-route-ports-adonet-v12.md)
- [C y C ++](sql-database-develop-cplusplus-simple.md)
- [Excel](sql-database-connect-excel.md)

## <a name="design-applications"></a>Diseño de aplicaciones

- [Diseño de la recuperación ante desastres](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Diseño para grupos elásticos](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)
- [Diseño para actualización de aplicaciones](sql-database-manage-application-rolling-upgrade.md)

### <a name="design-multi-tenant-saas-applications"></a>Diseño de aplicaciones SaaS multiinquilino

- [Patrones de diseño SaaS](saas-tenancy-app-design-patterns.md)
- [Video Indexer SaaS](saas-tenancy-video-index-wingtip-brk3120-20171011.md)
- [Seguridad de aplicaciones SaaS](saas-tenancy-elastic-tools-multi-tenant-row-level-security.md)

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre las [guías paso a paso para instancias administradas](sql-database-howto-managed-instance.md).
- Obtenga más información sobre las [guías paso a paso para las bases de datos únicas](sql-database-howto-single-database.md).
