---
title: Trabajar con valores de fecha y hora en consultas de registro de Azure Monitor | Microsoft Docs
description: Describe cómo trabajar con datos de fecha y hora en consultas de registro de Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/16/2018
ms.openlocfilehash: ea7c98a1b5b4059c5fea0cf1e8ea2ff5ef08d9d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655385"
---
# <a name="working-with-date-time-values-in-azure-monitor-log-queries"></a>Trabajar con valores de fecha y hora en consultas de registro de Azure Monitor

> [!NOTE]
> Debe completar la [Introducción al portal de Analytics](get-started-portal.md) y la [Introducción a las consultas en Log Analytics](get-started-queries.md) antes de completar esta lección.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

En este artículo se describe cómo trabajar con datos de fecha y hora en consultas de registro de Azure Monitor.


## <a name="date-time-basics"></a>Conceptos básicos de fecha y hora
El lenguaje de consulta de Kusto tiene dos tipos de datos principales asociados con las fechas y horas: datetime y timespan. Todas las fechas se expresan en formato UTC. Aunque se admiten varios formatos de datetime, se prefiere el formato ISO8601. 

Los valores timespan se expresan como un decimal seguido de una unidad de tiempo:

|Abreviatura   | Unidad de tiempo    |
|:---|:---|
|d           | day          |
|h           | hour         |
|m           | minute       |
|s           | second       |
|ms          | milisegundo  |
|microsegundo | microsegundo  |
|tic        | nanosegundo   |

Los valores datetime se pueden crear mediante la conversión de una cadena con el operador `todatetime`. Por ejemplo, para revisar los latidos de máquina virtual enviados en un período de tiempo específico, use el operador `between` para especificar un intervalo de tiempo.

```Kusto
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Otro escenario común es comparar un valor datetime hasta el presente. Por ejemplo, para ver todos los latidos durante los últimos dos minutos, puede usar el operador `now` junto con un valor timespan que represente dos minutos:

```Kusto
Heartbeat
| where TimeGenerated > now() - 2m
```

También está disponible un acceso directo para esta función:
```Kusto
Heartbeat
| where TimeGenerated > now(-2m)
```

Sin embargo, el método más corto y legible es usar el operador `ago`:
```Kusto
Heartbeat
| where TimeGenerated > ago(2m)
```

Supongamos que en lugar de saber la hora de inicio y finalización, conoce la hora de inicio y la duración. Puede volver a escribir la consulta como sigue:

```Kusto
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Conversión de unidades de tiempo
Quizá quiera expresar un valor datetime o timespan en una unidad de tiempo distinta de la predeterminada. Por ejemplo, si está revisando los eventos de error de los últimos 30 minutos y necesita una columna calculada que muestre cuánto tiempo hace que se produjo el evento:

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

La columna `timeAgo` contiene valores como: "00:09:31.5118992", que significa que tienen el formato hh:mm:ss.fffffff. Si desea dar formato a estos valores en `numver` de minutos desde la hora de inicio, divida ese valor por "1 minuto":

```Kusto
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Agregaciones y creación de cubos por intervalos de tiempo
Otro escenario común es la necesidad de obtener estadísticas de un período de tiempo determinado en un intervalo de agregación determinado. Para este escenario, se puede usar un operador `bin` como parte de una cláusula summarize.

Use la siguiente consulta para obtener el número de eventos que se produjeron cada 5 minutos durante la última media hora:

```Kusto
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Esta consulta genera la tabla siguiente:  

|TimeGenerated(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

Otra forma de crear cubos de resultados es usar funciones, como `startofday`:

```Kusto
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

Esta consulta genera los resultados siguientes:

|timestamp|count_|
|--|--|
|2018-07-28T00:00:00.000|7136|
|2018-07-29T00:00:00.000|12 315|
|2018-07-30T00:00:00.000|16 847|
|2018-07-31T00:00:00.000|12 616|
|2018-08-01T00:00:00.000|5416|


## <a name="time-zones"></a>Zonas horarias
Puesto que todos los valores datetime se expresan en formato UTC, a menudo resulta útil convertir estos valores al formato de la zona horaria local. Por ejemplo, use este cálculo para convertir la hora UTC a la hora estándar del Pacífico:

```Kusto
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>Funciones relacionadas

| Category | Función |
|:---|:---|
| Convertir tipos de datos | [todatetime](/azure/kusto/query/todatetimefunction)  [totimespan](/azure/kusto/query/totimespanfunction)  |
| Redondear valor al tamaño de bin | [bin](/azure/kusto/query/binfunction) |
| Obtener una fecha u hora especificas | [ago](/azure/kusto/query/agofunction) [now](/azure/kusto/query/nowfunction)   |
| Obtener parte del valor | [datetime_part](/azure/kusto/query/datetime-partfunction) [getmonth](/azure/kusto/query/getmonthfunction) [monthofyear](/azure/kusto/query/monthofyearfunction) [getyear](/azure/kusto/query/getyearfunction) [dayofmonth](/azure/kusto/query/dayofmonthfunction) [dayofweek](/azure/kusto/query/dayofweekfunction) [dayofyear](/azure/kusto/query/dayofyearfunction) [weekofyear](/azure/kusto/query/weekofyearfunction) |
| Obtener un valor de fecha relativa  | [endofday](/azure/kusto/query/endofdayfunction) [endofweek](/azure/kusto/query/endofweekfunction) [endofmonth](/azure/kusto/query/endofmonthfunction) [endofyear](/azure/kusto/query/endofyearfunction) [startofday](/azure/kusto/query/startofdayfunction) [startofweek](/azure/kusto/query/startofweekfunction) [startofmonth](/azure/kusto/query/startofmonthfunction) [startofyear](/azure/kusto/query/startofyearfunction) |

## <a name="next-steps"></a>Pasos siguientes
Consulte otras lecciones para usar el [lenguaje de consulta de Kusto](/azure/kusto/query/) con datos de registro de Azure Monitor:

- [Operaciones de cadena](string-operations.md)
- [Funciones de agregación](aggregations.md)
- [Agregaciones avanzadas](advanced-aggregations.md)
- [JSON y estructuras de datos](json-data-structures.md)
- [Escritura de consultas avanzadas](advanced-query-writing.md)
- [Combinaciones](joins.md)
- [Gráficos](charts.md)
