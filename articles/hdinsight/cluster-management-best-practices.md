---
title: 'Procedimientos recomendados de administración de clústeres: Azure HDInsight'
description: Aprenda los procedimientos recomendados para la administración de clústeres de HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: fd79568944d81e267a45287104bd0fa9698df2fb
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "89648693"
---
# <a name="hdinsight-cluster-management-best-practices"></a>Procedimientos recomendados de administración de clústeres de HDInsight

Aprenda los procedimientos recomendados para la administración de clústeres de HDInsight.

## <a name="how-do-i-create-hdinsight-clusters"></a>¿Cómo se crean los clústeres de HDInsight?

| Opción | Documentos |
|---|---|
| Azure Data Factory | [Creación de clústeres de Apache Hadoop a petición en HDInsight mediante Azure Data Factory](./hdinsight-hadoop-create-linux-clusters-adf.md) |
| Personalización de una plantilla de Resource Manager | [Creación de clústeres de Apache Hadoop en HDInsight con plantillas de Resource Manager](./hdinsight-hadoop-create-linux-clusters-arm-templates.md) |
| Plantillas de inicio rápido | [Plantillas de inicio rápido de HDInsight](https://azure.microsoft.com/resources/templates/?term=hdinsight) |
| Ejemplos de Azure | [Ejemplos de Azure en HDInsight](https://docs.microsoft.com/samples/browse/?products=azure-hdinsight) |
| Portal de Azure | [Creación de clústeres basados en Linux en HDInsight mediante Azure Portal](./spark/apache-spark-intellij-tool-plugin.md) |
| Azure CLI | [Creación de clústeres de HDInsight mediante la CLI de Azure](./hdinsight-hadoop-create-linux-clusters-azure-cli.md) |
| Azure PowerShell | [Creación de clústeres basados en Linux en HDInsight con Azure PowerShell](./hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |
| cURL | [Creación de clústeres de Apache Hadoop mediante la API REST de Azure](./hdinsight-hadoop-create-linux-clusters-curl-rest.md) |
| SDK (.NET, Python, Java) | [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet), [Python](https://docs.microsoft.com/python/api/overview/azure/hdinsight?view=azure-python), [Java](https://docs.microsoft.com/java/api/overview/azure/hdinsight?view=azure-java-stable), [Go](https://docs.microsoft.com/azure/hdinsight/hdinsight-go-sdk-overview) |

> [!Note]
> Si va a crear un clúster y quiere volver a usar el nombre de uno creado anteriormente, espere hasta que se complete la eliminación del clúster anterior para crear otro.

## <a name="how-do-i-customize-hdinsight-clusters"></a>¿Cómo se personalizan los clústeres de HDInsight?

| Opción | Documentos |
|---|---|
| Acciones de script | [Personalización de los clústeres de Azure HDInsight mediante acciones de script](./hdinsight-hadoop-customize-cluster-linux.md) |
| Bootstrap | [Personalización de los clústeres de HDInsight con Bootstrap](./hdinsight-hadoop-customize-cluster-bootstrap.md) |
| Tiendas de metadatos externas | [Uso de repositorios de metadatos externos en Azure HDInsight](./hdinsight-use-external-metadata-stores.md) |
| Base de datos de Ambari personalizada | [Configuración de los clústeres de HDInsight con una base de datos de Ambari personalizada](./hdinsight-custom-ambari-db.md) |

## <a name="what-are-some-errors-i-might-face-when-creating-clusters"></a>¿Cuáles son algunos de los errores que pueden producirse al crear clústeres?

| Error | Más información |
|---|---|
| No quota (Sin cuota) | Existen cuotas para el número de clústeres que se pueden crear en la suscripción en cada región. Para más información, consulte [Planeamiento de la capacidad: cuotas](./hdinsight-capacity-planning.md). |
| No more IP addresses available (No hay más direcciones IP disponibles) | Cada red virtual tiene un número limitado de direcciones IP. Al crear un clúster de HDInsight, cada nodo (incluidos los nodos de Zookeeper y de puerta de enlace) usa algunas de estas direcciones IP asignadas. Este error se produce cuando todas las direcciones IP estén en uso.  |
| Network security group (NSG) rules don't allow communication with HDInsight resource providers (Las reglas del grupo de seguridad de red no permiten la comunicación con los proveedores de recursos de HDInsight) | Si usa grupos de seguridad de red o rutas definidas por el usuario (UDR) para controlar el tráfico entrante a su clúster de HDInsight, debe asegurarse de que este pueda comunicarse con los servicios críticos de mantenimiento y administración de Azure. Para más información, consulte [Etiquetas de servicio del grupo de seguridad de red (NSG) para Azure HDInsight](./hdinsight-service-tags.md). |
| Reuse of cluster name (Reutilización del nombre del clúster) | Al usar un nombre de clúster que ya se haya usado anteriormente, es necesario esperar X minutos para volver a crear el clúster. De lo contrario, verá un mensaje que indicará que el recurso ya existe. |

## <a name="how-do-i-manage-running-hdinsight-clusters"></a>¿Cómo se administran los clústeres de HDInsight en ejecución?

| Opción | Documentos |
|---|---|
| Escalado automático | [Escalado automático de clústeres de Azure HDInsight](./hdinsight-autoscale-clusters.md) |
| Escala manual | [Escala de clústeres de Azure HDInsight](./hdinsight-scaling-best-practices.md) |
| Supervisión con Ambari| [Supervisión del rendimiento de un clúster en Azure HDInsight](./hdinsight-key-scenarios-to-monitor.md) |
| Supervisión con registros de Azure Monitor | [Uso de los registros de Azure Monitor para supervisar clústeres de HDInsight](./hdinsight-hadoop-oms-log-analytics-tutorial.md) |
| Problemas de servicio, mantenimiento planeado y advertencias de estado y seguridad | [Suscripción a alertas de estado de servicio específicas de la suscripción](https://docs.microsoft.com/azure/service-health/alerts-activity-log-service-notifications-portal) |


## <a name="how-do-i-check-on-deleted-hdinsight-clusters"></a>¿Cómo se comprueban los clústeres de HDInsight eliminados?

### <a name="azure-monitor-logs"></a>Registros de Azure Monitor

Puede usar la siguiente consulta con registros de Azure Monitor para supervisar los clústeres eliminados.

```loganalytics
AzureActivity
| where ResourceProvider == "Microsoft.HDInsight" and (OperationName == "Create or Update Cluster" or OperationName == "Delete Cluster") and ActivityStatus == "Succeeded"
```

## <a name="next-steps"></a>Pasos siguientes

* [Planeamiento de la capacidad de los clústeres de HDInsight](./hdinsight-capacity-planning.md)
* [¿Cuáles son las configuraciones de nodos predeterminadas y recomendadas para Azure HDInsight?](./hdinsight-supported-node-configuration.md)
