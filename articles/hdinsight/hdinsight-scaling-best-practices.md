---
title: Escalabilidad de los tamaños de clúster en Azure HDInsight
description: Escale un clúster de Apache Hadoop de forma elástica para ajustarlo a la carga de trabajo de Azure HDInsight.
author: ashishthaps
ms.author: ashish
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/26/2020
ms.openlocfilehash: 96a72541255ad0059abe5ad280f1728518dbf68c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234733"
---
# <a name="scale-azure-hdinsight-clusters"></a>Escala de clústeres de Azure HDInsight

HDInsight proporciona elasticidad, lo que ofrece la opción de escalar y reducir verticalmente el número de nodos de trabajo de los clústeres. Esta elasticidad permite reducir un clúster una vez finalizada la jornada laboral o durante los fines de semana, así como expandirlo durante períodos de máxima demanda empresarial.

Si tiene un procesamiento por lotes periódico, el clúster de HDInsight se puede escalar verticalmente unos minutos antes de dicha operación, con el fin de que el clúster tenga suficiente memoria y potencia de CPU.  Después, una vez realizado el procesamiento y de que el uso vuelva a bajar, puede reducir verticalmente el clúster de HDInsight a menos nodos de trabajo.

Los clústeres se pueden escalar manualmente mediante uno de los métodos que se describen a continuación, pero también se pueden usar las opciones de [escalado automático](hdinsight-autoscale-clusters.md) para que el sistema realice automáticamente un escalado vertical y una reducción vertical en respuesta a la CPU, la memoria y otras métricas.

> [!NOTE]  
> Solo son compatibles los clústeres con la versión 3.1.3 de HDInsight, o superior. Si no está seguro de la versión del clúster, puede comprobar la página de propiedades.

## <a name="utilities-to-scale-clusters"></a>Utilidades para escalar clústeres

Microsoft proporciona las siguientes utilidades para escalar clústeres:

|Utilidad | Descripción|
|---|---|
|[PowerShell Az](https://docs.microsoft.com/powershell/azure)|[Set-AzHDInsightClusterSize](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) -ClusterName \<NombreDeClúster> -TargetInstanceCount \<NuevoTamaño>|
|[PowerShell AzureRM](https://docs.microsoft.com/powershell/azure/azurerm) |[Set-AzureRmHDInsightClusterSize](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) -ClusterName \<Cluster Name> -TargetInstanceCount \<NewSize>|
|[CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)| [az hdinsight resize](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) --resource-group \<GrupoDeRecursos> --name \<NombreDeClúster> --workernode-count \<NuevoTamaño>|
|[CLI de Azure clásica](hdinsight-administer-use-command-line.md)|azure hdinsight cluster resize \<NombreDeClúster> \<NúmeroDeInstanciasDeDestino> |
|[Azure Portal](https://portal.azure.com)|Abra el panel del clúster de HDInsight, seleccione **Tamaño de clúster** en el menú izquierdo y, luego, en el panel Tamaño de clúster, escriba el número de nodos de trabajo y seleccione Guardar.|  

![Opción de escalado de clústeres de Azure Portal](./media/hdinsight-scaling-best-practices/azure-portal-settings-nodes.png)

Con cualquiera de estos métodos, puede escalar o reducir verticalmente el clúster de HDInsight en cuestión de minutos.

> [!IMPORTANT]  
> * La CLI clásica de Azure está en desuso y solo debe usarse con el modelo de implementación clásico. Para las demás implementaciones, utilice la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).  
> * El módulo AzureRM de PowerShell está en desuso.  Use el [módulo Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) siempre que sea posible.

## <a name="impact-of-scaling-operations"></a>Impacto de las operaciones de escalado

Cuando se **agregan** nodos al clúster de HDInsight en ejecución (escalar verticalmente), ni los trabajos pendientes ni los que estén en ejecución resultarán afectados. Los trabajos nuevos pueden enviarse de forma segura mientras el proceso de escalado está en ejecución. Si se produce un error en la operación de escalado por cualquier motivo, se controlará para que el clúster sigue funcionando.

Si se **eliminan** nodos (reducir verticalmente), se producirá un error en los trabajos pendientes o en ejecución cuando finalice la operación de escalado. Dicho error se debe a que algunos de los servicios se reinician durante el proceso de escalado. También existe el riesgo de que el clúster se quede atascado en modo seguro durante una operación de escalado manual.

A continuación se muestra cómo el efecto de cambiar el número de nodos de datos varía para cada tipo de clúster compatible con HDInsight:

* Apache Hadoop

    Puede aumentar sin ningún problema la cantidad de nodos de trabajo en un clúster de Hadoop que se encuentre en ejecución, sin que afecte a ningún trabajo pendiente o en ejecución. También se pueden enviar trabajos nuevos mientras la operación está en curso. Los errores que puedan surgir en una operación de escalado se enfrentan oportunamente, por lo que el clúster siempre queda en estado funcional.

    Cuando se realiza la reducción vertical de un clúster de Hadoop al disminuir la cantidad de nodos de datos, se reinician algunos de los servicios del clúster. Este comportamiento hace que todos los trabajos pendientes y en ejecución fallen al completarse la operación de escalado. Sin embargo, puede volver a enviar los trabajos una vez finalizada la operación.

* HBase Apache

    Puede agregar nodos sin problemas al clúster de HBase mientras se encuentra en ejecución, así como eliminarlos. Los servidores regionales se equilibran automáticamente en unos pocos minutos tras completar la operación de escalado. Sin embargo, puede equilibrar manualmente los servidores regionales iniciando sesión en el nodo principal del clúster y ejecutando los comandos siguientes desde una ventana del símbolo del sistema:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

    Para más información sobre el uso del shell de HBase, vea [Introducción a un ejemplo de Apache HBase en HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md).

* Apache Storm

    Puede agregar o quitar sin problemas nodos de datos de su clúster de Storm mientras se encuentra en ejecución. Sin embargo, después de finalizar correctamente la operación de escalado, deberá volver a equilibrar la topología.

    Esto se puede realizar de dos formas:

  * La interfaz de usuario web de Storm
  * La herramienta de la interfaz de línea de comandos (CLI)

    Consulte la [documentación de Apache Storm](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) para obtener más detalles.

    La interfaz de usuario web de Storm se encuentra disponible en el clúster de HDInsight:

    ![Reequilibrio de escalado de HDInsight Storm](./media/hdinsight-scaling-best-practices/hdinsight-portal-scale-cluster-storm-rebalance.png)

    El siguiente es un comando de la CLI de ejemplo para volver a equilibrar la topología de Storm:

    ```console
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

## <a name="how-to-safely-scale-down-a-cluster"></a>Reducción vertical segura de clústeres

### <a name="scale-down-a-cluster-with-running-jobs"></a>Reducción vertical de clústeres con trabajos en ejecución

Para evitar que se produzcan errores en los trabajos en ejecución durante una operación de reducción vertical, puede intentar tres cosas:

1. Espere hasta que finalicen los trabajos antes de realizar la reducción vertical del clúster.
1. Finalice los trabajos manualmente.
1. Vuelva a enviar los trabajos una vez haya concluido la operación de escalado.

Para ver una lista de los trabajos pendientes y en ejecución, puede usar la **interfaz de usuario de Resource Manager** de YARN; para ello, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), seleccione su clúster.  Consulte [Enumeración y visualización de clústeres](./hdinsight-administer-use-portal-linux.md#showClusters) para obtener instrucciones. El clúster se abre en una nueva página del portal.
2. En la vista principal, vaya a **Paneles de clúster** > **Inicio de Ambari**. Escriba las credenciales del clúster.
3. En la interfaz de usuario de Ambari, seleccione **YARN** en la lista de servicios del menú de la izquierda.  
4. En la página de YARN, seleccione **Quick Links** (Vínculos rápidos), mantenga el puntero sobre el nodo principal activo y después seleccione **ResourceManager UI** (UI de ResourceManager).

    ![Interfaz de usuario ResourceManager de vínculos rápidos de Apache Ambari](./media/hdinsight-scaling-best-practices/resource-manager-ui1.png)

Puede acceder directamente a la UI de ResourceManager con `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`.

Verá una lista de trabajos, junto con su estado actual. En la captura de pantalla, hay un trabajo en ejecución:

![Aplicaciones de la UI de ResourceManager](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Para terminar manualmente dicha aplicación en ejecución, ejecute el siguiente comando del shell de SSH:

```bash
yarn application -kill <application_id>
```

Por ejemplo:

```bash
yarn application -kill "application_1499348398273_0003"
```

### <a name="getting-stuck-in-safe-mode"></a>Bloqueo en modo seguro

Al reducir verticalmente un clúster, HDInsight utiliza las interfaces de administración de Apache Ambari para retirar antes los nodos de trabajo adicionales, que replican sus bloques DFS en otros nodos de trabajo en línea. Después, HDInsight reduce el nodo verticalmente de forma segura. HDFS entra en modo seguro durante la operación de escalado y se supone que sale una vez que el escalado ha finalizado. Sin embargo, en algunos casos, el HDFS se atasca en modo seguro durante una operación de escalado debido a la infra-replicación del bloque de archivos.

De forma predeterminada, HDFS se configura con el valor 1 en `dfs.replication`, que controla el número de copias de cada bloque de archivos que hay disponibles. Cada una de las copias de un bloque de archivos se almacena en un nodo diferente del clúster.

Cuando HDFS detecta que el número esperado de copias del bloque no están disponible, HDFS entra en modo seguro y Ambari genera alertas. Si HDFS entra en modo seguro para realizar una operación de escalado, pero luego no puede salir del modo seguro porque no se detecta el número necesario de nodos para la replicación, el clúster puede quedarse bloqueado en modo seguro.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Ejemplos de errores cuando el modo seguro está activado

```output
org.apache.hadoop.hdfs.server.namenode.SafeModeException: Cannot create directory /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. Name node is in safe mode.
```

```output
org.apache.http.conn.HttpHostConnectException: Connect to active-headnode-name.servername.internal.cloudapp.net:10001 [active-headnode-name.servername. internal.cloudapp.net/1.1.1.1] failed: Connection refused
```

Puede revisar los registros del nodo de nombre en la carpeta `/var/log/hadoop/hdfs/`, cerca del momento en que se escaló el clúster, para ver cuándo entró en modo seguro. Los archivos de registro se denominan `Hadoop-hdfs-namenode-<active-headnode-name>.*`.

La causa de los errores anteriores es que Hive depende de los archivos temporales de HDFS durante la ejecución de consultas. Cuando HDFS entra en modo seguro, Hive no puede ejecutar las consultas porque no puede escribir en HDFS. Los archivos temporales de HDFS se encuentran en la unidad local montada en las máquinas virtuales de los nodos de trabajo individuales y se replican entre otros nodos de trabajo en tres réplicas, como mínimo.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode"></a>Evitar que HDInsight se quede bloqueado en modo seguro

Hay varias maneras de evitar que HDInsight se quede en modo seguro:

* Detenga todos los trabajos de Hive antes de reducir verticalmente HDInsight. Como alternativa, programe el proceso de reducción vertical para evitar que entre en conflicto con los trabajos de Hive en ejecución.
* Limpie manualmente los archivos temporales del directorio `tmp` de Hive en HDFS antes de la reducción vertical.
* Solo reduzca verticalmente HDInsight a tres nodos de trabajo como mínimo. Evite reducir a un solo nodo de trabajo.
* Ejecute el comando para salir del modo seguro, si es necesario.

Estas opciones se describen en las secciones siguientes.

#### <a name="stop-all-hive-jobs"></a>Detención de todos los trabajos de Hive

Detenga todos los trabajos de Hive antes de reducir verticalmente a un nodo de trabajo. Si la carga de trabajo está programada, ejecute la reducción vertical una vez realizado el trabajo de Hive.

Detener los trabajos de Hive ante del escalado ayuda a minimizar el número de archivos temporales en la carpeta tmp (en caso de que haya).

#### <a name="manually-clean-up-hives-scratch-files"></a>Limpieza manual de los archivos temporales de Hive

Si Hive ha dejado archivos temporales, puede limpiarlos manualmente antes de la reducción vertical para evitar la activación del modo seguro.

1. Compruebe qué ubicación se usa para los archivos temporales de Hive, para lo que puede examinar la propiedad de la configuración `hive.exec.scratchdir`. Este parámetro se establece en `/etc/hive/conf/hive-site.xml`:

    ```xml
    <property>
        <name>hive.exec.scratchdir</name>
        <value>hdfs://mycluster/tmp/hive</value>
    </property>
    ```

1. Detenga los servicios de Hive y asegúrese de que se hayan completado todas las consultas y los trabajos.
2. Enumere el contenido del directorio temporal `hdfs://mycluster/tmp/hive/` para ver si contiene archivos:

    ```bash
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```

    Este es un ejemplo de resultado cuando existen archivos:

    ```output
    sshuser@scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Si sabe que Hive ya no necesita estos archivos, puede eliminarlos. Asegúrese de que Hive no tiene ninguna consulta en ejecución; para ello, consulte la página de la interfaz de usuario de ResourceManager en Yarn.

    Línea de comandos de ejemplo para quitar archivos de HDFS:

    ```bash
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```

#### <a name="scale-hdinsight-to-three-or-more-worker-nodes"></a>Escalado de HDInsight a tres, o más, nodos de trabajo

Si los clústeres se quedan bloqueados en modo seguro cuando se realiza una reducción vertical a menos de tres nodos de trabajo y los pasos anteriores no funcionan, puede evitar que el clúster entre en modo seguro por completo manteniendo al menos tres nodos de trabajo.

Conservar tres nodos de trabajo es más caro que realizar una reducción vertical a uno solo, pero impedirá que el clúster se quede bloqueado en modo seguro.

### <a name="scale-hdinsight-down-to-one-worker-node"></a>Reducción vertical de HDInsight a un nodo de trabajo

Incluso cuando el clúster se reduce verticalmente a 1 nodo, el nodo de trabajo 0 aún sobrevive. El nodo de trabajo 0 nunca se puede retirar.

#### <a name="run-the-command-to-leave-safe-mode"></a>Ejecución del comando para salir del modo seguro

La última opción es ejecutar el comando para salir del modo seguro. Si sabe que el motivo para que HDFS entre en modo seguro es que hay poca replicación de archivos de Hive, puede ejecutar el siguiente comando para salir del modo seguro:

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

### <a name="scale-down-an-apache-hbase-cluster"></a>Reducción vertical de un clúster de Apache HBase

Los servidores regionales se equilibran automáticamente en pocos minutos tras completar una operación de escalado. Para equilibrar manualmente los servidores regionales, siga estos pasos:

1. Conéctate al clúster de HDInsight con SSH: Para más información, consulte [Uso SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Inicie el shell de HBase:

    ```bash
    hbase shell
    ```

3. Use el comando siguiente para equilibrar manualmente los servidores regionales:

    ```bash
    balancer
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Escalado automático de clústeres de Azure HDInsight](hdinsight-autoscale-clusters.md)
* [Introducción a Azure HDInsight](hadoop/apache-hadoop-introduction.md)
