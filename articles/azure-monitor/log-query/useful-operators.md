---
title: Operadores útiles en las consultas de registros de Azure Monitor | Microsoft Docs
description: Funciones comunes que se usan en diferentes escenarios de consultas de registros de Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/21/2018
ms.openlocfilehash: ff63b9b7027e99c70971230936ed98186c2208e8
ms.sourcegitcommit: ffa7a269177ea3c9dcefd1dea18ccb6a87c03b70
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2020
ms.locfileid: "91607269"
---
# <a name="useful-operators-in-azure-monitor-log-queries"></a>Operadores útiles en las consultas de registros de Azure Monitor

En la tabla siguiente se proporcionan algunas funciones comunes que se usan en diferentes escenarios de consultas de registros de Azure Monitor.

## <a name="useful-operators"></a>Operadores útiles

Category                                |Función apropiada de Analytics
----------------------------------------|----------------------------------------
Alias de columna y de selección            |`project`, `project-away`, `extend`
Constantes y tablas temporales          |`let scalar_alias_name = …;` <br> `let table_alias_name =  …  …  … ;`| 
Operadores de cadena y comparación         |`startswith`, `!startswith`, `has`, `!has` <br> `contains`, `!contains`, `containscs` <br> `hasprefix`, `!hasprefix`, `hassuffix`, `!hassuffix`, `in`, `!in` <br> `matches regex` <br> `==`, `=~`, `!=`, `!~`
Funciones de cadena comunes                 |`strcat()`, `replace()`, `tolower()`, `toupper()`, `substring()`, `strlen()`
Funciones matemáticas comunes                   |`sqrt()`, `abs()` <br> `exp()`, `exp2()`, `exp10()`, `log()`, `log2()`, `log10()`, `pow()` <br> `gamma()`, `gammaln()`
Análisis de texto                            |`extract()`, `extractjson()`, `parse`, `split()`
Limitación de la salida                         |`take`, `limit`, `top`, `sample`
Funciones de fecha                          |`now()`, `ago()` <br> `datetime()`, `datepart()`, `timespan` <br> `startofday()`, `startofweek()`, `startofmonth()`, `startofyear()` <br> `endofday()`, `endofweek()`, `endofmonth()`, `endofyear()` <br> `dayofweek()`, `dayofmonth()`, `dayofyear()` <br> `getmonth()`, `getyear()`, `weekofyear()`, `monthofyear()`
Agrupación y agregación                |`summarize by` <br> `max()`, `min()`, `count()`, `dcount()`, `avg()`, `sum()` <br> `stddev()`, `countif()`, `dcountif()`, `argmax()`, `argmin()` <br> `percentiles()`, `percentile_array()`
Combinaciones y uniones                        |`join kind=leftouter`, `inner`, `rightouter`, `fullouter`, `leftanti` <br> `union`
Clasificar, ordenar                             |`sort`, `order` 
Objeto dinámico (JSON y matriz)         |`parsejson()` <br> `makeset()`, `makelist()` <br> `split()`, `arraylength()` <br> `zip()`, `pack()`
Operadores lógicos                       |`and`, `or`, `iff(condition, value_t, value_f)` <br> `binary_and()`, `binary_or()`, `binary_not()`, `binary_xor()`
Machine Learning                        |`evaluate autocluster`, `basket`, `diffpatterns`, `extractcolumns`


## <a name="next-steps"></a>Pasos siguientes

- Repase una lección sobre la [escritura de consultas de registro en Azure Monitor](get-started-queries.md).
