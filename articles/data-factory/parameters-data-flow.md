---
title: Parametrización de flujos de datos de asignación
description: Obtenga información sobre cómo parametrizar una asignación de Data Flow desde canalizaciones de Data Factory.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: 82660cdb4ab6523bae7608fe3b071f20cb3603f8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81419177"
---
# <a name="parameterizing-mapping-data-flows"></a>Parametrización de flujos de datos de asignación

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)] 

La asignación de flujos de datos de asignación en Azure Data Factory admite el uso de parámetros. Es posible definir parámetros en la definición del flujo de datos que, después, se pueden usar en las expresiones. Posteriormente, la canalización que realiza la llamada puede establecer los valores de parámetro mediante la actividad de ejecución de Data Flow. Dispone de tres opciones para establecer los valores en las expresiones de la actividad del flujo de datos:

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

### <a name="passing-in-a-column-name-as-a-parameter"></a>Paso de un nombre de columna como un parámetro

Un patrón común es pasar un nombre de columna como un valor de parámetro. Para hacer referencia a la columna asociada al parámetro, utilice la función `byName()`. Recuerde convertir la columna a su tipo adecuado con una función de conversión como `toString()`.

Por ejemplo, si desea asignar una columna de cadena basada en un parámetro `columnName`, puede agregar una transformación de columna derivada igual a `toString(byName($columnName))`.

![Paso de un nombre de columna como un parámetro](media/data-flow/parameterize-column-name.png "Paso de un nombre de columna como un parámetro")

## <a name="assign-parameter-values-from-a-pipeline"></a>Asignación de valores de parámetros de una canalización

Una vez que haya creado el flujo de datos con parámetros, podrá ejecutarlo desde una canalización con la actividad de ejecución de Data Flow. Después de agregar la actividad al lienzo de la canalización, se le mostrarán los parámetros de flujo de datos disponibles en la pestaña **Parámetros** de la actividad.

![Establecimiento de un parámetro de Data Flow](media/data-flow/parameter-assign.png "Establecimiento de un parámetro de Data Flow")

Si el tipo de datos del parámetro es una cadena, al hacer clic en el cuadro de texto para establecer los valores del parámetro, puede especificar una expresión de canalización o de flujo de datos. Si opta por una expresión de canalización, aparecerá el panel de expresiones de canalización. Asegúrese de incluir funciones de canalización en la sintaxis de interpolación de cadenas mediante `'@{<expression>}'`, por ejemplo:

```'@{pipeline().RunId}'```

Si el parámetro no es de tipo cadena, se le mostrará el Generador de expresiones de Data Flow. Aquí puede escribir cualquier expresión o valores literales que coincidan con el tipo de datos del parámetro. A continuación se muestran ejemplos de una expresión de flujo de datos y una cadena literal del Generador de expresiones:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Cada flujo de datos de asignación puede tener cualquier combinación de parámetros de expresión de canalización y flujo de datos. 

![Ejemplo de parámetros de flujo de datos](media/data-flow/parameter-example.png "Ejemplo de parámetros de flujo de datos")



## <a name="next-steps"></a>Pasos siguientes
* [Actividad de ejecución de Data Flow](control-flow-execute-data-flow-activity.md)
* [Expresiones del flujo de control](control-flow-expression-language-functions.md)
