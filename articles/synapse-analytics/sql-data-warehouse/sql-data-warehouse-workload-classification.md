---
title: Clasificación de la carga de trabajo
description: Instrucciones para usar la clasificación para administrar la simultaneidad, la importancia y los recursos de proceso de las consultas en Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 6b66b8a9fb3b5eb7dc78c00ba084e8609877dec7
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2020
ms.locfileid: "93323874"
---
# <a name="azure-synapse-analytics-workload-classification"></a>Clasificación de la carga de trabajo de Azure Synapse Analytics

En este artículo se explica el proceso de clasificación de la carga de trabajo de asignar un grupo de cargas de trabajo y la importancia a las solicitudes entrantes con grupos de SQL dedicados en Azure Synapse.

## <a name="classification"></a>clasificación

> [!Video https://www.youtube.com/embed/QcCRBAhoXpM]

La clasificación de la administración de cargas de trabajo permite aplicar directivas de carga de trabajo a las solicitudes al asignar [clases de recursos](resource-classes-for-workload-management.md#what-are-resource-classes) e [importancia](sql-data-warehouse-workload-importance.md).

Aunque hay muchas maneras de clasificar las cargas de trabajo de almacenamiento de datos, la clasificación más sencilla y común es la carga y la consulta. Los datos se cargan con instrucciones insert, update y delete.  Los datos se consultan mediante instrucciones select. Una solución de almacenamiento de datos a menudo tendrá una directiva de carga de trabajo para la actividad de carga, como la asignación de una clase de recursos superior con más recursos. Se podría aplicar una directiva de carga de trabajo diferente a las consultas, como la importancia menor en comparación con las actividades de carga.

También puede subclasificar las cargas de trabajo de carga y consulta. La subclasificación ofrece un mayor control de las cargas de trabajo. Por ejemplo, las cargas de trabajo de consulta pueden constar de actualizaciones de cubos, consultas del panel o consultas ad hoc. Puede clasificar cada una de estas cargas de trabajo de consulta con diferentes clases de recursos o configuración de importancia. La carga también puede beneficiarse de la subclasificación. Transformaciones de gran tamaño se pueden asignar a clases de recursos más grandes. Una mayor importancia se puede usar para asegurarse de que los datos de ventas clave se carguen antes de los datos meteorológicos o una fuente de distribución de datos sociales.

No todas las instrucciones se clasifican, ya que no requieren recursos ni necesitan importancia para influir en la ejecución.  Los comandos DBCC y las instrucciones BEGIN, COMMIT y ROLLBACK TRANSACTION no se clasifican.

## <a name="classification-process"></a>Proceso de clasificación

Hoy en día, la clasificación de grupos de SQL dedicados en Azure Synapse se logra mediante la asignación de usuarios a un rol que tiene una clase de recursos correspondiente asignada mediante [sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest). Con esta funcionalidad, se limita la capacidad para caracterizar las consultas más allá de un inicio de sesión en una clase de recursos. Ahora hay un método más completo disponible para la clasificación mediante la sintaxis [CREATE WORKLOAD CLASSIFIER](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  Con esta sintaxis, los usuarios de grupos de SQL dedicados pueden asignar importancia y el número de recursos del sistema que se asignan a una solicitud a través del parámetro `workload_group`.

> [!NOTE]
> La clasificación se evalúa en función de la solicitud. Varias solicitudes en una única sesión se pueden clasificar de forma diferente.

## <a name="classification-weighting"></a>Ponderación de la clasificación

Como parte del proceso de clasificación, la ponderación se aplica para determinar qué grupo de cargas de trabajo se asigna.  La ponderación es como se indica a continuación:

|Parámetro clasificador |Peso   |
|---------------------|---------|
|MEMBERNAME:USER      |64       |
|MEMBERNAME:ROLE      |32       |
|WLM_LABEL            |16       |
|WLM_CONTEXT          |8        |
|START_TIME/END_TIME  |4        |

El parámetro `membername` es obligatorio.  Sin embargo, si el membername especificado es un usuario de base de datos, en lugar de un rol de base de datos, la ponderación del usuario es mayor y, por consiguiente, se elige ese clasificador.

Si un usuario es miembro de varios roles con diferentes clases de recursos asignadas o que coinciden con varios clasificadores, el usuario tendrá la asignación de clase de recursos más alta.  Este comportamiento es coherente con el comportamiento de asignación de clases de recursos existente.

## <a name="system-classifiers"></a>Clasificadores de sistema

La clasificación de la carga de trabajo tiene clasificadores de carga de trabajo del sistema. Los clasificadores de sistema asignan miembros de rol de clase de recursos existentes a asignaciones de recursos de clase de recursos con importancia normal. Los clasificadores de sistema no se pueden eliminar. Para ver los clasificadores de sistema, puede ejecutar la siguiente consulta:

```sql
SELECT * FROM sys.workload_management_workload_classifiers where classifier_id <= 12
```

## <a name="mixing-resource-class-assignments-with-classifiers"></a>Combinación de asignaciones de clase de recursos con clasificadores

Los clasificadores del sistema que se crean en nombre del usuario proporcionan una ruta sencilla para migrar a la clasificación de la carga de trabajo. El uso de asignaciones de rol de clase de recursos con prioridad de clasificación puede resultar en clasificaciones incorrectas cuando empiece a crear nuevas clasificadores con importancia.

Considere el caso siguiente:

- Un almacenamiento de datos existente tiene un usuario de base de datos DBAUser asignado al rol de clase de recursos largerc. La asignación de la clase de recursos se realizaba mediante sp_addrolemember.
- El almacenamiento de datos se habrá actualizado con la administración de cargas de trabajo.
- Para probar la nueva sintaxis de clasificación, el rol de base de datos DBARole (al que pertenece DBAUser) tiene un clasificador creado para asignarlo a mediumrc e importancia alta.
- Cuando DBAUser inicia sesión y ejecuta una consulta, la consulta se asignará a largerc. Esto se debe a que un usuario tiene prioridad sobre una pertenencia a roles.

Para simplificar la solución de problemas de clasificaciones incorrectas, se recomienda quitar las asignaciones de rol de clase de recursos a medida que se creen los clasificadores de carga de trabajo.  El código siguiente devuelve las pertenencias existentes a roles de clase de recurso.  Ejecute [sp_droprolemember](/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para cada nombre de miembro que se devuelve desde la clase de recursos correspondiente.

```sql
SELECT  r.name AS [Resource Class]
,       m.name AS membername
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r ON rm.role_principal_id = r.principal_id
JOIN    sys.database_principals AS m ON rm.member_principal_id = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc','staticrc10','staticrc20','staticrc30','staticrc40','staticrc50','staticrc60','staticrc70','staticrc80');

--for each row returned run
sp_droprolemember '[Resource Class]', membername
```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre cómo crear un clasificador, consulte [CREATE WORKLOAD CLASSIFIER (Transact-SQL)](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest).  
- Consulte la Guía de inicio rápido sobre cómo crear un clasificador de carga de trabajo en [Creación del clasificador de carga de trabajo](quickstart-create-a-workload-classifier-tsql.md).
- Consulte los artículos de procedimientos sobre cómo [configurar la importancia de la carga de trabajo](sql-data-warehouse-how-to-configure-workload-importance.md) y cómo [administrar y supervisar la administración de la carga de trabajo](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md).
- Consulte [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) para ver las consultas y la importancia asignada.
