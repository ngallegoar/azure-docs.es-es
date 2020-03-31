---
title: Apache HBase Master no se inicia en Azure HDInsight
description: Apache HBase Master (HMaster) no se inicia en Azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/14/2019
ms.openlocfilehash: 290b541d9b5e86616373d2e426241fca07e780ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887213"
---
# <a name="apache-hbase-master-hmaster-fails-to-start-in-azure-hdinsight"></a>Apache HBase Master (HMaster) no se inicia en Azure HDInsight

En este artículo se describen los pasos de solución de problemas y las posibles soluciones para los problemas que se producen al usar clústeres de Azure HDInsight.

## <a name="scenario-atomic-renaming-failure"></a>Escenario: Error de cambio de nombre atómico

### <a name="issue"></a>Problema

Archivos inesperados identificados durante el proceso de inicio.

### <a name="cause"></a>Causa

Durante el proceso de inicio, HMaster realiza muchos pasos de inicialización, incluido el movimiento de datos desde la carpeta temporal (.tmp) a la carpeta de datos. HMaster también examina la carpeta de registros de escritura previa (WAL) para ver si hay servidores de la región que no responden.

HMaster realiza un comando de lista básico en las carpetas de WAL. Si en algún momento HMaster ve un archivo inesperado en cualquiera de estas carpetas, genera una excepción y no se inicia.

### <a name="resolution"></a>Solución

Compruebe la pila de llamadas e intente determinar qué carpeta puede ser la causante del problema (por ejemplo, puede ser la carpeta de WAL o la carpeta .tmp). Después, en Cloud Explorer o mediante los comandos de HDFS, pruebe a buscar el archivo problemático. Normalmente, se trata de un archivo `*-renamePending.json`. (`*-renamePending.json` es un archivo de diario que se utiliza para implementar la operación de cambio de nombre atómico en el controlador WASB. Debido a los errores de esta implementación, estos archivos se pueden dejar después de que el proceso se bloquea, y así sucesivamente). Fuerce la eliminación de este archivo en Cloud Explorer o mediante los comandos de HDFS.

En ocasiones, en esta ubicación también puede haber un archivo temporal con un nombre similar a `$$$.$$$`. Para ver este archivo, tiene que usar el comando `ls` de HDFS; en Cloud Explorer no se puede ver. Para eliminar este archivo, use el comando `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$` de HDFS.

Una vez que haya ejecutado estos comandos, HMaster debería iniciarse de inmediato.

---

## <a name="scenario-no-server-address-listed"></a>Escenario: No se muestra ninguna dirección de servidor

### <a name="issue"></a>Problema

Es posible que vea un mensaje que indica que la tabla `hbase: meta` no está en línea. La ejecución de `hbck` puede notificar que `hbase: meta table replicaId 0 is not found on any region.`. En los registros de HMaster, es posible que vea el mensaje: `No server address listed in hbase: meta for region hbase: backup <region name>`.  

### <a name="cause"></a>Causa

No se pudo inicializar HMaster después de reiniciar HBase.

### <a name="resolution"></a>Solución

1. En el shell de HBase, especifique los siguientes comandos (cambie los valores reales según corresponda):

    ```hbase
    scan 'hbase:meta'
    delete 'hbase:meta','hbase:backup <region name>','<column name>'
    ```

1. Elimine la entrada `hbase: namespace`. Esta entrada puede ser el mismo error que se notifica cuando se examina la tabla `hbase: namespace`.

1. Reinicie el HMaster activo desde la interfaz de usuario de Ambari para que aparezca en estado de ejecución.

1. En el shell de HBase, para activar todas las tablas sin conexión, ejecute el siguiente comando:

    ```hbase
    hbase hbck -ignorePreCheckPermission -fixAssignments
    ```

---

## <a name="scenario-javaioioexception-timedout"></a>Escenario: java.io.IOException: Timedout

### <a name="issue"></a>Problema

HMaster agota el tiempo de espera con una excepción grave, similar a `java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned`.

### <a name="cause"></a>Causa

Este problema puede aparecer si hay muchas tablas y regiones que no se han vaciado al reiniciar los servicios de HMaster. El tiempo de espera es un defecto conocido de HMaster. Las tareas de inicio de los clústeres generales pueden tardar mucho tiempo. HMaster se cierra si la tabla de espacios de nombres todavía no está asignada. Las tareas de inicio largas tienen lugar cuando existe una gran cantidad de datos sin vaciar y no basta con un tiempo de espera de cinco minutos.

### <a name="resolution"></a>Solución

1. En la interfaz de usuario de Apache Ambari, vaya a **HBase** > **Configs** (Configuraciones). En el archivo `hbase-site.xml` personalizado, agregue la siguiente configuración:

    ```
    Key: hbase.master.namespace.init.timeout Value: 2400000  
    ```

1. Reinicie los servicios requeridos (HMaster y posiblemente otros servicios de HBase).

---

## <a name="scenario-frequent-region-server-restarts"></a>Escenario: Reinicios frecuentes del servidor de regiones

### <a name="issue"></a>Problema

Los nodos se reinician periódicamente. En los registros del servidor de regiones puede ver entradas parecidas a estas:

```
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
2017-05-09 17:45:07,683 WARN  [JvmPauseMonitor] util.JvmPauseMonitor: Detected pause in JVM or host machine (eg GC): pause of approximately 31000ms
```

### <a name="cause"></a>Causa

Pausa larga de la recolección de elementos no utilizados de Máquina virtual Java en `regionserver`. La pausa hará que `regionserver` deje de responder y no pueda enviar latidos a HMaster dentro del tiempo de espera de sesión de Zookeeper de 40 s. HMaster creerá que `regionserver` está inactivo, anulará `regionserver` y reiniciará.

### <a name="resolution"></a>Solución

Cambie el tiempo de espera de sesión de Zookeeper; no solo es necesario cambiar el valor `zookeeper.session.timeout` de `hbase-site`, sino también el valor `maxSessionTimeout` de `zoo.cfg` de Zookeeper.

1. Acceda a la interfaz de usuario de Ambari, vaya a **HBase -> Configs -> Settings** (HBase -> Configuraciones -> Configuración) y, en la sección, Timeouts (Tiempos de espera), cambie el valor del tiempo de espera de la sesión de Zookeeper.

1. Acceda a la interfaz de usuario de Ambari UI, vaya a **Zookeeper -> Configs -> Custom** (Zookeeper -> Configuraciones -> Personalizar) `zoo.cfg` y agregue o cambie el siguiente valor. Asegúrese de que el valor sea el mismo que `zookeeper.session.timeout` de HBase.

    ```
    Key: maxSessionTimeout Value: 120000  
    ```

1. Reinicie los servicios necesarios.

---

## <a name="scenario-log-splitting-failure"></a>Escenario: Error de división de registro

### <a name="issue"></a>Problema

HMasters no se pudo mostrar en un clúster de HBase.

### <a name="cause"></a>Causa

Error de configuración de HDFS y HBase en una cuenta de almacenamiento secundaria.

### <a name="resolution"></a>Solución

Establezca hbase.rootdir: wasb://@.blob.core.windows.net/hbase y reinicie los servicios de Ambari.

---

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

* Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
