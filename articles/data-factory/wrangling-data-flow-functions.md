---
title: Funciones de transformación de flujos de datos de limpieza y transformación en Azure Data Factory
description: Introducción a las funciones disponibles de los flujos de datos de limpieza y transformación en Azure Data Factory
author: djpmsft
ms.author: daperlov
ms.reviewer: gamal
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/01/2019
ms.openlocfilehash: e2517ec4a02a5d61fb3ce1d9ca9ffa2b5f4e8bf8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74287027"
---
# <a name="transformation-functions-in-wrangling-data-flow"></a>Funciones de transformación en flujos de datos de limpieza y transformación

El flujo de datos de limpieza y transformación en Azure Data Factory permite la ágil preparación, y limpieza y transformación de datos sin código a escala de nube. El flujo de datos de limpieza y transformación se integra en [Power Query Online](https://docs.microsoft.com/powerquery-m/power-query-m-reference) y pone las funciones de Power Query M a disposición para la limpieza y transformación de datos a través de la ejecución de Spark. 

Actualmente no se admiten todas las funciones de Power Query M para la limpieza y transformación de datos, a pesar de estar disponibles durante la creación. Al compilar los flujos de datos de limpieza y transformación, aparecerá el siguiente mensaje de error si no se admite una función:

`The wrangling data flow is invalid. Expression.Error: The transformation logic isn't supported. Please try a simpler expression`

A continuación se muestra una lista de funciones admitidas de Power Query M.

## <a name="column-management"></a>Administración de columnas

* Selección: [Table.SelectColumns](https://docs.microsoft.com/powerquery-m/table-selectcolumns)
* Eliminación: [Table.RemoveColumns](https://docs.microsoft.com/powerquery-m/table-removecolumns)
* Cambio de nombre: [Table.RenameColumns](https://docs.microsoft.com/powerquery-m/table-renamecolumns), [Table.PrefixColumns](https://docs.microsoft.com/powerquery-m/table-prefixcolumns), [Table.TransformColumnNames](https://docs.microsoft.com/powerquery-m/table-transformcolumnnames)
* Reordenación: [Table.ReorderColumns](https://docs.microsoft.com/powerquery-m/table-reordercolumns)

## <a name="row-filtering"></a>Filtrado de filas

Use la función de M [Table.SelectRows](https://docs.microsoft.com/powerquery-m/table-selectrows) para filtrar según las condiciones siguientes:

* Igualdad y desigualdad
* Comparaciones numéricas, de texto y de fechas (pero no de fecha y hora)
* Información numérica, como [Number.IsEven](https://docs.microsoft.com/powerquery-m/number-iseven)/[Odd](https://docs.microsoft.com/powerquery-m/number-iseven)
* Contención de texto mediante [Text.Contains](https://docs.microsoft.com/powerquery-m/text-contains), [Text.StartsWith](https://docs.microsoft.com/powerquery-m/text-startswith) o [Text.EndsWith](https://docs.microsoft.com/powerquery-m/text-endswith)
* Intervalos de fechas (incluidas todas las [funciones de fecha](https://docs.microsoft.com/powerquery-m/date-functions) "IsIn") 
* Combinaciones de estas mediante las condiciones and, or o not

## <a name="adding-and-transforming-columns"></a>Adición y transformación de columnas

Las siguientes funciones de M agregan o transforman columnas: [Table.AddColumn](https://docs.microsoft.com/powerquery-m/table-addcolumn), [Table.TransformColumns](https://docs.microsoft.com/powerquery-m/table-transformcolumns), [Table.ReplaceValue](https://docs.microsoft.com/powerquery-m/table-replacevalue), [Table.DuplicateColumn](https://docs.microsoft.com/powerquery-m/table-duplicatecolumn). A continuación se muestran las funciones de transformación admitidas.

* Aritmética numérica
* Concatenación de texto
* Aritmética de fecha y hora (operadores aritméticos, [Date.AddDays](https://docs.microsoft.com/powerquery-m/date-adddays), [Date.AddMonths](https://docs.microsoft.com/powerquery-m/date-addmonths), [Date.AddQuarters](https://docs.microsoft.com/powerquery-m/date-addquarters), [Date.AddWeeks](https://docs.microsoft.com/powerquery-m/date-addweeks), [Date.AddYears](https://docs.microsoft.com/powerquery-m/date-addyears))
* Las duraciones se pueden usar para operaciones aritméticas de fecha y hora, pero se deben transformar en otro tipo antes de que se escriban en un receptor (operadores aritméticos, [#duration](https://docs.microsoft.com/powerquery-m/sharpduration), [Duration.Days](https://docs.microsoft.com/powerquery-m/duration-days), [Duration.Hours](https://docs.microsoft.com/powerquery-m/duration-hours), [Duration.Minutes](https://docs.microsoft.com/powerquery-m/duration-minutes), [Duration.Seconds](https://docs.microsoft.com/powerquery-m/duration-seconds), [Duration.TotalDays](https://docs.microsoft.com/powerquery-m/duration-totaldays), [Duration.TotalHours](https://docs.microsoft.com/powerquery-m/duration-totalhours), [Duration.TotalMinutes](https://docs.microsoft.com/powerquery-m/duration-totalminutes), [Duration.TotalSeconds](https://docs.microsoft.com/powerquery-m/duration-totalseconds))    
* La mayoría de las funciones numéricas estándar, científicas y trigonométricas (todas las funciones de [Operaciones](https://docs.microsoft.com/powerquery-m/number-functions#operations), [Redondeo](https://docs.microsoft.com/powerquery-m/number-functions#rounding) y [Trigonometría](https://docs.microsoft.com/powerquery-m/number-functions#trigonometry) *excepto* Number.Factorial, Number.Permutations y Number.Combinations)
* Reemplazo ([Replacer.ReplaceText](https://docs.microsoft.com/powerquery-m/replacer-replacetext), [Replacer.ReplaceValue](https://docs.microsoft.com/powerquery-m/replacer-replacevalue), [Text.Replace](https://docs.microsoft.com/powerquery-m/text-replace), [Text.Remove](https://docs.microsoft.com/powerquery-m/text-remove))
* Extracción de texto posicional ([Text.PositionOf](https://docs.microsoft.com/powerquery-m/text-positionof), [Text.Length](https://docs.microsoft.com/powerquery-m/text-length), [Text.Start](https://docs.microsoft.com/powerquery-m/text-start), [Text.End](https://docs.microsoft.com/powerquery-m/text-end), [Text.Middle](https://docs.microsoft.com/powerquery-m/text-middle), [Text.ReplaceRange](https://docs.microsoft.com/powerquery-m/text-replacerange), [Text.RemoveRange](https://docs.microsoft.com/powerquery-m/text-removerange))
* Formato básico de texto ([Text.Lower](https://docs.microsoft.com/powerquery-m/text-lower), [Text.Upper](https://docs.microsoft.com/powerquery-m/text-upper), [Text.Trim](https://docs.microsoft.com/powerquery-m/text-trim)/[Start](https://docs.microsoft.com/powerquery-m/text-trimstart)/[End](https://docs.microsoft.com/powerquery-m/text-trimend), [Text.PadStart](https://docs.microsoft.com/powerquery-m/text-padstart)/[End](https://docs.microsoft.com/powerquery-m/text-padend), [Text.Reverse](https://docs.microsoft.com/powerquery-m/text-reverse))
* Funciones de fecha y hora ([Date.Day](https://docs.microsoft.com/powerquery-m/date-day), [Date.Month](https://docs.microsoft.com/powerquery-m/date-month), [Date.Year](https://docs.microsoft.com/powerquery-m/date-year) [Time.Hour](https://docs.microsoft.com/powerquery-m/time-hour), [Time.Minute](https://docs.microsoft.com/powerquery-m/time-minute), [Time.Second](https://docs.microsoft.com/powerquery-m/time-second), [Date.DayOfWeek](https://docs.microsoft.com/powerquery-m/date-dayofweek), [Date.DayOfYear](https://docs.microsoft.com/powerquery-m/date-dayofyear), [Date.DaysInMonth](https://docs.microsoft.com/powerquery-m/date-daysinmonth))
* Expresiones if (pero las ramas deben tener tipos coincidentes)
* Filtros de fila como columna lógica
* Constantes de número, texto, lógica, fecha, y fecha y hora

<a name="mergingjoining-tables"></a>Combinar o unir tablas
----------------------
* Power Query generará una combinación anidada (Table.NestedJoin; los usuarios también pueden escribir manualmente [Table.AddJoinColumn](https://docs.microsoft.com/powerquery-m/table-addjoincolumn)).
    Los usuarios deben expandir la columna de combinación anidada en una combinación no anidada (Table.ExpandTableColumn no se admite en ningún otro contexto).
* La función de M [Table.join](https://docs.microsoft.com/powerquery-m/table-join) puede escribirse directamente para evitar la necesidad de un paso adicional de expansión, pero el usuario debe asegurarse de que no haya nombres de columna duplicados entre las tablas combinadas.
* Tipos de combinación admitidos:   [Inner](https://docs.microsoft.com/powerquery-m/joinkind-inner), [LeftOuter](https://docs.microsoft.com/powerquery-m/joinkind-leftouter), [RightOuter](https://docs.microsoft.com/powerquery-m/joinkind-rightouter), [FullOuter](https://docs.microsoft.com/powerquery-m/joinkind-fullouter)
* Tanto [Value.Equals](https://docs.microsoft.com/powerquery-m/value-equals) como [Value.NullableEquals](https://docs.microsoft.com/powerquery-m/value-nullableequals) se admiten como comparadores de igualdad de claves

## <a name="group-by"></a>Agrupar por

Utilice [Table.Group](https://docs.microsoft.com/powerquery-m/table-group) para agregar valores.
* Debe usarse con una función de agregación
* Funciones de agregación compatibles:   [Table.RowCount](https://docs.microsoft.com/powerquery-m/table-rowcount), [List.Sum](https://docs.microsoft.com/powerquery-m/list-sum), [List.Count](https://docs.microsoft.com/powerquery-m/list-count), [List.Average](https://docs.microsoft.com/powerquery-m/list-average), [List.Min](https://docs.microsoft.com/powerquery-m/list-min), [List.Max](https://docs.microsoft.com/powerquery-m/list-max), [List.StandardDeviation](https://docs.microsoft.com/powerquery-m/list-standarddeviation), [List.First](https://docs.microsoft.com/powerquery-m/list-first), [List.Last](https://docs.microsoft.com/powerquery-m/list-last)

## <a name="sorting"></a>Ordenación

Use [Table.Sort](https://docs.microsoft.com/powerquery-m/table-sort) para ordenar los valores.

## <a name="reducing-rows"></a>Reducir filas

Mantener y quitar la parte superior, mantener el rango (funciones M correspondientes, solo se admiten recuentos, no condiciones: [Table.FirstN](https://docs.microsoft.com/powerquery-m/table-firstn), [Table.Skip](https://docs.microsoft.com/powerquery-m/table-skip), [Table.RemoveFirstN](https://docs.microsoft.com/powerquery-m/table-removefirstn), [Table.Range](https://docs.microsoft.com/powerquery-m/table-range), [Table.MinN](https://docs.microsoft.com/powerquery-m/table-minn), [Table.MaxN](https://docs.microsoft.com/powerquery-m/table-maxn))

## <a name="known-unsupported-functions"></a>Funciones conocidas no admitidas

| Función | Status |
| -- | -- |
| Table.PromoteHeaders | No compatible. Se puede lograr el mismo resultado si se establece "Primera fila como encabezado" en el conjunto de resultados. |
| Table.CombineColumns | Se trata de un escenario habitual que no se admite directamente, pero se puede realizar si se agrega una nueva columna que concatene dos columnas concretas.  Por ejemplo, Table.AddColumn(RemoveEmailColumn, “Name”, each [FirstName] & ” ” & [LastName]) |
| Table.TransformColumnTypes | Esto se admite en la mayoría de los casos. No se admiten los siguientes escenarios: transformar una cadena al tipo de moneda, transformar una cadena al tipo de hora, transformar una cadena al tipo de porcentaje. |
| Table.NestedJoin | Si realiza una combinación, se producirá un error de validación. Las columnas deben expandirse para que funcione. |
| Table.Distinct | No se admite la eliminación de filas duplicadas. |
| Table.RemoveLastN | No se admite la eliminación de las filas inferiores. |
| Table.RowCount | No se admite, pero se puede realizar si agrega una columna con todas las celdas vacías (se puede usar la columna condición) y, a continuación, usa Agrupar por en esa columna. Se admite Table.Group. | 
| Control de errores de nivel de fila | El control de errores de nivel de fila no se admite actualmente. Por ejemplo, para filtrar los valores no numéricos de una columna, una opción sería transformar la columna de texto en números. Cada celda que no se pueda transformar tendrá un estado de error y debe filtrarse. Este escenario no es posible en el flujo de datos de limpieza y transformación. |
| Table.Transpose | No compatible |
| Table.Pivot | No compatible |

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [crear un flujo de datos de limpieza y transformación](wrangling-data-flow-tutorial.md).