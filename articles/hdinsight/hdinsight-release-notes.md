---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/07/2020
ms.openlocfilehash: 0c14e5537385b7518fd08d9d3599993bc6d82f88
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92535508"
---
# <a name="azure-hdinsight-release-notes"></a>Notas de la versión de Azure HDInsight

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto en Azure.

## <a name="release-date-10082020"></a>Fecha de lanzamiento: 08/10/2020

Esta versión se aplica a HDInsight 3.6 y HDInsight 4.0. La versión de HDInsight se pone a disposición de todas las regiones durante varios días. Esta fecha de lanzamiento indica la fecha de lanzamiento de la primera región. Si no ve los cambios siguientes, espere unos días a que la versión se active en su región.

## <a name="new-features"></a>Nuevas características
### <a name="hdinsight-private-clusters-with-no-public-ip-and-private-link-preview"></a>Clústeres privados de HDInsight sin dirección IP pública y vínculo privado (versión preliminar)
HDInsight ahora admite la creación de clústeres sin una dirección IP pública y acceso de vínculo privado a los clústeres en la versión preliminar. Los clientes pueden usar la nueva configuración de red avanzada para crear un clúster totalmente aislado sin ninguna dirección IP pública y usar sus propios puntos de conexión privados para tener acceso al clúster. 

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Movimiento a conjuntos de escalado de máquinas virtuales
Ahora HDInsight usa máquinas virtuales de Azure para aprovisionar el clúster. A partir de esta versión, el servicio se migrará gradualmente a [conjuntos de escalado de máquinas virtuales de Azure](../virtual-machine-scale-sets/overview.md). Todo el proceso puede tardar meses. Después de migrar las regiones y las suscripciones, los clústeres de HDInsight recién creados se ejecutarán en conjuntos de escalado de máquinas virtuales sin acciones del cliente. No se espera ningún cambio importante.

## <a name="deprecation"></a>Desuso
#### <a name="deprecation-of-hdinsight-36-ml-services-cluster"></a>Desuso del clúster de Machine Learning de HDInsight 3.6
El soporte técnico del tipo de clúster de servicios de Machine Learning de HDInsight 3.6 finalizará el 31 de diciembre de 2020. Los clientes no crearán nuevos clústeres de servicios de Machine Learning 3.6 a partir de entonces. Los clústeres existentes se ejecutarán tal cual sin la compatibilidad de Microsoft. Compruebe la expiración del soporte técnico de las versiones y los tipos de clúster de HDInsight [aquí](./hdinsight-component-versioning.md#available-versions).

## <a name="behavior-changes"></a>Cambios de comportamiento
No hay cambios de comportamiento en esta versión.

## <a name="upcoming-changes"></a>Próximos cambios
En las próximas versiones, se realizarán los siguientes cambios.

### <a name="ability-to-select-different-zookeeper-virtual-machine-sizes-for-spark-hadoop-and-ml-services"></a>Posibilidad de seleccionar diferentes tamaños de máquina virtual de Zookeeper para Spark, Hadoop y Machine Learning Services
Actualmente, HDInsight no admite la personalización del tamaño de nodo de Zookeeper para los tipos de clúster de Spark, Hadoop y Machine Learning Services. De manera predeterminada, serán tamaños de máquina virtual A2_v2 o A2, que se proporcionan de forma gratuita. En la próxima versión, puede seleccionar el tamaño de máquina virtual de Zookeeper que sea más adecuado para su escenario. Se cobrarán los nodos de Zookeeper con un tamaño de máquina virtual que no sea A2_v2 o A2. Las máquinas virtuales A2_v2 y A2 todavía se proporcionan de forma gratuita.

## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 

## <a name="component-version-change"></a>Cambio de versión de componentes
No hay cambio de versión de componentes para esta versión. En [este documento](./hdinsight-component-versioning.md) puede encontrar las versiones actuales de los componentes para HDInsight 4.0 y HDInsight 3.6.
