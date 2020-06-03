---
title: Transformación de búsqueda en el flujo de datos de asignación
description: Haga referencia a los datos de otro origen mediante la transformación búsqueda en el flujo de datos de asignación.
author: kromerm
ms.reviewer: daperlov
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/28/2020
ms.openlocfilehash: a4fcdad0efda1ab2a43be65865e3aac59f7ef3e3
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84187604"
---
# <a name="lookup-transformation-in-mapping-data-flow"></a>Transformación de búsqueda en el flujo de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Utilice la transformación búsqueda para hacer referencia a los datos de otro origen en una secuencia de flujo de datos. La transformación búsqueda anexa columnas de los datos coincidentes a los datos de origen.

Una transformación búsqueda es similar a una combinación externa izquierda. Todas las filas del flujo principal existirán en el flujo de salida con columnas adicionales de la secuencia de búsqueda.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4xsVT]

## <a name="configuration"></a>Configuración

![Transformación Búsqueda](media/data-flow/lookup1.png "Búsqueda")

**Flujo principal:** El flujo de datos entrante. Este flujo es equivalente a la parte izquierda de una combinación.

**Secuencia de búsqueda:** Los datos que se anexan al flujo principal. Los datos que se agregan están determinados por las condiciones de búsqueda. Este flujo es equivalente a la parte derecha de una combinación.

**Coincidencia con varias filas:** Si está habilitada, una fila con varias coincidencias en el flujo principal devolverá varias filas. De lo contrario, solo se devolverá una sola fila en función de la condición de coincidencia.

**Coincidencia en:** solo es visible si no está habilitada la opción "Coincidencia con varias filas". Elija si desea buscar coincidencias en cualquier fila, la primera coincidencia o la última coincidencia. Se recomienda cualquier fila a medida que se ejecuta la más rápida. Si se selecciona la primera o la última fila, se le pedirá que especifique las condiciones de organización.

**Condiciones de búsqueda:** Elija las columnas en las que desea buscar coincidencias. Si se cumple la condición de igualdad, las filas se considerarán una coincidencia. Mantenga el puntero y seleccione 'columna calculada' para extraer un valor mediante el [lenguaje de expresiones de flujo de datos](data-flow-expression-functions.md).

La transformación búsqueda solo admite coincidencias de igualdad. Para personalizar la expresión de búsqueda para que incluya otros operadores como mayor que, se recomienda usar una [Combinación cruzada en la transformación combinada](data-flow-join.md#custom-cross-join). Una combinación cruzada evitará cualquier posible error de producto cartesiano en la ejecución.

Todas las columnas de ambas secuencias se incluyen en los datos de salida. Para quitar las columnas duplicadas o no deseadas, agregue una [transformación de selección](data-flow-select.md) después de la transformación búsqueda. También se pueden quitar o cambiar el nombre de las columnas en una transformación de receptor.

### <a name="non-equi-joins"></a>Combinaciones no equivalentes

Para usar un operador condicional como no es igual a (! =) o mayor que (>) en las condiciones de búsqueda, cambie la lista desplegable de operadores entre las dos columnas. Las combinaciones no equivalentes requieren que al menos uno de los dos flujos se difunda mediante la retransmisión **fija** en la pestaña **Optimizar**.

![Búsqueda no equivalente](media/data-flow/non-equi-lookup.png "Búsqueda no equivalente")

## <a name="analyzing-matched-rows"></a>Analizar filas coincidentes

Después de la transformación de búsqueda, se puede usar la función `isMatch()` para ver si la búsqueda coincide con las filas individuales.

![Patrón de búsqueda](media/data-flow/lookup111.png "Patrón de búsqueda")

Un ejemplo de este patrón es usar la transformación división condicional para dividirla en la función `isMatch()`. En el ejemplo anterior, las filas coincidentes pasan por el flujo anterior y las filas no coincidentes fluyen a través del flujo de ```NoMatch```.

## <a name="testing-lookup-conditions"></a>Probar condiciones de búsqueda

Cuando pruebe la transformación de búsqueda con la vista previa de datos en modo de depuración, use un conjunto pequeño de datos conocidos. Cuando se realiza un muestreo de filas de un conjunto de filas grande, no se puede predecir qué filas y claves se leerán para las pruebas. El resultado es no determinista, lo que significa que las condiciones de combinación pueden no devolver ninguna coincidencia.

## <a name="broadcast-optimization"></a>Optimización de difusión

![Combinación de difusión](media/data-flow/broadcast.png "Combinación de difusión")

En las combinaciones, búsquedas y transformaciones Existe, si uno o ambos flujos de datos caben en la memoria del nodo de trabajo, puede optimizar el rendimiento al habilitar la opción **Difusión**. De forma predeterminada, el motor de Spark decidirá automáticamente si difundir o no una parte. Para elegir manualmente la parte que se va a difundir, seleccione **Fijo**.

No se recomienda deshabilitar la difusión a través de la opción **Desactivado** a menos que las combinaciones experimenten errores de tiempo de espera.

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<leftStream>, <rightStream>
    lookup(
        <lookupConditionExpression>,
        multiple: { true | false },
        pickup: { 'first' | 'last' | 'any' },  ## Only required if false is selected for multiple
        { desc | asc }( <sortColumn>, { true | false }), ## Only required if 'first' or 'last' is selected. true/false determines whether to put nulls first
        broadcast: { 'auto' | 'left' | 'right' | 'both' | 'off' }
    ) ~> <lookupTransformationName>
```
### <a name="example"></a>Ejemplo

![Transformación Búsqueda](media/data-flow/lookup-dsl-example.png "Búsqueda")

El script de flujo de datos para la configuración de búsqueda anterior se encuentra en el siguiente fragmento de código.

```
SQLProducts, DimProd lookup(ProductID == ProductKey,
    multiple: false,
    pickup: 'first',
    asc(ProductKey, true),
    broadcast: 'auto')~> LookupKeys
```
## 
Pasos siguientes

* Las transformaciones [combinación](data-flow-join.md) y [existencia](data-flow-exists.md) toman múltiples entradas de flujo
* Use una [transformación de división condicional](data-flow-conditional-split.md) con ```isMatch()``` para dividir las filas en valores coincidentes y no coincidentes
