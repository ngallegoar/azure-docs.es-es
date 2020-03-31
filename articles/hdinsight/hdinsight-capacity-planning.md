---
title: Planificación de la capacidad del clúster en Azure HDInsight
description: Identifique las principales preguntas para planear la capacidad y el rendimiento de un clúster de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 69627c961d9224a124fda09f40901f837d627281
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233676"
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

Si ya tiene una cuenta de almacenamiento o un recurso de Data Lake Storage que contiene los datos y desea usar dicho almacenamiento como el predeterminado para el clúster, entonces debe implementar el clúster en esa misma ubicación.

### <a name="storage-size"></a>Tamaño de almacenamiento

Una vez que un clúster de HDInsight ya está implementado, puede adjuntar cuentas adicionales de Azure Storage o acceder a recursos de Data Lake Storage. Todas las cuentas de almacenamiento deben residir en la misma ubicación que el clúster. Un almacén de Data Lake Storage puede estar en una ubicación distinta, aunque esto puede ocasionar algo de latencia de lectura y escritura de datos.

Azure Storage tiene algunos [límites de capacidad](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits), mientras que Data Lake Storage Gen1 tiene una capacidad prácticamente ilimitada.

Un clúster puede acceder a una combinación de distintas cuentas de almacenamiento. Estos son ejemplos típicos:

* Cuando es probable que la cantidad de datos exceda la capacidad de almacenamiento de un único contenedor de Blob Storage.
* Cuando la velocidad de acceso al contenedor de blobs puede superar el umbral de limitación.
* Cuando desee que los datos que ya están cargados en un contenedor de blobs estén disponibles en el clúster.
* Si desea aislar diferentes partes del almacenamiento por motivos de seguridad o para simplificar la administración.

Para mejorar el rendimiento, utilice un único contenedor por cada cuenta de almacenamiento.

## <a name="choose-a-cluster-type"></a>Elección de un tipo de clúster

El tipo de clúster determina la carga de trabajo configurada para que el clúster de HDInsight la ejecute, como [Apache Hadoop](https://hadoop.apache.org/), [Apache Storm](https://storm.apache.org/), [Apache Kafka](https://kafka.apache.org/) o [Apache Spark](https://spark.apache.org/). Para obtener una descripción detallada de los tipos de clúster disponibles, consulte [Introducción a Azure HDInsight](hdinsight-overview.md#cluster-types-in-hdinsight). Cada tipo de clúster tiene una topología de implementación específica que incluye los requisitos del tamaño y el número de nodos.

## <a name="choose-the-vm-size-and-type"></a>Elección del tipo y el tamaño de la máquina virtual

Cada tipo de clúster tiene un conjunto de tipos de nodos, y cada uno de ellos incluye opciones específicas para el tamaño y el tipo de máquina virtual.

Para determinar el tamaño de clúster óptimo de la aplicación, puede realizar pruebas comparativas de la capacidad del clúster y aumentar el tamaño como se indica. Por ejemplo, puede usar una carga de trabajo simulada o una *consulta de valor controlado*. Con una carga de trabajo simulada, ejecute las cargas de trabajo esperadas en clústeres de diferente tamaño, aumentando gradualmente el tamaño hasta alcanzar el rendimiento deseado. Una consulta de valor controlado puede insertarse periódicamente entre las demás consultas de producción para mostrar si el clúster tiene suficientes recursos.

Para obtener más información sobre cómo elegir la familia de máquinas virtuales adecuada para su carga de trabajo, consulte [Selección del tamaño de máquina virtual adecuado para el clúster](hdinsight-selecting-vm-size.md).

## <a name="choose-the-cluster-scale"></a>Elección de la escala del clúster

La cantidad de nodos de una máquina virtual determina la escala de un clúster. Para todos los tipos de clúster, hay tipos de nodos que tienen una escala específica y otros que admiten el escalado horizontal. Por ejemplo, un clúster puede requerir exactamente tres nodos de [Apache ZooKeeper](https://zookeeper.apache.org/) o dos nodos principales. Los nodos de trabajo que procesan datos de forma distribuida pueden beneficiarse del escalado horizontal mediante la adición de nodos de trabajo adicionales.

Dependiendo del tipo de clúster, aumentar el número de nodos de trabajo agrega capacidad de cálculo adicional (por ejemplo, más núcleos), pero también puede agregar la cantidad total de memoria necesaria para que todo el clúster admita el almacenamiento en memoria de los datos procesados. Al igual que sucede con la elección del tamaño y el tipo de máquina virtual, la selección de la escala correcta del clúster suele realizarse de forma empírica con la utilización de cargas de trabajo simuladas o consultas de valores controlados.

Puede escalar horizontalmente el clúster para satisfacer los picos de demandas de carga y, después, volver a reducir verticalmente cuando dichos nodos adicionales ya no sean necesarios. La [característica de escalabilidad automática](hdinsight-autoscale-clusters.md) le permite escalar automáticamente el clúster en función de las métricas y los intervalos predeterminados. Para obtener más información sobre cómo escalar los clústeres de forma manual, consulte [Escalabilidad de clústeres de HDInsight](hdinsight-scaling-best-practices.md).

### <a name="cluster-lifecycle"></a>Ciclo de vida del clúster

Se le cobra en función de la duración del clúster. Si solo necesita que el clúster funcione en momentos específicos, puede [crear clústeres a petición mediante Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md). También puede crear scripts de PowerShell que aprovisionen y eliminen el clúster y después programar dichos scripts con [Azure Automation](https://azure.microsoft.com/services/automation/).

> [!NOTE]  
> Cuando se elimina un clúster, su instancia de Hive Metastore predeterminada también se elimina. Para conservar Metastore para volver a crear el siguiente clúster, use un repositorio de metadatos externo, como Azure Database u [Apache Oozie](https://oozie.apache.org/).
<!-- see [Using external metadata stores](hdinsight-using-external-metadata-stores.md). -->

### <a name="isolate-cluster-job-errors"></a>Aislamiento de errores de trabajo del clúster

A veces, los errores pueden producirse debido a la ejecución en paralelo de varios componentes de asignación y reducción en un clúster de varios nodos. Para facilitar el aislamiento del problema, intente realizar pruebas distribuidas mediante la ejecución de varios trabajos simultáneos en un clúster de un solo nodo de trabajo y luego expanda este enfoque para ejecutar varios trabajos simultáneamente en clústeres que contienen más de un nodo. Para crear un clúster de HDInsight de un solo nodo en Azure, use la opción *Custom (size,settings,apps)* (Personalizar [tamaño, configuración, aplicaciones]) y use un valor de 1 en *Number of Worker nodes* (Número de nodos de trabajo) en la sección **Tamaño de grupo** al aprovisionar un nuevo clúster en el portal.

## <a name="quotas"></a>Cuotas

Después de determinar el tamaño, la escala y el tipo de la máquina virtual del clúster de destino, compruebe los límites de capacidad de cuota actual de la suscripción. Cuando alcance un límite de cuota, puede que no sea capaz de implementar clústeres nuevos o de escalar horizontalmente los clústeres con la adición de más nodos de trabajo. El límite de cuota única es la cuota de núcleos de CPU que existe en el nivel de región para cada suscripción. Por ejemplo, la suscripción puede tener el límite de 30 núcleos en la región Este de EE. UU. 

Siga estos pasos para comprobar los núcleos disponibles:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Vaya a la página **información general** para el clúster de HDInsight. 
3. En el menú de la izquierda, haga clic en **Límites de cuota**.

   La página muestra el número de núcleos en uso, el número de núcleos disponibles y el total de núcleos.

Si fuera necesario solicitar un aumento de la cuota, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
1. Seleccione **Ayuda y soporte técnico** de la parte inferior izquierda de la página.
1. Seleccione **Nueva solicitud de soporte técnico**.
1. En la página **Nueva solicitud de soporte técnico**, en la pestaña **Fundamentos**, seleccione las opciones siguientes:

   - **Tipo de problema**: **Límites de servicio y suscripción (cuotas)**
   - **Suscripción**: la suscripción que desea modificar.
   - **Tipo de cuota**: **HDInsight**

     ![Creación de una solicitud de soporte técnico para aumentar la cuota de núcleos de HDInsight](./media/hdinsight-capacity-planning/hdinsight-quota-support-request.png)

1. Seleccione **Siguiente: Soluciones >>** .
1. En la página **Detalles**, escriba una descripción del problema, seleccione su gravedad y método de contacto preferido, y complete los demás campos obligatorios.
1. Seleccione **Siguiente: Revisar y crear >>** .
1. En la pestaña **Revisar y crear**, seleccione **Crear**.

> [!NOTE]  
> Si necesita aumentar la cuota de núcleos de HDInsight en una región privada, [envíe una solicitud de lista de permitidos](https://aka.ms/canaryintwhitelist).

Puede [ponerse en contacto con el servicio de soporte técnico para solicitar un aumento de la cuota](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request).

Sin embargo, hay algunos límites de cuota fijos; por ejemplo, una única suscripción de Azure puede tener como máximo diez mil núcleos. Para obtener información detallada sobre estos límites, vea [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de clústeres en HDInsight con Hadoop, Spark, Kafka, etc.](hdinsight-hadoop-provision-linux-clusters.md): Aprenda a instalar y configurar clústeres en HDInsight con Apache Hadoop, Spark, Kafka, Interactive Hive, HBase, ML Services o Storm.
* [Supervisión del rendimiento del clúster](hdinsight-key-scenarios-to-monitor.md): obtenga información sobre los escenarios claves para supervisar el clúster de HDInsight que podría afectar a la capacidad del clúster.
