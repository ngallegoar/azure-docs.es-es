---
title: Solución de errores de creación de clústeres con Azure HDInsight
description: Aprenda a solucionar problemas de creación de clústeres de Apache en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: troubleshooting
ms.date: 08/26/2019
ms.openlocfilehash: 1e13c7ef8eae81ef2a12a8761b0596f6329f94dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76937906"
---
# <a name="troubleshoot-cluster-creation-failures-with-azure-hdinsight"></a>Solución de errores de creación de clústeres con Azure HDInsight

A continuación se indican las principales causas más habituales de los errores de creación de clústeres:

- Problemas de permisos
- Restricciones en las directivas de recursos
- Firewalls
- Bloqueos de recursos
- Versiones de componentes no compatibles
- Restricciones en los nombres de cuentas de almacenamiento
- Interrupciones del servicio

## <a name="permissions-issues"></a>Incidencias de permisos

Si usa Azure Data Lake Storage Gen2 y recibe el error ```AmbariClusterCreationFailedErrorCode```, ```Internal server error occurred while processing the request. Please retry the request or contact support.```, abra Azure Portal, vaya a la cuenta de almacenamiento y, en Access Control (IAM), asegúrese de que el rol **colaborador de datos de blobs de almacenamiento** o el rol **propietario de datos de blobs de almacenamiento** tienen asignado el acceso a la **identidad administrada asignada por el usuario** para la suscripción. Consulte [Configuración de permisos para la identidad administrada en la cuenta de Data Lake Storage Gen2](../hdinsight-hadoop-use-data-lake-storage-gen2.md#set-up-permissions-for-the-managed-identity-on-the-data-lake-storage-gen2-account) para obtener instrucciones detalladas.

Si usa Azure Data Lake Storage Gen1, consulte las instrucciones de instalación y configuración [aquí](../hdinsight-hadoop-use-data-lake-store.md). Data Lake Storage Gen1 no es compatible con los clústeres de HBase y no se admite en la versión 4.0 de HDInsight.

Si usa Azure Storage, asegúrese de que el nombre de la cuenta de almacenamiento es válido durante la creación del clúster.

## <a name="resource-policy-restrictions"></a>Restricciones en las directivas de recursos

Las directivas de Azure basadas en suscripciones pueden denegar la creación de direcciones IP públicas. Para crear clústeres de HDInsight se necesitan dos direcciones IP públicas.  

En general, las siguientes directivas pueden afectar a la creación de clústeres:

* Directivas que impiden la creación de equilibradores de carga y direcciones IP en la suscripción.
* Directiva que impide la creación de una cuenta de almacenamiento.
* Directiva que impide la eliminación de recursos de red (direcciones IP o equilibradores de carga).

## <a name="firewalls"></a>Firewalls

Los firewalls de la red virtual o la cuenta de almacenamiento pueden denegar la comunicación con las direcciones IP de administración de HDInsight.

Permita el tráfico procedente de las direcciones IP indicadas en la tabla siguiente.

| Dirección IP de origen | Destination | Dirección |
|---|---|---|
| 168.61.49.99 | *:443 | Entrada |
| 23.99.5.239 | *:443 | Entrada |
| 168.61.48.131 | *:443 | Entrada |
| 138.91.141.162 | *:443 | Entrada |

Agregue también las direcciones IP específicas de la región en la que se crea el clúster. Consulte en [Direcciones IP de administración de HDInsight](../hdinsight-management-ip-addresses.md) la lista de las direcciones de cada región de Azure.

Si usa una instancia de ExpressRoute o su propio servidor DNS personalizado, consulte [Planeamiento de una red virtual para Azure HDInsight: Conectar varias redes](../hdinsight-plan-virtual-network-deployment.md#multinet).

## <a name="resources-locks"></a>Bloqueos de recursos  

Asegúrese de que no hay [bloqueos en la red virtual ni el grupo de recursos](../../azure-resource-manager/management/lock-resources.md). Los clústeres no se pueden crear ni eliminar si el grupo de recursos está bloqueado. 

## <a name="unsupported-component-versions"></a>Versiones de componentes no compatibles

Asegúrese de que está usando una [versión compatible de Azure HDInsight](../hdinsight-component-versioning.md) y los [componentes de Apache Hadoop](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions) en la solución.  

## <a name="storage-account-name-restrictions"></a>Restricciones en los nombres de cuentas de almacenamiento

Los nombres de las cuentas de almacenamiento no pueden superar los 24 caracteres ni contener caracteres especiales. Estas restricciones se aplican también al nombre del contenedor predeterminado de la cuenta de almacenamiento.

También se aplican otras restricciones de nomenclatura para la creación de clústeres. Consulte [Restricciones de nomenclatura de clústeres](../hdinsight-hadoop-provision-linux-clusters.md#cluster-name) para más información.

## <a name="service-outages"></a>Interrupciones del servicio

Compruebe en [Estado de Azure](https://status.azure.com) si hay interrupciones o incidencias del servicio.

## <a name="next-steps"></a>Pasos siguientes

* [Extender Azure HDInsight mediante una instancia de Azure Virtual Network](../hdinsight-plan-virtual-network-deployment.md)
* [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](../hdinsight-hadoop-use-data-lake-storage-gen2.md)  
* [Uso de Azure Storage con clústeres de Azure HDInsight](../hdinsight-hadoop-use-blob-storage.md)
* [Configuración de clústeres en HDInsight con Apache Hadoop, Apache Spark, Apache Kafka, etc.](../hdinsight-hadoop-provision-linux-clusters.md)
