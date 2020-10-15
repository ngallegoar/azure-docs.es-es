---
title: Parámetros temporales de los libros de Azure Monitor
description: Aprenda a definir los parámetros temporales para permitir a los usuarios establecer el contexto temporal de los análisis. La mayoría de los informes usan los parámetros temporales.
services: azure-monitor
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 471b775abfebec646ab3950803622271c0bd559b
ms.sourcegitcommit: 50802bffd56155f3b01bfb4ed009b70045131750
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91932301"
---
# <a name="workbook-time-parameters"></a>Parámetros temporales de los libros

Los parámetros temporales permiten a los usuarios establecer el contexto temporal del análisis y se usan en casi todos los informes. Son relativamente fáciles de configurar y usar, lo que permite a los autores especificar los intervalos de tiempo que se van a mostrar en la lista desplegable, incluida la opción de intervalos de tiempo personalizados. 

## <a name="creating-a-time-parameter"></a>Creación de un parámetro temporal
1. Comience con un libro vacío en modo de edición.
2. Elija _Agregar parámetros_ desde los vínculos del libro.
3. Haga clic en el botón azul _Agregar parámetro_.
4. En el panel de nuevo parámetro que aparece, escriba:
    1. Nombre de parámetro: `TimeRange`
    2. Tipo de parámetro: `Time range picker`
    3. Obligatorio: `checked`
    4. Intervalos de tiempo disponibles: Última hora, Últimas 12 horas, Últimas 24 horas, Últimas 48 horas, Últimos 3 días, Últimos 7 días y Permitir la selección de intervalos de tiempo personalizados
5. Elija "Guardar" en la barra de herramientas para crear el parámetro.

    ![Imagen que muestra la creación de un parámetro de intervalo de tiempo](./media/workbooks-time/time-settings.png)

Este es el aspecto que tendrá el libro en modo de lectura.

![Imagen que muestra un parámetro de intervalo de tiempo en modo de lectura](./media/workbooks-time/parameters-time.png)

## <a name="referencing-a-time-parameter"></a>Referencia de un parámetro temporal
### <a name="via-bindings"></a>Mediante enlaces
1. Agregue un control de consulta al libro y seleccione un recurso de Application Insights.
2. La mayoría de los controles de libro admiten un selector de ámbito de _Intervalo de tiempo_. Abra la lista desplegable _Intervalo de tiempo_ y seleccione `{TimeRange}` en el grupo de parámetros de intervalo de tiempo de la parte inferior.
3. Esta acción enlaza el parámetro de intervalo de tiempo con el intervalo de tiempo del gráfico. El ámbito de tiempo de la consulta de ejemplo es ahora Últimas 24 horas.
4. Ejecución de la consulta para ver los resultados

    ![Imagen que muestra un parámetro de intervalo de tiempo al que se hace referencia mediante enlaces](./media/workbooks-time/time-binding.png)

### <a name="in-kql"></a>En KQL
1. Agregue un control de consulta al libro y seleccione un recurso de Application Insights.
2. En KQL, escriba un filtro de ámbito de tiempo mediante el parámetro: `| where timestamp {TimeRange}`.
3. El tiempo de evaluación de consulta se expande a `| where timestamp > ago(1d)`, que es el valor del intervalo de tiempo del parámetro.
4. Ejecución de la consulta para ver los resultados

    ![Imagen que muestra un intervalo de tiempo al que se hace referencia en KQL](./media/workbooks-time/time-in-code.png)

### <a name="in-text"></a>En texto 
1. Agregue un control de texto al libro.
2. En el Markdown, escriba `The chosen time range is {TimeRange:label}`.
3. Elija _Edición finalizada_.
4. El control de texto mostrará el texto: _El intervalo de tiempo elegido es Últimas 24 horas_

## <a name="time-parameter-options"></a>Opciones de parámetro temporal
| Parámetro | Explicación | Ejemplo |
| ------------- |:-------------|:-------------|
| `{TimeRange}` | Etiqueta de intervalo de tiempo | Últimas 24 horas |
| `{TimeRange:label}` | Etiqueta de intervalo de tiempo | Últimas 24 horas |
| `{TimeRange:value}` | Valor de intervalo de tiempo | > ago(1d) |
| `{TimeRange:query}` | Consulta de intervalo de tiempo | > ago(1d) |
| `{TimeRange:start}` | Hora de inicio del intervalo de tiempo | 3/20/2019 4:18 PM |
| `{TimeRange:end}` | Hora de finalización del intervalo de tiempo | 3/21/2019 4:18 PM |
| `{TimeRange:grain}` | Grano del intervalo de tiempo | 30 m |


### <a name="using-parameter-options-in-a-query"></a>Uso de opciones de parámetros en una consulta
```kusto
requests
| make-series Requests = count() default = 0 on timestamp from {TimeRange:start} to {TimeRange:end} step {TimeRange:grain}
```

## <a name="next-steps"></a>Pasos siguientes

* [Comience](workbooks-visualizations.md) a aprender más sobre las muchas opciones de visualizaciones enriquecidas de los libros.
* [Controle](workbooks-access-control.md) y comparta el acceso a los recursos del libro.
