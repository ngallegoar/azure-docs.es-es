---
title: Guía de referencia de los iconos del Diseñador de vistas en Azure Monitor | Microsoft Docs
description: Con el uso del Diseñador de vistas en Azure Monitor se pueden crear vistas personalizadas que se muestran en Azure Portal y que contienen diferentes visualizaciones de datos en el área de trabajo de Log Analytics. Este artículo es una guía de referencia para la configuración de los iconos que están disponibles en las vistas personalizadas.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/17/2018
ms.openlocfilehash: 0320be3a2cfbb96367799577a6e56bcf5da87dcb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658513"
---
# <a name="reference-guide-to-view-designer-tiles-in-azure-monitor"></a>Guía de referencia de los iconos del Diseñador de vistas en Azure Monitor
Con el uso del Diseñador de vistas en Azure Monitor, puede crear una variedad de vistas personalizadas en Azure Portal que pueden ayudarle a visualizar datos en el área de trabajo de Log Analytics. Este artículo es una guía de referencia para la configuración de los iconos que están disponibles en las vistas personalizadas.

Para más información acerca del Diseñador de vistas, consulte:

* [Diseñador de vistas](view-designer.md): proporciona información general del Diseñador de vistas, y procedimientos para crear y editar vistas personalizadas.
* [Referencia de los elementos de visualización](view-designer-parts.md): proporciona una guía de referencia de la configuración de los elementos de visualización que están disponibles en las vistas personalizadas.


En la tabla siguiente se describen los iconos disponibles del Diseñador de vistas:  

| Icono | Descripción |
|:--- |:--- |
| [Number](#number-tile) |Número de registros de una consulta. |
| [Dos números](#two-numbers-tile) |Número de registros de dos consultas diferentes. |
| [Anillo](#donut-tile) | Gráfico basado en una consulta con un valor de resumen en el centro. |
| Gráfico de líneas y llamada | Gráfico de líneas basado en una consulta y una llamada con un valor de resumen. |
| [Gráfico de líneas](#line-chart-tile) |Gráfico de líneas basado en una consulta. |
| [Dos escalas de tiempo](#two-timelines-tile) | Gráfico de columnas con dos series, cada una de ellas basada en una consulta independiente. |

Las siguientes secciones describen los tipos de icono y sus propiedades en detalle.

> [!NOTE]
> Los iconos de las vistas se basan en [consultas de registros](../log-query/log-query-overview.md) del área de trabajo de Log Analytics. Actualmente no se admiten las [consultas entre recursos](../log-query/cross-workspace-query.md) para recuperar datos de Application Insights.

## <a name="number-tile"></a>Icono de Número
El icono de **Número** muestra tanto el número de registros de una consulta de registro como una etiqueta.

![Icono de Número](media/view-designer-tiles/tile-number.png)

| Configuración | Descripción |
|:--- |:--- |
| Nombre |Texto que se muestra en la parte superior del icono. |
| Descripción |Texto que se muestra en la parte inferior del nombre del icono. |
| **Icono** | |
| Leyenda |Texto que se muestra en la parte inferior del valor. |
| Consultar |Consulta que se ejecuta. Se muestra el número de los registros devueltos por la consulta. |
| **Avanzadas** |**&gt; Comprobación del flujo de datos** |
| habilitado |Seleccione este vínculo si debe habilitarse la comprobación del flujo de datos en el icono. Este enfoque proporciona un mensaje alternativo si no hay datos disponibles. Normalmente, este enfoque se utiliza para ofrecer un mensaje durante el período temporal en el que se instala la vista y los datos están disponibles. |
| Consultar |Consulta que se ejecuta para determinar si los datos están disponibles para la vista. Si la consulta no devuelve resultados, se muestra un mensaje en lugar del valor de la consulta principal. |
| Message |Mensaje que se muestra si la consulta de comprobación del flujo de datos no devuelve datos. Si no proporciona ningún mensaje, se muestra el mensaje de estado *Realizando evaluación*. |


## <a name="two-numbers-tile"></a>Icono de Dos números
El icono muestra el número de registros de dos consultas de registro diferentes y una etiqueta en cada uno de ellos.

![Icono de Dos números](media/view-designer-tiles/tile-two-numbers.png)

| Configuración | Descripción |
|:--- |:--- |
| Nombre |Texto que se muestra en la parte superior del icono. |
| Descripción |Texto que se muestra en la parte inferior del nombre del icono. |
| **Primer icono** | |
| Leyenda |Texto que se muestra en la parte inferior del valor. |
| Consultar |Consulta que se ejecuta. Se muestra el número de los registros devueltos por la consulta. |
| **Segundo icono** | |
| Leyenda |Texto que se muestra en la parte inferior del valor. |
| Consultar |Consulta que se ejecuta. Se muestra el número de los registros devueltos por la consulta. |
| **Avanzadas** |**&gt; Comprobación del flujo de datos** |
| habilitado |Seleccione este vínculo si debe habilitarse la comprobación del flujo de datos en el icono. Este enfoque proporciona un mensaje alternativo si no hay datos disponibles. Normalmente, este enfoque se utiliza para ofrecer un mensaje durante el período temporal en el que se instala la vista y los datos están disponibles. |
| Consultar |Consulta que se ejecuta para determinar si los datos están disponibles para la vista. Si la consulta no devuelve resultados, se muestra un mensaje en lugar del valor de la consulta principal. |
| Message |Mensaje que se muestra si la consulta de comprobación del flujo de datos no devuelve datos. Si no proporciona ningún mensaje, se muestra el mensaje de estado *Realizando evaluación*. |


## <a name="donut-tile"></a>Icono Anillo
El icono **Anillo** muestra un solo número que resume una columna de valor de una consulta de registro. El anillo muestra gráficamente los resultados de los tres registros principales.

![Icono Anillo](media/view-designer-tiles/tile-donut.png)

| Configuración | Descripción |
|:--- |:--- |
| Nombre |Texto que se muestra en la parte superior del icono. |
| Descripción |Texto que se muestra en la parte inferior del nombre del icono. |
| **Anillo** | |
| Consultar |Consulta que se ejecuta en el anillo. La primera propiedad es un valor de texto y la segunda, un valor numérico. Normalmente, esta consulta usa la palabra clave *measure* para resumir los resultados. |
| **Anillo** |**&gt; Centro** |
| Texto |Texto que se muestra en la parte inferior del valor dentro del anillo. |
| Operación |Operación que se va a realizar en la propiedad value para resumirla en un solo valor.<ul><li>Sum: suma los valores de todos los registros con el valor de propiedad.</li><li>Percentage: porcentaje de los valores sumados de los registros con el valor de propiedad en comparación con los valores de suma de todos los registros.</li></ul> |
| Valores de resultado usados en la operación central |También puede seleccionar el signo más (+) para agregar uno o varios valores. Los resultados de la consulta se limitan a los registros con los valores de propiedad especificados. Si no se agrega ningún valor, se incluirán todos los registros en la consulta. |
| **Anillo** |**&gt; Opciones adicionales** |
| Colores |Color que se muestra en cada una de las tres propiedades principales. Para especificar colores alternativos para valores de propiedad concretos, utilice *Advanced Color Mapping* (Asignación de color avanzada). |
| Asignación de color avanzada |Muestra un color que representa valores de propiedad concretos. Si el valor especificado se encuentra en las tres principales, se muestra el color alternativo en lugar del estándar. Si la propiedad no está en las tres principales, no se muestra el color. |
| **Avanzadas** |**&gt; Comprobación del flujo de datos** |
| habilitado |Seleccione este vínculo si debe habilitarse la comprobación del flujo de datos en el icono. Este enfoque proporciona un mensaje alternativo si no hay datos disponibles. Normalmente, este enfoque se utiliza para ofrecer un mensaje durante el período temporal en el que se instala la vista y los datos están disponibles. |
| Consultar |Consulta que se ejecuta para determinar si los datos están disponibles para la vista. Si la consulta no devuelve resultados, se muestra un mensaje en lugar del valor de la consulta principal. |
| Message |Mensaje que se muestra si la consulta de comprobación del flujo de datos no devuelve datos. Si no proporciona ningún mensaje, se muestra el mensaje de estado *Realizando evaluación*. |


## <a name="line-chart-tile"></a>Icono de Gráfico de líneas
El icono es un gráfico de líneas que muestra varias series de una consulta de registro en un período. 

![Icono de gráfico de líneas y llamada](media/view-designer-tiles/tile-line-chart.png)

| Configuración | Descripción |
|:--- |:--- |
| Nombre |Texto que se muestra en la parte superior del icono. |
| Descripción |Texto que se muestra en la parte inferior del nombre del icono. |
| **Gráfico de líneas** | |
| Consultar |Consulta que se ejecuta en el gráfico de líneas. La primera propiedad es un valor de texto y la segunda, un valor numérico. Normalmente, esta consulta usa la palabra clave *measure* para resumir los resultados. Si la consulta utiliza la palabra clave *interval*, el eje X emplea este intervalo de tiempo. Si la consulta no utiliza la palabra clave *interval*, el eje X emplea intervalos horarios. |
| **Gráfico de líneas** |**> Eje Y** |
| Usar escala logarítmica |Seleccione este vínculo para usar una escala logarítmica en el eje Y. |
| Unidades |Especifica las unidades de los valores que devuelve la consulta. Esta información se utiliza para mostrar etiquetas en el gráfico que indican los tipos de valores y, de manera opcional, para convertirlos. **Tipo de unidad** especifica la categoría de la unidad y define los valores disponibles del **tipo de unidad actual**. Si selecciona un valor en **Convertir en**, los valores numéricos se convertirán del tipo **Unidad actual** a **Convertir en**. |
| Etiqueta personalizada |Texto que se muestra en el eje Y junto a la etiqueta del tipo *Unidad*. Si no se especifica ninguna etiqueta, solo se mostrará el tipo *Unidad*. |
| **Avanzadas** |**&gt; Comprobación del flujo de datos** |
| habilitado |Seleccione este vínculo si debe habilitarse la comprobación del flujo de datos en el icono. Este enfoque proporciona un mensaje alternativo si no hay datos disponibles. Normalmente, este enfoque se utiliza para ofrecer un mensaje durante el período temporal en el que se instala la vista y los datos están disponibles. |
| Consultar |Consulta que se ejecuta para determinar si los datos están disponibles para la vista. Si la consulta no devuelve resultados, se muestra un mensaje en lugar del valor de la consulta principal. |
| Message |Mensaje que se muestra si la consulta de comprobación del flujo de datos no devuelve datos. Si no proporciona ningún mensaje, se muestra el mensaje de estado *Realizando evaluación*. |


## <a name="line-chart-and-callout-tile"></a>Icono de gráfico de líneas y llamada
Este icono tiene tanto un gráfico de líneas que muestra varias series de una consulta de registro en un período como una llamada con un valor resumido. 

![Icono de gráfico de líneas y llamada](media/view-designer-tiles/tile-line-chart-callout.png)

| Configuración | Descripción |
|:--- |:--- |
| Nombre |Texto que se muestra en la parte superior del icono. |
| Descripción |Texto que se muestra en la parte inferior del nombre del icono. |
| **Gráfico de líneas** | |
| Consultar |Consulta que se ejecuta en el gráfico de líneas. La primera propiedad es un valor de texto y la segunda, un valor numérico. Normalmente, esta consulta usa la palabra clave *measure* para resumir los resultados. Si la consulta utiliza la palabra clave *interval*, el eje X emplea este intervalo de tiempo. Si la consulta no utiliza la palabra clave *interval*, el eje X emplea intervalos horarios. |
| **Gráfico de líneas** |**&gt; Llamada** |
| Título de la llamada | Texto que se muestra en la parte superior del valor de la llamada. |
| Nombre de la serie |Valor de propiedad de la serie que se usará como valor de la llamada. Si no se proporciona ninguna serie, se utilizarán todos los registros de la consulta. |
| Operación |Operación que se va a realizar en la propiedad value para resumirla en un solo valor en la llamada.<ul><li>Average: promedio de los valores de todos los registros.</li><li>Count: número de todos los registros devueltos por la consulta.</li><li>Last Sample: valor del último intervalo incluido en el gráfico.</li><li>Max: valor máximo de los intervalos incluidos en el gráfico.</li><li>Min: valor mínimo de los intervalos incluidos en el gráfico.</li><li>Sum: suma de los valores de todos los registros.</li></ul> |
| **Gráfico de líneas** |**> Eje Y** |
| Usar escala logarítmica |Seleccione este vínculo para usar una escala logarítmica en el eje Y. |
| Unidades |Especifica las unidades de los valores que devuelve la consulta. Esta información se utiliza para mostrar etiquetas de gráfico que indican los tipos de valores y, de manera opcional, para convertirlos. El tipo *Unidad* especifica la categoría de la unidad y define los valores disponibles del tipo *Unidad actual*. Si selecciona un valor en *Convertir a*, los valores numéricos se convertirán del tipo *Unidad actual* al tipo *Convertir a*. |
| Etiqueta personalizada |Texto que se muestra en el eje Y junto a la etiqueta del tipo *Unidad*. Si no se especifica ninguna etiqueta, solo se mostrará el tipo *Unidad*. |
| **Avanzadas** |**&gt; Comprobación del flujo de datos** |
| habilitado |Seleccione este vínculo si debe habilitarse la comprobación del flujo de datos en el icono. Este enfoque proporciona un mensaje alternativo si no hay datos disponibles. Normalmente, este enfoque se utiliza para ofrecer un mensaje durante el período temporal en el que se instala la vista y los datos están disponibles. |
| Consultar |Consulta que se ejecuta para determinar si los datos están disponibles para la vista. Si la consulta no devuelve resultados, se muestra un mensaje en lugar del valor de la consulta principal. |
| Message |Mensaje que se muestra si la consulta de comprobación del flujo de datos no devuelve datos. Si no proporciona ningún mensaje, se muestra el mensaje de estado *Realizando evaluación*. |


## <a name="two-timelines-tile"></a>Icono de Dos escalas de tiempo
El icono de **Dos escalas de tiempo** muestra los resultados de dos consultas de registro en un periodo como gráficos de columnas. Para cada serie, se muestra una llamada. 

![Icono de Dos escalas de tiempo](media/view-designer-tiles/tile-two-timelines.png)

| Configuración | Descripción |
|:--- |:--- |
| Nombre |Texto que se muestra en la parte superior del icono. |
| Descripción |Texto que se muestra en la parte inferior del nombre del icono. |
| Primer gráfico | |
| Leyenda |Texto que se muestra en la parte inferior de la llamada de la primera serie. |
| Color |Color que se utiliza para las columnas de la primera serie. |
| Consulta de gráfico |Consulta que se ejecuta en la primera serie. El número de registros en cada intervalo de tiempo se representa mediante las columnas de gráfico. |
| Operación |Operación que se va a realizar en la propiedad value para resumirla en un solo valor en la llamada.<ul><li>Average: promedio de los valores de todos los registros.</li><li>Count: número de todos los registros devueltos por la consulta.</li><li>Last Sample: valor del último intervalo incluido en el gráfico.</li><li>Max: valor máximo de los intervalos incluidos en el gráfico.</li></ul> |
| **Segundo gráfico** | |
| Leyenda |Texto que se muestra en la parte inferior de la llamada de la segunda serie. |
| Color |Color que se utiliza para las columnas de la segunda serie. |
| Consulta de gráfico |Consulta que se ejecuta en la segunda serie. El número de registros en cada intervalo de tiempo se representa mediante las columnas de gráfico. |
| Operación |Operación que se va a realizar en la propiedad value para resumirla en un solo valor en la llamada.<ul><li>Average: promedio de los valores de todos los registros.</li><li>Count: número de todos los registros devueltos por la consulta.</li><li>Last Sample: valor del último intervalo incluido en el gráfico.</li><li>Max: valor máximo de los intervalos incluidos en el gráfico. |
| **Avanzadas** |**&gt; Comprobación del flujo de datos** |
| habilitado |Seleccione este vínculo si debe habilitarse la comprobación del flujo de datos en el icono. Este enfoque proporciona un mensaje alternativo si no hay datos disponibles. Normalmente, este enfoque se utiliza para ofrecer un mensaje durante el período temporal en el que se instala la vista y los datos están disponibles. |
| Consultar |Consulta que se ejecuta para determinar si los datos están disponibles para la vista. Si la consulta no devuelve resultados, se muestra un mensaje en lugar del valor de la consulta principal. |
| Message |Mensaje que se muestra si la consulta de comprobación del flujo de datos no devuelve datos. Si no proporciona ningún mensaje, se muestra el mensaje de estado *Realizando evaluación*. |


## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre las [consultas de registros](../log-query/log-query-overview.md) para poder realizar consultas en iconos.
* Agregue [elementos de visualización](view-designer-parts.md) a la vista personalizada.
