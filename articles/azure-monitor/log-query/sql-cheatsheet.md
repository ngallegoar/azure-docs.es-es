---
title: Hoja de referencia rápida de consultas de registro de SQL en Azure Monitor | Microsoft Docs
description: Ayuda para usuarios familiarizados con SQL para escribir consultas de registro en Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/21/2018
ms.openlocfilehash: c76ab145fd2fdd077075b345ecac9c6a473f2369
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "75365196"
---
# <a name="sql-to-azure-monitor-log-query-cheat-sheet"></a>Hoja de referencia rápida de consultas de registro de SQL en Azure Monitor 

La tabla siguiente sirve de ayuda para que los usuarios que están familiarizados con SQL obtengan información sobre el lenguaje de consulta de Kusto para escribir consultas de registro en Azure Monitor. Eche un vistazo al comando T-SQL para resolver escenarios comunes y su equivalente en una consulta de registro de Azure Monitor.

## <a name="sql-to-azure-monitor"></a>De SQL a Azure Monitor

Descripción                             |Consulta SQL                                                                                          |Consulta de registro de Azure Monitor
----------------------------------------|---------------------------------------------------------------------------------------------------|----------------------------------------
Seleccionar todos los datos de una tabla            |`SELECT * FROM dependencies`                                                                       |<code>dependencies</code>
Selección de columnas específicas en una tabla    |`SELECT name, resultCode FROM dependencies`                                                        |<code>dependencies <br>&#124; project name, resultCode</code>
Seleccionar 100 registros de una tabla         |`SELECT TOP 100 * FROM dependencies`                                                               |<code>dependencies <br>&#124; take 100</code>
Evaluación de un valor nulo                         |`SELECT * FROM dependencies WHERE resultCode IS NOT NULL`                                          |<code>dependencies <br>&#124; where isnotnull(resultCode)</code>
Comparación de cadenas: igualdad             |`SELECT * FROM dependencies WHERE name = "abcde"`                                                  |<code>dependencies <br>&#124; where name == "abcde"</code>
Comparación de cadenas: subcadena            |`SELECT * FROM dependencies WHERE name like "%bcd%"`                                                   |<code>dependencies <br>&#124; where name contains "bcd"</code>
Comparación de cadenas: carácter comodín             |`SELECT * FROM dependencies WHERE name like "abc%"`                                                |<code>dependencies <br>&#124; where name startswith "abc"</code>
Comparación de fechas: último día             |`SELECT * FROM dependencies WHERE timestamp > getdate()-1`                                         |<code>dependencies <br>&#124; where timestamp > ago(1d)</code>
Comparación de fechas: intervalo de fechas             |`SELECT * FROM dependencies WHERE timestamp BETWEEN '2016-10-01' AND '2016-11-01'`                 |<code>dependencies <br>&#124; where timestamp between (datetime(2016-10-01) .. datetime(2016-10-01))</code>
Comparación de valores booleanos                      |`SELECT * FROM dependencies WHERE !(success)`                                                      |<code>dependencies <br>&#124; where success == "False" </code>
Sort                                    |`SELECT name, timestamp FROM dependencies ORDER BY timestamp asc`                                  |<code>dependencies <br>&#124; order by timestamp asc </code>
Distinct                                |`SELECT DISTINCT name, type  FROM dependencies`                                                    |<code>dependencies <br>&#124; summarize by name, type </code>
Agrupación, agregación                   |`SELECT name, AVG(duration) FROM dependencies GROUP BY name`                                       |<code>dependencies <br>&#124; summarize avg(duration) by name </code>
Alias de columna, extensión                  |`SELECT operation_Name as Name, AVG(duration) as AvgD FROM dependencies GROUP BY name`             |<code>dependencies <br>&#124; summarize AvgD=avg(duration) by operation_Name <br>&#124; project Name=operation_Name, AvgD</code>
Primeros n registros por medida                |`SELECT TOP 100 name, COUNT(*) as Count FROM dependencies GROUP BY name ORDER BY Count asc`        |<code>dependencies <br>&#124; summarize Count=count() by name <br>&#124; top 100 by Count asc</code>
Union                                   |`SELECT * FROM dependencies UNION SELECT * FROM exceptions`                                        |<code>union dependencies, exceptions</code>
Unión: con condiciones                  |`SELECT * FROM dependencies WHERE value > 4 UNION SELECT * FROM exceptions WHERE value < 5`                |<code>dependencies <br>&#124; where value > 4 <br>&#124; union (exceptions <br>&#124; where value < 5)</code>
Join                                    |`SELECT * FROM dependencies JOIN exceptions ON dependencies.operation_Id = exceptions.operation_Id`|<code>dependencies <br>&#124; join (exceptions) on operation_Id == operation_Id</code>


## <a name="next-steps"></a>Pasos siguientes

- Consulte las lecciones sobre la [escritura de consultas de registro en Azure Monitor](get-started-queries.md).
