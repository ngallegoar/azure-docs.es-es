---
title: Supervisión de Azure Machine Learning | Microsoft Docs
description: Aprenda a usar Azure Monitor para ver, analizar y crear alertas de métricas de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 03/05/2020
ms.openlocfilehash: eb4f46322bec57fb4412d3ddebb345640556ca5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399099"
---
# <a name="monitoring-azure-machine-learning"></a>Supervisión de Azure Machine Learning

En este artículo se describen los datos de supervisión generados por Azure Machine Learning. También se describe cómo puede usar Azure Monitor para analizar los datos y definir alertas.

> [!TIP]
> La información de este documento está destinada principalmente a los administradores, ya que describe la supervisión de Azure Machine Learning. Los científicos de datos o desarrolladores que desee supervisar información específica acerca de las ejecuciones de entrenamiento de un modelo deben ver los siguientes documentos:
>
> * [Inicio, supervisión y cancelación de las ejecuciones de entrenamiento](how-to-manage-runs.md)
> * [Métricas de registro de las ejecuciones de entrenamientos](how-to-track-experiments.md)
> * [Seguimiento de experimentos con MLflow](how-to-use-mlflow.md)
> * [Visualización de ejecuciones con TensorBoard](how-to-monitor-tensorboard.md)

## <a name="azure-monitor"></a>Azure Monitor

Azure Machine Learning registra los datos de supervisión mediante Azure Monitor, que es un servicio de supervisión de pila completo en Azure. Azure Monitor proporciona un conjunto completo de características para supervisar los recursos de Azure. También puede supervisar los recursos de otras nubes y de forma local.

Empiece con el artículo [Introducción a Azure Monitor](/azure/azure-monitor/overview), que proporciona información general sobre las capacidades de supervisión. Las secciones siguientes se basan en esta información al proporcionar aspectos específicos de uso de Azure Monitor con Azure Machine Learning.

Para comprender los costos asociados a Azure Monitor, consulte [Uso y costos estimados](/azure/azure-monitor/platform/usage-estimated-costs). Para comprender el tiempo necesario para que los datos aparezcan en Azure Monitor, consulte [Tiempo de ingesta de datos de registro](/azure/azure-monitor/platform/data-ingestion-time).

## <a name="monitoring-data-from-azure-machine-learning"></a>Supervisión de datos de Azure Machine Learning

Azure Machine Learning recopila los mismos tipos de datos de supervisión que otros recursos de Azure, que se describen en [Supervisión de datos de recursos de Azure](/azure/azure-monitor/insights/monitor-azure-resource#monitoring-data). Consulte [Referencia de datos de supervisión de Azure Machine Learning](monitor-resource-reference.md) para obtener una referencia detallada de los registros y las métricas creados por Azure Machine Learning.

## <a name="analyzing-metric-data"></a>Analizar datos de métricas

Puede analizar las métricas de Azure Machine Learning abriendo **Métricas** en el menú **Azure Monitor**. Consulte [Introducción al explorador de métricas de Azure](/azure/azure-monitor/platform/metrics-getting-started) para más información sobre esta herramienta.

Todas las métricas de Azure Machine Learning se encuentran en el espacio de nombres **Área de trabajo de servicio de Machine Learning**.

![Explorador de métricas con Machine Learning Service Workspace seleccionada](./media/monitor-azure-machine-learning/metrics.png)

### <a name="filtering-and-splitting"></a>Filtrar y dividir

En el caso de las métricas que admiten dimensiones, puede aplicar filtros con un valor de dimensión. Por ejemplo, el filtrado de **Núcleos activos** para un **Nombre de clúster** de `cpu-cluster`. 

Puede dividir una métrica por dimensión para visualizar la comparación de unos segmentos de la métrica con otros. Por ejemplo, dividir el **Tipo de paso de canalización** para ver un recuento de los tipos de pasos que se usan en la canalización.

Para más información sobre el filtrado y la división, vea [Características avanzadas de Azure Monitor](/azure/azure-monitor/platform/metrics-charts).

## <a name="alerts"></a>Alertas

Puede acceder a las métricas de Azure Machine Learning abriendo **Alertas** en el menú **Azure Monitor**. Vea [Creación, visualización y administración de alertas de métricas mediante Azure Monitor](/azure/azure-monitor/platform/alerts-metric) para más detalles sobre la creación de alertas.

En la tabla siguiente se enumeran las reglas de alertas de métricas comunes y recomendadas para Azure Machine Learning:

| Tipo de alerta | Condición | Descripción |
|:---|:---|:---|
| Error al realizar la implementación de modelo | Tipo de agregación: Total, operador: Mayor que, valor de umbral: 0 | Cuando se han producido errores en una o varias implementaciones de modelos |
| Porcentaje de uso de la cuota | Tipo de agregación: Media, operador: Mayor que, valor de umbral: 90| Cuando el porcentaje de uso de la cuota es superior al 90 % |
| Nodos no utilizables | Tipo de agregación: Total, operador: Mayor que, valor de umbral: 0 | Cuando hay uno o más nodos no utilizables |

## <a name="configuration"></a>Configuración

> [!IMPORTANT]
> __No es necesario configurar las métricas de Azure Machine Learning__, sino que se recopilan automáticamente y están disponibles en el explorador de métricas para supervisión y alertas.

Puede agregar una configuración de diagnóstico para configurar la siguiente funcionalidad:

* Archive la información del registro y las métricas en una cuenta de almacenamiento de Azure.
* Transmita la información del registro y de las métricas a un centro de eventos de Azure.
* Envíe la información de registro y métricas al análisis de registros de Azure Monitor.

La habilitación de esta configuración requiere servicios adicionales de Azure (cuenta de almacenamiento, centro de eventos o Log Analytics), lo que puede aumentar el costo. Para calcular un costo estimado, visite la [Calculadora de precios de Azure](https://azure.microsoft.com/pricing/calculator).

Para más información sobre cómo crear una configuración de diagnóstico, consulte [Creación de una configuración de diagnóstico para recopilar registros de plataforma y métricas en Azure](/azure/azure-monitor/platform/diagnostic-settings).

Puede configurar los siguientes registros para Azure Machine Learning:

| Category | Descripción |
|:---|:---|
| AmlComputeClusterEvent | Eventos de clústeres de proceso de Azure Machine Learning. |
| AmlComputeClusterNodeEvent | Eventos de nodos dentro de un clúster de proceso de Azure Machine Learning. |
| AmlComputeJobEvent | Eventos de trabajos en ejecución en un proceso de Azure Machine Learning. |

> [!NOTE]
> Cuando se habilitan las métricas en una configuración de diagnóstico, la información de dimensión no se incluye actualmente como parte de la información que se envía a una cuenta de almacenamiento, a un centro de eventos o al análisis de registros.

## <a name="analyzing-log-data"></a>Analizar datos de registro

El uso de Log Analytics de Azure Monitor requiere la creación de una configuración de diagnóstico y habilitar __Enviar información a Log Analytics__. Para más información, consulte la sección [Configuración](#configuration).

Los datos de los registros de Azure Monitor se almacenan en tablas, cada tabla tiene su propio conjunto de propiedades únicas. Azure Machine Learning almacena los datos en las tablas siguientes:

| Tabla | Descripción |
|:---|:---|
| AmlComputeClusterEvent | Eventos de clústeres de proceso de Azure Machine Learning. |
| AmlComputeClusterNodeEvent | Eventos de nodos dentro de un clúster de proceso de Azure Machine Learning. |
| AmlComputeJobEvent | Eventos de trabajos en ejecución en un proceso de Azure Machine Learning. |

> [!IMPORTANT]
> Al seleccionar **Registros** en el menú Azure Machine Learning, Log Analytics se abre con el ámbito de la consulta establecido en el área de trabajo actual. Esto significa que las consultas de registro solo incluirán datos de ese recurso. Si desea ejecutar una consulta que incluya datos de otras bases de datos o de otros servicios de Azure, elija **Registros** en el menú **Azure Monitor**. Consulte [Ámbito e intervalo de tiempo de una consulta de registro en Log Analytics de Azure Monitor](/azure/azure-monitor/log-query/scope/) para obtener más información.

Consulte [Referencia de datos de supervisión de Azure Machine Learning](monitor-resource-reference.md) para una referencia detallada de los registros y las métricas.

### <a name="sample-queries"></a>Consultas de ejemplo

A continuación se muestran las consultas que puede usar para supervisar los recursos de Azure Machine Learning: 

+ Obtener trabajos con errores en los últimos cinco días:

    ```Kusto
    AmlComputeJobEvent
    | where TimeGenerated > ago(5d) and EventType == "JobFailed"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Obtener registros para un nombre de trabajo específico:

    ```Kusto
    AmlComputeJobEvent
    | where JobName == "automl_a9940991-dedb-4262-9763-2fd08b79d8fb_setup"
    | project  TimeGenerated , ClusterId , EventType , ExecutionState , ToolType
    ```

+ Obtiene los eventos de clúster en los últimos cinco días para los clústeres en los que el tamaño de la máquina virtual es Standard_D1_V2:

    ```Kusto
    AmlComputeClusterEvent
    | where TimeGenerated > ago(4d) and VmSize == "STANDARD_D1_V2"
    | project  ClusterName , InitialNodeCount , MaximumNodeCount , QuotaAllocated , QuotaUtilized
    ```

+ Obtener nodos asignados en los últimos ocho días:

    ```Kusto
    AmlComputeClusterNodeEvent
    | where TimeGenerated > ago(8d) and NodeAllocationTime  > ago(8d)
    | distinct NodeId
    ```

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Referencia de datos de supervisión de Azure Machine Learning](monitor-resource-reference.md) como referencia de los registros y las métricas.
- Para obtener información sobre cómo trabajar con cuotas relacionadas con Azure Machine Learning, consulte [Administración y solicitud de cuotas para recursos de Azure](how-to-manage-quotas.md).
- Para más información sobre la supervisión de recursos de Azure, consulte [Supervisión de recursos de Azure con Azure Monitor](/azure/azure-monitor/insights/monitor-azure-resource).
