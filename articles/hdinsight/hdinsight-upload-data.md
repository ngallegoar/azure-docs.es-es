---
title: Carga de datos para trabajos de Apache Hadoop en HDInsight
description: Aprenda a cargar datos de trabajos de Apache Hadoop, y a acceder a ellos, con la CLI de Azure clásica, el Explorador de Azure Storage, Azure PowerShell, la línea de comandos de Hadoop o Sqoop.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdiseo17may2017
ms.topic: conceptual
ms.date: 10/29/2019
ms.openlocfilehash: 7eb1f7e1ce02a30f84cb520438f60fcbcfa3a965
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73100144"
---
# <a name="upload-data-for-apache-hadoop-jobs-in-hdinsight"></a>Carga de datos para trabajos de Apache Hadoop en HDInsight

Azure HDInsight ofrece un sistema de archivos distribuido de Hadoop (HDFS) completo a través de Azure Storage y Azure Data Lake Storage (Gen1 y Gen2). Azure Storage y Data Lake Storage Gen1 y Gen2 están diseñados como una extensión de HDFS a fin de ofrecer una experiencia sin problemas para los clientes. Habilitan el conjunto completo de componentes en el ecosistema de Hadoop para que opere directamente en los datos que administra. Azure Storage y Data Lake Storage Gen1 y Gen2 son sistemas de archivos diferentes que se han optimizado para el almacenamiento de datos y el cálculo en ellos. Para más información sobre las ventajas del uso de Azure Storage, consulte [Uso de Azure Storage con HDInsight](hdinsight-hadoop-use-blob-storage.md), [Uso de Data Lake Storage Gen1 con HDInsight](hdinsight-hadoop-use-data-lake-store.md) y [Uso de Data Lake Storage Gen2 con HDInsight](hdinsight-hadoop-use-data-lake-storage-gen2.md).

## <a name="prerequisites"></a>Prerrequisitos

Tenga en cuenta los siguientes requisitos antes de empezar:

* Un clúster de HDInsight de Azure. Para obtener instrucciones, consulte [Introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md) o [Creación de clústeres de HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* Conocimientos de los artículos siguientes:
    * [Uso de Azure Storage con HDInsight](hdinsight-hadoop-use-blob-storage.md)
    * [Uso de Data Lake Storage Gen1 con HDInsight](hdinsight-hadoop-use-data-lake-store.md)
    * [Uso de Data Lake Storage Gen2 con HDInsight](hdinsight-hadoop-use-data-lake-storage-gen2.md)  

## <a name="upload-data-to-azure-storage"></a>Carga de datos en Azure Storage

## <a name="utilities"></a>Sectores públicos

Microsoft proporciona las utilidades siguientes para trabajar con Azure Storage:

| Herramienta | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [CLI de Azure](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |
| [Línea de comandos de Hadoop](#commandline) |✔ |✔ |✔ |

> [!NOTE]  
> El comando de Hadoop solo está disponible en el clúster de HDInsight. El comando solo permite cargar datos desde el sistema de archivos local en Azure Storage.  

## <a name="hadoop-command-line"></a><a id="commandline"></a>Línea de comandos de Hadoop

La línea de comandos de Hadoop solo es útil para almacenar datos en Azure Storage Blob cuando los datos ya están presentes en el nodo principal del clúster.

Para poder usar el comando de Hadoop, primero debe conectarse al nodo principal mediante [SSH o PuTTY](hdinsight-hadoop-linux-use-ssh-unix.md).

Una vez conectado, puede utilizar la siguiente sintaxis para cargar un archivo al almacenamiento.

```bash
hadoop fs -copyFromLocal <localFilePath> <storageFilePath>
```

Por ejemplo: `hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Como el sistema de archivos predeterminado de HDInsight está en Azure Storage, /example/data/data.txt está en realidad en Azure Storage. También puede referirse al archivo como:

    wasbs:///example/data/data.txt

or

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Para obtener una lista de otros comandos de Hadoop que funcionan con archivos, consulte [https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/FileSystemShell.html)

> [!WARNING]  
> En los clústeres de Apache HBase, el tamaño de bloque predeterminado al escribir datos es de 256 KB. Aunque esto funciona bien cuando se usan API de REST o API de HBase, el uso de los comandos `hadoop` o `hdfs dfs` para escribir más de ~ 12 GB de datos genera un error. Para más información, vea la sección [Excepción de almacenamiento para escritura en blob](#storageexception) de este artículo.

## <a name="graphical-clients"></a>Clientes gráficos

También hay varias aplicaciones que proporcionan una interfaz gráfica para trabajar con Azure Storage. La siguiente tabla es una lista de algunas de estas aplicaciones:

| Remoto | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Microsoft Visual Studio Tools para HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md#explore-linked-resources) |✔ |✔ |✔ |
| [Explorador de Azure Storage](../storage/blobs/storage-quickstart-blobs-storage-explorer.md) |✔ |✔ |✔ |
| [Cerulea](https://www.cerebrata.com/products/cerulean/features/azure-storage) | | |✔ |
| [CloudXplorer](https://clumsyleaf.com/products/cloudxplorer) | | |✔ |
| [Explorador de CloudBerry para Microsoft Azure](https://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | |✔ |
| [Cyberduck](https://cyberduck.io/) | |✔ |✔ |

## <a name="mount-azure-storage-as-local-drive"></a>Montaje de Azure Storage como unidad local

Vea [Montaje de Azure Storage como unidad local](https://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

## <a name="upload-using-services"></a>Carga mediante servicios

### <a name="azure-data-factory"></a>Azure Data Factory

El servicio Azure Data Factory es un servicio completamente administrado para crear servicios de almacenamiento de datos, procesamiento de datos y movimiento en canalizaciones de producción de datos confiable, escalable y simplificado.

|Tipo de almacenamiento|Documentación|
|----|----|
|Azure Blob Storage|[Copia de datos con Azure Blob Storage como origen o destino mediante Azure Data Factory](../data-factory/connector-azure-blob-storage.md)|
|Azure Data Lake Storage Gen1|[Copia de datos con Azure Data Lake Storage Gen1 como origen o destino mediante Azure Data Factory](../data-factory/connector-azure-data-lake-store.md)|
|Azure Data Lake Storage Gen2 |[Carga de datos en Azure Data Lake Storage Gen2 con Azure Data Factory](../data-factory/load-azure-data-lake-storage-gen2.md)|

### <a name="apache-sqoop"></a><a id="sqoop"></a>Apache Sqoop

Sqoop es una herramienta diseñada para transferir datos entre Hadoop y las bases de datos relacionales. Puede usarla para importar datos desde un sistema de administración de bases de datos relacionales (RDBMS), como SQL Server, MySQL u Oracle en el sistema de archivos distribuidos de Hadoop (HDFS), transformar los datos de Hadoop con MapReduce o Hive y, a continuación, exportar los datos en un RDBMS.

Para más información, consulte [Uso de Sqoop con HDInsight](hadoop/hdinsight-use-sqoop.md).

### <a name="development-sdks"></a>SDK de desarrollo

También se puede acceder a Azure Storage mediante un SDK de Azure desde los siguientes lenguajes de programación:

* .NET
* Java
* Node.js
* PHP
* Python
* Ruby

Para obtener más información acerca de cómo instalar los SDK de Azure, consulte [Descargas de Azure](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Solución de problemas

### <a name="storage-exception-for-write-on-blob"></a><a id="storageexception"></a>Excepción de almacenamiento para escritura en blob

**Síntomas**: al usar los comandos `hadoop` o `hdfs dfs` para escribir archivos de aproximadamente 12 GB, o mayores, en un clúster de HBase, puede experimentarse el siguiente error:

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

**Causa**: HBase en clústeres de HDInsight toma como valor predeterminado un tamaño de bloque de 256 KB al escribir en Azure Storage. Si bien esto funciona para API de HBase o API de REST, se produce un error al usar las utilidades de línea de comandos `hadoop` o `hdfs dfs`.

**Solución:** use `fs.azure.write.request.size` para especificar un tamaño de bloque mayor. Puede hacerlo mediante un sistema por uso con el parámetro `-D`. El siguiente comando es un ejemplo de uso de este parámetro con el comando `hadoop`:

```bash
hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data
```

También puede aumentar el valor de `fs.azure.write.request.size` globalmente mediante Apache Ambari. Los siguientes pasos se pueden usar para cambiar el valor en la IU web de Ambari:

1. En el explorador, vaya a la IU web de Ambari para el clúster. Es `https://CLUSTERNAME.azurehdinsight.net`, donde `CLUSTERNAME` es el nombre del clúster.

    Cuando se le solicite, escriba el nombre de usuario y la contraseña de administrador para el clúster.
2. En el lado izquierdo de la pantalla, seleccione **HDFS** y luego seleccione la pestaña **Configs** (Configuraciones).
3. En el campo **Filter...** (Filtro...), escriba `fs.azure.write.request.size`. Esto muestra el campo y el valor actual en el centro de la página.
4. Cambie el valor de 262144 (256 KB) al nuevo valor. Por ejemplo, 4194304 (4 MB).

    ![Imagen de cambiar el valor en la IU web de Ambari](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Para más información acerca del uso de Ambari, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya sabe cómo enviar datos a HDInsight, consulte los artículos siguientes para aprender a realizar el análisis:

* [Introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Envío de trabajos de Apache Hadoop mediante programación](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Uso de Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md)
* [Uso de Apache Pig con HDInsight](hadoop/hdinsight-use-pig.md)
