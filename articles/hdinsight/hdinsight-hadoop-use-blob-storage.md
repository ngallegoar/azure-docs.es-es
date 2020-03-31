---
title: 'Consulta de datos desde Azure Storage compatible con HDFS: Azure HDInsight'
description: Aprenda a consultar datos desde Azure Storage y Azure Data Lake Storage para almacenar los resultados del análisis.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/28/2020
ms.openlocfilehash: e62f6f8df51c6acf4e2ad8e28e431d264c2c99e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79037237"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Uso de Azure Storage con clústeres de Azure HDInsight

Para analizar datos del clúster de HDInsight, puede almacenar los datos en [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Storage Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake Storage Gen 2](../storage/blobs/data-lake-storage-introduction.md), o en una combinación de ambos. Estas opciones de almacenamiento permiten eliminar de forma segura clústeres de HDInsight que se usan para el cálculo sin perder datos del usuario.

Apache Hadoop admite una noción del sistema de archivos predeterminado. El sistema de archivos predeterminado implica una autoridad y un esquema predeterminados. También se puede usar para resolver rutas de acceso relativas. Durante el proceso de creación del clúster de HDInsight, puede especificar un contenedor de blobs en Azure Storage como el sistema de archivos predeterminado; también, con HDInsight 3.6, puede seleccionar Azure Storage o Azure Data Lake Storage Gen 1/Azure Data Lake Storage Gen 2 como el sistema de archivos predeterminado con algunas excepciones. Para más información sobre la compatibilidad con el uso de Data Lake Store Gen 1 como almacenamiento predeterminado y como almacenamiento vinculado, consulte [Disponibilidades de los clústeres de HDInsight](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

En este artículo, aprenderá cómo funciona Azure Storage con clústeres de HDInsight. Para más información sobre cómo funciona Data Lake Storage Gen 1 con clústeres de HDInsight, consulte [Use Azure Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md) (Uso de Azure Data Lake Store con clústeres de Azure HDInsight). Consulte [Creación de clústeres de Apache Hadoop en HDInsight](hdinsight-hadoop-provision-linux-clusters.md) para obtener información sobre la creación de un clúster de HDInsight.

> [!IMPORTANT]  
> El tipo de cuenta de almacenamiento **BlobStorage** solo puede usarse como almacenamiento secundario para los clústeres de HDInsight.

| Tipo de cuenta de almacenamiento | Servicios admitidos | Niveles de rendimiento admitidos |Sin niveles de rendimiento admitidos| Niveles de acceso admitidos |
|----------------------|--------------------|-----------------------------|---|------------------------|
| StorageV2 (uso general v2)  | Blob     | Estándar                    |Premium| Frecuente, esporádico, archivo\*   |
| Storage (uso general v1)   | Blob     | Estándar                    |Premium| N/D                    |
| BlobStorage                    | Blob     | Estándar                    |Premium| Frecuente, esporádico, archivo\*   |

No se recomienda usar el contenedor de blobs predeterminado para almacenar datos empresariales. Conviene eliminar el contenedor de blobs predeterminado después de cada uso para reducir los costos de almacenamiento. El contenedor predeterminado contiene los registros del sistema y de la aplicación. Asegúrese de recuperar los registros antes de eliminar el contenedor.

No se permite compartir un contenedor de blobs como el sistema de archivos predeterminado entre varios clústeres.

> [!NOTE]  
> El nivel de acceso de archivo es un nivel sin conexión que tiene una latencia de recuperación de varias horas y no se recomienda para su uso con HDInsight. Para más información, consulte [Nivel de acceso de archivo](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

## <a name="access-files-from-within-cluster"></a>Acceso a los archivos desde dentro del clúster

Existen varias maneras de acceder a los archivos de Data Lake Storage desde un clúster de HDInsight. El esquema URI proporciona acceso sin cifrar (con el prefijo *wasb:* ) y acceso cifrado SSL con *wasbs*). Se recomienda usar *wasbs* siempre que sea posible, incluso cuando se acceda a datos que se encuentren en la misma región de Azure.

* **Con el nombre completo**. Con este enfoque, proporciona la ruta de acceso completa al archivo al que quiere acceder.

    ```
    wasb://<containername>@<accountname>.blob.core.windows.net/<file.path>/
    wasbs://<containername>@<accountname>.blob.core.windows.net/<file.path>/
    ```

* **Con el formato abreviado de la ruta de acceso**. Con este enfoque, reemplazará la ruta de acceso a la raíz del clúster por:

    ```
    wasb:///<file.path>/
    wasbs:///<file.path>/
    ```

* **Con la ruta de acceso relativa**. Con este enfoque, solo proporciona la ruta de acceso relativa al archivo al que quiere acceder.

    ```
    /<file.path>/
    ```

### <a name="data-access-examples"></a>Ejemplos de acceso a datos

Los ejemplos se basan en una [conexión SSH](./hdinsight-hadoop-linux-use-ssh-unix.md) al nodo principal del clúster. En los ejemplos se usan los tres esquemas de URI. Reemplace `CONTAINERNAME` y `STORAGEACCOUNT` por los valores pertinentes.

#### <a name="a-few-hdfs-commands"></a>Algunos comandos hdfs

1. Crear un archivo simple en el almacenamiento local.

    ```bash
    touch testFile.txt
    ```

1. Crear directorios en el almacenamiento de clúster.

    ```bash
    hdfs dfs -mkdir wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -mkdir wasbs:///sampledata2/
    hdfs dfs -mkdir /sampledata3/
    ```

1. Copia de datos del almacenamiento local al almacenamiento de clúster.

    ```bash
    hdfs dfs -copyFromLocal testFile.txt  wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -copyFromLocal testFile.txt  wasbs:///sampledata2/
    hdfs dfs -copyFromLocal testFile.txt  /sampledata3/
    ```

1. Enumerar el contenido del directorio en el almacenamiento de clúster.

    ```bash
    hdfs dfs -ls wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/sampledata1/
    hdfs dfs -ls wasbs:///sampledata2/
    hdfs dfs -ls /sampledata3/
    ```

> [!NOTE]  
> Al trabajar con blobs fuera de HDInsight, la mayoría de las utilidades no reconocen el formato WASB y en su lugar, espera un formato básico de ruta de acceso, como `example/jars/hadoop-mapreduce-examples.jar`.

#### <a name="creating-a-hive-table"></a>Creación de una tabla de Hive

Con fines de ilustración, se muestran tres ubicaciones de archivos. En la ejecución real, use solo una de las entradas `LOCATION`.

```hql
DROP TABLE myTable;
CREATE EXTERNAL TABLE myTable (
    t1 string,
    t2 string,
    t3 string,
    t4 string,
    t5 string,
    t6 string,
    t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE
LOCATION 'wasbs://CONTAINERNAME@STORAGEACCOUNT.blob.core.windows.net/example/data/';
LOCATION 'wasbs:///example/data/';
LOCATION '/example/data/';
```

## <a name="access-files-from-outside-cluster"></a>Acceso a los archivos desde fuera del clúster

Microsoft proporciona las siguientes herramientas para trabajar con Azure Storage:

| Herramienta | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [CLI de Azure](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="identify-storage-path-from-ambari"></a>Identificación de la ruta de acceso de almacenamiento de Ambari

* Para identificar la ruta de acceso completa al almacén predeterminado configurado, vaya a:

    **HDFS** > **Configs** y escriba `fs.defaultFS` en el cuadro de entrada de filtro.

* Para comprobar si el almacén wasb está configurado como el almacenamiento secundario, vaya a:

    **HDFS** > **Configs** y escriba `blob.core.windows.net` en el cuadro de entrada de filtro.

Para obtener la ruta de acceso mediante la API REST de Ambari, consulte [Obtención del almacenamiento predeterminado](./hdinsight-hadoop-manage-ambari-rest-api.md#get-the-default-storage).

## <a name="blob-containers"></a>Contenedores de blobs

Para usar blobs, en primer lugar debe crear una [cuenta de Azure Storage](../storage/common/storage-create-storage-account.md). Como parte de esto, especifica una región de Azure donde se crea la cuenta de almacenamiento. El clúster y la cuenta de almacenamiento deben ubicarse en la misma región. La base de datos de SQL Server de la tienda de metadatos Hive y la base de datos de SQL Server de la tienda de metadatos Apache Oozie también deben encontrarse en la misma región.

Cualquiera que sea su ubicación, todos los blobs que cree pertenecerán a un contenedor de su cuenta de Azure Storage. Este contenedor puede ser un blob existente creado fuera de HDInsight, o bien un contenedor que se crea para un clúster de HDInsight.

El contenedor de blobs predeterminado almacena información específica del clúster, como registros y el historial de trabajos. No comparta un contenedor de blobs predeterminado con varios clústeres de HDInsight. Se podría dañar el historial de trabajos. Es recomendable usar un contenedor diferente para cada clúster y colocar los datos compartidos en una cuenta de almacenamiento vinculada especificada en la implementación de todos los clústeres pertinentes en lugar de la cuenta de almacenamiento predeterminada. Para más información acerca de cómo configurar cuentas de almacenamiento vinculadas, consulte el artículo de [Creación de clústeres de HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Sin embargo, puede volver a usar un contenedor de almacenamiento predeterminado después de que se haya eliminado el clúster de HDInsight original. En el caso de los clústeres de HBase, para conservar el esquema y los datos de tabla de HBase se puede crear un nuevo clúster de HBase mediante el contenedor de blobs predeterminado que usaba un clúster de HBase eliminado.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="use-additional-storage-accounts"></a>Uso de cuentas de almacenamiento adicionales

Al crear un clúster de HDInsight, se especifica la cuenta de Azure Storage a la que quiere asociarlo. Además de esta cuenta de almacenamiento, puede agregar otras desde la misma suscripción de Azure o desde otras diferentes tanto durante el proceso de creación como después de que el clúster se haya creado. Para obtener instrucciones sobre cómo agregar más cuentas de almacenamiento, consulte [Creación de clústeres de HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> No se admite el uso de una cuenta de almacenamiento adicional en una ubicación diferente a la del clúster de HDInsight.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a usar Azure Storage compatible con HDFS con HDInsight. Esto le permite crear soluciones de adquisición de datos de archivado escalables y a largo plazo y usar HDInsight para desbloquear la información que hay dentro de los datos estructurados y no estructurados almacenados.

Para más información, consulte:

* [Introducción a Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Introducción a Azure Data Lake Storage](../data-lake-store/data-lake-store-get-started-portal.md)
* [Carga de datos en HDInsight](hdinsight-upload-data.md)
* [Uso de Apache Hive con HDInsight](hadoop/hdinsight-use-hive.md)
* [Uso de firmas de acceso compartido de Azure Storage para restringir el acceso a datos en HDInsight](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Tutorial: Extracción, transformación y carga de datos mediante Interactive Query en Azure HDInsight](./interactive-query/interactive-query-tutorial-analyze-flight-data.md)
