---
title: Transformación División condicional en el flujo de datos de asignación
description: Divida los datos en diferentes flujos mediante la transformación División condicional en el flujo de datos de asignación en Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: bd9241e526d7cf42f0697afb8635c085a08c80d8
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606482"
---
# <a name="conditional-split-transformation-in-mapping-data-flow"></a>Transformación División condicional en el flujo de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

La transformación División condicional enruta las filas de datos a diferentes flujos en función de las condiciones coincidentes. La transformación División condicional es similar a una estructura de decisión CASE en un lenguaje de programación. La transformación evalúa las expresiones y, según los resultados, dirige la fila de datos a la secuencia especificada.

## <a name="configuration"></a>Configuración

El valor **Dividir** determina si la fila de datos fluye hacia el primer flujo coincidente o hacia todos los flujos que coincidan.

Utilice el generador de expresiones de flujo de datos para especificar una expresión como condición de división. Para agregar una nueva condición, haga clic en el icono del signo más de una fila existente. También se puede agregar un flujo predeterminada para las filas que no coinciden con ninguna condición.

![división condicional](media/data-flow/conditionalsplit1.png "opciones de división condicional")

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream>
    split(
        <conditionalExpression1>
        <conditionalExpression2>
        ...
        disjoint: {true | false}
    ) ~> <splitTx>@(stream1, stream2, ..., <defaultStream>)
```

### <a name="example"></a>Ejemplo

El ejemplo siguiente es una transformación División condicional denominada `SplitByYear` que toma el flujo entrante `CleanData`. Esta transformación tiene dos condiciones de división, `year < 1960` y `year > 1980`. `disjoint` es false porque los datos se dirigen a la primera condición de coincidencia. Cada fila que coincide con la primera condición se dirige al flujo de salida `moviesBefore1960`. Todas las filas restantes que coinciden con la segunda condición se dirigen al flujo de salida `moviesAFter1980`. Todas las demás filas pasan a través del flujo predeterminado `AllOtherMovies`.

En la experiencia de usuario de Data Factory, esta transformación es similar a la siguiente imagen:

![división condicional](media/data-flow/conditionalsplit1.png "opciones de división condicional")

En el siguiente fragmento de código se muestra el script del flujo de datos para esta transformación:

```
CleanData
    split(
        year < 1960,
        year > 1980,
        disjoint: false
    ) ~> SplitByYear@(moviesBefore1960, moviesAfter1980, AllOtherMovies)
```

## <a name="next-steps"></a>Pasos siguientes

Las transformaciones de flujo de datos comunes que se usan con la división condicional son la [transformación Combinación](data-flow-join.md), la [transformación Búsqueda](data-flow-lookup.md) y la [transformación Selección](data-flow-select.md)
