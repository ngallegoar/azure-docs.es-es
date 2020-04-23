---
title: Transformación Agregar en Asignación de Data Flow
description: Obtenga información sobre cómo agregar datos a escala en Azure Data Factory con la transformación Agregar en Asignación de Data Flow.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: 871f2b49e2dce9d762ef8a54923da04b0f24e4be
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606531"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Transformación Agregar en Asignación de Data Flow

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En la transformación Agregar se definen las agregaciones de columnas de flujos de datos. Mediante el Generador de expresiones, puede definir diferentes tipos de agregaciones, tales como SUM, MIN, MAX y COUNT agrupados por columnas calculadas o existentes.

## <a name="group-by"></a>Agrupar por

Seleccione una columna existente o cree una nueva columna calculada para usarla como una cláusula Agrupar por para la agregación. Para usar una columna existente, selecciónela en la lista desplegable. Para crear una nueva columna calculada, mantenga el mouse sobre la cláusula y haga clic en **Columna calculada**. Se abrirá el [generador de expresiones de flujo de datos](concepts-data-flow-expression-builder.md). Una vez creada la columna calculada, escriba el nombre de la columna de salida en el campo **Name as** (Nombre como). Si desea agregar una cláusula Agrupar por adicional, mantenga el mouse sobre una cláusula existente y haga clic en el icono "más".

![Configuración de Agrupar por de una transformación Agregar](media/data-flow/agg.png "Configuración de Agrupar por de una transformación Agregar")

Una cláusula Agrupar por es opcional en una transformación Agregar.

## <a name="aggregate-column"></a>Columna de agregación 

Vaya a la pestaña **Agregados** para generar las expresiones de agregación. Puede sobrescribir una columna existente con una agregación, o bien crear un campo nuevo con un nombre nuevo. La expresión de agregación se escribirá en el cuadro de la derecha, junto al selector de nombre de columna. Para editar la expresión, haga clic en el cuadro de texto para abrir el Generador de expresiones. Para incorporar una agregación adicional, mantenga el mouse sobre una expresión existente y haga clic en icono "más" para crear una nueva columna de agregación o un [patrón de columnas](concepts-data-flow-column-pattern.md).

Cada expresión de agregación debe contener al menos una función de agregado.

![Configuración de agregado de la transformación Agregar](media/data-flow/agg2.png "Configuración de agregado de la transformación Agregar")


> [!NOTE]
> En el modo Depurar, el Generador de expresiones no puede generar vistas previas de los datos con las funciones de agregado. Para ver las vistas previas de los datos para las transformaciones Agregar, cierre el Generador de expresiones y vea los datos a través de la pestaña "Vista previa de los datos".

## <a name="reconnect-rows-and-columns"></a>Reconexión de filas y columnas

Las transformaciones Agregar son similares a las consultas de selección de agregado de SQL. Las columnas que no estén incluidas en la cláusula Agrupar por o en las funciones de agregado no fluirán a través de la salida de la transformación Agregar. Si desea incluir otras columnas en la salida agregada, siga uno de estos métodos:

* Use una función de agregado como `last()` o `first()` para incluir esa columna adicional.
* Vuelva a unir las columnas al flujo de salida con el [patrón self join](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/).

## <a name="removing-duplicate-rows"></a>Eliminación de filas duplicadas

Un uso común de la transformación de agregados es quitar o identificar entradas duplicadas en los datos de origen. Este proceso se conoce como desduplicación. En función de un conjunto de claves Group by, utilice una heurística de su elección para determinar qué fila duplicada desea conservar. Las heurísticas comunes son `first()`, `last()`, `max()` y `min()`. Use [patrones de columna](concepts-data-flow-column-pattern.md) para aplicar la regla a todas las columnas excepto para las columnas Group by.

![Desduplicación](media/data-flow/agg-dedupe.png "Desduplicación")

En el ejemplo anterior, las columnas `ProductID` y `Name` se usan para la agrupación. Si dos filas tienen los mismos valores para esas dos columnas, se consideran duplicadas. En esta transformación de agregados, se conservarán los valores de la primera fila coincidente y se quitarán todos los demás. Con la sintaxis de patrón de columnas, todas las columnas cuyos nombres no son `ProductID` y `Name` se asignan a su nombre de columna existente y se les proporciona el valor de las primeras filas coincidentes. El esquema de salida es el mismo que el esquema de entrada.

En escenarios de validación de datos, la función `count()` se puede usar para contar el número de duplicados que hay.

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream>
    aggregate(
           groupBy(
                <groupByColumnName> = <groupByExpression1>,
                <groupByExpression2>
               ),
           <aggregateColumn1> = <aggregateExpression1>,
           <aggregateColumn2> = <aggregateExpression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <aggregateTransformationName>
```

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se toma un flujo entrante `MoviesYear` y las filas se agrupan por columna `year`. La transformación crea una columna de agregado `avgrating` que se evalúa como el promedio de la columna `Rating`. Esta transformación Agregar se denomina `AvgComedyRatingsByYear`.

En la experiencia de usuario de Data Factory, esta transformación es similar a la siguiente imagen:

![Ejemplo de Agrupar por](media/data-flow/agg-script1.png "Ejemplo de Agrupar por")

![Ejemplo de Agregar](media/data-flow/agg-script2.png "Ejemplo de Agregar")

En el siguiente fragmento de código se muestra el script del flujo de datos para esta transformación.

```
MoviesYear aggregate(
                groupBy(year),
                avgrating = avg(toInteger(Rating))
            ) ~> AvgComedyRatingByYear
```

![Script del flujo de datos de agregado](media/data-flow/aggdfs1.png "Script del flujo de datos de agregado")

```MoviesYear```: columna derivada que define las columnas year y title ```AvgComedyRatingByYear```: transformación de agregados para la clasificación media de comedias agrupadas por año ```avgrating```: nombre de la nueva columna que se va a crear para contener el valor agregado

```
MoviesYear aggregate(groupBy(year),
    avgrating = avg(toInteger(Rating))) ~> AvgComedyRatingByYear
```

## <a name="next-steps"></a>Pasos siguientes

* Definir un agregación basada en ventanas mediante la [transformación Ventana](data-flow-window.md).
