---
title: Parametrización de flujos de datos de asignación
description: Obtenga información sobre cómo parametrizar una asignación de Data Flow desde canalizaciones de Data Factory.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 8e88e5e8a9fbe1881959c5183dc01b11ac681bdf
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780423"
---
# <a name="parameterizing-mapping-data-flows"></a>Parametrización de flujos de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)] 

La asignación de flujos de datos de asignación en Azure Data Factory admite el uso de parámetros. Defina parámetros en la definición del flujo de datos y úselos después en las expresiones. La canalización que realiza la llamada puede establecer los valores de parámetro mediante la actividad de ejecución de Data Flow. Dispone de tres opciones para establecer los valores en las expresiones de la actividad del flujo de datos:

* Usar el lenguaje de expresiones del flujo de control de canalizaciones para establecer un valor dinámico
* Usar el lenguaje de expresiones del flujo de datos para establecer un valor dinámico
* Usar cualquiera de estos dos lenguajes de expresiones para establecer un valor literal estático

Use esta funcionalidad para hacer que los flujos de datos sean flexibles y reutilizables y estén destinados para un uso general. Puede parametrizar las expresiones y los valores del flujo de datos con estos parámetros.

## <a name="create-parameters-in-a-mapping-data-flow"></a>Creación de parámetros en un flujo de datos de asignación

Para agregar parámetros a un flujo de datos, haga clic en la parte en blanco del lienzo del flujo de datos para ver las propiedades generales. En el panel de configuración, verá una pestaña denominada **Parámetro**. Seleccione **Nuevo** para generar un nuevo parámetro. Para cada parámetro, debe asignar un nombre, seleccionar un tipo y opcionalmente establecer un valor predeterminado.

![Creación de parámetros de Data Flow](media/data-flow/create-params.png "Creación de parámetros de Data Flow")

## <a name="use-parameters-in-a-mapping-data-flow"></a>Uso de parámetros en un flujo de datos de asignación 

Se puede hacer referencia a los parámetros en cualquier expresión de flujo de datos. Los parámetros comienzan por $ y son inmutables. Encontrará la lista de parámetros disponibles en el Generador de expresiones en la pestaña **Parámetros**.

![Expresión de parámetros de flujo de datos](media/data-flow/parameter-expression.png "Expresión de parámetros de flujo de datos")

Para agregar parámetros adicionales rápidamente, puede seleccionar **Nuevo parámetro** y especificar el nombre y el tipo.

![Expresión de parámetros de flujo de datos](media/data-flow/new-parameter-expression.png "Expresión de parámetros de flujo de datos")

## <a name="assign-parameter-values-from-a-pipeline"></a>Asignación de valores de parámetros de una canalización

Una vez que haya creado un flujo de datos con parámetros, podrá ejecutarlo desde una canalización con la actividad de ejecución de Data Flow. Después de agregar la actividad al lienzo de la canalización, se le mostrarán los parámetros de flujo de datos disponibles en la pestaña **Parámetros** de la actividad.

Al asignar valores de parámetro, puede usar el [lenguaje de expresiones de canalización](control-flow-expression-language-functions.md) o el [lenguaje de expresiones de flujo de datos](data-flow-expression-functions.md) basado en tipos de Spark. Cada flujo de datos de asignación puede tener cualquier combinación de parámetros de expresión de canalización y flujo de datos.

![Establecimiento de un parámetro de Data Flow](media/data-flow/parameter-assign.png "Establecimiento de un parámetro de Data Flow")

### <a name="pipeline-expression-parameters"></a>Parámetros de expresión de canalización

Los parámetros de expresión de canalización permiten hacer referencia a variables del sistema, funciones, parámetros de canalización y variables similares a otras actividades de canalización. Al hacer clic en **Pipeline expression** (Expresión de canalización), se abre un panel de navegación lateral que le permite escribir una expresión mediante el generador de expresiones.

![Establecimiento de un parámetro de Data Flow](media/data-flow/parameter-pipeline.png "Establecimiento de un parámetro de Data Flow")

Al hacer referencia a los parámetros de canalización, estos se evalúan y, a continuación, se usa su valor en el lenguaje de expresiones de flujo de datos. No es necesario que el tipo de expresión de canalización coincida con el tipo de parámetro de flujo de datos. 

#### <a name="string-literals-vs-expressions"></a>Literales de cadena y expresiones

Al asignar un parámetro de expresión de canalización de tipo string, se agregarán comillas de forma predeterminada y el valor se evaluará como literal. Para leer el valor del parámetro como una expresión de flujo de datos, active la casilla de expresión situada junto al parámetro.

![Establecimiento de un parámetro de Data Flow](media/data-flow/string-parameter.png "Establecimiento de un parámetro de Data Flow")

Si el parámetro de flujo de datos `stringParam` hace referencia a un parámetro de canalización con el valor `upper(column1)`. 

- Si se comprueba la expresión, `$stringParam` se evalúa como el valor de column1 en mayúsculas.
- Si no se comprueba la expresión (comportamiento predeterminado), `$stringParam` se evalúa como `'upper(column1)'`

#### <a name="passing-in-timestamps"></a>Pasar marcas de tiempo

En el lenguaje de expresiones de canalización, las variables del sistema como `pipeline().TriggerTime` y las funciones como `utcNow()` devuelven marcas de tiempo como cadenas con el formato "aaaa-MM-dd\'T\'HH:mm:ss.SSSSSSZ". Para convertirlas en parámetros de flujo de datos de tipo timestamp, use la interpolación de cadenas para incluir la marca de tiempo deseada en una función `toTimestamp()`. Por ejemplo, para convertir la hora del desencadenador de la canalización en un parámetro de flujo de datos, puede utilizar `toTimestamp(left('@{pipeline().TriggerTime}', 23), 'yyyy-MM-dd\'T\'HH:mm:ss.SSS')`. 

![Establecimiento de un parámetro de Data Flow](media/data-flow/parameter-timestamp.png "Establecimiento de un parámetro de Data Flow")

> [!NOTE]
> Los flujos de datos solo admiten un máximo 3 dígitos de milisegundo. La función `left()` se usa para recortar los dígitos adicionales.

#### <a name="pipeline-parameter-example"></a>Ejemplo de parámetro de canalización

Supongamos que tiene un parámetro de entero `intParam` que hace referencia a un parámetro de canalización de tipo string, `@pipeline.parameters.pipelineParam`. 

![Establecimiento de un parámetro de Data Flow](media/data-flow/parameter-pipeline-2.png "Establecimiento de un parámetro de Data Flow")

A `@pipeline.parameters.pipelineParam` se le asigna un valor de `abs(1)` en tiempo de ejecución.

![Establecimiento de un parámetro de Data Flow](media/data-flow/parameter-pipeline-4.png "Establecimiento de un parámetro de Data Flow")

Cuando se hace referencia a `$intParam` en una expresión como una columna derivada, evaluará `abs(1)` y devolverá `1`. 

![Establecimiento de un parámetro de Data Flow](media/data-flow/parameter-pipeline-3.png "Establecimiento de un parámetro de Data Flow")

### <a name="data-flow-expression-parameters"></a>Parámetros de expresiones de flujo de datos

Al seleccionar **Data flow expression** (Expresión de flujo de datos), se abrirá el generador de expresiones de flujo de datos. Podrá hacer referencia a funciones, a otros parámetros y a cualquier columna de esquema definida en el flujo de datos. Esta expresión se evaluará tal cual cuando se haga referencia a ella.

> [!NOTE]
> Si pasa una expresión no válida o hace referencia a una columna de esquema que no existe en esa transformación, el parámetro se evaluará como null.


### <a name="passing-in-a-column-name-as-a-parameter"></a>Paso de un nombre de columna como un parámetro

Un patrón común es pasar un nombre de columna como un valor de parámetro. Si la columna está definida en el esquema de flujo de datos, puede hacer referencia a ella directamente como una expresión de cadena. Si la columna no está definida en el esquema, use la función `byName()`. Recuerde convertir la columna a su tipo adecuado con una función de conversión como `toString()`.

Por ejemplo, si desea asignar una columna de cadena basada en un parámetro `columnName`, puede agregar una transformación de columna derivada igual a `toString(byName($columnName))`.

![Paso de un nombre de columna como un parámetro](media/data-flow/parameterize-column-name.png "Paso de un nombre de columna como un parámetro")

## <a name="next-steps"></a>Pasos siguientes
* [Actividad de ejecución de Data Flow](control-flow-execute-data-flow-activity.md)
* [Expresiones del flujo de control](control-flow-expression-language-functions.md)
