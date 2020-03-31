---
title: 'Modelado de datos en entornos de la versión preliminar: Azure Time Series Insights | Microsoft Docs'
description: Información acerca del modelado de datos en la versión preliminar de Azure Time Series Insights.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/18/2020
ms.custom: seodec18
ms.openlocfilehash: 39ebbf99ad31cce20eabc20fbdc056c889235713
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77470758"
---
# <a name="data-modeling-in-azure-time-series-insights-preview"></a>Modelado de datos en la versión preliminar de Azure Time Series Insights

En este artículo se describe cómo usar el modelo de serie temporal de la versión preliminar de Azure Time Series Insights. Aquí se detallan varios escenarios comunes de datos.

> [!TIP]
> * Lea sobre la versión preliminar del [modelo de serie temporal](time-series-insights-update-tsm.md).
> * Obtenga más información acerca de la interfaz de usuario de la versión preliminar en [Explorador de Azure Time Series Insights (versión preliminar)](./time-series-insights-update-explorer.md).

## <a name="instances"></a>Instancias

El explorador de Azure Time Series Insights admite las operaciones de instancia **CREATE**, **READ**, **UPDATE** y **DELETE** dentro del explorador. 

Para comenzar, seleccione la vista **Modelo** en la vista **Analizar** del explorador de Time Series Insights.

### <a name="create-a-single-instance"></a>Crear una sola instancia

1. Vaya al panel de selección del modelos de serie temporal y seleccione **Instances** (Instancias) en el menú. Se mostrarán todas las instancias asociadas con el entorno de Time Series Insights seleccionado.

    [![Cree una instancia única al seleccionar primero la opción Instancias.](media/v2-update-how-to-tsm/how-to-tsm-instances-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-instances-panel.png#lightbox)

1. Seleccione **+Agregar**.

    [![Agregue una instancia al seleccionar el botón + Agregar.](media/v2-update-how-to-tsm/how-to-tsm-add-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-add-instance.png#lightbox)

1. Escriba los detalles de la instancia, seleccione la asociación de tipo y de jerarquía y seleccione **Create** (Crear).

### <a name="bulk-upload-one-or-more-instances"></a>Carga en bloque de una o más instancias

> [!TIP]
> Puede guardar las instancias en el escritorio en formato JSON. Después, se puede cargar el archivo JSON descargado mediante los pasos siguientes.

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la instancia.

    [![Cargue de forma masiva las instancias mediante JSON.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-instances.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-instances.png#lightbox)

1. Seleccione **Cargar**.

### <a name="edit-a-single-instance"></a>Editar una sola instancia

1. Seleccione la instancia y después el icono **Editar** o **de lápiz**. 
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Edite una sola instancia.](media/v2-update-how-to-tsm/how-to-tsm-edit-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-instance.png#lightbox)

### <a name="delete-an-instance"></a>Eliminar una instancia

1. Seleccione la instancia y después el icono de **eliminación** o el **icono de papelera**.

   [![Seleccione Eliminar para eliminar una instancia.](media/v2-update-how-to-tsm/how-to-tsm-delete-instance.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-instance.png#lightbox)

1. Para confirmar la eliminación, seleccione **Eliminar**.

> [!NOTE]
> Para eliminarse, la instancia debe superar satisfactoriamente una comprobación de validación de campo.

## <a name="hierarchies"></a>Jerarquías

El explorador de Azure Time Series Insights admite las operaciones de jerarquía **CREATE**, **READ**, **UPDATE**, y **DELETE** dentro del explorador. 

Para comenzar, seleccione la vista **Modelo** en la vista **Analizar** del explorador de Time Series Insights.

### <a name="create-a-single-hierarchy"></a>Crear una sola jerarquía

1. Vaya al panel de selección del modelo de serie temporal y seleccione **Hierarchies** (Jerarquías) en el menú. Se mostrarán todas las jerarquías asociadas con el entorno de Time Series Insights seleccionado.

    [![Use el panel para crear una jerarquía.](media/v2-update-how-to-tsm/how-to-tsm-hierarchy-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-hierarchy-panel.png#lightbox)

1. Seleccione **+Agregar**.

    [![Botón + Agregar para las jerarquías.](media/v2-update-how-to-tsm/how-to-tsm-add-new-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-add-new-hierarchy.png#lightbox)

1. Seleccione **+Agregar nivel** en el panel derecho.

    [![Agregue un nivel a la jerarquía.](media/v2-update-how-to-tsm/how-to-tsm-save-hierarchy-levels.png)](media/v2-update-how-to-tsm/how-to-tsm-save-hierarchy-levels.png#lightbox)

1. Escriba los detalles de la jerarquía y seleccione **Guardar**.

    [![Especifique los detalles de la jerarquía.](media/v2-update-how-to-tsm/how-to-tsm-add-hierarchy-level.png)](media/v2-update-how-to-tsm/how-to-tsm-add-hierarchy-level.png#lightbox)

### <a name="bulk-upload-one-or-more-hierarchies"></a>Carga en bloque de una o más jerarquías

> [!TIP]
> Puede guardar las jerarquías en formato JSON en el escritorio. Después, se puede cargar el archivo JSON descargado mediante los pasos siguientes.

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene la carga útil de la jerarquía.
1. Seleccione **Cargar**.

    [![Selecciones para la carga masiva de jerarquías.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-hierarchies.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-hierarchies.png#lightbox)

### <a name="edit-a-single-hierarchy"></a>Editar una sola jerarquía

1. Seleccione la jerarquía y después el icono **Editar** o **de lápiz**.
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Selecciones para la edición de una sola jerarquía.](media/v2-update-how-to-tsm/how-to-tsm-edit-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-hierarchy.png#lightbox)

### <a name="delete-a-hierarchy"></a>Eliminar una jerarquía

1. Seleccione la jerarquía y después el icono **Eliminar** o **de papelera**. 

    [![Para eliminar una jerarquía, seleccione el botón Eliminar.](media/v2-update-how-to-tsm/how-to-tsm-delete-hierarchy.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-hierarchy.png#lightbox)

1. Para confirmar la eliminación, seleccione **Eliminar**.

## <a name="types"></a>Tipos

El explorador de Azure Time Series Insights admite las operaciones de tipo **CREATE**, **READ**, **UPDATE** y **DELETE** dentro del explorador. 

Para comenzar, seleccione la vista **Modelo** en la vista **Analizar** del explorador de Time Series Insights.

### <a name="create-a-single-type"></a>Crear un solo tipo

1. Vaya al panel de selección del modelos de serie temporal y seleccione **Types** (Tipos) en el menú. Se mostrarán todas las instancias asociadas con el entorno de Time Series Insights seleccionado.

    [![Panel de los tipos de modelo de serie temporal.](media/v2-update-how-to-tsm/how-to-tsm-type-panel.png)](media/v2-update-how-to-tsm/how-to-tsm-type-panel.png#lightbox)

1. Seleccione **+ Agregar** para mostrar el elemento emerge+ modal **Agregar un nuevo tipo**.
1. Escriba las propiedades y las variables del tipo. Una vez escritas, seleccione **Guardar**. 

    [![Valores de configuración para agregar un tipo.](media/v2-update-how-to-tsm/how-to-tsm-add-new-type.png)](media/v2-update-how-to-tsm/how-to-tsm-add-new-type.png#lightbox)

### <a name="bulk-upload-one-or-more-types"></a>Carga en bloque de uno o más tipos

> [!TIP]
> Puede guardar los tipos en el escritorio en formato JSON. Después, se puede cargar el archivo JSON descargado mediante los pasos siguientes.

1. Seleccione **Upload JSON** (Cargar JSON).
1. Seleccione el archivo que contiene el tipo de carga útil.
1. Seleccione **Cargar**.

    [![Opciones de carga masiva de tipos.](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-types-json.png)](media/v2-update-how-to-tsm/how-to-tsm-bulk-upload-types-json.png#lightbox)

### <a name="edit-a-single-type"></a>Editar un solo tipo

1. Seleccione el tipo y seleccione el icono **Editar** o **de lápiz**.
1. Realice los cambios necesarios y seleccione **Save** (Guardar).

    [![Edite un tipo en el panel.](media/v2-update-how-to-tsm/how-to-tsm-edit-type.png)](media/v2-update-how-to-tsm/how-to-tsm-edit-type.png#lightbox)

### <a name="delete-a-type"></a>Eliminar un tipo

1. Seleccione la instancia y después el icono **Eliminar**  o **de papelera**. .

   [![Elimine un tipo al seleccionar la opción Eliminar.](media/v2-update-how-to-tsm/how-to-tsm-delete-type.png)](media/v2-update-how-to-tsm/how-to-tsm-delete-type.png#lightbox)

1. Para confirmar la eliminación, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre el modelo de serie temporal, consulte [Modelado de datos](./time-series-insights-update-tsm.md).

- Para obtener más información acerca de la versión preliminar, lea [Visualize data in the Azure Time Series Insights Preview explorer](./time-series-insights-update-explorer.md) (Visualizar datos en el explorador de la versión preliminar de Azure Time Series Insights).

- Para obtener información sobre las formas JSON admitidas, consulte [Formas JSON admitidas](./time-series-insights-send-events.md#supported-json-shapes).
