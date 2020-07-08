---
title: Solución de problemas de Storm mediante Azure HDInsight
description: Obtenga respuestas a las preguntas comunes sobre cómo usar con Apache Storm con Azure HDInsight.
keywords: Azure HDInsight, Storm, preguntas más frecuentes, guía de solución de problemas, problemas comunes
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 11/08/2019
ms.custom: seodec18
ms.openlocfilehash: b51b2c21fd9256c93f6947386a48336af2b75d88
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700372"
---
# <a name="troubleshoot-apache-storm-by-using-azure-hdinsight"></a>Solución de problemas de Apache Storm mediante Azure HDInsight

Obtenga información sobre los principales problemas y sus soluciones al trabajar con cargas útiles de [Apache Storm](https://storm.apache.org/) en [Apache Ambari](https://ambari.apache.org/).

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a>¿Cómo se accede a la interfaz de usuario de Storm en un clúster?

Tiene dos opciones para acceder a la interfaz de usuario de Storm desde un explorador:

### <a name="apache-ambari-ui"></a>Interfaz de usuario de Apache Ambari

1. Vaya al panel de Ambari.
2. En la lista de servicios, seleccione **Storm**.
3. En el menú **Vínculos rápidos**, seleccione **Interfaz de usuario de Storm**.

### <a name="direct-link"></a>Vínculo directo

Puede acceder a la interfaz de usuario de Storm en la siguiente URL:

`https://<cluster DNS name>/stormui`

Ejemplo: `https://stormcluster.azurehdinsight.net/stormui`

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a>¿Cómo se transfiere información del punto de control del spout del centro de eventos de Storm de una topología a otra

Cuando se desarrollan topologías que leen desde instancias de Azure Event Hubs mediante el archivo .jar del spout del centro de eventos Storm en HDInsight, debe implementar una topología que tenga el mismo nombre en un nuevo clúster. Sin embargo, debe conservar los datos del punto de control que se confirmaron en [Apache ZooKeeper](https://zookeeper.apache.org/) en el clúster antiguo.

### <a name="where-checkpoint-data-is-stored"></a>Dónde se almacenan los datos de punto de control

El spout del centro de eventos almacena los datos de punto de control para los desplazamientos en ZooKeeper en dos rutas de acceso raíz:

- Los puntos de control de spout no transaccionales se almacenan en `/eventhubspout`.

- Los datos de punto de control de spout de transacciones se almacenan en `/transactional`.

### <a name="how-to-restore"></a>Cómo restaurar

Para obtener los scripts y las bibliotecas que se utilizan para exportar datos de ZooKeeper y, después, importar de nuevo los datos a ZooKeeper con un nuevo nombre, consulte [Ejemplos de Storm de HDInsight](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

La carpeta lib tiene archivos .jar que contienen la implementación para la operación de exportación e importación. La carpeta bash tiene un script de ejemplo que muestra como exportar datos desde el servidor de ZooKeeper en el clúster anterior y, después, volver a importarlos al servidor de ZooKeeper en el nuevo clúster.

Ejecuta el script [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) desde los nodos de ZooKeeper para importar y después exportar datos. Actualice el script a la versión correcta de Hortonworks Data Platform (HDP). (Estamos trabajando en que estos scripts sean genéricos en HDInsight. Los scripts genéricos pueden ejecutarse desde cualquier nodo en el clúster sin modificaciones por el usuario).

El comando export escribe los metadatos en una ruta de acceso del Sistema de archivos distribuido de Apache Hadoop (HDFS) (en una instancia de Azure Blob Storage o de Azure Data Lake Storage) en la ubicación que haya establecido.

### <a name="examples"></a>Ejemplos

#### <a name="export-offset-metadata"></a>Exportación de los metadatos de desplazamiento

1. Use SSH para ir al clúster de ZooKeeper en el clúster desde el que se debe exportar el desplazamiento de punto de control.
2. Ejecute el comando siguiente (después de actualizar la cadena de versión de HDP) para exportar los datos de desplazamiento de ZooKeeper a la ruta de acceso de HDFS `/stormmetadta/zkdata`:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Importación de los metadatos de desplazamiento

1. Use SSH para ir al clúster de ZooKeeper en el clúster desde el que se debe importar el desplazamiento de punto de control.
2. Ejecute el comando siguiente (después de actualizar la cadena de versión de HDP) para importar los datos de desplazamiento de ZooKeeper desde la ruta de acceso de HDFS `/stormmetadata/zkdata` hasta el servidor de ZooKeeper en el clúster de destino:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```

#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a>Elimine los metadatos de desplazamiento para que las topologías puedan empezar a procesar los datos desde el principio o desde una marca de tiempo que elija el usuario.

1. Use SSH para ir al clúster de ZooKeeper en el clúster desde el que se debe eliminar el desplazamiento de punto de control.
2. Ejecute el comando siguiente (después de actualizar la cadena de versión de HDP) para eliminar todos los datos de desplazamiento de ZooKeeper del clúster actual:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>¿Cómo se buscan los archivos binarios de Storm en un clúster?

Los archivos binarios de Storm para la pila HDP actual están en `/usr/hdp/current/storm-client`. La ubicación es la misma para los nodos principales y para los nodos de trabajo.

Puede haber varios archivos binarios para versiones específicas de HDP en /usr/hdp (por ejemplo, `/usr/hdp/2.5.0.1233/storm`). La carpeta `/usr/hdp/current/storm-client` tiene un vínculo simbólico a la versión más reciente que se ejecuta en el clúster.

Para más información, consulte cómo [conectarse a un clúster de HDInsight a través de SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) y [Apache Storm](https://storm.apache.org/).

## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a>¿Cómo se determina la topología de implementación de un clúster de Storm?

En primer lugar, identifique todos los componentes que se instalan con Storm de HDInsight. Un clúster de Storm consta de cuatro categorías de nodo:

* Nodos de puerta de enlace
* Nodos principales
* Nodos de ZooKeeper
* Nodos de trabajo

### <a name="gateway-nodes"></a>Nodos de puerta de enlace

Un nodo de puerta de enlace es una puerta de enlace y un servicio de proxy inverso que permite el acceso público a un servicio de administración de Ambari activo. También controla la elección del líder de Ambari.

### <a name="head-nodes"></a>Nodos principales

Los nodos principales de Storm ejecutan los siguientes servicios:
* Nimbus
* Servidor Ambari
* Servidor de métricas de Ambari
* Recopilador de métricas de Ambari
 
### <a name="zookeeper-nodes"></a>Nodos de ZooKeeper

HDInsight incluye un cuórum de ZooKeeper de tres nodos. El tamaño del cuórum es fijo y no se puede volver a configurar.

Los servicios de Storm en el clúster se configuran para usar automáticamente el cuórum de ZooKeeper.

### <a name="worker-nodes"></a>Nodos de trabajo

Los nodos de trabajo de Storm ejecutan los siguientes servicios:
* Supervisor
* Máquinas virtuales Java (JVM) de trabajo, para topologías en ejecución
* Agente de Ambari

## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>¿Cómo se buscan los archivos binarios del spout del centro de eventos de Storm para el desarrollo?

Para más información sobre el uso de archivos .jar del centro de eventos de Storm con su topología, consulte los siguientes recursos.

### <a name="java-based-topology"></a>Topología basada en Java

[Procesamiento de eventos desde Azure Event Hubs con Apache Storm en HDInsight (Java)](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub)

### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>Topología basada en C# (Mono en clústeres de Storm Linux HDInsight 3.4 o superior)

[Procesamiento de eventos desde Azure Event Hubs con Apache Storm en HDInsight (C#)](https://docs.microsoft.com/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)

### <a name="latest-apache-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Archivos binarios de spout del centro de eventos de Apache Storm para clústeres de Storm Linux HDInsight 3.5 y superior

Para información sobre cómo usar el spout del centro de eventos de Storm más reciente que funciona con los clústeres de Linux Storm de HDInsight 3.5 o superior, consulte el [archivo Léame de mvn-repo](https://github.com/hdinsight/mvn-repo/blob/master/README.md).

### <a name="source-code-examples"></a>Ejemplos de código fuente

Consulte algunos [ejemplos](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) sobre cómo leer y escribir desde un centro de eventos de Azure Event Hubs mediante una topología de Apache Storm (escrita en Java) en un clúster de Azure HDInsight.

## <a name="how-do-i-locate-storm-log4j-2-configuration-files-on-clusters"></a>¿Cómo se buscan los archivos de configuración Log4J 2 de Storm en clústeres?

Para identificar los archivos de configuración de [Log4j 2 de Apache](https://logging.apache.org/log4j/2.x/) para servicios de Storm.

### <a name="on-head-nodes"></a>En nodos principales

La configuración de Nimbus Log4J se lee en `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`.

### <a name="on-worker-nodes"></a>En nodos de trabajo

La configuración de Log4J del supervisor se lee en `/usr/hdp/\<HDP version>/storm/log4j2/cluster.xml`.

El archivo de configuración de Log4J de trabajo se lee en `/usr/hdp/\<HDP version>/storm/log4j2/worker.xml`.

Ejemplos: `/usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml`
`/usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml`

---

## <a name="not-a-leader-exception"></a>No es una excepción de líder

Al enviar una topología, el usuario puede recibir un mensaje de error similar a: `Topology submission exception, cause not a leader, the current leader is NimbusInfo`.

Para resolverlo, es posible que el usuario tenga que abrir una incidencia de soporte técnico para que los nodos se reinicien. Para más información, vea [https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html](https://community.hortonworks.com/content/supportkb/150287/error-ignoring-exception-while-trying-to-get-leade.html).

---

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

- Obtenga respuestas de expertos de Azure mediante el [soporte técnico de la comunidad de Azure](https://azure.microsoft.com/support/community/).

- Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport), la cuenta oficial de Microsoft Azure para mejorar la experiencia del cliente. Esta cuenta pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.

- Si necesita más ayuda, puede enviar una solicitud de soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Seleccione **Soporte técnico** en la barra de menús o abra la central **Ayuda + soporte técnico**. Para obtener información más detallada, revise [Creación de una solicitud de soporte técnico de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). La suscripción a Microsoft Azure incluye acceso al soporte técnico para facturación y administración de suscripciones. El soporte técnico se proporciona a través de uno de los [planes de soporte técnico de Azure](https://azure.microsoft.com/support/plans/).
