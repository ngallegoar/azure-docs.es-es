---
title: Libros de Azure Monitor con parámetros desplegables
description: Simplifique la creación de informes complejos con libros parametrizados predefinidos y personalizados.
services: azure-monitor
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: 73b6029dfe52a4b32c9a8ce092fcd284ac1ec0e7
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2020
ms.locfileid: "85965062"
---
# <a name="workbook-drop-down-parameters"></a>Parámetros de lista desplegable de libros

Las listas desplegables permiten al usuario recopilar uno o varios valores de entrada de un conjunto conocido (por ejemplo, seleccionar una de las solicitudes de la aplicación). Proporcionan una manera fácil de recopilar entradas arbitrarias de los usuarios. Resultan especialmente útiles para habilitar el filtrado en los informes interactivos. 

La forma más fácil de especificar una lista desplegable es proporcionar una lista estática en la configuración de parámetros. Una manera más interesante es obtener la lista dinámicamente a través de una consulta de KQL. La configuración de parámetros también le permite especificar si se trata de una selección única o múltiple y, en este último caso, qué formato debe tener el conjunto de resultados (delimitador, comillas, etc.).

## <a name="creating-a-static-drop-down-parameter"></a>Creación de un parámetro de lista desplegable estática

1. Comience con un libro vacío en modo de edición.
2. Elija _Agregar parámetros_ desde los vínculos del libro.
3. Haga clic en el botón azul _Agregar parámetro_.
4. En el panel de nuevo parámetro que aparece, escriba:
    1. Nombre de parámetro: `Environment`
    2. Tipo de parámetro: `Drop down`
    3. Obligatorio: `checked`
    4. Permitir `multiple selection`: `unchecked`
    5. Obtener datos de: `JSON`
5. En el bloque de texto de entrada JSON, inserte este fragmento de código JSON:
    ```json
    [
        { "value":"dev", "label":"Development" },
        { "value":"ppe", "label":"Pre-production" },
        { "value":"prod", "label":"Production", "selected":true }
    ]
    ```
6. Presione el botón azul `Update`.
7. Elija "Guardar" en la barra de herramientas para crear el parámetro.
8. El parámetro Environment será una lista desplegable con los tres valores.

    ![Imagen que muestra la creación de una lista desplegable estática](./media/workbook-dropdowns/dropdown-create.png)

## <a name="creating-a-static-dropdown-with-groups-of-items"></a>Creación de una lista desplegable estática con grupos de elementos

Si el resultado de la consulta o el código JSON contienen un campo "group", la lista desplegable mostrará grupos de valores. Siga el ejemplo anterior, pero use en su lugar el siguiente código JSON:

```json
[
    { "value":"dev", "label":"Development", "group":"Development" },
    { "value":"dev-cloud", "label":"Development (Cloud)", "group":"Development" },
    { "value":"ppe", "label":"Pre-production", "group":"Test" },
    { "value":"ppe-test", "label":"Pre-production (Test)", "group":"Test" },
    { "value":"prod1", "label":"Prod 1", "selected":true, "group":"Production" },
    { "value":"prod2", "label":"Prod 2", "group":"Production" }
]
```

![Imagen que muestra un ejemplo de una lista desplegable agrupada](./media/workbook-dropdowns/grouped-dropDown.png)


## <a name="creating-a-dynamic-drop-down-parameter"></a>Creación de un parámetro de lista desplegable dinámica
1. Comience con un libro vacío en modo de edición.
2. Elija _Agregar parámetros_ desde los vínculos del libro.
3. Haga clic en el botón azul _Agregar parámetro_.
4. En el panel de nuevo parámetro que aparece, escriba:
    1. Nombre de parámetro: `RequestName`
    2. Tipo de parámetro: `Drop down`
    3. Obligatorio: `checked`
    4. Permitir `multiple selection`: `unchecked`
    5. Obtener datos de: `Query`
5. En el bloque de texto de entrada JSON, inserte este fragmento de código JSON:

    ```kusto
        requests
        | summarize by name
        | order by name asc
    ```
1. Presione el botón azul `Run Query`.
2. Elija "Guardar" en la barra de herramientas para crear el parámetro.
3. El parámetro RequestName será una lista desplegable de los nombres de todas las solicitudes de la aplicación.

    ![Imagen que muestra la creación de una lista desplegable dinámica](./media/workbook-dropdowns/dropdown-dynamic.png)

## <a name="referencing-drop-down-parameter"></a>Referencia a parámetro de lista desplegable

### <a name="in-kql"></a>En KQL
1. Agregue un control de consulta al libro y seleccione un recurso de Application Insights.
2. En el editor de KQL, escriba este fragmento de código.

    ```kusto
        requests
        | where name == '{RequestName}'
        | summarize Requests = count() by bin(timestamp, 1h)

    ```
3. El tiempo de evaluación de consulta se expande a:

    ```kusto
        requests
        | where name == 'GET Home/Index'
        | summarize Requests = count() by bin(timestamp, 1h)
    ```

4. Ejecute la consulta para ver los resultados. También se puede representar como un gráfico.

    ![Imagen que muestra una lista desplegable a la que se hace referencia en KQL](./media/workbook-dropdowns/dropdown-reference.png)


## <a name="parameter-value-label-selection-and-group"></a>Valor del parámetro, etiqueta, selección y grupo
La consulta utilizada en el parámetro de lista desplegable dinámica anterior solo devuelve una lista de valores que se representan fielmente en el menú desplegable. Pero, ¿qué ocurre si quisiera seleccionar un nombre para mostrar diferente o uno de estos? Los parámetros de lista desplegable permiten esto mediante las columnas de valor, etiqueta, selección y grupo.

En el ejemplo siguiente se muestra cómo obtener una lista de dependencias de Application Insights cuyos nombres para mostrar tienen el estilo de un emoticono, tiene la primera seleccionada y está agrupada por el nombre de la operación.

```kusto
dependencies
| summarize by operation_Name, name
| where name !contains ('.')
| order by name asc
| serialize Rank = row_number()
| project value = name, label = strcat('🌐 ', name), selected = iff(Rank == 1, true, false), group = operation_Name
```

![Imagen que muestra un parámetro desplegable con las opciones de valor, etiqueta, selección y grupo](./media/workbook-dropdowns/dropdown-more-options.png)


## <a name="drop-down-parameter-options"></a>Opciones de parámetros lista desplegable
| Parámetro | Explicación | Ejemplo |
| ------------- |:-------------|:-------------|
| `{DependencyName}` | El valor seleccionado | GET fabrikamaccount |
| `{DependencyName:label}` | La etiqueta seleccionada | 🌐 GET fabrikamaccount |
| `{DependencyName:value}` | El valor seleccionado | GET fabrikamaccount |

## <a name="multiple-selection"></a>Selección múltiple
Hasta ahora, los ejemplos establecen explícitamente el parámetro para seleccionar solo un valor de la lista desplegable. Los parámetros de lista desplegable también admiten `multiple selection`; para habilitarlo, basta con marcar la opción `Allow multiple selection`. 

El usuario también tiene la opción de especificar el formato del conjunto de resultados a través de la configuración de `delimiter` y `quote with`. De forma predeterminada, solo se devuelven los valores como una colección con este formato: "a", "b", "c". También tienen la opción de limitar el número de selecciones.

El KQL que hace referencia al parámetro tendrá que cambiar para que funcione con el formato del resultado. La forma más común de habilitarlo es a través del operador `in`.

```kusto
dependencies
| where name in ({DependencyName})
| summarize Requests = count() by bin(timestamp, 1h), name
```

Este es un ejemplo de una lista desplegable de selección múltiple en el trabajo:

![Imagen que muestra un parámetro de lista desplegable de selección múltiple](./media/workbook-dropdowns/dropdown-multiselect.png)

## <a name="next-steps"></a>Pasos siguientes

* [Comience](workbooks-visualizations.md) a aprender más sobre las muchas opciones de visualizaciones enriquecidas de los libros.
* [Controle](workbooks-access-control.md) y comparta el acceso a los recursos del libro.
