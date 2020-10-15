---
title: Consulta de registro de Splunk en Azure Monitor | Microsoft Docs
description: Ayuda para los usuarios que están familiarizados con Splunk a la hora de obtener información sobre las consultas de registro de Azure Monitor.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/21/2018
ms.openlocfilehash: 00fdaf93553c97112c67caa66cb2246756b63c33
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86207479"
---
# <a name="splunk-to-azure-monitor-log-query"></a>Consulta de registro de Splunk en Azure Monitor

Este artículo sirve de ayuda para que los usuarios que están familiarizados con Splunk obtengan información sobre el lenguaje de consulta de Kusto para escribir consultas de registro en Azure Monitor. Se realizan comparaciones directas entre ambos para conocer las principales diferencias y similitudes donde puede aprovechar los conocimientos que posee.

## <a name="structure-and-concepts"></a>Estructura y conceptos

En la tabla siguiente se comparan los conceptos y las estructuras de datos en los registros de Splunk y Azure Monitor.

 | Concepto  | Splunk | Azure Monitor |  Comentario
 | --- | --- | --- | ---
 | Unidad de implementación  | cluster |  cluster |  Azure Monitor permite consultas arbitrarias entre clústeres. Splunk no lo permite. |
 | Memorias caché de datos |  cubos  |  Directivas de retención y almacenamiento en caché |  Controla el período y el nivel de almacenamiento en caché de los datos. Este valor afecta directamente al rendimiento de las consultas y al costo de la implementación. |
 | Partición lógica de los datos  |  índice  |  database  |  Permite la separación lógica de los datos. Ambas implementaciones permiten uniones y la combinación de estas particiones. |
 | Metadatos de eventos estructurados | N/D | table |  Splunk no tiene el concepto que se expone al lenguaje de búsqueda de metadatos de eventos. Los registros de Azure Monitor siguen el concepto de tabla, con columnas. Cada instancia de un evento se asigna a una fila. |
 | Registro de datos | event | fila |  Solo cambio de terminología. |
 | Atributo de registro de datos | campo |  columna |  En Azure Monitor, se predefine como parte de la estructura de la tabla. En Splunk, cada evento tiene su propio conjunto de campos. |
 | Tipos | tipo de datos |  tipo de datos |  Los tipos de datos de Azure Monitor son más explícitos, ya que se establecen en las columnas. Ambos tienen la capacidad de trabajar dinámicamente con tipos de datos y con un conjunto más o menos equivalente de tipos de datos, lo que incluye la compatibilidad con JSON. |
 | Búsqueda y consulta  | búsqueda | Query |  Los conceptos son esencialmente los mismos en Azure Monitor y Splunk. |
 | Tiempo de ingesta de eventos | Hora del sistema | ingestion_time() |  En Splunk, cada evento obtiene una marca de tiempo del sistema del tiempo en que el evento se indexó. En Azure Monitor, se puede definir una directiva denominada ingestion_time que expone una columna del sistema a la que se puede hacer referencia a través de la función ingestion_time(). |

## <a name="functions"></a>Functions

En la tabla siguiente se especifican las funciones de Azure Monitor que son equivalentes a funciones de Splunk.

|Splunk | Azure Monitor |Comentario
|---|---|---
|strcat | strcat()| (1) |
|split  | split() | (1) |
|if     | iff()   | (1) |
|tonumber | todouble()<br>tolong()<br>toint() | (1) |
|upper<br>lower |toupper()<br>tolower()|(1) |
| replace | replace() | (1)<br> Tenga también en cuenta que aunque `replace()` usa tres parámetros en ambos productos, los parámetros son diferentes. |
| substr | substring() | (1)<br>Tenga en cuenta también que Splunk utiliza índices con base uno. Azure Monitor usa índices con base cero. |
| tolower |  tolower() | (1) |
| toupper | toupper() | (1) |
| match | matches regex |  (2)  |
| regex | matches regex | En Splunk, `regex` es un operador. En Azure Monitor, es un operador relacional. |
| searchmatch | == | En Splunk, `searchmatch` permite buscar la cadena exacta.
| random | rand()<br>rand(n) | La función de Splunk devuelve un número entre cero y 2<sup>31</sup>-1. Azure Monitor devuelve un número entre 0,0 y 1,0, o si se ha proporcionado un parámetro, entre 0 y n-1.
| ahora | now() | (1)
| relative_time | totimespan() | (1)<br>En Azure Monitor, el equivalente de Splunk de relative_time (datetimeVal, offsetVal) es datetimeVal + totimespan(offsetVal).<br>Por ejemplo, <code>search &#124; eval n=relative_time(now(), "-1d@d")</code> se convierte en <code>...  &#124; extend myTime = now() - totimespan("1d")</code>.

(1) En Splunk, se invoca la función con el operador `eval`. En Azure Monitor, se usa como parte de `extend` o `project`.<br>(2) En Splunk, se invoca la función con el operador `eval`. En Azure Monitor, se puede usar con el operador `where`.


## <a name="operators"></a>Operadores

En las secciones siguientes se proporcionan ejemplos del uso de distintos operadores entre Splunk y Azure Monitor.

> [!NOTE]
> Para el ejemplo siguiente, el campo de Splunk _rule_ se asigna a una tabla de Azure Monitor y la marca de tiempo predeterminada de Splunk se asigna a la columna _ingestion_time()_ de Logs Analytics.

### <a name="search"></a>Search
En Splunk se puede omitir la palabra clave `search` y especifique una cadena sin comillas. En Azure Monitor todas las consultas deben comenzar por `find`, una cadena sin comillas es un nombre de columna y el valor de búsqueda debe ser una cadena entre comillas. 

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **search** | <code>search Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" earliest=-24h</code> |
| **Azure Monitor** | **find** | <code>find Session.Id=="c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time()> ago(24h)</code> |


### <a name="filter"></a>Filter
Las consultas de registro de Azure Monitor comienzan en un conjunto de resultados tabulares donde se establece el filtro. En Splunk, el filtrado es la operación predeterminada del índice actual. En Splunk también se puede usar el operador `where`, pero no es aconsejable.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **search** | <code>Event.Rule="330009.2" Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" _indextime>-24h</code> |
| **Azure Monitor** | **where** | <code>Office_Hub_OHubBGTaskError<br>&#124; where Session_Id == "c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time() > ago(24h)</code> |

### <a name="getting-n-eventsrows-for-inspection"></a>Obtención de n eventos o filas para su inspección 
Las consultas de registro de Azure Monitor también admiten `take` como alias para `limit`. En Splunk, si los resultados están ordenados, `head` devolverá los primeros n resultados. En Azure Monitor, el límite no está ordenado, pero devuelve las n primeras filas que se encuentran.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **head** | <code>Event.Rule=330009.2<br>&#124; head 100</code> |
| **Azure Monitor** | **limit** | <code>Office_Hub_OHubBGTaskError<br>&#124; limit 100</code> |

### <a name="getting-the-first-n-eventsrows-ordered-by-a-fieldcolumn"></a>Obtención de los n primeros eventos o filas ordenados por una columna o campo
Para los resultados inferiores, en Splunk se usa `tail`. En Azure Monitor se puede especificar la dirección de ordenación con `asc`.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **head** |  <code>Event.Rule="330009.2"<br>&#124; sort Event.Sequence<br>&#124; head 20</code> |
| **Azure Monitor** | **top** | <code>Office_Hub_OHubBGTaskError<br>&#124; top 20 by Event_Sequence</code> |

### <a name="extending-the-result-set-with-new-fieldscolumns"></a>Extensión del conjunto de resultados con nuevos campos o columnas
Splunk también tiene una función `eval`, que no se debe comparar con el operador `eval`. Tanto el operador `eval` de Splunk como el operador `extend` de Azure Monitor solo admiten operadores aritméticos y funciones escalares.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **eval** |  <code>Event.Rule=330009.2<br>&#124; eval state= if(Data.Exception = "0", "success", "error")</code> |
| **Azure Monitor** | **extend** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend state = iif(Data_Exception == 0,"success" ,"error")</code> |

### <a name="rename"></a>Cambiar nombre 
Azure Monitor usa el operador `project-rename` para cambiar el nombre de un campo. `project-rename` permite a la consulta aprovechar los índices pregenerados para un campo. Splunk tiene el operador `rename` para hacer lo mismo.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **rename** |  <code>Event.Rule=330009.2<br>&#124; rename Date.Exception as execption</code> |
| **Azure Monitor** | **project-rename** | <code>Office_Hub_OHubBGTaskError<br>&#124; project-rename exception = Date_Exception</code> |

### <a name="format-resultsprojection"></a>Formato de resultados o proyección
Splunk no parece tener un operador similar a `project-away`. Puede usar la interfaz de usuario para filtrar campos y eliminarlos.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **table** |  <code>Event.Rule=330009.2<br>&#124; table rule, state</code> |
| **Azure Monitor** | **project**<br>**project-away** | <code>Office_Hub_OHubBGTaskError<br>&#124; project exception, state</code> |

### <a name="aggregation"></a>Agregación
Consulte [Agregaciones en consultas de registros de Azure Monitor](aggregations.md) para conocer las distintas funciones de agregación.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **stats** |  <code>search (Rule=120502.*)<br>&#124; stats count by OSEnv, Audience</code> |
| **Azure Monitor** | **summarize** | <code>Office_Hub_OHubBGTaskError<br>&#124; summarize count() by App_Platform, Release_Audience</code> |


### <a name="join"></a>Join
En Splunk, la combinación tiene limitaciones importantes. La subconsulta tiene un límite de 10 000 resultados (definido en el archivo de configuración de implementación), y existe un número limitado de tipos de combinación.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **join** |  <code>Event.Rule=120103* &#124; stats by Client.Id, Data.Alias \| join Client.Id max=0 [search earliest=-24h Event.Rule="150310.0" Data.Hresult=-2147221040]</code> |
| **Azure Monitor** | **join** | <code>cluster("OAriaPPT").database("Office PowerPoint").Office_PowerPoint_PPT_Exceptions<br>&#124; where  Data_Hresult== -2147221040<br>&#124; join kind = inner (Office_System_SystemHealthMetadata<br>&#124; summarize by Client_Id, Data_Alias)on Client_Id</code>   |

### <a name="sort"></a>Sort
En Splunk, para ordenar de forma ascendente es preciso usar el operador `reverse`. Azure Monitor también permite definir dónde se colocan valores NULL, al principio o al final.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **sort** |  <code>Event.Rule=120103<br>&#124; sort Data.Hresult<br>&#124; reverse</code> |
| **Azure Monitor** | **order by** | <code>Office_Hub_OHubBGTaskError<br>&#124; order by Data_Hresult,  desc</code> |

### <a name="multivalue-expand"></a>Expansión de varios valores
Se trata de un operador similar en Splunk y Azure Monitor.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **mvexpand** |  `mvexpand foo` |
| **Azure Monitor** | **mvexpand** | `mvexpand foo` |

### <a name="results-facets-interesting-fields"></a>Facetas de resultados, campos interesantes
En Log Analytics en Azure Portal, solo se expone la primera columna. Todas las columnas están disponibles a través de la API.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **fields** |  <code>Event.Rule=330009.2<br>&#124; fields App.Version, App.Platform</code> |
| **Azure Monitor** | **facets** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; facet by App_Branch, App_Version</code> |

### <a name="de-duplicate"></a>Desduplicación
Puede usar `summarize arg_min()` en su lugar para invertir el orden en que se elige el registro.

| | Operator | Ejemplo |
|:---|:---|:---|
| **Splunk** | **dedup** |  <code>Event.Rule=330009.2<br>&#124; dedup device_id sortby -batterylife</code> |
| **Azure Monitor** | **summarize arg_max()** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; summarize arg_max(batterylife, *) by device_id</code> |

## <a name="next-steps"></a>Pasos siguientes

- Repase una lección sobre la [escritura de consultas de registro en Azure Monitor](get-started-queries.md).
