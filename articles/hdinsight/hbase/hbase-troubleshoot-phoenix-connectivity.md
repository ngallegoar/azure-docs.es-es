---
title: Problemas de conectividad de Apache Phoenix en Azure HDInsight
description: Problemas de conectividad entre Apache HBase y Apache Phoenix en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/14/2019
ms.openlocfilehash: b886f51bcb2bb7308c49c76563dcb70148bbc583
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887298"
---
# <a name="scenario-apache-phoenix-connectivity-issues-in-azure-hdinsight"></a>Escenario: Problemas de conectividad de Apache Phoenix en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="issue"></a>Problema

No se puede conectar a Apache HBase con Apache Phoenix. Los motivos pueden variar.

## <a name="cause-incorrect-ip"></a>Causa: IP incorrecta

Dirección IP incorrecta del nodo activo de Zookeeper.

### <a name="resolution"></a>Solución

La dirección IP del nodo activo de Zookeeper se puede identificar desde la interfaz de usuario de Ambari mediante el seguimiento de los vínculos a **HBase** > **Quick Links** > **ZK (Active)**  > **Zookeeper Info** (HBAse > Vínculos rápidos > ZK (activo) > Información de Zookeeper). Corrija la dirección IP según sea necesario.

---

## <a name="cause-systemcatalog-table-offline"></a>Causa: Desconexión de la tabla SYSTEM.CATALOG

Al ejecutar comandos, como `!tables`, recibe un mensaje de error parecido a este:

```output
Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings.
```

Al ejecutar comandos, como `count 'SYSTEM.CATALOG'`, recibe un mensaje de error parecido a este:

```output
ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189)
```

### <a name="resolution"></a>Solución

En la interfaz de usuario de Apache Ambari, realice los siguientes pasos para reiniciar el servicio HMaster en todos los nodos de ZooKeeper:

1. En la sección **Summary** (Resumen) de HBase, vaya a **HBase** > **Active HBase Master**.

1. En la sección **Components** (Componentes), reinicie el servicio HBase Master.

1. Repita estos para todos los servicios **Standby HBase Master** restantes.

El servicio HBase Master puede tener un máximo de cinco minutos en estabilizar y finalizar la recuperación. Después de que la tabla `SYSTEM.CATALOG` vuelva al estado normal, el problema de conectividad a Apache Phoenix debería resolverse automáticamente.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
