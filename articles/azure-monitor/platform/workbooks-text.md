---
title: Parámetros de texto de los libros de Azure Monitor
description: Simplifique la creación de informes complejos con libros parametrizados predefinidos y personalizados. Aprenda más sobre los parámetros de texto de libros.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: c804cc8942a40e2f30c980636194daa82e0fb0e8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687322"
---
# <a name="workbook-text-parameters"></a>Parámetros de texto de libros

Los parámetros TextBox proporcionan una manera sencilla de recopilar entrada de texto de los usuarios de libros. Se usan cuando no es práctico usar una lista desplegable para recopilar la entrada (por ejemplo, un umbral arbitrario o filtros genéricos). Los libros permiten a los autores obtener el valor predeterminado del cuadro de texto de una consulta, lo que hace posible escenarios interesantes, como establecer el umbral predeterminado en función del P95 de la métrica.

Un uso común de los cuadros de texto es como variables internas que otros controles del libro utilizan. Para ello, se aprovecha una consulta de valores predeterminados y se hace invisible el control de entrada en modo de lectura. Por ejemplo, puede que a un usuario le interese que un umbral venga de una fórmula (no de un usuario) y, luego, usar el umbral en las sucesivas consultas.

## <a name="creating-a-text-parameter"></a>Creación de un parámetro de texto
1. Comience con un libro vacío en modo de edición.
2. Elija _Agregar parámetros_ desde los vínculos del libro.
3. Haga clic en el botón azul _Agregar parámetro_.
4. En el panel de nuevo parámetro que aparece, escriba:
    1. Nombre de parámetro: `SlowRequestThreshold`
    2. Tipo de parámetro: `Text`
    3. Obligatorio: `checked`
    4. Obtener el valor predeterminado de la consulta: `unchecked`
5. Elija "Guardar" en la barra de herramientas para crear el parámetro.

    ![Imagen que muestra la creación de un parámetro de texto](./media/workbooks-text/text-create.png)

Este es el aspecto que tendrá el libro en modo de lectura.

![Imagen que muestra un parámetro de texto en modo de lectura](./media/workbooks-text/text-readmode.png)

## <a name="referencing-a-text-parameter"></a>Referencia a un parámetro de texto
1. Para agregar un control de consulta al libro, seleccione el vínculo azul `Add query` y seleccione un recurso de Application Insights.
2. En el cuadro KQL, agregue este fragmento de código:
    ```kusto
    requests
    | summarize AllRequests = count(), SlowRequests = countif(duration >= {SlowRequestThreshold}) by name
    | extend SlowRequestPercent = 100.0 * SlowRequests / AllRequests
    | order by SlowRequests desc
    ```
3. Con el uso del parámetro de texto con un valor de 500 junto con el control de consulta, la siguiente consulta se ejecutará de forma eficaz:
    ```kusto
    requests
    | summarize AllRequests = count(), SlowRequests = countif(duration >= 500) by name
    | extend SlowRequestPercent = 100.0 * SlowRequests / AllRequests
    | order by SlowRequests desc
    ```
4. Ejecución de la consulta para ver los resultados

    ![Imagen que muestra un parámetro de texto al que se hace referencia en KQL](./media/workbooks-text/text-reference.png)

> [!NOTE]
> En el ejemplo anterior, `{SlowRequestThreshold}` representa un valor entero. Si estaba consultando una cadena como `{ComputerName}`, tendría que modificar la consulta de Kusto y agregar comillas, `"{ComputerName}"`, para que el campo del parámetro aceptara una entrada sin comillas.

## <a name="setting-default-values"></a>Configuración de los valores predeterminados
1. Comience con un libro vacío en modo de edición.
2. Elija _Agregar parámetros_ desde los vínculos del libro.
3. Haga clic en el botón azul _Agregar parámetro_.
4. En el panel de nuevo parámetro que aparece, escriba:
    1. Nombre de parámetro: `SlowRequestThreshold`
    2. Tipo de parámetro: `Text`
    3. Obligatorio: `checked`
    4. Obtener el valor predeterminado de la consulta: `checked`
5. En el cuadro KQL, agregue este fragmento de código:
    ```kusto
    requests
    | summarize round(percentile(duration, 95), 2)
    ```
    Esta consulta establece el valor predeterminado del cuadro de texto en la duración del percentil 95 para todas las solicitudes de la aplicación.
6. Ejecución de la consulta para ver el resultado
7. Elija "Guardar" en la barra de herramientas para crear el parámetro.

    ![Imagen que muestra un parámetro de texto con el valor predeterminado de KQL](./media/workbooks-text/text-default-value.png)

> [!NOTE]
> Aunque en este ejemplo se consultan los datos de Application Insights, el enfoque se puede usar con cualquier origen de datos basado en registros: Log Analytics, Azure Resource Graph, etc.

## <a name="next-steps"></a>Pasos siguientes

* [Comience](workbooks-visualizations.md) a aprender más sobre las muchas opciones de visualizaciones enriquecidas de los libros.
* [Controle](workbooks-access-control.md) y comparta el acceso a los recursos del libro.
