---
title: Consultas avanzadas en Azure Monitor | Microsoft Docs
description: En este artículo se proporciona un tutorial de uso del portal de Analytics para escribir consultas en Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/15/2018
ms.openlocfilehash: 3d228c62cd2d1bcb7f4515cd698186e2ebcbe929
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670294"
---
# <a name="writing-advanced-queries-in-azure-monitor"></a>Escritura de consultas avanzadas en Azure Monitor

> [!NOTE]
> Debe completar [Introducción a los análisis de registros de Azure Monitor](get-started-portal.md) e [Introducción a las consultas de registros de Azure Monitor](get-started-queries.md) antes de completar esta lección.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

## <a name="reusing-code-with-let"></a>Reutilización de código con let
Use el elemento `let` para asignar los resultados a una variable y hacer referencia a ella más adelante en la consulta:

```Kusto
// get all events that have level 2 (indicates warning level)
let warning_events=
Event
| where EventLevel == 2;
// count the number of warning events per computer
warning_events
| summarize count() by Computer 
```

También puede asignar valores constantes a variables. Esto es compatible con un método para configurar los parámetros para los campos que necesite cambiar cada vez que ejecute la consulta. Modifique los parámetros según sea necesario. Por ejemplo, para calcular el espacio libre en disco y la memoria libre (en percentiles), en un período de tiempo determinado:

```Kusto
let startDate = datetime(2018-08-01T12:55:02);
let endDate = datetime(2018-08-02T13:21:35);
let FreeDiskSpace =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Logical Disk" and CounterName=="Free Megabytes"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
let FreeMemory =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Memory" and CounterName=="Available MBytes Memory"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
union FreeDiskSpace, FreeMemory
```

Facilita el cambio de la hora de inicio y finalización la próxima vez que ejecute la consulta.

### <a name="local-functions-and-parameters"></a>Funciones y parámetros locales
Use instrucciones `let` para crear funciones que puedan usarse en la misma consulta. Por ejemplo, defina una función que tome un campo de fecha y hora (en formato UTC) y lo convierta en un formato estándar de Estados Unidos. 

```Kusto
let utc_to_us_date_format = (t:datetime)
{
  strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
  bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
};
Event 
| where TimeGenerated > ago(1h) 
| extend USTimeGenerated = utc_to_us_date_format(TimeGenerated)
| project TimeGenerated, USTimeGenerated, Source, Computer, EventLevel, EventData 
```

## <a name="print"></a>Print
La función `print` devolverá una tabla con una sola columna y una sola fila, que muestra el resultado de un cálculo. Se suele usar en los casos en los que es necesario un cálculo simple. Por ejemplo, para buscar la hora actual en la hora estándar del Pacífico y agregar una columna con EST:

```Kusto
print nowPst = now()-8h
| extend nowEst = nowPst+3h
```

## <a name="datatable"></a>Datatable
La función `datatable` permite definir un conjunto de datos. Se proporciona un esquema y un conjunto de valores y, a continuación, se canaliza la tabla a cualquier otro elemento de consulta. Por ejemplo, para crear una tabla de utilización de RAM y calcular su valor promedio por hora:

```Kusto
datatable (TimeGenerated: datetime, usage_percent: double)
[
  "2018-06-02T15:15:46.3418323Z", 15.5,
  "2018-06-02T15:45:43.1561235Z", 20.2,
  "2018-06-02T16:16:49.2354895Z", 17.3,
  "2018-06-02T16:46:44.9813459Z", 45.7,
  "2018-06-02T17:15:41.7895423Z", 10.9,
  "2018-06-02T17:44:23.9813459Z", 24.7,
  "2018-06-02T18:14:59.7283023Z", 22.3,
  "2018-06-02T18:45:12.1895483Z", 25.4
]
| summarize avg(usage_percent) by bin(TimeGenerated, 1h)
```

Las construcciones de Datatable también son muy útiles al crear una tabla de búsqueda. Por ejemplo, para asignar datos de tabla, como los id. de eventos, de la tabla _SecurityEvent_ a tipos de evento enumerados en otro lugar, cree una tabla de búsqueda con los tipos de eventos mediante la función `datatable` y combine esta datatable con los datos de  _SecurityEvent_:

```Kusto
let eventCodes = datatable (EventID: int, EventType:string)
[
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4672, "Access",
    4624, "Account activity",
    4799, "Access management",
    4798, "Access management",
    5059, "Key operation",
    4648, "A logon was attempted using explicit credentials",
    4768, "Access management",
    4662, "Other",
    8002, "Process action",
    4904, "Security event management",
    4905, "Security event management",
];
SecurityEvent
| where TimeGenerated > ago(1h) 
| join kind=leftouter (
  eventCodes
) on EventID
| project TimeGenerated, Account, AccountType, Computer, EventType
```

## <a name="next-steps"></a>Pasos siguientes
Consulte otras lecciones para usar el [lenguaje de consulta de Kusto](/azure/kusto/query/) con datos de registro de Azure Monitor:

- [Operaciones de cadena](string-operations.md)
- [Operaciones de fecha y hora](datetime-operations.md)
- [Funciones de agregación](aggregations.md)
- [Agregaciones avanzadas](advanced-aggregations.md)
- [JSON y estructuras de datos](json-data-structures.md)
- [Combinaciones](joins.md)
- [Gráficos](charts.md)
