---
title: Consultas de alertas de registro en Azure Monitor | Microsoft Docs
description: Proporciona recomendaciones sobre cómo escribir consultas eficaces para las alertas de registro en las actualizaciones de Azure Monitor y un proceso para convertir las consultas existentes.
author: yossi-y
ms.author: yossiy
ms.topic: conceptual
ms.date: 02/19/2019
ms.subservice: alerts
ms.openlocfilehash: be2d49a824066b8926ae455978facb34c0b44310
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86505472"
---
# <a name="log-alert-queries-in-azure-monitor"></a>Consultas de alertas de registro en Azure Monitor
Las [reglas de alertas basadas en registros de Azure Monitor](alerts-unified-log.md) se ejecutan a intervalos regulares, por lo que debe asegurarse de que están escritas de manera eficiente para minimizar la sobrecarga y la latencia. Este artículo proporciona recomendaciones sobre cómo escribir consultas eficaces para las alertas de registro y un proceso para convertir las consultas existentes. 

## <a name="types-of-log-queries"></a>Tipos de consultas de registro
Las [consultas de registro de Azure Monitor](../log-query/log-query-overview.md) comienzan con una tabla o un operador de [búsqueda](/azure/kusto/query/searchoperator) o de [unión](/azure/kusto/query/unionoperator).

Por ejemplo, la siguiente consulta se limita a la tabla _SecurityEvent_ y busca el identificador de evento específico. Esta es la única tabla que debe procesar la consulta.

``` Kusto
SecurityEvent | where EventID == 4624 
```

Las consultas que empiezan por `search` o `union` le permiten buscar en varias columnas de una tabla o incluso en varias tablas. Los ejemplos siguientes muestran varios métodos para buscar el término _Memory_:

```Kusto
search "Memory"
search * | where == "Memory"
search ObjectName: "Memory"
search ObjectName == "Memory"
union * | where ObjectName == "Memory"
```

Aunque `search` y `union` son útiles durante la exploración de datos, ya que buscan términos en todo el modelo de datos, son menos eficientes que el uso de una tabla puesto que aquellos deben examinar varias tablas. Dado que las consultas de las reglas de alertas se ejecutan a intervalos regulares, esto puede dar lugar a una sobrecarga excesiva que agrega latencia a la alerta. Debido a esta sobrecarga, las consultas para las reglas de alertas de registro en Azure siempre deben empezar con una tabla que defina un ámbito claro que mejore el rendimiento de las consultas y la pertinencia de los resultados.

## <a name="unsupported-queries"></a>Consultas no admitidas
A partir del 11 de enero de 2019, la creación o modificación de reglas de alertas de registro que usen los operadores `search` o `union` no se admitirán en Azure Portal. El uso de estos operadores en una regla de alertas devolverá un mensaje de error. Este cambio no afecta a las reglas de alertas existentes ni a aquellas creadas y editadas con la API de Log Analytics. No obstante, debería considerar la posibilidad de cambiar las reglas de alertas que usen estos tipos de consultas para mejorar su eficacia.  

A las reglas de alertas del registro que usan [consultas entre recursos](../log-query/cross-workspace-query.md) no les afecta este cambio ya que estas usan `union`, que permite limitar el ámbito de la consulta a recursos específicos. Este no es igual a `union *` que no se puede usar.  El ejemplo siguiente sería válido en una regla de alertas de registro:

```Kusto
union 
app('Contoso-app1').requests, 
app('Contoso-app2').requests, 
workspace('Contoso-workspace1').Perf 
```

>[!NOTE]
>La [consulta entre recursos](../log-query/cross-workspace-query.md) en las alertas de registro se admite en la nueva [API scheduledQueryRules](/rest/api/monitor/scheduledqueryrules). De forma predeterminada, Azure Monitor usa la [API de alerta heredada de Log Analytics](api-alerts.md) para crear nuevas reglas de alerta de registro desde Azure Portal, a menos que cambie de [API de alerta de registro heredada](alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api). Después del cambio, la nueva API se convierte en la predeterminada para las nuevas reglas de alerta en Azure Portal y le permite crear reglas de alertas de registro de consulta entre recursos. Puede crear reglas de alertas de registro de [consulta entre recursos](../log-query/cross-workspace-query.md) sin realizar el cambio mediante la [plantilla de ARM de la API scheduledQueryRules](alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template); sin embargo, esta regla de alertas se puede administrar mediante la [API scheduledQueryRules](/rest/api/monitor/scheduledqueryrules) y no desde Azure Portal.

## <a name="examples"></a>Ejemplos
Los siguientes ejemplos incluyen las consultas de registro que usan `search` y `union` y proporcionan los pasos que puede usar para modificar estas consultas para su uso con las reglas de alertas.

### <a name="example-1"></a>Ejemplo 1
Desea crear una regla de alertas de registro mediante la siguiente consulta que recupera información de rendimiento mediante `search`: 

``` Kusto
search * | where Type == 'Perf' and CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```
  

Para modificar esta consulta, empiece por usar la consulta siguiente para identificar la tabla a la que pertenecen las propiedades:

``` Kusto
search * | where CounterName == '% Free Space'
| summarize by $table
```
 

El resultado de esta consulta muestra que la propiedad _CounterName_ procede de la tabla _Perf_. 

Puede usar este resultado para crear la siguiente consulta que podría usar para la regla de alertas:

``` Kusto
Perf 
| where CounterName == '% Free Space' 
| where CounterValue < 30 
| summarize count()
```


### <a name="example-2"></a>Ejemplo 2
Desea crear una regla de alertas de registro mediante la siguiente consulta que recupera información de rendimiento mediante `search`: 

``` Kusto
search ObjectName =="Memory" and CounterName=="% Committed Bytes In Use"  
| summarize Avg_Memory_Usage =avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
  

Para modificar esta consulta, empiece por usar la consulta siguiente para identificar la tabla a la que pertenecen las propiedades:

``` Kusto
search ObjectName=="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize by $table 
```
 

El resultado de esta consulta muestra que la propiedad _ObjectName_ y _CounterName_ procede de la tabla _Perf_. 

Puede usar este resultado para crear la siguiente consulta que podría usar para la regla de alertas:

``` Kusto
Perf 
| where ObjectName =="Memory" and CounterName=="% Committed Bytes In Use" 
| summarize Avg_Memory_Usage=avg(CounterValue) by Computer 
| where Avg_Memory_Usage between(90 .. 95)  
| count 
```
 

### <a name="example-3"></a>Ejemplo 3

Desea crear una regla de alertas de registro mediante la siguiente consulta que usa `search` y `union` para recuperar información de rendimiento: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| where Computer !in ((union * | where CounterName == "% Processor Utility" | summarize by Computer))
| summarize Avg_Idle_Time = avg(CounterValue) by Computer|  count  
```
 

Para modificar esta consulta, empiece por usar la consulta siguiente para identificar la tabla a la que pertenecen las propiedades de la primera parte de la consulta: 

``` Kusto
search (ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total")  
| summarize by $table 
```

El resultado de esta consulta muestra que todas estas propiedades proceden de la tabla _Perf_. 

Ahora use `union` con el comando `withsource` para identificar qué tabla de origen ha contribuido a cada fila.

``` Kusto
union withsource=table * | where CounterName == "% Processor Utility" 
| summarize by table 
```
 

El resultado de esta consulta muestra que estas propiedades también proceden de la tabla _Perf_. 

Puede usar estos resultados para crear la siguiente consulta que podría usar para la regla de alertas: 

``` Kusto
Perf 
| where ObjectName == "Processor" and CounterName == "% Idle Time" and InstanceName == "_Total" 
| where Computer !in ( 
    (Perf 
    | where CounterName == "% Processor Utility" 
    | summarize by Computer)) 
| summarize Avg_Idle_Time = avg(CounterValue) by Computer 
| count 
``` 

### <a name="example-4"></a>Ejemplo 4
Desea crear una regla de alertas de registro mediante la siguiente consulta que combina los resultados de dos consultas `search`:

```Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by Computer, Hour = bin(TimeGenerated, 1h) 
| join kind = leftouter ( 
    search in (Heartbeat) OSType == 'Windows' 
    | summarize arg_max(TimeGenerated, Computer) by Computer , Hour = bin(TimeGenerated, 1h) 
    | project Hour , Computer  
)  
on Hour 
| count 
```
 

Para modificar la consulta, empiece por usar la consulta siguiente para identificar la tabla que contiene las propiedades del lado izquierdo de la combinación: 

``` Kusto
search Type == 'SecurityEvent' and EventID == '4625' 
| summarize by $table 
```
 

El resultado indica que las propiedades del lado izquierdo de la combinación pertenecen a la tabla _SecurityEvent_. 

Ahora, use la consulta siguiente para identificar la tabla que contiene las propiedades del lado derecho de la combinación: 

 
``` Kusto
search in (Heartbeat) OSType == 'Windows' 
| summarize by $table 
```

 
El resultado indica que las propiedades del lado derecho de la combinación pertenecen a la tabla Heartbeat. 

Puede usar estos resultados para crear la siguiente consulta que podría usar para la regla de alertas: 


``` Kusto
SecurityEvent
| where EventID == '4625'
| summarize by Computer, Hour = bin(TimeGenerated, 1h)
| join kind = leftouter (
    Heartbeat  
    | where OSType == 'Windows' 
    | summarize arg_max(TimeGenerated, Computer) by Computer , Hour = bin(TimeGenerated, 1h) 
    | project Hour , Computer  
)  
on Hour 
| count 
```

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre las [alertas de registro](alerts-log.md) de Azure Monitor.
- Más información acerca de las [consultas de registro](../log-query/log-query-overview.md).
