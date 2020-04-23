---
title: Transformación Combinación en el flujo de datos de asignación
description: Combine datos de dos orígenes de datos mediante la transformación Combinación en el flujo de datos de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/02/2020
ms.openlocfilehash: 5c388dd2b3e4f40fbf2ed75cf3f1b8ab31aee394
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606413"
---
# <a name="join-transformation-in-mapping-data-flow"></a>Transformación Combinación en el flujo de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Use la transformación Combinación para combinar datos de dos orígenes o flujos en un mismo flujo de datos de asignación. El flujo de salida incluirá todas las columnas de ambos orígenes que coincidan con una condición de combinación. 

## <a name="join-types"></a>Tipos de combinación

La asignación de flujos de datos admite actualmente cinco tipos de combinación diferentes.

### <a name="inner-join"></a>Combinación interna

La combinación interna solo genera filas que tienen valores coincidentes en ambas tablas.

### <a name="left-outer"></a>Externa izquierda

La combinación externa izquierda devuelve todas las filas del flujo izquierdo y los registros coincidentes del flujo derecho. Si una fila del flujo izquierdo no tiene ninguna coincidencia, las columnas de salida del flujo derecho se establecen en NULL. La salida constará de las filas devueltas por una combinación interna más las filas sin coincidencias del flujo izquierdo.

> [!NOTE]
> En ocasiones, el motor de Spark que los flujos de datos usan puede no funcionar debido a posibles productos cartesianos en las condiciones de combinación. Si esto sucede, puede cambiar a una combinación cruzada personalizada y escribir manualmente la condición de combinación. Esto puede dar lugar a un rendimiento más lento en los flujos de datos, ya que es posible que el motor de ejecución tenga que calcular todas las filas de ambos lados de la relación y luego filtrarlas.

### <a name="right-outer"></a>Externa derecha

La combinación externa derecha devuelve todas las filas del flujo derecho y los registros coincidentes del flujo izquierdo. Si una fila del flujo derecho no tiene ninguna coincidencia, las columnas de salida del flujo izquierdo se establecen en NULL. La salida constará de las filas devueltas por una combinación interna más las filas sin coincidencias del flujo derecho.

### <a name="full-outer"></a>Externa completa

La combinación externa completa devuelve todas las columnas y filas de ambos lados con valores NULL para las columnas que no coinciden.

### <a name="custom-cross-join"></a>Combinación cruzada personalizada

La combinación cruzada genera el producto cruzado de los dos flujos en función de una condición. Si utiliza una condición que no es de igualdad, debe especificar una expresión personalizada como condición de combinación cruzada. El flujo de salida constará de todas las filas que cumplan la condición de combinación.

Puede usar este tipo de combinación para combinaciones no equivalentes y condiciones ```OR```.

Si quiere generar explícitamente un producto cartesiano completo, utilice la transformación Columna derivada en cada uno de los dos flujos independientes antes de la combinación para crear una clave sintética en la que buscar coincidencias. Por ejemplo, cree una columna en la columna derivada de cada flujo denominada ```SyntheticKey``` y establézcala como igual a ```1```. A continuación, use ```a.SyntheticKey == b.SyntheticKey``` como expresión de combinación personalizada.

> [!NOTE]
> Asegúrese de incluir al menos una columna a cada lado de la relación izquierda y derecha de una combinación cruzada personalizada. La ejecución de combinaciones cruzadas con valores estáticos en lugar de columnas a cada lado produce exámenes completos de todo el conjunto de datos, lo que provoca que el flujo de datos se realice de manera deficiente.

## <a name="configuration"></a>Configuración

1. Elija el flujo de datos que va a combinar en la lista desplegable **Right stream** (Flujo derecho).
1. Seleccione el **tipo de combinación**
1. Elija las columnas de clave con las que quiere hacer coincidir la condición de combinación. De forma predeterminada, el flujo de datos busca la igualdad entre una columna de cada flujo. Para comparar a través de un valor de proceso, mantenga el mouse sobre la lista desplegable y seleccione **Columna calculada**.

![Transformación de combinación](media/data-flow/join.png "Join")

## <a name="optimizing-join-performance"></a>Optimización del rendimiento de combinación

A diferencia de la unión de combinación en herramientas como SSIS, la transformación Combinación no es una operación de unión de combinación obligatoria. No es necesario ordenar las claves de combinación. La operación de combinación se realiza en función de la operación de combinación óptima en Spark, ya sea una combinación de difusión o del lado de la asignación.

![Optimización de la transformación de combinación](media/data-flow/joinoptimize.png "Optimización de la combinación")

Si uno o ambos flujos de datos caben en la memoria del nodo de trabajo, puede optimizar aún más el rendimiento si habilita **Difusión** en la pestaña Optimizar. También puede volver a particionar los datos en la operación de combinación para que se adapten mejor a la memoria por nodo de trabajo.

## <a name="self-join"></a>Autocombinación

Para autocombinar un flujo de datos consigo mismo, asigne un alias a un flujo existente mediante una transformación Selección. Para crear una nueva rama, haga clic en el icono de signo más junto a una transformación y seleccione **Nueva rama**. Agregue una transformación Selección para asignar un alias al flujo original. Agregue una transformación Combinación y elija el flujo original como **Left stream** (flujo izquierdo) y la transformación Selección como el **Right stream** (flujo derecho).

![Autocombinación](media/data-flow/selfjoin.png "Autocombinación")

## <a name="testing-join-conditions"></a>Condiciones de combinación de pruebas

Cuando pruebe las transformaciones Combinación con la vista previa de datos en modo de depuración, use un conjunto pequeño de datos conocidos. Cuando se realiza un muestreo de filas de un conjunto de filas grande, no se puede predecir qué filas y claves se leerán para las pruebas. El resultado es no determinista, lo que significa que las condiciones de combinación pueden no devolver ninguna coincidencia.

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<leftStream>, <rightStream>
    join(
        <conditionalExpression>,
        joinType: { 'inner'> | 'outer' | 'left_outer' | 'right_outer' | 'cross' }
        broadcast: { 'none' | 'left' | 'right' | 'both' }
    ) ~> <joinTransformationName>
```

### <a name="inner-join-example"></a>Ejemplo de combinación interna

El ejemplo siguiente es una transformación Combinación denominada `JoinMatchedData` que toma el flujo izquierdo `TripData` y el flujo derecho `TripFare`.  La condición de combinación es la expresión `hack_license == { hack_license} && TripData@medallion == TripFare@medallion && vendor_id == { vendor_id} && pickup_datetime == { pickup_datetime}` que devuelve true si coinciden las columnas `hack_license`, `medallion`, `vendor_id` y `pickup_datetime` de cada flujo. El valor de `joinType` es `'inner'`. Vamos a habilitar la difusión solo en el flujo izquierdo, por lo que `broadcast` tiene el valor `'left'`.

En la experiencia de usuario de Data Factory, esta transformación es similar a la siguiente imagen:

![Ejemplo de combinación](media/data-flow/join-script1.png "Ejemplo de combinación")

En el siguiente fragmento de código se muestra el script del flujo de datos para esta transformación:

```
TripData, TripFare
    join(
        hack_license == { hack_license}
        && TripData@medallion == TripFare@medallion
        && vendor_id == { vendor_id}
        && pickup_datetime == { pickup_datetime},
        joinType:'inner',
        broadcast: 'left'
    )~> JoinMatchedData
```

### <a name="custom-cross-join-example"></a>Ejemplo de combinación cruzada personalizada

El ejemplo siguiente es una transformación Combinación denominada `JoiningColumns` que toma el flujo izquierdo `LeftStream` y el flujo derecho `RightStream`. Esta transformación toma dos flujos y combina todas las filas en las que la columna `leftstreamcolumn` es mayor que la columna `rightstreamcolumn`. El valor de `joinType` es `cross`. La difusión no está habilitada, `broadcast` tiene el valor `'none'`.

En la experiencia de usuario de Data Factory, esta transformación es similar a la siguiente imagen:

![Ejemplo de combinación](media/data-flow/join-script2.png "Ejemplo de combinación")

En el siguiente fragmento de código se muestra el script del flujo de datos para esta transformación:

```
LeftStream, RightStream
    join(
        leftstreamcolumn > rightstreamcolumn,
        joinType:'cross',
        broadcast: 'none'
    )~> JoiningColumns
```

## <a name="next-steps"></a>Pasos siguientes

Después de combinar los datos, puede crear una [columna derivada](data-flow-derived-column.md) y [recibir](data-flow-sink.md) sus datos en un almacén de datos de destino.
