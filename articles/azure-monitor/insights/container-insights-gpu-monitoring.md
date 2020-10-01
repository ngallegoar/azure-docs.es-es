---
title: Configuración de la supervisión de GPU con Azure Monitor para contenedores | Microsoft Docs
description: En este artículo se describe cómo puede configurar los clústeres de Kubernetes de supervisión con nodos habilitados para GPU NVIDIA y AMD con Azure Monitor para contenedores.
ms.topic: conceptual
ms.date: 03/27/2020
ms.openlocfilehash: 4c6044d407dc4abd0e69bac0190cc19c901022c3
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/09/2020
ms.locfileid: "89569703"
---
# <a name="configure-gpu-monitoring-with-azure-monitor-for-containers"></a>Configuración de la supervisión de GPU con Azure Monitor para contenedores

A partir de la versión del agente *ciprod03022019*, el agente integrado de Azure Monitor para contenedores ahora admite la supervisión del uso de GPU (unidades de procesamiento gráfico) en los nodos de clúster de Kubernetes con reconocimiento de GPU y pods o contenedores de supervisión que solicitan y usan recursos de GPU.

## <a name="supported-gpu-vendors"></a>Proveedores de GPU admitidos

Azure Monitor para contenedores admite la supervisión de clústeres de GPU de los siguientes proveedores de GPU:

- [NVIDIA](https://developer.nvidia.com/kubernetes-gpu)

- [AMD](https://github.com/RadeonOpenCompute/k8s-device-plugin)

Azure Monitor para contenedores inicia automáticamente la supervisión del uso de GPU en los nodos y de los pods y las áreas de trabajo que solicitan GPU mediante la recopilación de las métricas siguientes a intervalos de 60 s y su almacenamiento en la tabla **InsightMetrics**.

>[!NOTE]
>Después de aprovisionar el clúster con nodos de GPU, asegúrese de que el [controlador de GPU](../../aks/gpu-cluster.md) esté instalado como requiere AKS para ejecutar cargas de trabajo de GPU. Azure Monitor para contenedores recopila métricas de GPU a través de los pods del controlador de GPU que se ejecutan en el nodo. 

|Nombre de métrica |Dimensión de métrica (etiquetas) |Descripción |
|------------|------------------------|------------|
|containerGpuDutyCycle |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor|Porcentaje de tiempo del período de muestra anterior (60 segundos) durante el cual la GPU estuvo ocupada o realizando un procesamiento activo para un contenedor. El ciclo de servicio es un número comprendido entre 1 y 100. |
|containerGpuLimits |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Cada contenedor puede especificar límites para una o varias GPU. No es posible solicitar ni limitar una fracción de GPU. |
|containerGpuRequests |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName |Cada contenedor puede solicitar una o varias GPU. No es posible solicitar ni limitar una fracción de GPU.|
|containerGpumemoryTotalBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Cantidad de memoria de GPU en bytes disponible para su uso en un contenedor específico. |
|containerGpumemoryUsedBytes |container.azm.ms/clusterId, container.azm.ms/clusterName, containerName, gpuId, gpuModel, gpuVendor |Cantidad de memoria de GPU en bytes que usa un contenedor específico. |
|nodeGpuAllocatable |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Número de GPU en un nodo que puede usar Kubernetes. |
|nodeGpuCapacity |container.azm.ms/clusterId, container.azm.ms/clusterName, gpuVendor |Número total de GPU en un nodo. |

## <a name="gpu-performance-charts"></a>Gráficos de rendimiento de GPU 

Azure Monitor para contenedores incluye gráficos preconfigurados para las métricas indicadas anteriormente en la tabla como libro de GPU para cada clúster. Puede encontrar el libro de GPU **GPU de nodo** directamente de un clúster de AKS. Para ello, seleccione **Libros** en el panel izquierdo y, en la lista desplegable **Ver libros** de Conclusión.

## <a name="next-steps"></a>Pasos siguientes

- Consulte [Uso de GPU para cargas de trabajo de cálculo intensivo en Azure Kubernetes Service (AKS)](../../aks/gpu-cluster.md) para obtener información sobre cómo implementar un clúster de AKS que incluya nodos habilitados para GPU.

- Obtenga más información sobre los [Tamaños de máquinas virtuales optimizadas para GPU](../../virtual-machines/sizes-gpu.md).

- Revise la [compatibilidad con GPU en Kubernetes](https://kubernetes.io/docs/tasks/manage-gpus/scheduling-gpus/) para obtener más información sobre la compatibilidad experimental de Kubernetes con la administración de GPU en uno o varios nodos de un clúster.
