---
title: Plantillas del Administrador de recursos de Azure
description: Use las plantillas de Azure Resource Manager para crear y configurar Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples
ms.devlang: ''
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/04/2019
ms.openlocfilehash: b254621cc414fb9b2b76263957adc80da6e9c22d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "79214002"
---
# <a name="azure-resource-manager-templates-for-azure-sql-database"></a>Plantillas de Azure Resource Manager para Azure SQL Database

Las plantillas de Azure Resource Manager permiten definir la infraestructura como código e implementar soluciones en la nube de Azure.

## <a name="single-database--elastic-pool"></a>[Base de datos única y grupo elástico](#tab/single-database)

En la tabla siguiente se incluyen vínculos a plantillas de Azure Resource Manager para Azure SQL Database.

| |  |
|---|---|
| [Base de datos única](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-database-transparent-encryption-create) | Esta plantilla de Azure Resource Manager crea una base de datos de Azure SQL única con un servidor lógico y configura las reglas del firewall. |
| [Servidor lógico](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server) | Esta plantilla de Azure Resource Manager crea un servidor lógico para Azure SQL Database. |
| [Grupo elástico](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-elastic-pool-create) | Esta plantilla permite implementar un grupo de bases de datos elásticas nuevo con su nuevo servidor con SQL Server y sus nuevas bases de datos SQL asociados para asignárselo. |
| [Grupos de conmutación por error](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-with-failover-group) | Esta plantilla crea dos servidores lógicos de Azure SQL, una base de datos SQL y un grupo de conmutación por error.|
| [Detección de amenazas](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-threat-detection-db-policy-multiple-databases) | Esta plantilla permite implementar un servidor lógico de SQL Azure y un conjunto de bases de datos de Azure SQLs con Detección de amenazas habilitado, con una dirección de correo electrónico para las alertas de cada base de datos. Detección de amenazas forma parte de la oferta de Advanced Threat Protection (ATP) de SQL y proporciona una capa de seguridad que responda a amenazas potenciales a través de las bases de datos SQL y servidores con SQL Server.|
| [Auditoría a Azure Blob Storage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-blob-storage) | Esta plantilla le permite implementar un servidor lógico de Azure SQL con la auditoría habilitada para escribir los registros de auditoría en un almacenamiento de blobs. La auditoría de Azure SQL Database y realiza un seguimiento de eventos de base de datos y los escribe en un registro de auditoría que se puede colocar en su cuenta de Azure Storage, en el área de trabajo de OMS o en Event Hubs.|
| [Auditoría a Azure Event Hub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-eventhub) | Esta plantilla le permite implementar un servidor con Azure SQL Server con la auditoría habilitada para escribir los registros de auditoría en centro de eventos existente. Para enviar eventos de auditoría al centro de eventos, establezca la configuración de auditoría con `Enabled``State` y establezca `IsAzureMonitorTargetEnabled` como `true`. Configure también los valores de diagnóstico con la categoría de registros `SQLSecurityAuditEvents` en la base de datos `master` (para la auditoría de nivel de servidor). La auditoría de Azure SQL Database y SQL Data Warehouse realiza un seguimiento de eventos de base de datos y los escribe en un registro de auditoría que se puede colocar en su cuenta de Azure Storage, área de trabajo de OMS o Event Hubs.|
| [Azure Web App con SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database) | Este ejemplo crea una aplicación web y una base de datos SQL de Azure gratuitas en el nivel de servicio "Básico".|
| [Azure Web App y Redis Cache con SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) | Esta plantilla crea una instancia de Web App, Redis Cache y SQL Database en el mismo grupo de recursos y crea dos cadenas de conexión en la aplicación web para SQL Database y Redis Cache.|
| [Importar datos del almacenamiento de blobs mediante ADF V2](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-v2-blob-to-sql-copy) | Esta plantilla de Azure Resource Manager crea Azure Data Factory V2 que copia datos de Azure Blob Storage a SQL Database.|
| [Clúster de HDInsight con SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-with-sql-database) | Esta plantilla permite crear un clúster de HDInsight, un servidor de SQL Database, una instancia SQL Database y dos tablas. Esta plantilla se usa en el artículo [Uso de Apache Sqoop con Hadoop en HDInsight](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-use-sqoop) |
| [Aplicación de lógica de Azure que ejecuta un procedimiento almacenado de SQL según una programación](https://github.com/Azure/azure-quickstart-templates/tree/master/101-logic-app-sql-proc) | Esta plantilla permite crear una aplicación lógica que ejecutará un procedimiento almacenado de SQL de forma programada. Los argumentos para el procedimiento se pueden colocar en la sección del cuerpo de la plantilla.|

## <a name="managed-instance"></a>[Instancia administrada](#tab/managed-instance)

En la tabla siguiente se incluyen vínculos a plantillas de Azure Resource Manager para Azure SQL Database - Instancia administrada.

| |  |
|---|---|
| [Instancia administrada en una red virtual nueva](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sqlmi-new-vnet) | Esta plantilla de Azure Resource Manager crea una red virtual de Azure configurada y una instancia administrada en la red virtual. |
| [Entorno de red para Instancia administrada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) | Esta implementación creará una red virtual de Azure con dos subredes: una que se dedicará a las instancias administradas y otra en la que se pueden colocar otros recursos (por ejemplo, máquinas virtuales, entornos de App Service, etc.). Esta plantilla creará un entorno de red configurado correctamente en el que puede implementar instancias administradas. |
| [Instancia administrada con conexión P2S](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sqlmi-new-vnet-w-point-to-site-vpn) | Esta implementación creará una red virtual de Azure con dos subredes, `ManagedInstance` y `GatewaySubnet`. Instancia administrada se implementará en la subred ManagedInstance. Se creará una puerta de enlace de red virtual en la subred `GatewaySubnet` y se configurará para la conexión VPN de punto a sitio. |
| [Instancia administrada con máquina virtual](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sqlmi-new-vnet-w-jumpbox) | Esta implementación creará una red virtual de Azure con dos subredes, `ManagedInstance` y `Management`. Instancia administrada se implementará en la subred `ManagedInstance`. La máquina virtual con la versión más reciente de SQL Server Management Studio (SSMS) se implementarán en la subred `Management`. |

---
