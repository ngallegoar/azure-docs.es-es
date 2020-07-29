---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 07/14/2020
ms.openlocfilehash: 0481f92666c8ca547dc427e117f9917afb257629
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86511881"
---
# <a name="azure-hdinsight-release-notes"></a>Notas de la versión de Azure HDInsight

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto en Azure.

## <a name="release-date-07132020"></a>Fecha de lanzamiento: 13/07/2020

Esta versión se aplica a HDInsight 3.6 y 4.0. La versión de HDInsight se pone a disposición de todas las regiones durante varios días. Esta fecha de lanzamiento indica la fecha de lanzamiento de la primera región. Si no ve los cambios siguientes, espere unos días a que la versión se active en su región.

## <a name="new-features"></a>Nuevas características
### <a name="support-for-customer-lockbox-for-microsoft-azure"></a>Compatibilidad con Caja de seguridad del cliente para Microsoft Azure
Azure HDInsight ahora es compatible con Caja de seguridad del cliente de Azure. Proporciona una interfaz para que los clientes puedan revisar y aprobar, o rechazar, las solicitudes de acceso a datos de los clientes. Se utiliza cuando un ingeniero de Microsoft necesita acceder a los datos del cliente durante una solicitud de soporte técnico. Para más información, consulte [Caja de seguridad del cliente de Microsoft Azure](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-preview).

### <a name="service-endpoint-policies-for-storage"></a>Directivas de punto de conexión de servicio para el almacenamiento
Los clientes ahora pueden usar directivas de puntos de conexión de servicio (SEP) en la subred del clúster de HDInsight. Más información sobre la [directiva de punto de conexión de servicio de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview).

## <a name="deprecation"></a>Desuso
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>Desuso de Spark 2.1 y 2.2 para el clúster de Spark de HDInsight 3.6
A partir del 1 de julio de 2020, los clientes no podrán crear nuevos clústeres de Spark con Spark 2.1 y 2.2 en HDInsight 3.6. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere la posibilidad de pasar a Spark 2.3 en HDInight 3.6 a partir del 30 de junio de 2020 para evitar la posible interrupción del sistema o del soporte técnico.
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>Desuso de Spark 2.3 y 4.0 para el clúster de Spark de HDInsight 4.0
A partir del 1 de julio de 2020, los clientes no podrán crear nuevos clústeres de Spark con Spark 2.3 en HDInsight 4.0. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere la posibilidad de pasar a Spark 2.4 en HDInight 4.0 a partir del 30 de junio de 2020 para evitar la posible interrupción del sistema o del soporte técnico.
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>Desuso de Kafka 1.1 en el clúster de Kafka de HDInsight 4.0
A partir del 1 de julio de 2020, los clientes no podrán crear nuevos clústeres de Kafka con Kafka 1.1 en HDInsight 4.0. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere la posibilidad de pasar a Kafka 2.1 en HDInight 4.0 a partir del 30 de junio de 2020 para evitar la posible interrupción del sistema o del soporte técnico.

## <a name="behavior-changes"></a>Cambios de comportamiento
No hay ningún comportamiento al que haya que prestar atención.

## <a name="upcoming-changes"></a>Próximos cambios
En las próximas versiones, se realizarán los siguientes cambios. 

### <a name="ability-to-select-different-zookeeper-sku-for-spark-hadoop-and-ml-services"></a>Posibilidad de seleccionar diferentes SKU de Zookeeper para Spark, Hadoop y Machine Learning Services
Actualmente, HDInsight no admite el cambio del SKU de Zookeeper para los tipos de clúster de Spark, Hadoop y Machine Learning Services. Usa el SKU A2_v2/A2 para los nodos de Zookeeper y a los clientes no se les cobra por ellos. En la próxima versión, los clientes podrán cambiar la SKU de Zookeeper SKU para Spark, Hadoop y Machine Learning Services cuando sea necesario. Se cobrarán los nodos Zookeeper con una SKU que no sea A2_v2/A2. La SKU predeterminada seguirá siendo A2_V2/A2 y no tendrá cargo alguno.

## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 
### <a name="fixed-hive-warehouse-connector-issue"></a>Problema de Hive Warehouse Connector corregido
En la versión anterior había un problema con la facilidad de uso de Hive Warehouse Connector, pero ya se ha corregido. 

### <a name="fixed-zeppelin-notebook-truncates-leading-zeros-issue"></a>Se ha reparado el problema de que Zeppelin Notebook trunca los ceros a la izquierda
Zeppelin truncaba de forma incorrecta los ceros a la izquierda en la salida de la tabla en formato de cadena. En esta versión se ha corregido ese problema.

## <a name="component-version-change"></a>Cambio de versión de componentes
No hay cambio de versión de componentes para esta versión. En [este documento](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions) puede encontrar las versiones actuales de los componentes para HDInsight 4.0 y HDInsight 3.6.
