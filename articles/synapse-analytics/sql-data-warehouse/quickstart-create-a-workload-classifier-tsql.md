---
title: 'Inicio rápido: creación de un clasificador de carga de trabajo: T-SQL'
description: Uso de T-SQL para crear un clasificador de cargas de trabajo con importancia alta.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: a63cd95fdcee7c9ed0c49ba41b4d7e7e6de8f4bf
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96458738"
---
# <a name="quickstart-create-a-workload-classifier-using-t-sql"></a>Inicio rápido: Creación un clasificador de carga de trabajo mediante T-SQL

En este inicio rápido creará un clasificador de carga de trabajo rápidamente con importancia alta para el director general de su organización. Este clasificador de carga de trabajo permitirá que las consultas del director general tengan prioridad sobre otras de menor importancia en la cola.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

> [!NOTE]
> La creación de una instancia de grupo de SQL dedicado en Azure Synapse Analytics puede dar lugar a un nuevo servicio facturable.  Para más información, consulte los [precios de Azure Synapse Analytics](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>
>

## <a name="prerequisites"></a>Prerrequisitos

En este inicio rápido se da por supuesto que ya ha aprovisionado un grupo de SQL dedicado en Azure Synapse Analytics y que tiene los permisos CONTROL DATABASE. Si tiene que crearlo, siga las instrucciones del artículo sobre la [creación y conexión desde Azure Portal](create-data-warehouse-portal.md) para crear un grupo de SQL dedicado llamado **mySampleDataWarehouse**.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="create-login-for-theceo"></a>Creación del inicio de sesión para TheCEO

Cree un inicio de sesión de autenticación de SQL Server mediante la base de datos `master` con [CREAR INICIO DE SESIÓN](/sql/t-sql/statements/create-login-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para "TheCEO".

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'TheCEO')
BEGIN
CREATE LOGIN [TheCEO] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Crear usuario

[Cree un usuario](/sql/t-sql/statements/create-user-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest), "TheCEO", en mySampleDataWarehouse

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'THECEO')
BEGIN
CREATE USER [TheCEO] FOR LOGIN [TheCEO]
END
;
```

## <a name="create-a-workload-classifier"></a>Creación del clasificador de carga de trabajo

Cree un [clasificador de carga de trabajo](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para "TheCEO" con importancia alta.

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO];
CREATE WORKLOAD CLASSIFIER [wgcTheCEO]
WITH (WORKLOAD_GROUP = 'xlargerc'
      ,MEMBERNAME = 'TheCEO'
      ,IMPORTANCE = HIGH);
```

## <a name="view-existing-classifiers"></a>Visualización de los clasificadores existentes

```sql
SELECT * FROM sys.workload_management_workload_classifiers
```

## <a name="clean-up-resources"></a>Limpieza de recursos

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO]
DROP USER [TheCEO]
;
```

Se le cobra por las unidades de almacenamiento de datos y por los datos almacenados en el grupo de SQL dedicado. Estos recursos de proceso y de almacenamiento se facturan por separado.

- Si quiere conservar los datos en el almacenamiento, puede pausar el proceso cuando no use el grupo de SQL dedicado. Si se hace una pausa en el proceso, solo se carga el almacenamiento de datos. Cuando esté listo para trabajar con los datos, reanude el proceso.
- Si quiere eliminar cobros futuros, puede eliminar el grupo de SQL dedicado.

Siga estos pasos para limpiar los recursos.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y seleccione el grupo de SQL dedicado.

    ![Limpieza de recursos](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. Para pausar el proceso, seleccione el botón **Pausar**. Cuando el grupo de SQL dedicado esté en pausa, verá un botón **Iniciar**.  Para reanudar el proceso, seleccione **Iniciar**.

3. Para quitar el grupo de SQL dedicado para que no se le cobre por proceso o almacenamiento, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

- Ahora ha creado un clasificador de carga de trabajo. Ejecute algunas consultas como TheCEO para ver cómo responden. Consulte [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para ver las consultas y la importancia asignada.
- Para más información sobre la administración de cargas de trabajo de un grupo de SQL dedicado, consulte [Importancia de la carga de trabajo](sql-data-warehouse-workload-importance.md) y [Clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md).
- Consulte los artículos de procedimientos para [configurar la importancia de la carga de trabajo](sql-data-warehouse-how-to-configure-workload-importance.md) y sobre cómo [administrar y supervisar la administración de la carga de trabajo](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
