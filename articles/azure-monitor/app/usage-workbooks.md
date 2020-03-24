---
title: Crear informes interactivos con libros de Azure Monitor | Microsoft Docs
description: Simplifique la creación de informes complejos con libros parametrizados creados previamente y personalizados.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 09/19/2018
ms.reviewer: mbullwin
ms.openlocfilehash: 15543f7f761c707e8eff8e0cc0a0e4532475ddf8
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671008"
---
# <a name="create-interactive-reports-with-azure-monitor-workbooks"></a>Crear informes interactivos con libros de Azure Monitor

Los libros combinan texto, [consultas de Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics), las métricas de Azure y los parámetros en informes interactivos avanzados. Otros miembros del equipo con acceso a los mismos recursos de Azure pueden editar los libros.

Los libros son útiles en escenarios como:

* Exploración del uso de la aplicación cuando no conoce de antemano las métricas de interés: número de usuarios, tasas de retención, tasas de conversión, etc. A diferencia de otras herramientas de análisis de uso, los libros le permiten combinar varios tipos de visualizaciones y análisis, lo que los hace idóneos para este tipo de exploración de forma libre.
* A la hora de explicar al equipo el rendimiento de una característica recientemente publicada, mostrando recuentos de usuarios para las interacciones principales y otras métricas.
* Uso compartido de los resultados de un experimento A o B de la aplicación con otros miembros del equipo. Puede explicar los objetivos del experimento con texto y luego mostrar cada métrica de uso y la consulta de Analytics que se usa para evaluar el experimento, junto con indicadores claros sobre si cada métrica está por encima o por debajo del objetivo.
* Notificación del impacto de una interrupción del servicio en el uso de la aplicación, combinación de datos, explicación del texto y análisis de los pasos siguientes para evitar más interrupciones en el futuro.

## <a name="starting-with-a-template-or-saved-workbook"></a>Uso de una plantilla o libro guardado para empezar

Un libro se compone de secciones que constan de tablas, texto, controles de entrada y gráficos que se pueden editar de forma independientemente. Para comprender mejor los libros, le recomendamos que abra uno. 

Seleccione **Libros** en el menú izquierdo dentro de experiencia de Application Insights de su aplicación.

![Captura de pantalla de navegación a los libros](./media/usage-workbooks/001-workbooks.png)

Esto inicia una galería de libros con varios libros creados previamente que le ayudarán a empezar a trabajar.

![Captura de pantalla de la galería de libros](./media/usage-workbooks/002-workbook-gallery.png)

Comenzaremos con la **Plantilla predeterminada**, que se encuentra en el encabezado **Inicio rápido**.

![Captura de pantalla de la galería de libros](./media/usage-workbooks/003-default-template.png)

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Edición, reorganización, clonación y eliminación de secciones de libro

Los libros tienen dos modos: **modo de edición** y **modo de lectura**. Cuando se inicia por primera vez el libro predeterminado, se abre en **modo de edición**. Este muestra todo el contenido del libro, incluidos los pasos y los parámetros que se encuentran ocultos. El **modo de lectura** presenta una vista simplificada de estilo del informe. De este modo, puede reducir la complejidad que conlleva la creación de un informe, a la vez que puede acceder a la mecánica subyacente con tan solo unos clics cuando la necesita para realizar modificaciones.

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/editing-controls-new.png)

1. Cuando haya terminado de editar una sección, haga clic en **Edición finalizada** en la esquina inferior izquierda de la sección.

2. Para crear un duplicado de una sección, haga clic en el icono **Clone this section** (Clonar este sección). La creación de secciones duplicadas es una forma ideal de iterar una consulta sin perder las iteraciones anteriores.

3. Para mover hacia arriba una sección de un libro, haga clic en el icono **Subir** o **Bajar**.

4. Para eliminar una sección de forma permanente, haga clic en el icono **Quitar**.

## <a name="adding-text-and-markdown-sections"></a>Adición de texto y secciones de Markdown

La adición de títulos, explicaciones y comentarios a los libros le ayuda a convertir un conjunto de tablas y gráficos en una descripción. Las secciones de texto de los libros son compatibles con la [sintaxis de Markdown](https://daringfireball.net/projects/markdown/) para dar formato a los textos como títulos, negrita, cursiva y listas con viñetas.

Para agregar una sección de texto al libro, use el botón **Agregar texto** situado en la parte inferior del libro o en la parte inferior de cualquier sección.

## <a name="adding-query-sections"></a>Incorporación de secciones de consulta

![Sección de consulta en los libros](./media/usage-workbooks/analytics-section-new.png)

Para agregar una sección de consulta al libro, use el botón **Agregar consulta** situado en la parte inferior del libro o en la parte inferior de cualquier sección.

Las secciones de consulta son muy flexibles y pueden usarse para responder preguntas como las siguientes:

* ¿Cuántas excepciones produjo el sitio durante un período de tiempo equivalente al de un declive del uso?
* ¿Cuál fue la distribución de los tiempos de carga de página de los usuarios que vieron alguna página?
* ¿Cuántos usuarios vieron un conjunto de páginas determinado de su sitio, pero no otro? Esto puede ser útil para comprender si tiene clústeres de usuarios que utilizan distintos subconjuntos de funcionalidad del sitio (use el operador `join` con el modificador `kind=leftanti` en el [lenguaje de consulta Kusto](/azure/kusto/query/)).

Además, no está limitado únicamente a realizar consultas del contexto de la aplicación con la que abrió el libro, sino que puede hacer consultas en varias aplicaciones supervisadas por Application Insights, así como a las áreas de trabajo de Log Analytics siempre que tenga permiso para acceder a esos recursos.

Para realizar consultas desde otros recursos externos de Application Insights, use el identificador de **aplicaciones**.

```
union app('app01').requests, app('app02').requests, requests
| summarize count() by bin(timestamp, 1h)
```  

Esta consulta combina solicitudes de tres aplicaciones distintas. Una aplicación llamada app01, una aplicación llamada app02 y las solicitudes del recurso local de Application Insights.

Para extraer datos de un área de trabajo de Log Analytics externa, use el identificador de **áreas de trabajo**.

Para obtener información acerca de las consultas entre recursos, consulte la [guía oficial](https://docs.microsoft.com/azure/log-analytics/log-analytics-cross-workspace-search).

### <a name="advanced-analytic-query-settings"></a>Configuración avanzada de consultas analíticas

Cada sección tiene su propia configuración avanzada, a la que se puede acceder mediante el icono de configuración ![Controles de edición de la sección de libros de Application Insights](./media/usage-workbooks/005-settings.png) situado a la derecha del botón **Agregar parámetros**.

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/0006-settings-expanded.png)

   |         |          |
   | ---------------- |:-----|
   | **Ancho personalizado**    | Establezca esta opción para definir un tamaño arbitrario para un elemento, de modo que puedan caber varios elementos en una sola línea. Esto le permitirá organizar mejor los gráficos y las tablas en informes interactivos enriquecidos.  |
   | **Conditionally visible** (Visible condicionalmente) | Use esta opción para ocultar los pasos en función de un parámetro en el modo de lectura. |
   | **Export a parameter** (Exportar un parámetro)| Permite que una fila seleccionada en la cuadrícula o en el gráfico genere más pasos para cambiar los valores o para hacerlos visibles.  |
   | **Mostrar la consulta cuando no se está editando** | Muestra la consulta por encima del gráfico o la tabla incluso en modo de lectura.
   | **Mostrar el botón Abrir en Analytics cuando no se esté editando** | Agrega el icono azul de Analytics a la esquina derecha del gráfico para permitir el acceso con un solo clic.|

La mayoría de estas opciones de configuración son bastante intuitivas, pero para comprender la opción **Exportar un parámetro** le recomendamos que examine un libro que use esta funcionalidad.

Uno de los libros creados previamente proporciona información sobre los usuarios activos.

La primera sección del libro se basa en datos de consulta de análisis:

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/003-active-users.png)

La segunda sección también se basa en datos de consulta de análisis, pero, si se selecciona una fila en la primera tabla, se actualizará el contenido del gráfico de forma interactiva:

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/004-active-users-trend.png)

 Esto es posible mediante el uso de la configuración avanzada **Cuando se seleccione un elemento, exporte un parámetro**, que está habilitada en la consulta de Analytics de la tabla.

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/007-settings-export.png)

Luego, la segunda consulta de análisis utiliza los valores exportados cuando se selecciona una fila. Si no se selecciona ninguna fila, el valor predeterminado es la fila que representa los valores generales. 

```
let start = startofday(ago({TimeRange} + {Metric}));
union customEvents, pageViews
| where timestamp >= start
| where name in ({Activities}) or '*' in ({Activities}) or ('%' in ({Activities}) and itemType == 'pageView') or ('#' in ({Activities}) and itemType == 'customEvent')
{OtherFilters}
| where '{Filter}' == '' or '{Filter}' == '🔸 Overall' or {AnalyzeBy} == replace('🔹 ', '', '{Filter}')
| evaluate activity_engagement(user_Id, timestamp, start, now(), 1d, {Metric})
| where timestamp >= startofday(ago({TimeRange}))
| project timestamp, ["Active User"] = dcount_activities_outer
| render timechart 
```

## <a name="adding-metrics-sections"></a>Incorporación de secciones de métricas

Las secciones de las métricas le proporcionan acceso total para incorporar datos de métricas de Azure Monitor en sus informes interactivos. Muchos de los libros creados previamente contendrán datos de consulta de análisis y de métricas, lo que le permite aprovechar al máximo lo mejor de ambas características en un mismo lugar. También tiene la capacidad de extraer datos de métricas de recursos en cualquiera de las suscripciones a las que tiene acceso.

Este es un ejemplo de datos de la máquina virtual que se extraen en un libro para proporcionar una visualización de cuadrícula del rendimiento de la CPU:

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/008-metrics-grid.png)

## <a name="adding-parameter-sections"></a>Incorporación de secciones de parámetros

Los parámetros de libros le permiten cambiar valores en el libro sin tener que editar manualmente las secciones de texto o consulta.  Esto elimina la necesidad de tener que comprender el lenguaje de consulta de análisis subyacente y amplía significativamente la audiencia potencial de creación de informes basados en el libro.

Para reemplazar los valores de parámetros en consultas, texto o en otras secciones de parámetros, se coloca el nombre del parámetro entre llaves, como ``{parameterName}``.  Los nombres de parámetros se limitan a reglas similares como identificadores de JavaScript, básicamente caracteres alfabéticos o de subrayado, seguidos de caracteres alfanuméricos o caracteres de subrayado. Por ejemplo, **a1** está permitido, pero **1a** no lo está.

Los parámetros son lineales, desde la parte superior de un libro y hasta los pasos posteriores.  Los parámetros que se declaran más adelante en un libro pueden reemplazar a aquellos que se declararon anteriormente.  Esto también permite a los parámetros que usan consultas tener acceso a los valores de parámetros definidos anteriormente.  En el propio paso de un parámetro, los parámetros también son lineales, de izquierda a derecha, donde los parámetros de la derecha pueden depender de un parámetro declarado anteriormente en ese mismo paso.
 
Hay cuatro tipos diferentes de parámetros que se admiten actualmente:

  |         |          |
   | ---------------- |:-----|
   | **Texto**    | El usuario modificará un cuadro de texto y, si usted quiere, puede proporcionar una consulta para rellenar el valor predeterminado. |
   | **Lista desplegable** | El usuario podrá elegir entre un conjunto de valores. |
   | **Selector de intervalo de tiempo**| El usuario elegirá entre un conjunto predefinido de valores de intervalo de tiempo o seleccionará entre un intervalo de tiempo personalizado.|
   | **Selector de recursos** | El usuario podrá elegir entre los recursos seleccionados para el libro.|

### <a name="using-a-text-parameter"></a>Uso de un parámetro de texto

El valor que un usuario escribe en el cuadro de texto se sustituye directamente en la consulta, sin caracteres de escape ni comillas. Si el valor necesario es una cadena, la consulta debe tener comillas alrededor del parámetro (como **"{parameter}"** ).

Esto permite que el valor de un cuadro de texto se use en cualquier lugar. Puede ser el nombre de la tabla, el nombre de la columna, el nombre de la función, el operador, etc.

El tipo de parámetro de texto tiene la configuración **Obtener el valor predeterminado de consulta de análisis**, lo que permite que el autor del libro use una consulta para rellenar el valor predeterminado de ese cuadro de texto.

Cuando se usa el valor predeterminado de una consulta de análisis, solo el primer valor de la primera fila (fila 0, columna 0) se utiliza como el valor predeterminado. Por lo tanto, se recomienda limitar la consulta para devolver solo una sola fila y una columna. Se omite cualquier otro dato devuelto por la consulta. 

Independientemente del valor que devuelva la consulta, se reemplazará directamente sin ningún carácter de escape ni comillas. Si la consulta no devuelve ninguna fila, el resultado del parámetro es una cadena vacía (si el parámetro no es necesario) o indefinido (si el parámetro es necesario).

### <a name="using-a-dropdown"></a>Uso de una lista desplegable

El tipo de parámetro de la lista desplegable le permite crear un control de lista desplegable, lo que permite seleccionar uno o varios valores.

La lista desplegable se rellena mediante una consulta de análisis. Si la consulta devuelve una columna, los valores de esa columna son el **valor** y la **etiqueta** en el control de la lista desplegable. Si la consulta devuelve dos columnas, la primera columna es el **valor** y la segunda columna es la **etiqueta** que se muestra en la lista desplegable.  Si la consulta devuelve tres columnas. La tercera se utiliza para indicar la selección predeterminada en esa lista desplegable.  Esta columna puede ser de cualquier tipo, pero lo más sencillo es usar tipos booleanos o numéricos, donde 0 es falso y 1 es verdadero.

 Si la columna es un tipo de cadena, la cadena nula o vacía se considera falsa y cualquier otro valor se considera verdadero. Para las listas desplegables de selección única, el primer valor con un valor verdadero se utiliza como selección predeterminada.  Para las listas desplegables de selección múltiple, todos los valores con un valor verdadero se utilizan como selección predeterminada. Los elementos en la lista desplegable se muestran en cualquier orden en el que la consulta devuelva las filas. 

Echemos un vistazo a los parámetros del informe Usuarios activos. Haga clic en el símbolo de edición junto a **TimeRange**.

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/009-time-range.png)

Esto iniciará el elemento de menú Editar parámetro:

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/010-time-range-edit.png)

La consulta usa una característica del lenguaje de consulta de análisis que se llama **datatable** y le permite generar una tabla arbitraria, llena de contenido que aparece de la nada. Por ejemplo, la siguiente consulta de análisis:

```
datatable( column1:string, column2:string )
[
 "row 1 column 1", "row 1 column 2",
"row 2 column 1", "row 2 column 2"
]
```

Genera el siguiente resultado:

![Controles de edición de secciones de libros de Application Insights](./media/usage-workbooks/011-data-table.png)

Un ejemplo más aplicable es usar una lista desplegable para elegir entre un conjunto de países o regiones por el nombre:

```
customEvents
| where timestamp >= ago(14d)
| summarize count() by client_CountryOrRegion
| top 100 by count_
| project  client_CountryOrRegion 
| order by client_CountryOrRegion asc
```

La consulta mostrará los resultados de la siguiente forma:

![Lista desplegable de países o regiones](./media/usage-workbooks/012-country-dropdown.png)

Las listas desplegables son herramientas increíblemente eficaces para personalizar y crear informes interactivos.

### <a name="time-range-parameters"></a>Parámetros de intervalo de tiempo

Aunque puede elaborar su propio parámetro de intervalo de tiempo personalizado mediante el tipo de parámetro de la lista desplegable, también puede usar el tipo de parámetro de intervalo de tiempo ya definido si no necesita el mismo grado de flexibilidad. 

Los tipos de parámetro de intervalo de tiempo tienen 15 intervalos predeterminados que van desde cinco minutos hasta los últimos 90 días. También hay una opción para permitir la selección del intervalo de tiempo personalizado, que permite que el operador del informe elija los valores explícitos de inicio y fin del intervalo de tiempo.

### <a name="resource-picker"></a>Selector de recursos

El tipo de parámetro del selector de recursos le permite definir el ámbito del informe para ciertos tipos de recursos. Un ejemplo de libro creado previamente que aprovecha el tipo de selector de recursos es el libro **Failure Insights**.

![Lista desplegable de países o regiones](./media/usage-workbooks/013-resource-picker.png)

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Almacenamiento y uso compartido de libros con el equipo

Los libros se guardarán en un recurso de Application Insights, ya sea en la sección privada de **Mis informes** o en la sección **Informes compartidos** a la que pueden acceder todos los usuarios con acceso al recurso de Application Insights. Para ver todos los libros del recurso, haga clic en el botón **Abrir** de la barra de acciones.

Para compartir un libro que está actualmente en **Mis informes**:

1. Haga clic en **Abrir** en la barra de acciones
2. Haga clic en el botón "..." situado junto al libro que desea compartir
3. Haga clic en **Move to Shared Reports** (Mover a informes compartidos).

Para compartir un libro con un vínculo o por correo electrónico, haga clic en **Compartir** en la barra de acciones. Tenga en cuenta que los destinatarios del vínculo necesitarán acceder a este recurso en Azure Portal para ver el libro. Para realizar ediciones, los destinatarios necesitan al menos permisos de colaborador para el recurso.

Para anclar un vínculo a un libro en un panel de Azure:

1. Haga clic en **Abrir** en la barra de acciones
2. Haga clic en el botón "..." situado junto al libro que desea anclar
3. Haga clic en **Anclar al panel**.

## <a name="contributing-workbook-templates"></a>Plantillas de contribución de libros

¿Ha creado una plantilla increíble de libros y quiere compartirla con la comunidad? Para obtener más información, visite nuestro [repositorio de GitHub](https://github.com/Microsoft/Application-Insights-Workbooks/blob/master/README.md).

## <a name="next-steps"></a>Pasos siguientes
- Para habilitar las experiencias de uso, empiece por enviar [eventos personalizados](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) o [vistas de páginas](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Si ya ha enviado eventos personalizados o vistas de página, explore las herramientas de uso para obtener información sobre cómo los usuarios utilizan el servicio.
    - [Usuarios, sesiones, eventos](../../azure-monitor/app/usage-segmentation.md)
    - [Embudos](../../azure-monitor/app/usage-funnels.md)
    - [Retención](../../azure-monitor/app/usage-retention.md)
    - [Flujos de usuario](../../azure-monitor/app/usage-flows.md)
    - [Adición de contexto de usuario](../../azure-monitor/app/usage-send-user-context.md)
