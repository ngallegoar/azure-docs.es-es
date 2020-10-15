---
title: Configuración de la importancia de la carga de trabajo
description: Aprenda a establecer la importancia del nivel de solicitud en Azure Synapse Analytics.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.topic: conceptual
ms.date: 05/15/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 83170f4090909e3edcc163312383773d088d8c57
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85212129"
---
# <a name="configure-workload-importance-in-azure-synapse-analytics"></a>Configuración de la importancia de la carga de trabajo en Azure Synapse Analytics

Establecer la importancia en SQL de Synapse para Azure Synapse permite influir en la programación de las consultas. Las consultas de mayor importancia se programarán para ejecutarse antes de las consultas con una importancia menor. Para asignar importancia a las consultas, deberá crear un clasificador de carga de trabajo.

## <a name="create-a-workload-classifier-with-importance"></a>Crear un clasificador de cargas de trabajo con importancia

En un escenario de almacenamiento de datos, a menudo tiene usuarios en un sistema ocupado que necesitan que sus consultas se ejecuten rápidamente.  El usuario podría ser un ejecutivo de la compañía que necesita ejecutar informes, o podría ser un analista que ejecuta una consulta ad hoc. Para asignar importancia a una consulta, deberá crear un clasificador de carga de trabajo y asignar importancia a una consulta.  Los ejemplos siguientes usan la sintaxis para [crear un clasificador de carga de trabajo](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) y crear dos clasificadores. `Membername` puede ser un usuario único o un grupo.  Para buscar los usuarios del almacén de datos existente, ejecute lo siguiente:

```sql
Select name from sys.sysusers
```

Para crear un clasificador de cargas de trabajo para un usuario con importancia más alta, ejecute lo siguiente:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name' 
         ,IMPORTANCE     = above_normal);
```

Para crear un clasificador de cargas de trabajo para un usuario que ejecuta consultas ad hoc con una importancia menor, ejecute lo siguiente:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name' 
         ,IMPORTANCE     = below_normal);
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre la administración de cargas de trabajo, consulte [Clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md).
- Para más información sobre la importancia, vea [Importancia de la carga de trabajo](sql-data-warehouse-workload-importance.md).

> [!div class="nextstepaction"]
> [Vaya a Administración y supervisión de la importancia de la carga de trabajo](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
