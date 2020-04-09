---
title: Referencia rápida de KQL
description: Una lista de funciones de KQL útiles y sus definiciones con ejemplos de sintaxis.
author: orspod
ms.author: orspodek
ms.reviewer: ''
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/19/2020
ms.openlocfilehash: e7196ccccb1203ce56b2e53fa358d091374cd3f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79139086"
---
# <a name="kql-quick-reference"></a>Referencia rápida de KQL

En este artículo se muestra una lista de funciones y sus descripciones para ayudarle a comenzar a usar Kusto Query Language.

| Operador o función                               | Descripción                           | Sintaxis                                           |
| :---------------------------------------------- | :------------------------------------ |:-------------------------------------------------|
|**Filtro, búsqueda o condición**                      |**_Búsqueda de datos pertinentes mediante filtrado o búsqueda_** |                      |
| [where](/azure/kusto/query/whereoperator)                      | Filtra por un predicado específico.           | `T | where Predicate`                         |
| [where contains/has](/azure/kusto/query/whereoperator)        | `Contains`: busca cualquier coincidencia de subcadena. <br> `Has`: busca una palabra específica (mejor rendimiento).  | `T | where col1 contains/has "[search term]"`|
| [search](/azure/kusto/query/searchoperator)                    | busca el valor en todas las columnas de la tabla. | `[TabularSource |] search [kind=CaseSensitivity] [in (TableSources)] SearchPredicate` |
| [take](/azure/kusto/query/takeoperator)                        | devuelve el número de registros especificado. Úselo para probar una consulta.<br>**_Nota_** : `_take`_ y `_limit`_ son sinónimos. | `T | take NumberOfRows` |
| [case](/azure/kusto/query/casefunction)                        | Agrega una instrucción de condición, similar a if/then/elseif en otros sistemas. | `case(predicate_1, then_1, predicate_2, then_2, predicate_3, then_3, else)` |
| [distinct](/azure/kusto/query/distinctoperator)                | Genera una tabla con la combinación distinta de las columnas proporcionadas de la tabla de entrada. | `distinct [ColumnName], [ColumnName]` |
| **Date/Time**                                   |**_Operaciones que usan funciones de fecha y hora_**               |                          |
|[ago](/azure/kusto/query/agofunction)                           | Devuelve el desfase horario en relación con la hora a la que se ejecuta la consulta. Por ejemplo, `ago(1h)` es una hora antes de la lectura del reloj actual. | `ago(a_timespan)` |
| [format_datetime](/azure/kusto/query/format-datetimefunction)  | Devuelve datos en [varios formatos de fecha](/azure/kusto/query/format-datetimefunction#supported-formats). | `format_datetime(datetime , format)` |
| [bin](/azure/kusto/query/binfunction)                          | Redondea todos los valores de un período de tiempo y los agrupa. | `bin(value,roundTo)` |
| **Creación o eliminación de columnas**                   |**_Adición o eliminación de columnas de una tabla_** |                                                    |
| [print](/azure/kusto/query/printoperator)                      | Genera una sola fila con una o más expresiones escalares. | `print [ColumnName =] ScalarExpression [',' ...]` |
| [project](/azure/kusto/query/projectoperator)                  | Selecciona las columnas que se van a incluir en el orden especificado. | `T | project ColumnName [= Expression] [, ...]` <br> Or <br> `T | project [ColumnName | (ColumnName[,]) =] Expression [, ...]` |
| [project-away](/azure/kusto/query/projectawayoperator)         | Selecciona las columnas que se van a excluir de la salida. | `T | project-away ColumnNameOrPattern [, ...]` |
| [extend](/azure/kusto/query/extendoperator)                    | Crea una columna calculada y la agrega al conjunto de resultados. | `T | extend [ColumnName | (ColumnName[, ...]) =] Expression [, ...]` |
| **Ordenación y agregación del conjunto de datos**                 |**_Reestructuración de los datos mediante su ordenación y agrupación de formas significativas_**|                  |
| [sort](/azure/kusto/query/sortoperator)                        | Ordena las filas de la tabla de entrada por una o varias columnas en orden ascendente o descendente. | `T | sort by expression1 [asc|desc], expression2 [asc|desc], …` |
| [top](/azure/kusto/query/topoperator)                          | Devuelve las n primeras filas del conjunto de datos cuando este se ordena mediante `by` | `T | top numberOfRows by expression [asc|desc] [nulls first|last]` |
| [summarize](/azure/kusto/query/summarizeoperator)              | Agrupa las filas según las columnas de grupo `by` y calcula las agregaciones en cada grupo. | `T | summarize [[Column =] Aggregation [, ...]] [by [Column =] GroupExpression [, ...]]` |
| [count](/azure/kusto/query/countoperator)                       | Cuenta los registros de la tabla de entrada (por ejemplo, T).<br>Este operador es una abreviatura de `summarize count() `.| `T | count` |
| [join](/azure/kusto/query/joinoperator)                        | Combina las filas de dos tablas para formar una nueva tabla haciendo coincidir los valores de las columnas especificadas de cada tabla. Admite una gama completa de tipos de combinación: `flouter`, `inner`, `innerunique`, `leftanti`, `leftantisemi`, `leftouter`, `leftsemi`, `rightanti`, `rightantisemi`, `rightouter`, `rightsemi` | `LeftTable | join [JoinParameters] ( RightTable ) on Attributes` |
| [union](/azure/kusto/query/unionoperator)                      | Toma dos o más tablas y devuelve todas sus filas. | `[T1] | union [T2], [T3], …` |
| [range](/azure/kusto/query/rangeoperator)                      | Genera una tabla con una serie aritmética de valores. | `range columnName from start to stop step step` |
| **Aplicación de formato a los datos**                                 | **_Reestructuración de los datos para que se generen de forma útil_** | |
| [lookup](/azure/kusto/query/lookupoperator)                    | Extiende las columnas de una tabla de hechos con valores buscados en una tabla de dimensiones. | `T1 | lookup [kind = (leftouter|inner)] ( T2 ) on Attributes` |
| [mv-expand](/azure/kusto/query/mvexpandoperator)               | Convierte las matrices dinámicas en filas (expansión de varios valores). | `T | mv-expand Column` |
| [parse](/azure/kusto/query/parseoperator)                      | evalúa una expresión de cadena y analiza su valor en una o más columnas calculadas. Úselo para estructurar datos no estructurados. | `T | parse [kind=regex  [flags=regex_flags] |simple|relaxed] Expression with * (StringConstant ColumnName [: ColumnType]) *...` |
| [make-series](/azure/kusto/query/make-seriesoperator)          | Crea una serie de valores agregados especificados a lo largo de un eje definido. | `T | make-series [MakeSeriesParamters] [Column =] Aggregation [default = DefaultValue] [, ...] on AxisColumn from start to end step step [by [Column =] GroupExpression [, ...]]` |
| [let](/azure/kusto/query/letstatement)                         | Enlaza un nombre a expresiones que pueden hacer referencia a su valor enlazado. Los valores pueden ser expresiones lambda para crear funciones ad-hoc como parte de la consulta. Use `let` para crear expresiones sobre tablas cuyos resultados se parezcan a una nueva tabla. | `let Name = ScalarExpression | TabularExpression | FunctionDefinitionExpression` |
| **General**                                     | **_Operaciones y funciones mixtas_** | |
| [invoke](/azure/kusto/query/invokeoperator)                    | Ejecuta la función en la tabla que recibe como entrada. | `T | invoke function([param1, param2])` |
| [evaluate pluginName](/azure/kusto/query/evaluateoperator)     | Evalúa las extensiones del lenguaje de consulta (complementos). | `[T |] evaluate [ evaluateParameters ] PluginName ( [PluginArg1 [, PluginArg2]... )` |
| **Visualización**                               | **_Operaciones que muestran los datos en formato gráfico_** | |
| [render](/azure/kusto/query/renderoperator) | Representa los resultados como una salida gráfica. | `T | render Visualization [with (PropertyName = PropertyValue [, ...] )]` |
