---
title: Transformación Filtrar en el flujo de datos de asignación
description: Filtre las filas mediante la transformación Filtrar en el flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/16/2019
ms.openlocfilehash: 3460c789909c6acd0537ef89a9ec8509bf6e135d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81413730"
---
# <a name="filter-transformation-in-mapping-data-flow"></a>Transformación Filtrar en el flujo de datos de asignación

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

La transformación Filtrar permiten el filtrado de filas en función de una condición. El flujo de salida incluye todas las filas que coinciden con la condición de filtrado. La transformación Filtrar es similar a una cláusula WHERE de SQL.

## <a name="configuration"></a>Configuración

Utilice el generador de expresiones de flujo de datos para especificar una expresión como condición de filtro. Para abrir el generador de expresiones, haga clic en el cuadro azul. La condición de filtro debe ser de tipo booleano. Para obtener más información sobre cómo crear una expresión, consulte la documentación del [generador de expresiones](concepts-data-flow-expression-builder.md).

![Transformación Filtrar](media/data-flow/filter1.png "Transformación Filtrar")

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream>
    filter(
        <conditionalExpression>
    ) ~> <filterTransformationName>
```

### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra una transformación Filtrar denominada `FilterBefore1960` que toma el flujo entrante `CleanData`. La condición de filtro es la expresión `year <= 1960`.

En la experiencia de usuario de Data Factory, esta transformación es similar a la siguiente imagen:

![Transformación Filtrar](media/data-flow/filter1.png "Transformación Filtrar")

En el siguiente fragmento de código se muestra el script del flujo de datos para esta transformación:

```
CleanData
    filter(
        year <= 1960
    ) ~> FilterBefore1960

```

## <a name="next-steps"></a>Pasos siguientes

Filtre columnas con la [transformación Selección](data-flow-select.md)
