---
title: El servidor Apache ZooKeeper no puede formar un cuórum en Azure HDInsight
description: El servidor Apache ZooKeeper no puede formar un cuórum en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 05/20/2020
ms.openlocfilehash: dc93121d7565b95b9bd604160028659f3a741b0c
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860501"
---
# <a name="apache-zookeeper-server-fails-to-form-a-quorum-in-azure-hdinsight"></a>El servidor Apache ZooKeeper no puede formar un cuórum en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas relacionados con Zookeepers en clústeres de Azure HDInsight.

## <a name="symptoms"></a>Síntomas

* Ambos administradores de recursos van a modo de espera
* Ambos nodos de nombre están en modo de espera
* Los trabajos de Spark, Hive y Yarn o las consultas de Hive producen errores debido a errores de conexión de Zookeeper
* Los demonios de LLAP tampoco se pueden iniciar en los clústeres de Spark o Hive interactivo seguros

## <a name="sample-log"></a>Registro de muestra

Es posible que vea un mensaje de error similar al siguiente:

```output
2020-05-05 03:17:18.3916720|Lost contact with Zookeeper. Transitioning to standby in 10000 ms if connection is not reestablished.
Message
2020-05-05 03:17:07.7924490|Received RMFatalEvent of type STATE_STORE_FENCED, caused by org.apache.zookeeper.KeeperException$NoAuthException: KeeperErrorCode = NoAuth
...
2020-05-05 03:17:08.3890350|State store operation failed 
2020-05-05 03:17:08.3890350|Transitioning to standby state
```

## <a name="related-issues"></a>Problemas relacionados

* Los servicios de alta disponibilidad, como Yarn, NameNode y Livy, pueden dejar de funcionar por muchas razones.
* Confirmar de los registros que está relacionado con las conexiones de Zookeeper
* Asegúrese de que el problema se produce repetidamente (no use estas soluciones para los casos no utilizados)
* Se puede producir un error temporal en los trabajos debido a problemas de conexión de Zookeeper

## <a name="common-causes-for-zookeeper-failure"></a>Causas comunes del error de Zookeeper

* Uso intensivo de la CPU en los servidores de Zookeeper
  * En la interfaz de usuario de Ambari, si observa que el uso de CPU sostenido es cercano al 100% en los servidores de Zookeeper, las sesiones de Zookeeper abiertas durante ese tiempo pueden expirar y agotar el tiempo de espera
* Los clientes de Zookeeper informan de tiempos de espera frecuentes
  * En los registros de Resource Manager, NameNode y otros, verá tiempos de espera de conexión de cliente frecuentes
  * Esto podría provocar la pérdida de cuórum, las conmutaciones por error frecuentes y otros problemas

## <a name="check-for-zookeeper-status"></a>Compruebe el estado de Zookeeper

* Busque los servidores de Zookeeper desde el archivo/etc/hosts o desde la interfaz de usuario de Ambari
* Ejecute el siguiente comando.
  * `echo stat | nc <ZOOKEEPER_HOST_IP> 2181` (o 2182)  
  * El puerto 2181 es la instancia de Apache Zookeeper
  * El puerto 2182 se usa en HDInsight Zookeeper (para proporcionar una alta disponibilidad para los servicios que no lo son de forma nativa)
  * Si el comando no muestra ninguna salida, significa que los servidores Zookeeper no se están ejecutando
  * Si los servidores están en ejecución, el resultado incluirá las estáticas de las conexiones de cliente y otras estadísticas

```output
Zookeeper version: 3.4.6-8--1, built on 12/05/2019 12:55 GMT
Clients:
 /10.2.0.57:50988[1](queued=0,recved=715,sent=715)
 /10.2.0.57:46632[1](queued=0,recved=138340,sent=138347)
 /10.2.0.34:14688[1](queued=0,recved=264653,sent=353420)
 /10.2.0.52:49680[1](queued=0,recved=134812,sent=134814)
 /10.2.0.57:50614[1](queued=0,recved=19812,sent=19812)
 /10.2.0.56:35034[1](queued=0,recved=2586,sent=2586)
 /10.2.0.52:63982[1](queued=0,recved=72215,sent=72217)
 /10.2.0.57:53024[1](queued=0,recved=19805,sent=19805)
 /10.2.0.57:45126[1](queued=0,recved=19621,sent=19621)
 /10.2.0.56:41270[1](queued=0,recved=1348743,sent=1348788)
 /10.2.0.53:59097[1](queued=0,recved=72215,sent=72217)
 /10.2.0.56:41088[1](queued=0,recved=788,sent=802)
 /10.2.0.34:10246[1](queued=0,recved=19575,sent=19575)
 /10.2.0.56:40944[1](queued=0,recved=717,sent=717)
 /10.2.0.57:45466[1](queued=0,recved=19861,sent=19861)
 /10.2.0.57:59634[0](queued=0,recved=1,sent=0)
 /10.2.0.34:14704[1](queued=0,recved=264622,sent=353355)
 /10.2.0.57:42244[1](queued=0,recved=49245,sent=49248)

Latency min/avg/max: 0/3/14865
Received: 238606078
Sent: 239139381
Connections: 18
Outstanding: 0
Zxid: 0x1004f99be
Mode: follower
Node count: 133212
```

## <a name="cpu-load-peaks-up-every-hour"></a>La carga de la CPU alcanza el máximo cada hora

* Inicie sesión en el servidor de Zookeeper y compruebe el /etc/crontab
* Si hay trabajos por hora en ejecución en este momento, haga aleatoria la hora de inicio en los diferentes servidores de Zookeeper.
  
## <a name="purging-old-snapshots"></a>Purga de instantáneas antiguas

* Los Zookeepers se configuran para purgar automáticamente las instantáneas antiguas
* De forma predeterminada, se conservan las últimas 30 instantáneas
* El número de instantáneas que se conservan se controla mediante la clave de configuración `autopurge.snapRetainCount`. Esta propiedad se puede encontrar en los siguientes archivos:
  * `/etc/zookeeper/conf/zoo.cfg` para Zookeeper de Hadoop
  * `/etc/hdinsight-zookeeper/conf/zoo.cfg` para Zookeeper de HDInsight
* Establezca `autopurge.snapRetainCount` en un valor de 3 y reinicie los servidores Zookeeper
  * La configuración de Zookeeper de Hadoop se puede actualizar y el servicio se puede reiniciar a través de Ambari
  * Detención y reinicio de Zookeeper de HDInsight manualmente
    * `sudo lsof -i :2182` le proporcionará el id. de proceso a terminar
    * `sudo python /opt/startup_scripts/startup_hdinsight_zookeeper.py`
* No purgar instantáneas manualmente: la eliminación manual de instantáneas podría provocar la pérdida de datos

## <a name="cancelledkeyexception-in-the-zookeeper-server-log-doesnt-require-snapshot-cleanup"></a>CancelledKeyException en el registro del servidor de Zookeeper no requiere limpieza de instantáneas

* Normalmente, esta excepción significa que el cliente ya no está activo y el servidor no puede enviar un mensaje
* Esta excepción también indica que el cliente de Zookeeper está finalizando las sesiones prematuramente
* Busque los otros síntomas descritos en este documento

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).
- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
- Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).