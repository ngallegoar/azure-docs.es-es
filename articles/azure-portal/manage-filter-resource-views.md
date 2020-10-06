---
title: Visualización y filtrado de información de los recursos de Azure
description: Filtre la información y use vistas diferentes para conocer mejor los recursos de Azure.
author: mgblythe
ms.service: azure-portal
ms.topic: quickstart
ms.author: mblythe
ms.date: 09/11/2020
ms.openlocfilehash: 21dcadaa929b90e72c7d429229fc6ac4e0a66683
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2020
ms.locfileid: "90605480"
---
# <a name="view-and-filter-azure-resource-information"></a>Visualización y filtrado de información de los recursos de Azure

Azure Portal le permite examinar información detallada sobre los recursos de sus suscripciones de Azure. En este artículo se indica cómo filtrar la información y usar vistas diferentes para conocer mejor los recursos de Azure.

El artículo se centra en la pantalla **Todos los recursos** que se muestra en la captura de pantalla siguiente. Las pantallas de tipos de recursos individuales como, por ejemplo, las máquinas virtuales, tienen opciones diferentes, como iniciar y detener una máquina virtual.

:::image type="content" source="media/manage-filter-resource-views/all-resources.png" alt-text="Vista de todos los recursos en Azure Portal":::

## <a name="filter-resources"></a>Filtrado de recursos

Empiece a explorar **Todos los recursos** y use los filtros para centrarse en un subconjunto de los recursos. En la captura de pantalla siguiente se muestra el filtrado de grupos de recursos y la selección de dos de los seis grupos de recursos de una suscripción.

:::image type="content" source="media/manage-filter-resource-views/filter-resource-group.png" alt-text="Vista de todos los recursos en Azure Portal" en uno de los dos grupos de recursos ya seleccionados.

:::image type="content" source="media/manage-filter-resource-views/filter-simplewinvm.png" alt-text="Vista de todos los recursos en Azure Portal":::

Para cambiar las columnas que se incluyen en una vista, seleccione **Administrar vista** y, a continuación, **Editar columnas**.

:::image type="content" source="media/manage-filter-resource-views/edit-columns.png" alt-text="Vista de todos los recursos en Azure Portal":::

## <a name="save-use-and-delete-views"></a>Guardar, usar y eliminar vistas

Puede guardar las vistas que incluyen los filtros y columnas que ha seleccionado. Para guardar y usar una vista:

1. Seleccione **Administrar vista** y, a continuación, **Guardar vista**.

1. Escriba un nombre para la vista y, a continuación, seleccione **Aceptar**. La vista guardada aparece ahora en el menú **Administrar vista**.

    :::image type="content" source="media/manage-filter-resource-views/simple-view.png" alt-text="Vista de todos los recursos en Azure Portal":::

1. Para usar una vista, cambie de **Predeterminada** a una de sus propias vistas para ver cómo afecta a la lista de los recursos que aparecen.

Para eliminar una vista:

1. Seleccione **Administrar vista** y, después, **Examinar todas las visualizaciones**.

1. En el panel **Vistas guardadas para "Todos los recursos"** , seleccione la vista y, a continuación, seleccione el icono **Eliminar** ![icono de la vista Eliminar](media/manage-filter-resource-views/icon-delete.png).

## <a name="summarize-resources-with-visuals"></a>Resumen de los recursos con objetos visuales

Las vistas que hemos examinado hasta ahora han sido _vistas de lista_, pero también hay _vistas de resumen_ que incluyen objetos visuales. Puede guardar y usar estas vistas de la misma manera que puede ver las vistas de listas. Los filtros se mantienen entre los dos tipos de vistas. Para guardar y usar una vista de resumen:

1. En el menú de la vista, seleccione **Vista de resumen**.

    :::image type="content" source="media/manage-filter-resource-views/menu-summary-view.png" alt-text="Vista de todos los recursos en Azure Portal":::

1. La vista de resumen le permite resumir según diferentes atributos, como **Ubicación** y **Tipo**. Seleccione una opción **Resumir por** y un objeto visual adecuado. En la siguiente captura de pantalla aparece el **tipo de resumen** con el objeto visual **Gráfico de barras**.

    :::image type="content" source="media/manage-filter-resource-views/type-summary-bar-chart.png" alt-text="Vista de todos los recursos en Azure Portal":::

1. Seleccione **Administrar vista** y, después, **Guardar** para guardar esta vista como hizo con la vista de lista.

1. En la vista de resumen, en **Tipo de resumen**, seleccione una barra del gráfico. Al seleccionar la barra, se proporciona una lista filtrada para un tipo de recurso.

    :::image type="content" source="media/manage-filter-resource-views/all-resources-filtered-type.png" alt-text="Vista de todos los recursos en Azure Portal":::

## <a name="run-queries-in-azure-resource-graph"></a>Ejecución de consultas en Azure Resource Graph

Azure Resource Graph proporciona una exploración de recursos eficaz con la posibilidad de realizar consultas a escala en un conjunto de suscripciones. La pantalla **Todos los recursos** de Azure Portal incluye un vínculo para abrir una consulta de Resource Graph cuyo ámbito es la vista filtrada actual.

Para ejecutar una consulta de Resource Graph:

1. Seleccione **Abrir consulta**.

    :::image type="content" source="media/manage-filter-resource-views/open-query.png" alt-text="Vista de todos los recursos en Azure Portal":::

1. En **Azure Resource Graph Explorer**, seleccione **Ejecutar consulta** para ver los resultados.

    :::image type="content" source="media/manage-filter-resource-views/run-query.png" alt-text="Vista de todos los recursos en Azure Portal":::

    Para más información, consulte [Ejecución de la primera consulta de Resource Graph mediante Azure Resource Graph Explorer](../governance/resource-graph/first-query-portal.md).

## <a name="next-steps"></a>Pasos siguientes

[Introducción a Azure Portal](azure-portal-overview.md)

[Creación y uso compartido de paneles en Azure Portal](azure-portal-dashboards.md)
