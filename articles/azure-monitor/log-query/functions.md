---
title: Funciones en las consultas de registros de Azure Monitor | Microsoft Docs
description: En este artículo se describe cómo utilizar funciones para llamar a una consulta desde otra consulta de registro de Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/31/2020
ms.openlocfilehash: 678a6f0dc19d966f3d15e713008c19c8fbb96f5e
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2020
ms.locfileid: "95024371"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Uso de funciones en consultas de registros de Azure Monitor

Para usar una consulta de registro con otra consulta, puede guardarla como una función. Esto le permite simplificar las consultas complejas al dividirlas en partes y, además, le permite reutilizar código común con varias consultas.

## <a name="create-a-function"></a>Creación de una función

Para crear una función con Log Analytics en Azure Portal, haga clic en **Guardar** y, después, proporcione la información de la tabla siguiente.

| Configuración | Descripción |
|:---|:---|
| Nombre           | Nombre para mostrar de la consulta en el **Explorador de consultas**. |
| Guardar como        | Función |
| Alias de función | Nombre corto para usar la función en otras consultas. No puede contener espacios y debe ser único. |
| Category       | Categoría para organizar las consultas y funciones guardadas en el **Explorador de consultas**. |




## <a name="use-a-function"></a>Uso de una función
Use una función al incluir su alias en otra consulta. Se puede usar como cualquier otra tabla.

## <a name="function-parameters"></a>Parámetros de función 
Puede agregar parámetros a una función para que pueda proporcionar valores de determinadas variables al llamarla. La única manera de crear una función con parámetros es usar una plantilla de Resource Manager. Consulte [Ejemplos de plantillas de Resource Manager para consultas de registros en Azure Monitor](../samples/resource-manager-log-queries.md#parameterized-function) para obtener un ejemplo.

## <a name="example"></a>Ejemplo
La siguiente consulta de ejemplo devuelve todas las actualizaciones de seguridad faltantes que se notificaron en el último día. Guarde esta consulta como función con el alias _security_updates_last_day_. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Cree otra consulta y llame a la función _security_updates_last_day_ para buscar las actualizaciones de seguridad relacionadas con SQL necesarias.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Pasos siguientes
Consulte otras lecciones para escribir consultas de registro de Azure Monitor:

- [Operaciones de cadena](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#string-operations)
- [Operaciones de fecha y hora](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#date-and-time-operations)
- [Funciones de agregación](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#aggregations)
- [Agregaciones avanzadas](/azure/data-explorer/write-queries#advanced-aggregations)
- [JSON y estructuras de datos](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#json-and-data-structures)
- [Combinaciones](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#joins)
- [Gráficos](/azure/data-explorer/kusto/query/samples?&pivots=azuremonitor#charts)
