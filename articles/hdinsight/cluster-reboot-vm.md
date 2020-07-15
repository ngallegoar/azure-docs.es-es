---
title: Reinicio de máquinas virtuales para el clúster de Azure HDInsight
description: Aprenda a reiniciar las máquinas virtuales que no respondan para el clúster de HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: c0f0bd9eb423b3de6a602647dff93fd9fce6e13e
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86077021"
---
# <a name="reboot-vms-for-hdinsight-cluster"></a>Reinicio de máquinas virtuales en un clúster de HDInsight

Los clústeres de HDInsight contienen grupos de máquinas virtuales como nodos de clúster. En el caso de los clústeres de larga duración, es posible que estos nodos dejen de responder por diversas razones. En este artículo, se describe cómo reiniciar máquinas virtuales que no respondan en un clúster de HDInsight.

## <a name="when-to-reboot"></a>Cuándo hay que reiniciar

> [!WARNING]  
> El reinicio de las máquinas virtuales en el clúster provoca un tiempo de inactividad del nodo y el reinicio de los servicios del mismo. 

Mientras el nodo se está reiniciando, es posible que el clúster pase a tener un estado incorrecto, que los trabajos se ralenticen o se produzca un error en ellos. Si está intentando reiniciar el nodo principal activo, se terminarán todos los trabajos en ejecución y no podrá enviar trabajos al clúster hasta que los servicios estén de nuevo en funcionamiento. La posibilidad de reiniciar las máquinas virtuales debe considerarse solo cuando sea necesario. A continuación se ofrecen una guía sobre cuándo se deben reiniciar las máquinas virtuales.

- No se puede ejecutar SSH en el nodo, pero este responde a los pings.
- El nodo de trabajo está fuera de servicio sin latido en la interfaz de usuario de Ambari.
- El disco temporal está lleno en el nodo.
- La tabla de procesos de la máquina virtual tiene muchas entradas en las que el proceso se ha completado, pero aparece con estado "Finalizado".

> [!WARNING]  
> Si reinicia máquinas virtuales para los clústeres de **HBase** y **Kafka**, debe tener aún más cuidado, ya que puede provocar la pérdida de datos.

## <a name="use-powershell-to-reboot-vms"></a>Uso de PowerShell para reiniciar máquinas virtuales

La operación de reinicio del nodo incluye dos pasos: enumerar los nodos y reiniciarlos.

1. Enumerar los nodos. Puede obtener la lista de nodos de clúster con [Get-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsighthost). 

  ```
  Get-AzHDInsightHost -ClusterName myclustername
  ```

2. Reiniciar los hosts. Después de obtener los nombres de los nodos que desea reiniciar, reinicie los nodos mediante [Restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost).

  ```
  Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
  ```

## <a name="use-rest-api-to-reboot-vms"></a>Uso de la API REST para reiniciar máquinas virtuales

Puede usar la característica **Probar** en el documento de la API para enviar solicitudes a HDInsight. La operación de reinicio del nodo incluye dos pasos: enumerar los nodos y reiniciarlos.

1. Enumerar los nodos. Puede obtener la lista de nodos de clúster en la API REST o en Ambari. Puede encontrar más detalles en [operación de lista de hosts HDInsight de API REST](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/listhosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

2. Reiniciar los hosts. Después de obtener los nombres de los nodos que desea reiniciar, use la API REST de reinicio de nodos para reiniciar los nodos. El nombre del nodo sigue el patrón de **"NodeType (wn/hn/zk/gw)" + "x" + "6 primeros caracteres del nombre del clúster"** . Puede encontrar más detalles en [operación de reinicio de hosts HDInsight de la API REST](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/restarthosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

Los nombres reales de los nodos que desea reiniciar se especifican en una matriz JSON en el cuerpo de la solicitud.

```json
[
  "wn0-abcdef",
  "zk1-abcdef"
]
```

## <a name="next-steps"></a>Pasos siguientes

* [Restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)
* [API REST de máquinas virtuales de HDInsight](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines)
* [API REST de HDInsight](https://docs.microsoft.com/rest/api/hdinsight/)
