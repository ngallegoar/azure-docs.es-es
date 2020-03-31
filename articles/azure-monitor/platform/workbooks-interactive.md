---
title: Libros de Azure Monitor con parámetros personalizados
description: Simplifique la creación de informes complejos con libros parametrizados creados previamente y personalizados.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 4d9f6e48722f01970a90a3a1d8d8b58b5d939774
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77658282"
---
# <a name="interactive-workbooks"></a>Libros interactivos

Los libros permiten a los autores crear experiencias e informes interactivos para sus consumidores. La interactividad se admite de varias maneras.

## <a name="parameter-changes"></a>Cambios de parámetros
Cuando un usuario de un libro actualiza un parámetro, todos los controles que usen el parámetro se actualizan automáticamente y se vuelven a dibujar para reflejar el nuevo estado. Así es como la mayoría de los informes de Azure Portal admiten interactividad. En los libros se proporciona de forma muy sencilla con un esfuerzo mínimo por parte del usuario.

Más información sobre los [parámetros de los libros](workbooks-parameters.md)

## <a name="grid-row-clicks"></a>Clics de fila de cuadrícula
Los libros permiten a los autores construir escenarios en los que al hacer clic en una fila de una cuadrícula se actualizan los gráficos posteriores según el contenido de la fila. 

Por ejemplo, un usuario puede tener una cuadrícula que muestre una lista de solicitudes y algunas estadísticas, como recuentos de errores. Esta cuadrícula se podría configurar de modo que al hacer clic en una fila correspondiente a una solicitud, el resultado fueran gráficos detallados que se actualizan para filtrar solo esa solicitud.

### <a name="setting-up-interactivity-on-grid-row-click"></a>Configuración de la interactividad al hacer clic en la fila de cuadrícula
1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas _Editar_.
2. Use el vínculo _Agregar consulta_ para agregar un control de consulta de registro al libro. 
3. Seleccione el tipo de consulta _Registro_, el tipo de recurso (por ejemplo, Application Insights) y los recursos de destino.
4. Use el editor de consultas para especificar el KQL para el análisis.
    ```kusto
    requests
    | summarize AllRequests = count(), FailedRequests = countif(success == false) by Request = name
    | order by AllRequests desc    
    ```
5. Use `Run query` para ver los resultados.
6. Haga clic en el icono _Configuración avanzada_ en el pie de consulta (el icono es similar a un engranaje). Se abrirá el panel Configuración avanzada. 
7. Compruebe la configuración: `When an item is selected, export a parameter`.
    1. Campo para exportar: `Request`
    2. Nombre de parámetro: `SelectedRequest`
    3. Valor predeterminado: `All requests`
    
    ![Imagen que muestra el editor avanzado con la configuración para exportar campos como parámetros](./media/workbooks-interactive/advanced-settings.png)

8. Haga clic en `Done Editing`.
9. Agregue otro control de consulta con los pasos 2 y 3.
10. Use el editor de consultas para especificar el KQL para el análisis.
    ```kusto
    requests
    | where name == '{SelectedRequest}' or 'All Requests' == '{SelectedRequest}'
    | summarize ['{SelectedRequest}'] = count() by bin(timestamp, 1h)
    ```
11. Use `Run query` para ver los resultados.
12. Cambie _Visualización_ a `Area chart`.
12. Haga clic en una fila de la primera cuadrícula. Observe cómo el gráfico de área que aparece debajo se filtra a la solicitud seleccionada.

El informe resultante tiene el siguiente aspecto en el modo de edición:

![Imagen que muestra la creación de una experiencia interactiva mediante clics de fila de cuadrícula](./media/workbooks-interactive//grid-click-create.png)

La imagen siguiente muestra un informe interactivo más elaborado en modo de lectura basado en los mismos principios. El informe usa clics de cuadrícula para exportar parámetros, que a su vez se usan en dos gráficos y en un bloque de texto.

![Imagen que muestra la creación de una experiencia interactiva mediante clics de fila de cuadrícula](./media/workbooks-interactive/grid-click-read-mode.png)

### <a name="exporting-the-contents-of-an-entire-row"></a>Exportación del contenido de una fila completa
A veces es conveniente exportar todo el contenido de la fila seleccionada en lugar de una sola columna determinada. En tales casos, deje la propiedad `Field to export` sin establecer en el paso 7.1 anterior. Los libros exportarán todo el contenido de la fila como JSON al parámetro. 

En el control KQL al que se hace referencia, use la función `todynamic` para analizar el código JSON y acceder a las columnas individuales.

 ## <a name="grid-cell-clicks"></a>Clics de celda de cuadrícula
Los libros permiten a los autores agregar interactividad mediante un tipo especial de representador de columnas de cuadrícula denominado `link renderer`. Un representador de vínculos convierte una celda de cuadrícula en un hipervínculo basado en el contenido de la celda. Los libros admiten muchos tipos de representadores de vínculos, incluidos los que permiten abrir hojas de información general de recursos, visores de contenedores de propiedades, búsqueda de App Insights, uso, seguimiento de transacciones, etc.

### <a name="setting-up-interactivity-using-grid-cell-clicks"></a>Configuración de la interactividad mediante clics de celda de cuadrícula
1. Para cambiar el libro al modo de edición, haga clic en el elemento de la barra de herramientas _Editar_.
2. Use el vínculo _Agregar consulta_ para agregar un control de consulta de registro al libro. 
3. Seleccione el tipo de consulta _Registro_, el tipo de recurso (por ejemplo, Application Insights) y los recursos de destino.
4. Use el editor de consultas para especificar el KQL para el análisis.
    ```kusto
    requests
    | summarize Count = count(), Sample = any(pack_all()) by Request = name
    | order by Count desc
    ```
5. Use `Run query` para ver los resultados.
6. Haga clic en _Configuración de columnas_ para abrir el panel de configuración.
7. En la sección _Columnas_, establezca:
    1. _Sample_ - Representador de columnas: `Link`, Vista para abrir: `Cell Details`, Etiqueta de vínculo: `Sample`
    2. _Count_ - Representador de columnas: `Bar`, Paleta de colores: `Blue`, Valor mínimo: `0`
    3. _Request_ - Representador de columnas: `Automatic`
    4. Haga clic en _Guardar y cerrar_ para aplicar los cambios.
8. Haga clic en uno de los vínculos `Sample` de la cuadrícula. Se abre un panel de propiedades con los detalles de una solicitud muestreada.

    ![Imagen que muestra la creación de una experiencia interactiva mediante clics de celda de cuadrícula](./media/workbooks-interactive/grid-cell-click-create.png)

### <a name="link-renderer-actions"></a>Acciones del representador de vínculos
| Acción de vínculo | Acción al hacer clic |
|:------------- |:-------------|
| `Generic Details` | Muestra los valores de fila de una hoja de contexto de cuadrícula de propiedades. |
| `Cell Details` | Muestra el valor de celda de una hoja de contexto de cuadrícula de propiedades. Resulta útil cuando la celda contiene un tipo dinámico con información (por ejemplo, JSON con propiedades de solicitud, como ubicación, instancia de rol, etc.). |
| `Cell Details` | Muestra el valor de celda de una hoja de contexto de cuadrícula de propiedades. Resulta útil cuando la celda contiene un tipo dinámico con información (por ejemplo, JSON con propiedades de solicitud, como ubicación, instancia de rol, etc.). |
| `Custom Event Details` | Abre los detalles de búsqueda de Application Insights con el identificador de evento personalizado (itemId) en la celda. |
| `* Details` | Similar a los detalles de eventos personalizados, exceptuando las dependencias, las excepciones, las vistas de página, las solicitudes y los seguimientos. |
| `Custom Event User Flows` | Abre la experiencia Flujos de usuario de Application Insights dinamizada en el nombre del evento personalizado de la celda. |
| `* User Flows` | Similar a Flujos de usuario de eventos personalizados, exceptuando las excepciones, las vistas de página y las solicitudes. |
| `User Timeline` | Abre la escala de tiempo del usuario con el identificador de usuario (user_Id) de la celda. |
| `Session Timeline` | Abre la experiencia de búsqueda de Application Insights para el valor de la celda (por ejemplo, buscar texto "abc" donde abc es el valor de la celda). |
| `Resource overview` | Abre la información general del recurso en el portal en función del valor del identificador del recurso de la celda. |

## <a name="conditional-visibility"></a>Visibilidad condicional
Los libros permiten a los usuarios hacer que determinados controles aparezcan o desaparezcan en función de los valores de los parámetros. De esta forma, los autores pueden hacer que los informes tengan una apariencia diferente en función de la entrada del usuario o del estado de la telemetría. Un ejemplo sería mostrar a los consumidores un resumen cuando las cosas van bien, pero mostrar detalles completos cuando algo va mal.

### <a name="setting-up-interactivity-using-conditional-visibility"></a>Configuración de la interactividad mediante la visibilidad condicional
1. Siga los pasos de la sección `Setting up interactivity on grid row click` para configurar dos controles interactivos.
2. Agregue un nuevo parámetro en la parte superior:
    1. Nombre: `ShowDetails`
    2. Tipo de parámetro: `Drop down`
    3. Obligatorio: `checked`
    4. Obtener datos de: `JSON`
    5. Entrada JSON: `["Yes", "No"]`
    6. Guarde para confirmar los cambios.
3. Establezca el valor del parámetro en `Yes`.
4. En el control de consulta con el gráfico de áreas, haga clic en el icono _Configuración avanzada_ (icono de engranaje).
5. Marque el valor `Make this item conditionally visible`.
    1. Este elemento está visible si el valor del parámetro `ShowDetails` `equals` `Yes`.
6. Haga clic en _Edición finalizada_ para confirmar los cambios.
7. Haga clic en _Edición finalizada_ en la barra de herramientas del libro para entrar en el modo de lectura.
8. Cambie el valor del parámetro `ShowDetails` a `No`. Observe que el gráfico siguiente desaparece.

En la imagen siguiente se muestra el caso visible en el que `ShowDetails` es `Yes`.

![Imagen que muestra la visibilidad condicional donde el gráfico está visible](./media/workbooks-interactive/conditional-visibility.png)

En la imagen siguiente se muestra el caso oculto en el que `ShowDetails` es `No`.

![Imagen que muestra la visibilidad condicional donde el gráfico está oculto](./media/workbooks-interactive/conditional-invisible.png)

## <a name="next-steps"></a>Pasos siguientes


* [Comience](workbooks-visualizations.md) a aprender más sobre las muchas opciones de visualizaciones enriquecidas de los libros.
* [Controle](workbooks-access-control.md) y comparta el acceso a los recursos del libro.
