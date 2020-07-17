---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/11/2020
ms.openlocfilehash: a4faab9ac6d5e1c39c1120e09dae792b95892d60
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564412"
---
# <a name="release-notes"></a>Notas de la versión

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto en Azure.

## <a name="release-date-06112020"></a>Fecha de lanzamiento: 11/06/2020

Esta versión se aplica a HDInsight 3.6 y 4.0. La versión de HDInsight se pone a disposición de todas las regiones durante varios días. Esta fecha de lanzamiento indica la fecha de lanzamiento de la primera región. Si no ve los cambios siguientes, espere unos días a que la versión se active en su región.

## <a name="new-features"></a>Nuevas características
### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Movimiento a conjuntos de escalado de máquinas virtuales
Ahora, HDInsight usa máquinas virtuales de Azure para aprovisionar el clúster. A partir de esta versión, los nuevos clústeres de HDInsight comienzan a usar el conjunto de escalado de máquinas virtuales de Azure. El cambio se implementa gradualmente, y no se producirá ningún cambio importante. Consulte más información sobre los [conjuntos de escalado de máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview).
 
### <a name="reboot-vms-in-hdinsight-cluster"></a>Reinicio de máquinas virtuales en un clúster de HDInsight
En esta versión, se admite el reinicio de máquinas virtuales en el clúster de HDInsight para reiniciar los nodos que no responden. Actualmente solo puede hacerlo a través de la API, pero en un futuro se admitirán PowerShell y la CLI. Para obtener más información sobre la API, vea [este documento](https://github.com/Azure/azure-rest-api-specs/codeowners/master/specification/hdinsight/resource-manager/Microsoft.HDInsight/stable/2018-06-01-preview/virtualMachines.json).
 
## <a name="deprecation"></a>Desuso
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>Desuso de Spark 2.1 y 2.2 para el clúster de Spark de HDInsight 3.6
A partir del 1 de julio de 2020, los clientes no podrán crear nuevos clústeres de Spark con Spark 2.1 y 2.2 en HDInsight 3.6. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere la posibilidad de pasar a Spark 2.3 en HDInight 3.6 a partir del 30 de junio de 2020 para evitar la posible interrupción del sistema o del soporte técnico.
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>Desuso de Spark 2.3 y 4.0 para el clúster de Spark de HDInsight 4.0
A partir del 1 de julio de 2020, los clientes no podrán crear nuevos clústeres de Spark con Spark 2.3 en HDInsight 4.0. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere la posibilidad de pasar a Spark 2.4 en HDInight 4.0 a partir del 30 de junio de 2020 para evitar la posible interrupción del sistema o del soporte técnico.
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>Desuso de Kafka 1.1 en el clúster de Kafka de HDInsight 4.0
A partir del 1 de julio de 2020, los clientes no podrán crear nuevos clústeres de Kafka con Kafka 1.1 en HDInsight 4.0. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere la posibilidad de pasar a Kafka 2.1 en HDInight 4.0 a partir del 30 de junio de 2020 para evitar la posible interrupción del sistema o del soporte técnico.
 
## <a name="behavior-changes"></a>Cambios de comportamiento
### <a name="esp-spark-cluster-head-node-size-change"></a>Cambio de tamaño del nodo principal del clúster de Spark de ESP 
El tamaño de nodo principal mínimo permitido para un clúster de Spark de ESP cambia a Standard_D13_V2. Las máquinas virtuales que tienen núcleos y memoria insuficientes como nodo principal pueden provocar problemas en el clúster de ESP debido a su capacidad relativamente baja de CPU y memoria. A partir de esta versión, use las SKU posteriores a Standard_D13_V2 y Standard_E16_V3 como nodo principal para los clústeres de Spark de ESP.
 
### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Se requiere una máquina virtual de 4 núcleos como mínimo para el nodo principal 
Se requiere una máquina virtual de 4 núcleos como mínimo para que el nodo principal garantice la alta disponibilidad y confiabilidad de los clústeres de HDInsight. A partir del 6 de abril de 2020, los clientes solo pueden elegir una máquina virtual de cuatro núcleos o más como nodo principal para los nuevos clústeres de HDInsight. Los clústeres existentes seguirán ejecutándose según lo previsto. 
 
### <a name="cluster-worker-node-provisioning-change"></a>Cambio de aprovisionamiento del nodo de trabajo del clúster
Cuando el 80 % de los nodos de trabajo están listos, el clúster entra en la fase **operativa**. En esta fase, los clientes pueden realizar todas las operaciones del plano de datos, como ejecutar scripts y trabajos. No obstante, no pueden realizar ninguna operación de plano de control, como el escalado o la reducción vertical. Solo se admite la eliminación.
 
Después de la fase **operativa**, el clúster espera otros 60 minutos por el 20 % restante de los nodos de trabajo. Al final de esos 60 minutos, el clúster pasa a la fase de **ejecución**, incluso aunque todos los nodos de trabajo sigan sin estar disponibles. Una vez que un clúster entra en la fase de **ejecución**, se puede usar de la forma habitual. Se aceptan las operaciones del plan de control, como el escalado o la reducción vertical, y las operaciones del plan de datos, como la ejecución de scripts y trabajos. Si algunos de los nodos de trabajo solicitados no están disponibles, el clúster se marcará como correcto parcialmente. Se le cobrará por los nodos que se hayan implementado correctamente. 
 
### <a name="create-new-service-principal-through-hdinsight"></a>Creación de una entidad de servicio nueva a través de HDInsight
Anteriormente, con la creación de un clúster, los clientes podían crear una nueva entidad de servicio para acceder a la cuenta de ADLS Gen 1 conectada en Azure Portal. A partir del 15 de junio de 2020, los clientes no pueden crear entidades de servicio en el flujo de trabajo de creación de HDInsight; tan solo se admite la entidad de servicio existente. Consulte [Creación de una entidad de servicio y certificados mediante Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

### <a name="time-out-for-script-actions-with-cluster-creation"></a>Tiempo de espera para acciones de script con creación de clústeres
HDInsight admite la ejecución de acciones de script con la creación de clústeres. A partir de esta versión, todas las acciones de script con creación de clústeres deben finalizar en **60 minutos**, o agotarán el tiempo de espera. Las acciones de script enviadas a los clústeres que se están ejecutando no se ven afectadas. Consulte más detalles [aquí](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#script-action-in-the-cluster-creation-process).
 
## <a name="upcoming-changes"></a>Próximos cambios
No habrá cambios importantes que requieran su atención.
 
## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 
 
## <a name="component-version-change"></a>Cambio de versión de componentes
### <a name="hbase-20-to-216"></a>HBase 2.0 a 2.1.6
La versión de HBase se actualiza de la versión 2.0 a 2.1.6.
 
### <a name="spark-240-to-244"></a>Spark 2.4.0 a 2.4.4
La versión de Spark se actualiza de la versión 2.4.0 a 2.4.4.
 
### <a name="kafka-210-to-211"></a>Kafka 2.1.0 a 2.1.1
La versión de Kafka se actualiza de la versión 2.1.0 a 2.1.1.
 
En [este documento](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions) puede encontrar las versiones actuales de los componentes para HDInsight 4.0 y HDInsight 3.6.

## <a name="known-issues"></a>Problemas conocidos

### <a name="hive-warehouse-connector-issue"></a>Problema con Hive Warehouse Connector
En esta versión hay un problema con Hive Warehouse Connector que se corregirá en la próxima versión. El problema no afecta a los clústeres creados antes de esta versión. Si es posible, evite quitar y volver a crear el clúster. Si necesita más ayuda al respecto, abra una incidencia de soporte técnico.
