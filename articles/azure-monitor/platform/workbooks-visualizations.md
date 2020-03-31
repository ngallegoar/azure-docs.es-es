---
title: Visualizaciones de libro de Azure Monitor
description: Obtenga información sobre todos los componentes de visualización del libro de Azure Monitor, como texto, charts, cuadrículas, árboles y gráficos.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: d05902f47dff3dd2f8a63ae240c0b8825a5c441f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658037"
---
# <a name="azure-monitor-workbook-visualizations"></a>Visualizaciones de libro de Azure Monitor

Los libros de Azure Monitor admiten una serie de estilos diferentes de visualizaciones que se adaptan a sus necesidades de informes. En este artículo se proporcionan ejemplos de cada tipo de visualización.

## <a name="text"></a>Texto

Los libros permiten a los autores incluir bloques de texto en sus libros. El texto puede ser un análisis humano de la telemetría, información para ayudar a los usuarios a interpretar los datos, los encabezados de sección, etc.

![Captura de pantalla de la tabla de texto Apdex](./media/workbooks-visualizations/apdex.png)

El texto se agrega a través de un control Markdown que proporciona el control de formato completo.

![Captura de pantalla de Markdown sin formato que compila la tabla representada](./media/workbooks-visualizations/markdown.png)

### <a name="add-a-text-control"></a>Agregar un control de texto

1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas **Editar**.
2. Use el vínculo **Agregar texto** para agregar un control de texto al libro.
3. Agregue Markdown al control.
4. Haga clic en el botón **Edición finalizada** para ver el texto con formato.

> [!TIP]
> Use esta [hoja de referencia rápida de Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) para obtener información acerca de las diferentes opciones de formato.

## <a name="charts"></a>Gráficos

Los libros permiten que los datos de supervisión se presenten como gráficos. Los tipos de gráficos admitidos son línea, barra, categoría de la barra, área, gráficos de dispersión, círculos y hora. Los autores pueden elegir personalizar el alto, el ancho, la paleta de colores, la leyenda, los títulos, los mensajes sin datos, etc. del gráfico.

Los libros admiten gráficos para registros y orígenes de datos de métricas. 

### <a name="adding-a-log-chart"></a>Agregar un gráfico de registro

1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas **Editar**.
2. Use el vínculo **Agregar consulta** para agregar un control de consulta de registro al libro.
3. Seleccione el tipo de consulta **Registro**, el tipo de recurso (por ejemplo, Application Insights) y los recursos de destino.
4. Utilice el editor de consultas para especificar el [KQL](https://docs.microsoft.com/azure/kusto/query/) para el análisis (por ejemplo, la tendencia de las solicitudes).
5. Establezca la visualización en una de las siguientes opciones: **Área**, **Barra**, **Barra (categoría)** , **Línea**, **Circular**, **Dispersión**o **tiempo**.
6. Si es necesario, establezca otros parámetros, como el intervalo de tiempo, la visualización, el tamaño, la paleta de colores y la leyenda.

![Captura de pantalla del gráfico de registro en modo de edición](./media/workbooks-visualizations/log-chart.png)

#### <a name="log-chart-parameters"></a>Parámetros de gráfico de registro

| Parámetro | Explicación | Ejemplo |
| ------------- |:-------------|:-------------|
| `Query Type` | Tipo de consulta que se va a usar | Registro, Resource Graph, etc. |
| `Resource Type` | El tipo de recurso que se creará | Application Insights, Log Analytics o Azure-first |
| `Resources` | Un conjunto de recursos del que obtener el valor de las métricas | MyApp1 |
| `Time Range` | La ventana de tiempo para ver el gráfico de registro | Última hora, últimas 24 horas, etc. |
| `Visualization` | La visualización que se utilizará | Área, barra, línea, círculo, dispersión, hora, categoría de la barra |
| `Size` | El tamaño vertical del control | Pequeño, mediano, grande o completo |
| `Color palette` | Paleta de colores que se va a utilizar en el gráfico. Se omite en modo de varias métricas o segmentadas. | Azul, verde, rojo, etc. |
| `Legend` | Función de agregación que se va a usar para la leyenda | Suma o promedio de valores o Máx, mín, primero, último valor |
| `Query` | Cualquier consulta de KQL que devuelva datos en el formato esperado por la visualización del gráfico | _solicitudes \| las solicitudes make-series= número () valor predeterminado= 0 en la marca de tiempo de hace(1D) a ahora() paso 1h_ |

### <a name="adding-a-metric-chart"></a>Agregar un gráfico de métricas

1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas **Editar**.
2. Use el vínculo **Agregar métricas** para agregar un control de métricas al libro.
3. Seleccione un tipo de recurso (por ejemplo, cuenta de almacenamiento), los recursos de destino, el espacio de nombres de métricas y el nombre, así como la agregación que se va a usar.
4. Si es necesario, establezca otros parámetros, como el intervalo de tiempo, dividir por, la visualización, el tamaño y la paleta de colores.

![Captura de pantalla del gráfico de métricas en modo de edición](./media/workbooks-visualizations/metric-chart.png)

#### <a name="metric-chart-parameters"></a>Parámetros de gráfico de métricas

| Parámetro | Explicación | Ejemplo |
| ------------- |:-------------|:-------------|
| `Resource Type` | El tipo de recurso que se creará | Almacenamiento o máquina virtual. |
| `Resources` | Un conjunto de recursos del que obtener el valor de las métricas | MyStorage1 |
| `Namespace` | El espacio de nombres con la métrica | Almacenamiento > Blob |
| `Metric` | La métrica para visualizar | Almacenamiento > Blob > Transacciones |
| `Aggregation` | La función de agregación que se va a aplicar a la métrica | Suma, recuento, promedio, etc. |
| `Time Range` | La ventana de tiempo para ver la métrica | Última hora, últimas 24 horas, etc. |
| `Visualization` | La visualización que se utilizará | Área, barras, líneas, dispersión, cuadrícula |
| `Split By` | Dividir, de manera opcional, la métrica en una dimensión | Transacciones por tipo geográfico |
| `Size` | El tamaño vertical del control | Pequeño, mediano o grande |
| `Color palette` | Paleta de colores que se va a utilizar en el gráfico. Se omite si se usa el parámetro `Split by` | Azul, verde, rojo, etc. |

## <a name="grids"></a>Cuadrículas

Las cuadrículas o las tablas son una forma habitual de presentar datos a los usuarios. Los libros permiten a los usuarios aplicar estilo a las columnas de la cuadrícula de forma individual para proporcionar una interfaz de usuario enriquecida para sus informes.

En el ejemplo siguiente se muestra una cuadrícula que combina iconos, mapas térmicos y minibarras para presentar información compleja. El libro también proporciona un ordenado, un cuadro de búsqueda y un botón de "ir a análisis".

![Captura de pantalla de una cuadrícula basada en registro](./media/workbooks-visualizations/grid.png)

### <a name="adding-a-log-based-grid"></a>Agregar una cuadrícula basada en registro

1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas **Editar**.
2. Use el vínculo **Agregar consulta** para agregar un control de consulta de registro al libro.
3. Seleccione el tipo de consulta **Registro**, el tipo de recurso (por ejemplo, Application Insights) y los recursos de destino.
4. Utilice el editor de consultas para ingresar el KQL para el análisis (por ejemplo, máquinas virtuales con memoria por debajo de un umbral)
5. Establezca la visualización a **Cuadrícula**
6. Si es necesario, establezca otros parámetros, como el intervalo de tiempo, el tamaño, la paleta de colores y la leyenda.

![Captura de pantalla de una cuadrícula basada en registro](./media/workbooks-visualizations/grid-query.png)

## <a name="tiles"></a>Iconos

Los iconos son una manera muy útil de presentar los datos de resumen en los libros. La imagen siguiente muestra un caso de uso común de iconos: el resumen a nivel de aplicación sobre una cuadrícula detallada.

![Captura de pantalla de la vista de un resumen de iconos](./media/workbooks-visualizations/tiles-summary.png)

Los iconos del libro permiten mostrar un título, un subtítulo, texto grande, iconos, degradados basados en métricas, minigráficos o minibarras, pies de página, etc.

### <a name="adding-a-tile"></a>Adición de un icono

1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas _Editar_.
2. Use el vínculo **Agregar consulta** para agregar un control de consulta de registro al libro. 
3. Seleccione el tipo de consulta **Registro**, el tipo de recurso (por ejemplo, Application Insights) y los recursos de destino.
4. Use el editor de consultas para especificar el KQL para el análisis.
    ```kusto
    requests
    | summarize Requests = count() by appName, name
    | top 7 by Requests desc
    ```
5. Establezca el tamaño en **Completo**
6. Establezca la visualización a **Iconos**
7. Haga clic en el botón **Configuración del icono** para abrir el panel de configuración
8. En **Campos del icono**, establezca:
    * Título: `name`
    * Izquierda: `Requests`, Representador: `Big Number`, Paleta de colores: `Green to Red`, Valor mínimo: `0`
    * Abajo: `appName`
9. Haga clic en el botón **Guardar y cerrar** de la parte inferior del panel.

![Captura de pantalla de la vista de un resumen de iconos](./media/workbooks-visualizations/tile-settings.png)

Este es el aspecto que tendrán los iconos en el modo de lectura:

![Captura de pantalla de la vista de un resumen de iconos](./media/workbooks-visualizations/tiles-read-mode.png)

## <a name="trees"></a>Árboles

Los libros admiten vistas jerárquicas a través de cuadrículas de árbol. Los árboles permiten expandir algunas filas en el siguiente nivel para obtener una experiencia de exploración en profundidad.

En el ejemplo siguiente se muestran las métricas de mantenimiento del contenedor (tamaño del espacio de trabajo) que se visualizan como una cuadrícula de árbol. Los nodos de nivel superior son nodos de Azure Kubernetes Service (AKS), los del siguiente nivel son pods y los del nivel final son contenedores. Observe que todavía puede dar formato a las columnas como en una cuadrícula (mapa térmico, iconos, vínculo). El origen de datos subyacente en este caso es un área de trabajo Log Analytics con registros de AKS.

![Captura de pantalla de la vista de un resumen de iconos](./media/workbooks-visualizations/trees.png)

### <a name="adding-a-tree-grid"></a>Agregar una cuadrícula de árbol
1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas _Editar_.
2. Use el vínculo **Agregar consulta** para agregar un control de consulta de registro al libro. 
3. Seleccione el tipo de consulta **Registro**, el tipo de recurso (por ejemplo, Application Insights) y los recursos de destino.
4. Use el editor de consultas para especificar el KQL para el análisis.
    ```kusto
    requests
    | summarize Requests = count() by ParentId = appName, Id = name
    | extend Kind = 'Request', Name = strcat('🌐 ', Id)
    | union (requests
    | summarize Requests = count() by Id = appName
    | extend Kind = 'Request', ParentId = '', Name = strcat('📱 ', Id))
    | project Name, Kind, Requests, Id, ParentId
    | order by Requests desc
    ```
5. Establezca la visualización a **Cuadrícula**
6. Haga clic en el botón **Configuración del icono** para abrir el panel de configuración
7. En la sección **Configuración de Árbol/agrupar por** de la parte inferior, establezca:
    * Tipo de árbol: `Parent/Child`
    * Campo de identificador: `Id`
    * Campo de identificador primario: `ParentId`
    * Mostrar el expansor en: `Name`
    * Expandir el nivel superior del árbol: `checked`
8. En la sección _Columnas_ en la parte superior, establezca:
    * _Identificador_: representador de columnas: `Hidden`
    * _Identificador_: representador de columnas: `Hidden`
    * _Solicitud_: representador de columnas: `Bar`; color: `Blue`; valor mínimo: `0`
9. Haga clic en el botón _Guardar y cerrar_ de la parte inferior del panel.    

![Captura de pantalla de la vista de un resumen de iconos](./media/workbooks-visualizations/tree-settings.png)

### <a name="tree-settings"></a>Opciones del árbol

| Configuración | Explicación |
|:------------- |:-------------|
| `Id Field` | El identificador único de cada fila en la cuadrícula |
| `Parent Id Field` | Identificador del elemento primario de la fila actual |
| `Show the expander on` | La columna en la que se va a mostrar el expansor de árbol. Es habitual que las cuadrículas de árbol oculten el identificador y el campo de identificador primario porque no son muy legibles. En su lugar, el expansor aparece en un campo con un valor más legible, como el nombre de la entidad |
| `Expand the top level of the tree` | Si está activada, la cuadrícula de árbol se expandirá en el nivel superior. Resulta útil si desea mostrar más información de forma predeterminada |

## <a name="graphs"></a>Gráficos

Los libros admiten la visualización de gráficos arbitrarios basados en datos de registros para mostrar las relaciones entre las entidades de supervisión.

En el gráfico siguiente se muestran los datos que fluyen dentro y fuera de un equipo a través de varios puertos hacia y desde equipos externos. Está coloreado por tipo (equipo frente a puerto frente a dirección IP externa) y los tamaños de borde se corresponden con la cantidad de datos que fluyen dentro. Los datos subyacentes provienen de las conexiones de las máquinas virtuales de destino de consultas de KQL.

![Captura de pantalla de la vista de un resumen de iconos](./media/workbooks-visualizations/graph.png)

### <a name="adding-a-graph"></a>Agregar un gráfico
1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas _Editar_.
2. Use el vínculo **Agregar consulta** para agregar un control de consulta de registro al libro. 
3. Seleccione el tipo de consulta **Registro**, el tipo de recurso (por ejemplo, Application Insights) y los recursos de destino.
4. Use el editor de consultas para especificar el KQL para el análisis.
    ```kusto
    let data = dependencies
    | summarize Calls = count() by App = appName, Request = operation_Name, Dependency = name
    | extend RequestId = strcat(App, '::', Request);
    let links = data
    | summarize Calls = sum(Calls) by App, RequestId
    | project SourceId = App, TargetId = RequestId, Calls, Kind = 'App -> Request'
    | union (data
        | project SourceId = RequestId, TargetId = Dependency, Calls, Kind = 'Request -> Dependency');
    let nodes = data
    | summarize Calls = sum(Calls) by App
    | project Id = App, Name = App, Calls, Kind = 'App'
    | union (data
        | summarize Calls = sum(Calls) by RequestId, Request
        | project Id = RequestId, Name = Request, Calls, Kind = 'Request')
    | union (data
        | summarize Calls = sum(Calls) by Dependency
        | project Id = Dependency, Name = Dependency, Calls, Kind = 'Dependency');
    nodes
    | union (links)
    ```
7. Establezca la visualización a **Gráfico**
8. Haga clic en el botón **Configuración del gráfico** para abrir el panel de configuración
9. En _Campos de diseño_, en la parte inferior, establezca:
    * `Node Id`: `Id`
    * `Source Id`: `SourceId`
    * `Target Id`: `TargetId`
    * `Edge Label`: `None`
    * `Edge Size`: `Calls`
    * `Node Size`: `None`
    * `Coloring Type`: `Categorical`
    * `Node Color Field`: `Kind`
    * `Color palette`: `Pastel`
10. En _Configuración de formato del nodo_, en la parte superior, establezca:
    * _Contenido principal_: columna de uso: `Name`; representador de columnas: `Text`
    * _Contenido del centro_: columna de uso: `Calls`; representador de columnas: `Big Number`; paleta de colores: `None`
    * _Contenido de la parte inferior_: columna de uso: `Kind`; representador de columnas: `Text`
10. Haga clic en el botón _Guardar y cerrar_ de la parte inferior del panel.

![Captura de pantalla de la vista de un resumen de iconos](./media/workbooks-visualizations/graph-settings.png)

## <a name="next-steps"></a>Pasos siguientes

* [Implemente](workbooks-automate.md) los libros con Azure Resource Manager.
* [Controle](workbooks-access-control.md) y comparta el acceso a los recursos del libro.
