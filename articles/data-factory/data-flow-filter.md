---
title: Transformación Filtrar en el flujo de datos de asignación
description: Filtre las filas mediante la transformación Filtrar en el flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/26/2020
ms.openlocfilehash: 8189228d6707812fb943e9925dc2bbf1b6da4972
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84112812"
---
# <a name="filter-transformation-in-mapping-data-flow"></a>Transformación Filtrar en el flujo de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

La transformación Filtrar permiten el filtrado de filas en función de una condición. El flujo de salida incluye todas las filas que coinciden con la condición de filtrado. La transformación Filtrar es similar a una cláusula WHERE de SQL.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4xnxN]

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
