---
title: Planificación de la capacidad del clúster en Azure HDInsight
description: Identifique las principales preguntas para planear la capacidad y el rendimiento de un clúster de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 05/07/2020
ms.openlocfilehash: 8e76f767470b9052b25cd2b2958f3f9e9780881b
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714753"
---
# <a name="capacity-planning-for-hdinsight-clusters"></a>Planeamiento de la capacidad de los clústeres de HDInsight

Antes de implementar un clúster de HDInsight, planee la capacidad del clúster deseada; para ello, determine el rendimiento y la escala necesarios. Este planeamiento ayuda a optimizar la facilidad de uso y los costos. Algunas decisiones sobre la capacidad del clúster no se pueden cambiar después de la implementación. Si cambian los parámetros de rendimiento, un clúster se puede desmantelar y volver a crear sin perder los datos almacenados.

Las preguntas claves para planear la capacidad son:

* ¿En qué región geográfica debe implementar el clúster?
* ¿Cuánto almacenamiento necesita?
* ¿Qué tipo de clúster debe implementar?
* ¿Qué tamaño y tipo de máquina virtual (VM) deben usar los nodos del clúster?
* ¿Cuántos nodos de trabajo debe tener el clúster?

## <a name="choose-an-azure-region"></a>Elección de una región de Azure

La región de Azure determina dónde se aprovisiona físicamente el clúster. Para minimizar la latencia de lecturas y escrituras, el clúster debe estar cerca de los datos.

HDInsight está disponible en muchas regiones de Azure. Para encontrar la región más cercana, consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=hdinsight).

## <a name="choose-storage-location-and-size"></a>Elección del tamaño y la ubicación de almacenamiento

### <a name="location-of-default-storage"></a>Ubicación del almacenamiento predeterminado

El almacenamiento predeterminado, ya sea una cuenta de Azure Storage o Azure Data Lake Storage, debe estar en la misma ubicación que el clúster. Azure Storage está disponible en todas las ubicaciones. Data Lake Storage Gen1 está disponible en algunas regiones: consulte la [disponibilidad actual de Data Lake Storage](https://azure.microsoft.com/global-infrastructure/services/?products=storage).

### <a name="location-of-existing-data"></a>Ubicación de los datos existentes

Si quiere usar una cuenta de almacenamiento existente o un recurso de Data Lake Store como el almacenamiento predeterminado del clúster, debe implementar el clúster en esa misma ubicación.

### <a name="storage-size"></a>Tamaño de almacenamiento

En un clúster implementado, puede adjuntar cuentas adicionales de Azure Storage o acceder a otros recursos de Data Lake Storage. Todas las cuentas de almacenamiento deben residir en la misma ubicación que el clúster. Un recurso de Data Lake Storage puede estar en una ubicación distinta, aunque una gran distancia puede presentar alguna latencia.

Azure Storage tiene algunos [límites de capacidad](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits), mientras que Data Lake Storage Gen1 tiene una capacidad casi ilimitada.

Un clúster puede acceder a una combinación de distintas cuentas de almacenamiento. Estos son ejemplos típicos:

* Cuando es probable que la cantidad de datos exceda la capacidad de almacenamiento de un único contenedor de Blob Storage.
* Cuando la velocidad de acceso al contenedor de blobs puede superar el umbral de limitación.
* Cuando desee que los datos que ya están cargados en un contenedor de blobs estén disponibles en el clúster.
* Si desea aislar diferentes partes del almacenamiento por motivos de seguridad o para simplificar la administración.

Para mejorar el rendimiento, utilice un único contenedor por cada cuenta de almacenamiento.

## <a name="choose-a-cluster-type"></a>Elección de un tipo de clúster

El tipo de clúster determina la carga de trabajo configurada para que el clúster de HDInsight la ejecute. Entre los tipos se incluyen [Apache Hadoop](./hadoop/apache-hadoop-introduction.md), [Apache Storm](./storm/apache-storm-overview.md), [Apache Kafka](./kafka/apache-kafka-introduction.md) o [Apache Spark](./spark/apache-spark-overview.md). Para obtener una descripción detallada de los tipos de clúster disponibles, consulte [Introducción a Azure HDInsight](hdinsight-overview.md#cluster-types-in-hdinsight). Cada tipo de clúster tiene una topología de implementación específica que incluye los requisitos del tamaño y el número de nodos.

## <a name="choose-the-vm-size-and-type"></a>Elección del tipo y el tamaño de la máquina virtual

Cada tipo de clúster tiene un conjunto de tipos de nodos, y cada uno de ellos incluye opciones específicas para el tamaño y el tipo de máquina virtual.

Para determinar el tamaño de clúster óptimo de la aplicación, puede realizar pruebas comparativas de la capacidad del clúster y aumentar el tamaño como se indica. Por ejemplo, puede usar una carga de trabajo simulada o una *consulta de valor controlado*. Ejecute las cargas de trabajo simuladas en clústeres de diferentes tamaños. Aumente gradualmente el tamaño hasta alcanzar el rendimiento previsto. Una consulta de valor controlado puede insertarse periódicamente entre las demás consultas de producción para mostrar si el clúster tiene suficientes recursos.

Para obtener más información sobre cómo elegir la familia de máquinas virtuales adecuada para su carga de trabajo, consulte [Selección del tamaño de máquina virtual adecuado para el clúster](hdinsight-selecting-vm-size.md).

## <a name="choose-the-cluster-scale"></a>Elección de la escala del clúster

La cantidad de nodos de una máquina virtual determina la escala de un clúster. Para todos los tipos de clúster, hay tipos de nodos que tienen una escala específica y otros que admiten el escalado horizontal. Por ejemplo, un clúster puede requerir exactamente tres nodos de [Apache ZooKeeper](https://zookeeper.apache.org/) o dos nodos principales. Los nodos de trabajo que procesan datos de forma distribuida pueden beneficiarse de los nodos de trabajo adicionales.

En función del tipo de clúster, el aumento del número de nodos de trabajo agrega capacidad de proceso adicional (por ejemplo, más núcleos). Más nodos aumentarán la memoria total necesaria para que todo el clúster admita el almacenamiento en memoria de los datos que se procesan. Al igual que sucede con la elección del tamaño y el tipo de máquina virtual, la selección de la escala correcta del clúster suele realizarse de forma empírica. Use cargas de trabajo simuladas o consultas controladas.

Puede escalar horizontalmente el clúster para satisfacer las demandas de carga máxima. A continuación, escale de nuevo cuando esos nodos adicionales ya no sean necesarios. La [característica de escalabilidad automática](hdinsight-autoscale-clusters.md) le permite escalar automáticamente el clúster en función de métricas y tiempos predeterminados. Para obtener más información sobre cómo escalar los clústeres de forma manual, consulte [Escalabilidad de clústeres de HDInsight](hdinsight-scaling-best-practices.md).

### <a name="cluster-lifecycle"></a>Ciclo de vida del clúster

Se le cobra en función de la duración del clúster. Si solo hay ocasiones específicas en las que necesita el clúster, puede [crear clústeres a petición mediante Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md). También puede crear scripts de PowerShell que aprovisionen y eliminen el clúster y después programar dichos scripts con [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!NOTE]  
> Cuando se elimina un clúster, su instancia de Hive Metastore predeterminada también se elimina. Para conservar Metastore para volver a crear el siguiente clúster, use un repositorio de metadatos externo, como Azure Database u [Apache Oozie](https://oozie.apache.org/).

### <a name="isolate-cluster-job-errors"></a>Aislamiento de errores de trabajo del clúster

A veces, los errores pueden producirse debido a la ejecución en paralelo de varios componentes de asignación y reducción en un clúster de varios nodos. Para ayudar a aislar el problema, intente pruebas distribuidas. Ejecute varios trabajos simultáneos en un clúster de un solo nodo de trabajo. A continuación, expanda este enfoque para ejecutar varios trabajos simultáneamente en clústeres que contienen más de un nodo. Para crear un clúster de HDInsight de un solo nodo en Azure, use la opción *`Custom(size, settings, apps)`* y use un valor de 1 en *Number of Worker nodes* (Número de nodos de trabajo) en la sección **Cluster size** (Tamaño del clúster) al aprovisionar un nuevo clúster en el portal.

## <a name="quotas"></a>Cuotas

Para más información sobre la administración de las cuotas de suscripción, vea [Solicitud de aumento de cuotas](quota-increase-request.md).

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de clústeres en HDInsight con Hadoop, Spark, Kafka, etc.](hdinsight-hadoop-provision-linux-clusters.md): Aprenda a instalar y configurar clústeres en HDInsight.
* [Supervisión del rendimiento del clúster](hdinsight-key-scenarios-to-monitor.md): obtenga información sobre los escenarios claves para supervisar el clúster de HDInsight que podría afectar a la capacidad del clúster.
