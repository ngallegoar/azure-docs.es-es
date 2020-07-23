---
title: 'Migración de datos: Apache Hadoop local a Azure HDInsight'
description: Obtenga información sobre los procedimientos recomendados de migración de datos para migrar clústeres locales de Hadoop a Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: ashishth
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 11/22/2019
ms.openlocfilehash: 9794dd47949dc7dea891893dbcf261808ab335fd
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86521384"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---data-migration-best-practices"></a>Migración de clústeres locales de Apache Hadoop a Azure HDInsight: prácticas recomendadas de la migración de datos

En este artículo se proporcionan recomendaciones para la migración de datos a Azure HDInsight. Forma parte de una serie de artículos que proporcionan prácticas recomendadas para ayudar a migrar sistemas locales de Apache Hadoop a Azure HDInsight.

## <a name="migrate-on-premises-data-to-azure"></a>Migración de datos locales a Azure

Hay dos opciones principales para migrar datos desde un entorno local a un entorno de Azure:

* Transferir datos a través de la red con TLS
    * A través de Internet: puede transferir datos a Azure Storage con una conexión a Internet normal mediante alguna de estas herramientas: Explorador de Azure Storage, AzCopy, Azure Powershell y la CLI de Azure. Para más información, consulte el artículo sobre cómo [mover datos desde y hasta Azure Storage](../../storage/common/storage-moving-data.md).

    * ExpressRoute: ExpressRoute es un servicio de Azure que permite crear conexiones privadas entre los centros de datos de Microsoft y la infraestructura local o en una instalación de coubicación. Las conexiones ExpressRoute no se realizan sobre una conexión a Internet pública y ofrecen mayor seguridad, confiabilidad y velocidad con menor latencia que las conexiones a Internet típicas a través de Internet. Para más información, consulte [Creación y modificación de un circuito ExpressRoute](../../expressroute/expressroute-howto-circuit-portal-resource-manager.md).

    * Transferencia de datos en línea con Data Box: Data Box Edge y Data Box Gateway son productos para la transferencia de datos en línea que actúan como puertas de enlace de almacenamiento en la red con el fin de administrar los datos entre su centro de datos y Azure. Data Box Edge, un dispositivo de red local, transfiere los datos a Azure, y al contrario, y utiliza el proceso perimetral con inteligencia artificial (IA) para procesar los datos. Data Box Gateway es un dispositivo virtual con funcionalidades de puerta de enlace de almacenamiento. Para más información, consulte [Documentación de Azure Data Box: Transferencia en línea](https://docs.microsoft.com/azure/databox-online/).

* Envío de datos sin conexión

    Transferencia de datos con Data Box: Los dispositivos Data Box, Data Box Disk y Data Box Heavy facilitan la transferencia de grandes cantidades de datos a Azure cuando la red no es una opción. Estos dispositivos de transferencia de datos sin conexión se envían entre su organización y el centro de datos Azure. Utilizan cifrado AES para proteger los datos en tránsito y se someten a un estricto proceso de saneamiento después de la carga para eliminar los datos del dispositivo. Para obtener más información sobre los dispositivos de transferencia sin conexión de Data Box, consulte la [Documentación de Azure Data Box: transferencia sin conexión](https://docs.microsoft.com/azure/databox/). Para obtener más información sobre la migración de clústeres de Hadoop, consulte [Usar Azure Data Box para migrar datos desde un almacén HDFS local a Azure Storage](../../storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster.md).

La tabla siguiente contiene la duración de la transferencia de datos aproximada según el volumen de datos y el ancho de banda de la red. Utilice un Data Box si la migración de datos se espera que tarde más de tres semanas.

|Cant. de datos | Ancho de banda de red<br>de<br>**45 Mbps (T3)**|Ancho de banda de red<br>de<br>**100 Mbps**|Ancho de banda de red<br>de<br>**1 Gbps**|Ancho de banda de red<br>de<br>**10 Gbps**|
|---|:---:|:---:|:---:|:---:|
|1 TB|2 días|1 día| 2 horas|14 minutos|
|10 TB|22 días|10 días|1 día|2 horas|
|35 TB|76 días|34 días|3 días|8 horas|
|80 TB|173 días|78 días|8 días|19 horas|
|100 TB|216 días|97 días|10 días|1 día|
|200 TB|1 año|194 días|19 días|2 días|
|500 TB|3 años|1 año|49 días|5 días|
|1 PB|6 años|3 años|97 días|10 días|
|2 PB|12 años|5 años|194 días|19 días|

Las herramientas nativas de Azure, como Apache Hadoop, DistCp, Azure Data Factory y AzureCp, se pueden usar para transferir datos a través de la red. La herramienta de terceros WANDisco también se puede usar para la misma finalidad. Apache Kafka Mirrormaker y Apache Sqoop se pueden usar para la transferencia de datos en curso desde un sistema local a sistemas de almacenamiento de Azure.

## <a name="performance-considerations-when-using-apache-hadoop-distcp"></a>Consideraciones de rendimiento a la hora de usar DistCp de Apache Hadoop

DistCp es un proyecto de Apache que usa un trabajo de asignación de MapReduce para transferir datos, controlar los errores y recuperarse de dichos errores. Asigna una lista de archivos de origen a cada tarea de asignación. Después, la tarea de asignación copia todos sus archivos asignados en el destino. Hay varias técnicas que pueden mejorar el rendimiento de DistCp.

### <a name="increase-the-number-of-mappers"></a>Aumento del número de asignadores

DistCp intenta crear las tareas de asignación para que cada una de ellas copie aproximadamente el mismo número de bytes. De forma predeterminada, los trabajos de DistCp utilizan a veinte asignadores. Mediante el uso de más asignadores para Distcp (con el parámetro "m" en la línea de comandos) aumenta el paralelismo durante el proceso de transferencia de datos y se reduce la longitud de dicha transferencia. Sin embargo, hay dos aspectos que deben considerarse al aumentar el número de asignadores:

* La granularidad más baja de DistCp es un único archivo. La especificación de un número de asignadores superior al número de archivos de origen no ayuda y se perderán los recursos de clúster disponibles.

* Tenga en cuenta la memoria de Yarn disponible en el clúster para determinar el número de asignadores. Cada tarea de asignación se inicia como un contenedor de Yarn. Suponiendo que no se están ejecutando otras cargas de trabajo pesadas en el clúster, el número de asignadores se puede determinar mediante la siguiente fórmula: m = (Número de nodos de trabajo \* Memoria de YARN para cada nodo de trabajo) / Tamaño del contenedor de YARN. Sin embargo, si otras aplicaciones usan memoria, elija usar solamente una parte de la memoria de YARN para trabajos de DistCp.

### <a name="use-more-than-one-distcp-job"></a>Uso de más de un trabajo de DistCp

Cuando el tamaño del conjunto de datos que desea mover es mayor que 1 TB, use más de un trabajo de DistCp. El uso de más de un trabajo limita el impacto de errores. Si se produce un error en cualquier trabajo, solo deberá reiniciar ese trabajo específico en lugar de todos los trabajos.

### <a name="consider-splitting-files"></a>Consideración de la posibilidad de dividir archivos

Si hay un pequeño número de archivos de gran tamaño, considere la posibilidad de dividirlos en fragmentos de archivo de 256 MB para obtener mayor simultaneidad en potencia con más asignadores.

### <a name="use-the-strategy-command-line-parameter"></a>Uso del parámetro de la línea de comandos "strategy"

Considere el uso del parámetro `strategy = dynamic` en la línea de comandos. El valor predeterminado del parámetro `strategy` es `uniform size`, en cuyo caso cada asignación copia aproximadamente el mismo número de bytes. Cuando este parámetro se cambia a `dynamic`, el archivo de lista se divide en varios "archivos de fragmento". El número de archivos de fragmento es un múltiplo del número de asignaciones. A cada tarea de asignación se asigna uno de los archivos de fragmento. Una vez procesadas todas las rutas de acceso de un fragmento, el fragmento actual se elimina y se adquiere un nuevo fragmento. El proceso continúa hasta que no hay ningún fragmento más disponible. Este enfoque "dinámico" permite tareas de asignación más rápidas consuman más rutas de acceso que las más lentas, lo que agiliza el trabajo de DistCp en general.

### <a name="increase-the-number-of-threads"></a>Aumento del número de subprocesos

Observe si el aumento del parámetro `-numListstatusThreads` mejora el rendimiento. Este parámetro controla el número de subprocesos que se utilizarán para crear el listado de archivos, siendo cuarenta el valor máximo.

### <a name="use-the-output-committer-algorithm"></a>Uso del algoritmo confirmador de salida

Observe si pasar el parámetro `-Dmapreduce.fileoutputcommitter.algorithm.version=2` mejora el rendimiento de DistCp. Este algoritmo confirmador de salida dispone de optimizaciones en torno a la escritura de archivos de salida en el destino. El comando siguiente es un ejemplo que muestra el uso de diferentes parámetros:

```bash
hadoop distcp -Dmapreduce.fileoutputcommitter.algorithm.version=2 -numListstatusThreads 30 -m 100 -strategy dynamic hdfs://nn1:8020/foo/bar wasb://<container_name>@<storage_account_name>.blob.core.windows.net/foo/
```

## <a name="metadata-migration"></a>Migración de metadatos

### <a name="apache-hive"></a>Apache Hive

Hive Metastore se puede migrar mediante el uso de scripts o mediante la replicación de base de datos.

#### <a name="hive-metastore-migration-using-scripts"></a>Migración de Hive Metastore mediante scripts

1. Genere los DDL de Hive desde Hive Metastore local. Este paso se puede realizar mediante un [script de Bash de contenedor](https://github.com/hdinsight/hdinsight.github.io/blob/master/hive/hive-export-import-metastore.md).
1. Edite el DDL generado para reemplazar la dirección URL de HDFS con direcciones URL de WASB, ADLS o ABFS.
1. Ejecute el DDL actualizado en la tienda de metadatos desde el clúster de HDInsight.
1. Asegúrese de que la versión de Hive Metastore es compatible entre el entorno local y la nube.

#### <a name="hive-metastore-migration-using-db-replication"></a>Migración de Hive Metastore mediante replicación de base de datos

- Establezca la replicación de base de datos entre la base de datos de Hive Metastore local y la base de datos de Metastore de HDInsight.
- Use "Hive MetaTool" para reemplazar la dirección URL de HDFS con las direcciones URL de WASB, ADLS o ABFS, por ejemplo:

    ```bash
    ./hive --service metatool -updateLocation hdfs://nn1:8020/ wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
    ```

### <a name="apache-ranger"></a>Apache Ranger

- Exporte las directivas de Ranger locales a archivos xml.
- Transforme las rutas de acceso basadas en HDFS específicas locales en WASB o ADLS mediante una herramienta como XSLT.
- Importe las directivas de Ranger que se ejecutan en HDInsight.

## <a name="next-steps"></a>Pasos siguientes

Lea el siguiente artículo de esta serie:

- [Security and DevOps best practices for on-premises to Azure HDInsight Hadoop migration](apache-hadoop-on-premises-migration-best-practices-security-devops.md) (Seguridad y procedimientos recomendados de DevOps para la migración de sistemas de Hadoop locales a Azure HDInsight)
