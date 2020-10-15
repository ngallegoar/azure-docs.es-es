---
title: Introducción a Azure Data Lake Storage Gen2 en HDInsight
description: Introducción a Data Lake Storage Gen2 en HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: a7ae1a0f77d0d19fc48695ddc5cc3d3a14d65ab9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "82195135"
---
# <a name="azure-data-lake-storage-gen2-overview-in-hdinsight"></a>Introducción a Azure Data Lake Storage Gen2 en HDInsight

Azure Data Lake Storage Gen2 toma las características principales de Azure Data Lake Storage Gen1 y las integra en Azure Blob Storage. Estas características incluyen un sistema de archivos que es compatible con Hadoop, Azure Active Directory (Azure AD) y listas de control de acceso basadas en POSIX. Esta combinación le permite aprovechar el rendimiento de Azure Data Lake Storage Gen1. Asimismo, le permite usar la administración del ciclo de vida de los datos y los niveles de Blob Storage.

Para obtener más información sobre Data Lake Storage Gen2, consulte [Introducción a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md).

## <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Funcionalidad básica de Azure Data Lake Storage Gen2

* **Acceso compatible con Hadoop:** En Azure Data Lake Storage Gen2, puede administrar datos y acceder a ellos igual que lo haría con un sistema de archivos distribuido de Hadoop (HDFS). El controlador del sistema de archivos de Azure Blob (ABFS) está disponible en todos los entornos de Apache Hadoop, incluidos Azure HDInsight y Azure Databricks. Use ABFS para acceder a los datos almacenados en Data Lake Storage Gen2.

* **Superconjunto de permisos POSIX**: el modelo de seguridad de Data Lake Gen2 es compatible con los permisos de ACL y POSIX junto con granularidad adicional específica de Data Lake Storage Gen2. Se puede realizar la configuración mediante herramientas de administración o marcos, como Apache Hive y Apache Spark.

* **Rentabilidad**: Data Lake Storage Gen2 ofrece transacciones y capacidad de almacenamiento de bajo costo. Los ciclos de vida de Azure Blob Storage ayudan a reducir los costos mediante el ajuste de las tarifas de facturación a medida que los datos avanzan en su ciclo de vida.

* **Compatibilidad con aplicaciones, marcos y herramientas de Blob Storage**: Data Lake Storage Gen2 sigue funcionando con una amplia gama de herramientas, marcos y aplicaciones para Blob Storage.

* **Controlador optimizado**: el controlador ABFS está optimizado específicamente para el análisis de macrodatos. Las API REST correspondientes se exponen a través del punto de conexiónde sistema de archivo distribuido (dfs),dfs.core.windows.net.

## <a name="whats-new-for-azure-data-lake-storage-gen-2"></a>Novedades de Azure Data Lake Storage Gen2

### <a name="managed-identities-for-secure-file-access"></a>Identidades administradas para proteger el acceso a archivos

Azure HDInsight usa identidades administradas para proteger el acceso del clúster a los archivos de Azure Data Lake Storage Gen2. Las identidades administradas son una característica de Azure Active Directory que proporciona servicios de Azure con un conjunto de credenciales administradas automáticamente. Estas credenciales se pueden usar para autenticarse en cualquier servicio que admita la autenticación de Active Directory. El uso de identidades administradas no requiere almacenar las credenciales en archivos de código o de configuración.

Para obtener más información, consulte [Managed identities for Azure resources](../active-directory/managed-identities-azure-resources/overview.md) (Identidades administradas para los recursos de Azure).

### <a name="azure-blob-file-system-driver"></a>Controlador Azure Blob File System

Las aplicaciones de Apache Hadoop de forma nativa esperan leer y escribir datos desde el almacenamiento en disco local. Un controlador del sistema de archivos de Hadoop como ABFS permite que las aplicaciones de Hadoop funcionen con el almacenamiento en la nube. Funciona mediante la emulación de operaciones normales del sistema de archivos de Hadoop. El controlador convierte los comandos que recibe desde la aplicación en operaciones que la plataforma real de almacenamiento en la nube reconoce.

Anteriormente, el controlador del sistema de archivos de Hadoop convertía todas las operaciones del sistema de archivos en llamadas API REST de Azure Storage en el lado cliente. A continuación, invocaba la API REST. Sin embargo, esta conversión del lado cliente resultaba en varias llamadas API de REST para una única operación del sistema de archivos, como cambiar el nombre a un archivo. ABFS movió la lógica de sistema de archivos de Hadoop del lado cliente al lado servidor. La API de Azure Data Lake Storage Gen2 ahora se ejecuta en paralelo con la API de Blob. Esta migración mejora el rendimiento porque ahora se pueden ejecutar operaciones comunes del sistema de archivos de Hadoop con una llamada API REST.

Para más información, vea [Controlador Azure Blob FileSystem (ABFS): un controlador de Azure Storage dedicado para Hadoop](../storage/blobs/data-lake-storage-abfs-driver.md).

### <a name="uri-scheme-for-azure-data-lake-storage-gen-2"></a>Esquema de URI de Azure Data Lake Storage Gen2

Azure Data Lake Storage Gen2 usa un nuevo esquema de identificador URI para acceder a los archivos de Azure Storage desde HDInsight:

`abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

El esquema de URI proporciona acceso con cifrado SSL.

`<FILE_SYSTEM_NAME>` identifica la ruta de acceso del sistema de archivos de Data Lake Storage Gen2.

`<ACCOUNT_NAME>` identifica el nombre de la cuenta de Azure Storage. Se necesita el nombre completo de dominio (FQDN).

`<PATH>` es el nombre de la ruta HDFS del archivo o el directorio.

Si no se especifican los valores de `<FILE_SYSTEM_NAME>` y `<ACCOUNT_NAME>`, se utiliza el sistema de archivos predeterminado. Para los archivos del sistema de archivos predeterminado, puede usar una ruta relativa o absoluta. Por ejemplo, se puede hacer referencia al archivo `hadoop-mapreduce-examples.jar` que se incluye con los clústeres de HDInsight mediante alguna de las rutas de acceso siguientes:

```
abfs://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfs:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]
> El nombre del archivo es `hadoop-examples.jar` en las versiones 2.1 y 1.6 de los clústeres de HDInsight. Al trabajar con archivos fuera de HDInsight, verá que la mayoría de las utilidades no reconocen el formato ABFS y, en su lugar, esperan un formato básico de ruta de acceso, como `example/jars/hadoop-mapreduce-examples.jar`.

Para más información, vea [Uso del URI de Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction-abfs-uri.md).

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-introduction.md)
* [Introducción a Almacenamiento de Azure](../storage/common/storage-introduction.md)
* [Introducción a Azure Data Lake Storage Gen1](./overview-data-lake-storage-gen1.md)