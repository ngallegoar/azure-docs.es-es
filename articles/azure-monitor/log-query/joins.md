---
title: Combinaciones en consultas de registros de Azure Monitor | Microsoft Docs
description: En este artículo se incluye una lección sobre el uso de combinaciones en consultas de registros de Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: 2dace6968fbbe69f806c27fb7a46e60c63f78b4f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "77670209"
---
# <a name="joins-in-azure-monitor-log-queries"></a>Combinaciones en consultas de registros de Azure Monitor

> [!NOTE]
> Debe completar [Introducción a Log Analytics de Azure Monitor](get-started-portal.md) y [Consultas de registros de Azure Monitor](get-started-queries.md) antes de realizar los pasos de esta lección.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Las combinaciones permiten analizar datos de varias tablas, en la misma consulta. Combinan las filas de dos conjuntos de datos haciendo coincidir los valores de columnas especificadas.


```Kusto
SecurityEvent 
| where EventID == 4624     // sign-in events
| project Computer, Account, TargetLogonId, LogonTime=TimeGenerated
| join kind= inner (
    SecurityEvent 
    | where EventID == 4634     // sign-out events
    | project TargetLogonId, LogoffTime=TimeGenerated
) on TargetLogonId
| extend Duration = LogoffTime-LogonTime
| project-away TargetLogonId1 
| top 10 by Duration desc
```

En este ejemplo, en el primer conjunto de datos se filtran todos los eventos de inicio de sesión. Esto se combina con un segundo conjunto de datos que en el que se filtran todos los eventos de cierre de sesión. Las columnas proyectadas son _Computer_, _Account_, _TargetLogonId_ y _TimeGenerated_. Los conjuntos de datos se correlacionan mediante una columna compartida, _TargetLogonId_. La salida es un único registro por correlación, que contiene las horas de inicio de sesión y de cierre de sesión.

Si ambos conjuntos de datos tienen columnas con los mismos nombres, se da a las columnas del conjunto de datos de la derecha un número de índice, por lo que en este ejemplo los resultados mostrarán _TargetLogonId_ con valores de la tabla del lado izquierdo y  _TargetLogonId1_ con valores de la tabla de la derecha. En este caso, se ha quitado la segunda columna _TargetLogonId1_ con el operador `project-away`.

> [!NOTE]
> Para mejorar el rendimiento, conserve solo las columnas relevantes de los conjuntos de datos combinados, con el operador `project`.


Use la siguiente sintaxis para unir dos conjuntos de datos; la clave combinada tiene un nombre distinto entre las dos tablas:
```
Table1
| join ( Table2 ) 
on $left.key1 == $right.key2
```

## <a name="lookup-tables"></a>Tablas de búsqueda
Un uso común de las combinaciones es la utilización de una asignación estática de valores mediante la función `datatable` que puede ayudar a transformar los resultados a una forma con mejor presentación. Por ejemplo, enriquecer los datos de eventos de seguridad con el nombre del evento para cada id. de evento.

```Kusto
let DimTable = datatable(EventID:int, eventName:string)
  [
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4658, "The handle to an object was closed",
    4656, "A handle to an object was requested",
    4690, "An attempt was made to duplicate a handle to an object",
    4663, "An attempt was made to access an object",
    5061, "Cryptographic operation",
    5058, "Key file operation"
  ];
SecurityEvent
| join kind = inner
 DimTable on EventID
| summarize count() by eventName
```

![Combinar con una función datatable](media/joins/dim-table.png)

## <a name="join-kinds"></a>Tipos de combinación
Especifique el tipo de combinación con el argumento _kind_. Cada tipo realiza a una coincidencia diferente entre los registros de las tablas dadas, como se describe en la tabla siguiente.

| Tipo de combinación | Descripción |
|:---|:---|
| innerunique | Este es el modo de combinación predeterminado. En primer lugar, se buscan los valores de la columna coincidente de la tabla izquierda y se quitan los valores duplicados.  A continuación, el conjunto de valores únicos se compara con la tabla derecha. |
| interna | Solo los registros coincidentes en ambas tablas se incluyen en los resultados. |
| leftouter | Todos los registros de la tabla izquierda y los registros coincidentes de la tabla derecha se incluyen en los resultados. Las propiedades de salida sin coincidencias contienen valores NULL.  |
| leftanti | Los registros del lado izquierdo que no tienen coincidencias en el derecho se incluyen en los resultados. La tabla de resultados tiene solo las columnas de la tabla izquierda. |
| leftsemi | Los registros del lado izquierdo que tienen coincidencias en el derecho se incluyen en los resultados. La tabla de resultados tiene solo las columnas de la tabla izquierda. |


## <a name="best-practices"></a>Procedimientos recomendados

Tenga en cuenta los siguientes puntos para obtener un rendimiento óptimo:

- Utilice un filtro de tiempo en cada tabla para reducir los registros que se deben evaluar para la combinación.
- Use los elementos `where` y `project` para reducir el número de filas y columnas en las tablas de entrada antes de la combinación.
- Si una tabla siempre es menor que la otra, úsela como lado izquierdo de la combinación.


## <a name="next-steps"></a>Pasos siguientes
Consulte otras lecciones para usar consultas de registro de Azure Monitor:

- [Operaciones de cadena](string-operations.md)
- [Funciones de agregación](aggregations.md)
- [Agregaciones avanzadas](advanced-aggregations.md)
- [JSON y estructuras de datos](json-data-structures.md)
- [Escritura de consultas avanzadas](advanced-query-writing.md)
- [Gráficos](charts.md)
