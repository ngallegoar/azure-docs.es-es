---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/12/2020
ms.openlocfilehash: d4983ecd1b8afe1ec6bd3cc31df8b711cebaddf1
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96350269"
---
# <a name="azure-hdinsight-release-notes"></a>Notas de la versión de Azure HDInsight

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto en Azure.

Si quiere suscribirse a las notas de la versión, vea las versiones de [este repositorio de GitHub](https://github.com/hdinsight/release-notes/releases).

## <a name="release-date-11182020"></a>Fecha de lanzamiento: 18/11/2020

Esta versión se aplica a HDInsight 3.6 y HDInsight 4.0. La versión de HDInsight se pone a disposición de todas las regiones durante varios días. Esta fecha de lanzamiento indica la fecha de lanzamiento de la primera región. Si no ve los cambios siguientes, espere unos días a que la versión se active en su región.

## <a name="new-features"></a>Nuevas características
### <a name="auto-key-rotation-for-customer-managed-key-encryption-at-rest"></a>Rotación de claves automática para el cifrado de claves administradas por el cliente en reposo
A partir de esta versión, los clientes pueden usar las direcciones URL de clave de cifrado sin versión de Azure Key Vault para el cifrado de clave administrado por el cliente en reposo. HDInsight rotará automáticamente las claves a medida que expiren o se reemplacen por versiones nuevas. Consulte más detalles [aquí](./disk-encryption.md).

### <a name="ability-to-select-different-zookeeper-virtual-machine-sizes-for-spark-hadoop-and-ml-services"></a>Posibilidad de seleccionar diferentes tamaños de máquina virtual de Zookeeper para Spark, Hadoop y Machine Learning Services
Anteriormente, HDInsight no admitía la personalización del tamaño de nodo de Zookeeper para los tipos de clúster de Spark, Hadoop y Machine Learning Services. De manera predeterminada, serán tamaños de máquina virtual A2_v2 o A2, que se proporcionan de forma gratuita. A partir de esta versión, puede seleccionar el tamaño de máquina virtual de Zookeeper que sea más adecuado para su escenario. Se cobrarán los nodos de Zookeeper con un tamaño de máquina virtual que no sea A2_v2 o A2. Las máquinas virtuales A2_v2 y A2 todavía se proporcionan de forma gratuita.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Movimiento a conjuntos de escalado de máquinas virtuales
Ahora HDInsight usa máquinas virtuales de Azure para aprovisionar el clúster. A partir de esta versión, el servicio se migrará gradualmente a [conjuntos de escalado de máquinas virtuales de Azure](../virtual-machine-scale-sets/overview.md). Todo el proceso puede tardar meses. Después de migrar las regiones y las suscripciones, los clústeres de HDInsight recién creados se ejecutarán en conjuntos de escalado de máquinas virtuales sin acciones del cliente. No se espera ningún cambio importante.

## <a name="deprecation"></a>Desuso
### <a name="deprecation-of-hdinsight-36-ml-services-cluster"></a>Desuso del clúster de Machine Learning de HDInsight 3.6
El soporte técnico del tipo de clúster de servicios de Machine Learning de HDInsight 3.6 finalizará el 31 de diciembre de 2020. A partir de entonces, los clientes no podrán crear clústeres de servicios de Machine Learning de HDInsight 3.6. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Compruebe la expiración del soporte técnico de las versiones y los tipos de clúster de HDInsight [aquí](./hdinsight-component-versioning.md#available-versions).

### <a name="disabled-vm-sizes"></a>Tamaños de máquina virtual deshabilitados
A partir del 16 de noviembre de 2020, HDInsight impide que los nuevos clientes creen clústeres con los tamaños de máquina virtual standand_A8, standand_A9, standand_A10 y standand_A11. Esta medida no afectará a los clientes existentes que hayan usado estos tamaños de máquina virtual en los últimos tres meses. A partir del 9 de enero de 2021, HDInsight impedirá que todos los clientes creen clústeres con los tamaños de máquina virtual standand_A8, standand_A9, standand_A10 y standand_A11. Los clústeres existentes se ejecutarán tal cual. Considere pasar a HDInsight 4.0 para evitar la posible interrupción del sistema o del soporte técnico.

### <a name="behavior-changes"></a>Cambios de comportamiento
No hay cambios de comportamiento en esta versión.

## <a name="upcoming-changes"></a>Próximos cambios
En las próximas versiones, se realizarán los siguientes cambios.

### <a name="default-cluster-version-will-be-changed-to-40"></a>La versión predeterminada del clúster cambiará a la 4.0
A partir de febrero de 2021, la versión predeterminada del clúster de HDInsight cambiará de la 3.6 a la 4.0. Para más información sobre las versiones disponibles, consulte [este artículo](./hdinsight-component-versioning.md#available-versions). Más información sobre las novedades de [HDInsight 4.0](./hdinsight-version-release.md)

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>Fin del soporte técnico de HDInsight 3.6 el 30 de junio de 2021
El soporte técnico para HDInsight 3.6 finaliza. A partir del 30 de junio de 2021, los clientes no pueden crear clústeres de HDInsight 3.6. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Considere pasar a HDInsight 4.0 para evitar la posible interrupción del sistema o del soporte técnico.

## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 

## <a name="component-version-change"></a>Cambio de versión de componentes
No hay cambio de versión de componentes para esta versión. En [este documento](./hdinsight-component-versioning.md) puede encontrar las versiones actuales de los componentes para HDInsight 4.0 y HDInsight 3.6.