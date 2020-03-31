---
title: Orígenes de datos de libros de Azure Monitor | Microsoft docs
description: Simplifique los informes complejos con libros de Azure Monitor parametrizados predefinidos y personalizados creados a partir de varios orígenes de datos.
services: azure-monitor
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: mbullwin
ms.openlocfilehash: d57910ae31d4db9be17b3dc46b5920a925ab4fcf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79226272"
---
# <a name="azure-monitor-workbooks-data-sources"></a>Orígenes de datos de libros de Azure Monitor

Los libros son compatibles con un gran número de orígenes de datos. Este artículo le guía por los orígenes de datos que están disponibles actualmente para libros de Azure Monitor.

## <a name="logs"></a>Registros

Los libros permiten consultar registros desde los orígenes siguientes:

* Registros de Azure Monitor (recursos de Application Insights y áreas de trabajo de Log Analytics)
* Datos orientados a recursos (registros de actividad)

Los autores de libros pueden usar consultas de KQL que transforman los datos de recursos subyacentes para seleccionar un conjunto de resultados que se puede visualizar como texto, gráficos o cuadrículas.

![Captura de pantalla de la interfaz de informes de registros de libros](./media/workbooks-overview/logs.png)

Los autores de libros pueden realizar consultas fácilmente en varios recursos y crear una experiencia de creación de informes realmente unificada.

## <a name="metrics"></a>Métricas

Los recursos de Azure emiten [métricas](data-platform-metrics.md) a las que se puede acceder mediante los libros. El acceso a las métricas de los libros se puede realizar mediante un control especializado que permite especificar los recursos de destino, las métricas deseadas y su agregación. Estos datos se pueden trazar en gráficos o cuadrículas.

![Captura de pantalla de gráficos de métricas de libro de uso de CPU](./media/workbooks-overview/metrics-graph.png)

![Captura de pantalla de la interfaz de métricas de libro](./media/workbooks-overview/metrics.png)

## <a name="azure-resource-graph"></a>Azure Resource Graph 

Los libros admiten la consulta de recursos y sus metadatos mediante Azure Resource Graph (ARG). Esta funcionalidad se usa principalmente para crear ámbitos de consulta personalizados para los informes. El ámbito de los recursos se expresa mediante un subconjunto KQL que es compatible con ARG, que suele ser suficiente en la mayoría de los casos.

Para que un control de consulta use este origen de datos, utilice la lista desplegable de tipo de consulta para elegir Azure Resource Graph y seleccione las suscripciones de destino. Use el control de consulta para agregar el subconjunto KQL para ARG que selecciona un subconjunto de recursos interesante.


![Captura de pantalla de la consulta KQL de Azure Resource Graph](./media/workbooks-overview/azure-resource-graph.png)

## <a name="alerts-preview"></a>Alertas (versión preliminar)

Los libros permiten a los usuarios visualizar las alertas activas relacionadas con sus recursos. Esta característica permite la creación de informes que reúnen los datos de notificación (alerta) y la información de diagnóstico (métricas, registros) en un informe. Esta información también se puede combinar para crear informes enriquecidos que combinen conclusiones de estos orígenes de datos.

Para que un control de consulta use este origen de datos, utilice la lista desplegable Tipo de consulta para elegir Alertas y seleccione las suscripciones, los grupos de recursos o los recursos de destino. Use las listas desplegables de filtros de alertas para seleccionar un subconjunto interesante de alertas para sus necesidades de análisis.

![Captura de pantalla de consulta de alertas](./media/workbooks-overview/alerts.png)

## <a name="workload-health-preview"></a>Mantenimiento de la carga de trabajo (versión preliminar)

Azure Monitor presenta funcionalidad que supervisa de forma proactiva la disponibilidad y el rendimiento de los sistemas operativos invitados Windows o Linux. Azure Monitor modela componentes clave y sus relaciones, criterios para medir el estado de dichos componentes y qué componentes generan una alerta cuando se detecta una condición incorrecta. Los libros permiten a los usuarios usar esta información para crear informes interactivos enriquecidos.

Para que un control de consulta use este origen de datos, utilice la lista desplegable **Tipo de consulta** para elegir Mantenimiento de la carga de trabajo y seleccione la suscripción, el grupo de recursos o los recursos de máquina virtual de destino. Use las listas desplegables de filtros de mantenimiento para seleccionar un subconjunto interesante de incidentes de mantenimiento para sus necesidades de análisis.

![Captura de pantalla de consulta de alertas](./media/workbooks-overview/workload-health.png)

## <a name="azure-resource-health"></a>Azure Resource Health 

Los libros permiten obtener el estado de los recursos de Azure y combinarlo con otros orígenes de datos para crear informes de mantenimiento completos e interactivos.

Para que un control de consulta use este origen de datos, utilice la lista desplegable **Tipo de consulta** para elegir Azure Health y seleccione los recursos de destino. Use las listas desplegables de filtros de mantenimiento para seleccionar un subconjunto interesante de problemas de recursos para sus necesidades de análisis.

![Captura de pantalla de consulta de alertas](./media/workbooks-overview/resource-health.png)

## <a name="azure-data-explorer-preview"></a>Azure Data Explorer (versión preliminar)

Los libros admiten ahora la consulta desde clústeres de [Azure Data Explorer](https://docs.microsoft.com/azure/data-explorer/) con el poderoso lenguaje de consulta de [Kusto](https://docs.microsoft.com/azure/kusto/query/index).   

![Captura de pantalla de la ventana de consulta de Kusto](./media/workbooks-overview/data-explorer.png)

## <a name="next-steps"></a>Pasos siguientes

* [Comience](workbooks-visualizations.md) a aprender más sobre las muchas opciones de visualizaciones enriquecidas de los libros.
* [Controle](workbooks-access-control.md) y comparta el acceso a los recursos del libro.
