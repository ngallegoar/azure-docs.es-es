---
title: 'Inicio rápido: configuración del aislamiento de cargas de trabajo: T-SQL'
description: Uso de T-SQL para configurar el aislamiento de cargas de trabajo.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/27/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 7f0aabf4bd18f82c247a43931e02e4b6890b2ef4
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650689"
---
# <a name="quickstart-configure-workload-isolation-using-t-sql"></a>Inicio rápido: configuración del aislamiento de carga de trabajo mediante T-SQL

En esta guía de inicio rápido, creará rápidamente un grupo de cargas de trabajo y un clasificador para reservar recursos para la carga de datos. El grupo de cargas de trabajo asignará un 20 % de los recursos del sistema a cargas de datos.  El clasificador de cargas de trabajo asignará solicitudes al grupo de cargas de trabajo de las cargas de datos.  Con un aislamiento del 20 % para las cargas de datos, se garantiza que los recursos cumplirán con los Acuerdos de Nivel de Servicio.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

> [!NOTE]
> La creación de una instancia de Synapse SQL en Azure Synapse Analytics puede dar lugar a un nuevo servicio facturable.  Para más información, consulte los [precios de Azure Synapse Analytics](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="prerequisites"></a>Prerrequisitos

En este inicio rápido se da por supuesto que ya tiene una instancia de Synapse SQL en Azure Synapse y que tiene permisos CONTROL DATABASE. Si tiene que crear una, consulte [Guía de inicio rápido: Creación de una instancia de Azure SQL Data Warehouse en Azure Portal, y realización de consultas en ella](create-data-warehouse-portal.md) para crear un almacenamiento de datos denominado **mySampleDataWarehouse**.

## <a name="create-login-for-dataloads"></a>Creación del inicio de sesión para cargas de datos

Cree un inicio de sesión de autenticación de SQL Server mediante la base de datos `master` con [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para "ELTLogin".

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'ELTLogin')
BEGIN
CREATE LOGIN [ELTLogin] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Crear usuario

[Cree un usuario](/sql/t-sql/statements/create-user-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), "ELTLogin", en mySampleDataWarehouse

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'ELTLogin')
BEGIN
CREATE USER [ELTLogin] FOR LOGIN [ELTLogin]
END
;
```

## <a name="create-a-workload-group"></a>Creación de un grupo de cargas de trabajo

Cree un [grupo de cargas de trabajo](/sql/t-sql/statements/create-workload-group-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para los elementos DataLoads con un aislamiento del 20 %.

```sql
CREATE WORKLOAD GROUP DataLoads
WITH ( MIN_PERCENTAGE_RESOURCE = 20
      ,CAP_PERCENTAGE_RESOURCE = 100
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5)
;
```

## <a name="create-a-workload-classifier"></a>Creación del clasificador de carga de trabajo

Cree un [clasificador de cargas de trabajo](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para asignar ELTLogin al grupo de cargas de trabajo de DataLoads.

```sql
CREATE WORKLOAD CLASSIFIER [wgcELTLogin]
WITH (WORKLOAD_GROUP = 'DataLoads'
      ,MEMBERNAME = 'ELTLogin')
;
```

## <a name="view-existing-workload-groups-and-classifiers-and-run-time-values"></a>Visualización de grupos de cargas de trabajo y clasificadores existentes y valores de tiempo de ejecución

```sql
--Workload groups
SELECT * FROM
sys.workload_management_workload_groups

--Workload classifiers
SELECT * FROM
sys.workload_management_workload_classifiers

--Run-time values
SELECT * FROM
sys.dm_workload_management_workload_groups_stats
```

## <a name="clean-up-resources"></a>Limpieza de recursos

```sql
DROP WORKLOAD CLASSIFIER [wgcELTLogin]
DROP WORKLOAD GROUP [DataLoads]
DROP USER [ELTLogin]
;
```

Se le está cobrando por unidades de almacenamiento de datos y por los datos almacenados en el almacenamiento de datos. Estos recursos de proceso y de almacenamiento se facturan por separado.

- Si quiere conservar los datos en almacenamiento, puede pausar el proceso cuando no use el grupo de SQL. Si se hace una pausa en el proceso, solo se carga el almacenamiento de datos. Cuando esté listo para trabajar con los datos, reanude el proceso.
- Si desea quitar cobros futuros, puede eliminar el almacenamiento de datos.

## <a name="next-steps"></a>Pasos siguientes

- Ahora ha creado un grupo de cargas de trabajo. Ejecute algunas consultas como ELTLogin para ver cómo responden. Consulte [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para ver las consultas y el grupo de cargas de trabajo asignado.
- Para más información sobre la administración de cargas de trabajo de SQL de Synapse, consulte [Administración de la carga de trabajo](sql-data-warehouse-workload-management.md) y [Clasificación de la carga de trabajo](sql-data-warehouse-workload-isolation.md).
