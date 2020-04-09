---
title: Notas de la versión de Azure HDInsight.
description: Notas más recientes de la versión de Azure HDInsight. Obtenga sugerencias de desarrollo y detalles sobre Hadoop, Spark, R Server, Hive, etc.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/20/2020
ms.openlocfilehash: e5a96d2eb67937ce4eeaa1999d8168e7f5d3d926
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80130184"
---
# <a name="release-notes"></a>Notas de la versión

Este artículo proporciona información sobre las **últimas** actualizaciones de versión de Azure HDInsight. Para más información sobre versiones anteriores, vea [Notas de la versión (en archivo) de los componentes de Hadoop en Azure HDInsight](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Resumen

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto en Azure.

## <a name="release-date-01092020"></a>Fecha de lanzamiento: 09/01/2020

Esta versión se aplica a HDInsight 3.6 y 4.0. La versión de HDInsight se pone a disposición de todas las regiones durante varios días. Esta fecha de lanzamiento indica la fecha de lanzamiento de la primera región. Si no ve los cambios siguientes, espere unos días a que la versión se active en su región.

> [!IMPORTANT]  
> Linux es el único sistema operativo que se usa en la versión 3.4 de HDInsight, o en las superiores. Para obtener más información, consulte el [artículo de control de versiones de HDInsight](hdinsight-component-versioning.md).

## <a name="new-features"></a>Nuevas características
### <a name="tls-12-enforcement"></a>Cumplimiento de TLS 1.2
Seguridad de la capa de transporte (TLS) y Capa de sockets seguros (SSL) son protocolos criptográficos que proporcionan la seguridad de las comunicaciones a través de una red de equipos. Más información sobre [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0). HDInsight usa TLS 1.2 en los puntos de conexión HTTPs públicos, pero todavía se admite TLS 1.1 por compatibilidad con versiones anteriores. 

Con esta versión, los clientes pueden optar por recibir solo TLS 1.2 para todas las conexiones a través del punto de conexión de clúster público. Para admitir esto, se introduce la nueva propiedad **minSupportedTlsVersion** que puede especificarse durante la creación del clúster. Si no se establece la propiedad, el clúster sigue siendo compatible con TLS 1.0, 1.1 y 1.2, que es el mismo comportamiento de hoy en día. Los clientes pueden establecer el valor de esta propiedad en "1.2", lo que significa que el clúster solo admite TLS 1.2 y versiones posteriores. Para más información, consulte [Planeamiento de una red virtual: Seguridad de la capa de transporte](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#transport-layer-security).

### <a name="bring-your-own-key-for-disk-encryption"></a>Traiga su propia clave para el cifrado de discos
Todos los discos administrados en HDInsight están protegidos con Azure Storage Service Encryption (SSE). Los datos en esos discos se cifran de forma predeterminada mediante claves administradas por Microsoft. A partir de esta versión, puede usar Bring Your Own Key (BYOK) para el cifrado de discos y administrarlo con Azure Key Vault. El cifrado de BYOK es una configuración en un paso durante la creación del clúster sin ningún costo adicional. Solo ha de registrar HDInsight como identidad administrada con Azure Key Vault y agregar la clave de cifrado al crear el clúster. Para obtener más información, consulte [Cifrado de disco mediante claves administradas por el cliente](https://docs.microsoft.com/azure/hdinsight/disk-encryption).

## <a name="deprecation"></a>Desuso
No hay elementos en desuso en esta versión. Para prepararse para las próximas entradas en desuso, consulte [Próximos cambios](#upcoming-changes).

## <a name="behavior-changes"></a>Cambios de comportamiento
No hay cambios de comportamiento en esta versión. Para prepararse para los próximos cambios, consulte [Próximos cambios](#upcoming-changes).

## <a name="upcoming-changes"></a>Próximos cambios
En las próximas versiones, se realizarán los siguientes cambios. 

### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Se requiere una máquina virtual de 4 núcleos como mínimo para el nodo principal 
Se requiere una máquina virtual de 4 núcleos como mínimo para que el nodo principal garantice la alta disponibilidad y confiabilidad de los clústeres de HDInsight. A partir del 6 de abril de 2020, los clientes solo pueden elegir una máquina virtual de 4 núcleos o más como nodo principal para los nuevos clústeres de HDInsight. Los clústeres existentes seguirán ejecutándose según lo previsto. 

### <a name="esp-spark-cluster-node-size-change"></a>Cambio de tamaño del nodo del clúster de Spark de ESP 
En la próxima versión, el tamaño de nodo mínimo permitido para un clúster de Spark de ESP se cambiará a Standard_D13_V2. Las máquinas virtuales de la serie A pueden provocar problemas en el clúster de ESP debido a su capacidad de CPU y memoria relativamente baja. Las máquinas virtuales de la serie A quedarán en desuso para crear nuevos clústeres de ESP.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Movimiento a conjuntos de escalado de máquinas virtuales
Ahora HDInsight usa máquinas virtuales de Azure para aprovisionar el clúster. En la próxima versión, HDInsight usará los conjuntos de escalado de máquinas virtuales de Azure en su lugar. Consulte más información sobre los conjuntos de escalado de máquinas virtuales de Azure.

### <a name="hbase-20-to-21"></a>HBase 2.0 a 2.1
En la próxima versión de HDInsight 4.0, la versión de HBase se actualizará de 2.0 a 2.1.

## <a name="bug-fixes"></a>Corrección de errores
HDInsight continúa realizando mejoras en la confiabilidad y el rendimiento del clúster. 

## <a name="component-version-change"></a>Cambio de versión de componentes
No hay cambio de versión de componentes para esta versión. Aquí puede encontrar las versiones actuales de los componentes para HDInsight 4.0 y HDInsight 3.6.

## <a name="known-issues"></a>Problemas conocidos

Desde el 18 de marzo de 2020, algunos clientes de Azure HDInsight del Oeste o el Norte de Europa han recibido notificaciones de error al crear o escalar clústeres de HDInsight en estas regiones. Los errores relacionados con este problema incluyen:

- Se produjo un error interno al procesar la solicitud. Vuelva a intentar la solicitud o póngase en contacto con el soporte técnico.
- Se ha producido un error al menos en una operación de implementación de recursos. Enumere las operaciones de implementación para obtener más información. Consulte https://aka.ms/DeployOperations para obtener detalles de uso.
- Al \<identificador de suscripción\> SubscriptionId no le quedan núcleos para crear el recurso "\<cluster name>". Requerido: \<X\>, disponible: 0.

Los ingenieros son conscientes de este problema y lo están investigando activamente.

Para obtener más ayuda, cree una [solicitud de soporte técnico](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
