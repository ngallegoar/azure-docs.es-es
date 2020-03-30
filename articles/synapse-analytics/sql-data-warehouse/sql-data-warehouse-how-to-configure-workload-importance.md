---
title: Configuración de la importancia de la carga de trabajo
description: Aprenda a establecer la importancia del nivel de solicitud en Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 85fa6a989ae1a6214392914ffd46cd12af27a8d2
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350493"
---
# <a name="configure-workload-importance-in-azure-synapse-analytics"></a>Configuración de la importancia de la carga de trabajo en Azure Synapse Analytics

Establecer la importancia en SQL Analytics para Azure Synapse le permite influir en la programación de las consultas. Las consultas de mayor importancia se programarán para ejecutarse antes de las consultas con una importancia menor. Para asignar importancia a las consultas, deberá crear un clasificador de carga de trabajo.

## <a name="create-a-workload-classifier-with-importance"></a>Crear un clasificador de cargas de trabajo con importancia

En un escenario de almacenamiento de datos a menudo tiene usuarios que necesitan que sus consultas se ejecuten rápidamente.  El usuario podría ser un ejecutivo de la compañía que necesita ejecutar informes, o podría ser un analista que ejecuta una consulta ad hoc. Para asignar importancia a una consulta, deberá crear un clasificador de carga de trabajo.  Los ejemplos siguientes usan la nueva sintaxis para [crear un clasificador de carga de trabajo](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) para crear dos clasificadores.  MemberName puede ser un usuario único o un grupo. Las clasificaciones de usuario individual tienen prioridad sobre las clasificaciones de rol. Para buscar los usuarios del almacén de datos existente, ejecute lo siguiente:

```sql
Select name from sys.sysusers
```

Para crear un clasificador de cargas de trabajo para un usuario con importancia más alta, ejecute lo siguiente:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name'  
         ,IMPORTANCE     =  above_normal);  

```

Para crear un clasificador de cargas de trabajo para un usuario que ejecuta consultas ad hoc con una importancia menor, ejecute lo siguiente:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name'  
         ,IMPORTANCE     =  below_normal);  
```

## <a name="next-steps"></a>Pasos siguientes
- Para obtener más información sobre la administración de cargas de trabajo, consulte [Clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md).
- Para más información sobre la importancia, vea [Importancia de la carga de trabajo](sql-data-warehouse-workload-importance.md).

> [!div class="nextstepaction"]
> [Vaya a Administración y supervisión de la importancia de la carga de trabajo](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
