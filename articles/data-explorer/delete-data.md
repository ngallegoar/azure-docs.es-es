---
title: Eliminación de datos desde el Explorador de datos de Azure
description: En este artículo se describen los escenarios de eliminación de Azure Data Explorer, incluidas la purga, la eliminación de extensiones y las eliminaciones basadas en la retención.
author: orspod
ms.author: orspodek
ms.reviewer: avneraa
ms.service: data-explorer
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: dd0f8740d148a7817bcfe2fbad591ceeb1610d0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501416"
---
# <a name="delete-data-from-azure-data-explorer"></a>Eliminación de datos desde el Explorador de datos de Azure

Azure Data Explorer admite los distintos escenarios de eliminación que se describen en este artículo. 

## <a name="delete-data-using-the-retention-policy"></a>Eliminación de datos mediante la directiva de retención

Azure Data Explorer elimina automáticamente los datos de acuerdo con la [directiva de retención](/azure/kusto/management/retentionpolicy). Este método es la forma más eficaz y sencilla de eliminar datos. Establezca la directiva de retención en el nivel de base de datos o de tabla.

Considere una base de datos o una tabla que se establece con 90 días de retención. Si solo se necesitan 60 días de datos, elimine los datos más antiguos de la siguiente manera:

```kusto
.alter-merge database <DatabaseName> policy retention softdelete = 60d

.alter-merge table <TableName> policy retention softdelete = 60d
```

## <a name="delete-data-by-dropping-extents"></a>Eliminación de datos mediante la eliminación de extensiones

La [extensión (partición de datos)](/azure/kusto/management/extents-overview) es la estructura interna en la que se almacenan los datos. Cada extensión puede contener millones de registros. Las extensiones se pueden eliminar individualmente o en grupo mediante los [comandos de eliminación de extensiones](/azure/kusto/management/extents-commands#drop-extents). 

### <a name="examples"></a>Ejemplos

Puede eliminar todas las filas de una tabla o solo una extensión específica.

* Eliminación de todas las filas de una tabla:

    ```kusto
    .drop extents from TestTable
    ```

* Eliminación de una extensión específica:

    ```kusto
    .drop extent e9fac0d2-b6d5-4ce3-bdb4-dea052d13b42
    ```

## <a name="delete-individual-rows-using-purge"></a>Eliminación de filas individuales mediante la purga

La [purga de datos](/azure/kusto/concepts/data-purge) se puede usar para eliminar filas individuales. La eliminación no es inmediata y requiere bastantes recursos del sistema. Por tanto, solo se recomienda para escenarios de cumplimiento.  

