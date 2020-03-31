---
title: Transformación Columna derivada en Asignación de Data Flow
description: Aprenda a transformar datos a escala en Azure Data Factory con la transformación Columna derivada en Asignación de Data Flow.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/15/2019
ms.openlocfilehash: 66396de52b3709c1d9357f32a375a29a8dcdbd1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77048747"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Transformación Columna derivada en Asignación de Data Flow

Use la transformación Columna derivada para generar nuevas columnas en el flujo de datos o para modificar los campos existentes.

## <a name="derived-column-settings"></a>Configuración de columna derivada

Para reemplazar una columna existente, selecciónela mediante la lista desplegable de la columna. De lo contrario, utilice el campo de selección de columna como un cuadro de texto y escriba el nombre de la nueva columna. Para crear la expresión de la columna derivada, haga clic en el cuadro "Escribir expresión" para abrir el [Generador de expresiones de Data Flow](concepts-data-flow-expression-builder.md).

![Configuración de columna derivada](media/data-flow/dc1.png "Configuración de columna derivada")

Para agregar columnas derivadas adicionales, mantenga el mouse sobre una columna derivada existente y haga clic en el icono "más". Elija **Agregar columna** o **Add column pattern** (Agregar patrón de columna). Los patrones de columna pueden resultar útiles si los nombres de columna son variables de los orígenes. Para más información, consulte [Patrones de columna](concepts-data-flow-column-pattern.md).

![Nueva selección de columna derivada](media/data-flow/columnpattern.png "Nueva selección de columna derivada")

## <a name="build-schemas-in-output-schema-pane"></a>Compilación de esquemas en el panel Output Schema (Esquema de salida)

Las columnas que va a modificar y agregar al esquema se muestran en el panel Esquema de salida. Aquí puede compilar de manera interactiva estructuras de datos simples y complejas. Para agregar campos adicionales, seleccione **Agregar columna**. Para crear jerarquías, seleccione **Agregar subcolumna**.

![Add subcolumn (Agregar subcolumna)](media/data-flow/addsubcolumn.png "Agregar subcolumna")

Para obtener más información sobre cómo controlar tipos complejos en el flujo de datos, consulte [Control de JSON en un flujo de datos de asignación](format-json.md#mapping-data-flow-properties).

![Agregar columna compleja](media/data-flow/complexcolumn.png "Add columns (Agregar columnas)")

## <a name="data-flow-script"></a>Script de flujo de datos

### <a name="syntax"></a>Sintaxis

```
<incomingStream>
    derive(
           <columnName1> = <expression1>,
           <columnName2> = <expression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <deriveTransformationName>
```

### <a name="example"></a>Ejemplo

El ejemplo siguiente es una columna derivada denominada `CleanData` que toma un flujo entrante `MoviesYear` y crea dos columnas derivadas. La primera columna derivada reemplaza la columna `Rating` por el valor de clasificación como un tipo entero. La segunda columna derivada es un patrón que coincide con cada columna cuyo nombre empieza con "movies". Para cada columna coincidente, crea una columna `movie` que es igual al valor de la columna coincidente con el prefijo "movie_". 

En la experiencia de usuario de Data Factory, esta transformación es similar a la siguiente imagen:

![Ejemplo de derivación](media/data-flow/derive-script1.png "Ejemplo de derivación")

En el siguiente fragmento de código se muestra el script del flujo de datos para esta transformación:

```
MoviesYear derive(
                Rating = toInteger(Rating),
                each(
                    match(startsWith(name,'movies')),
                    'movie' = 'movie_' + toString($$)
                )
            ) ~> CleanData
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el [lenguaje de expresiones de Mapping Data Flow](data-flow-expression-functions.md).
