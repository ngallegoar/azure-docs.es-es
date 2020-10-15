---
title: Escenarios de datos relacionados con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Descripción de los diferentes escenarios y herramientas mediante los cuales los datos se pueden ingerir, procesar, descargar y visualizar en Data Lake Storage Gen2 (anteriormente conocido como Azure Data Lake Store).
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/14/2020
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 6e183a1e8cac8eec2806a35603c40a44aadac502
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84193509"
---
# <a name="using-azure-data-lake-storage-gen2-for-big-data-requirements"></a>Uso de Azure Data Lake Storage Gen2 para requisitos de macrodatos

Hay cuatro fases principales en el procesamiento de macrodatos:

> [!div class="checklist"]
> * Introducción de grandes cantidades de datos en un almacén de datos, en tiempo real o por lotes
> * Procesamiento de los datos
> * Descarga de los datos
> * Visualización de los datos

En este artículo se resaltan las opciones y las herramientas de cada fase de procesamiento.

Para obtener una lista completa de los servicios de Azure que puede usar con Azure Data Lake Storage Gen2, consulte [Integración de Azure Data Lake Storage con los servicios de Azure](data-lake-storage-integrate-with-azure-services.md).

## <a name="ingest-the-data-into-data-lake-storage-gen2"></a>Ingesta de datos en Data Lake Storage Gen2

En esta sección se resaltan los distintos orígenes de datos y las distintas formas en que esos datos se pueden ingerir en una cuenta de Data Lake Storage Gen2.

![Ingesta de datos en Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/ingest-data.png "Ingesta de datos en Data Lake Storage Gen2")

### <a name="ad-hoc-data"></a>Datos ad-hoc

Representan conjuntos de datos más pequeños que se utilizan para la creación de un prototipo de una aplicación de macrodatos. Hay diferentes maneras de introducir datos ad hoc, según el origen de los datos. 

Esta es una lista de herramientas que puede usar para ingerir datos ad hoc.

| Origen de datos | Introducir mediante |
| --- | --- |
| Equipo local |[Azure PowerShell](data-lake-storage-directory-file-acl-powershell.md)<br><br>[CLI de Azure](data-lake-storage-directory-file-acl-cli.md)<br><br>[Explorador de Storage](https://azure.microsoft.com/features/storage-explorer/)<br><br>[Herramienta AzCopy](../common/storage-use-azcopy-v10.md)|
| Blob de Azure Storage |[Azure Data Factory](../../data-factory/connector-azure-data-lake-store.md)<br><br>[Herramienta AzCopy](../common/storage-use-azcopy-v10.md)<br><br>[DistCp ejecutándose en un clúster de HDInsight](data-lake-storage-use-distcp.md)|

### <a name="streamed-data"></a>Datos de streaming

Representa los datos que se pueden generar por diversos orígenes, como aplicaciones, dispositivos o sensores, entre otros. Estos datos se pueden ingerir en Data Lake Storage Gen2 mediante distintas herramientas. Estas herramientas normalmente capturan y procesan los datos en eventos individuales y en tiempo real y, después, escriben los eventos en lotes en Data Lake Storage Gen2 para que puedan procesarse posteriormente.

Esta es una lista de herramientas que puede usar para ingerir datos de streaming.

|Herramienta | Guía |
|---|--|
|Azure Stream Analytics|[Inicio rápido: Creación de un trabajo de Stream Analytics mediante Azure Portal](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-portal) <br> [Salida a Azure Data Lake Gen2](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-define-outputs#blob-storage-and-azure-data-lake-gen2)|
|Azure HDInsight Storm | [Escritura en HDFS de Apache Hadoop desde Apache Storm en HDInsight](https://docs.microsoft.com/azure/hdinsight/storm/apache-storm-write-data-lake-store) |

### <a name="relational-data"></a>Datos relacionales

También se pueden originar datos desde bases de datos relacionales. Durante un tiempo, las bases de datos relacionales recopilan grandes cantidades de datos que pueden proporcionar información clave si se procesan a través de una canalización de macrodatos. Puede usar las herramientas siguientes para mover estos datos a Data Lake Storage Gen2.

Esta es una lista de herramientas que puede usar para ingerir datos relacionales.

|Herramienta | Guía |
|---|--|
|Azure Data Factory | [Actividad de copia en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |

### <a name="web-server-log-data-upload-using-custom-applications"></a>Datos de registro de servidor web (carga mediante aplicaciones personalizadas)

Este tipo de conjunto de datos es específicamente necesario porque el análisis de los datos de registro del servidor web es un caso de uso común para aplicaciones de macrodatos y requiere que se carguen grandes volúmenes de archivos de registro en Data Lake Storage Gen2. Puede utilizar cualquiera de las herramientas siguientes para escribir sus propios scripts o aplicaciones para cargar dichos datos.

Esta es una lista de herramientas que puede usar para ingerir datos de registro de servidor web.

|Herramienta | Guía |
|---|--|
|Azure Data Factory | [Actividad de copia en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview)  |
|Azure CLI|[CLI de Azure](data-lake-storage-directory-file-acl-cli.md)|
|Azure PowerShell|[Azure PowerShell](data-lake-storage-directory-file-acl-powershell.md)|

Para cargar datos de registro del servidor web y también para cargar otros tipos de datos (por ejemplo, datos de opiniones sociales), un buen enfoque es escribir sus propios aplicaciones o scripts personalizados, porque le ofrece la flexibilidad de incluir el componente que carga los datos como parte de su aplicación de macrodatos mayor. En algunos casos, este código puede adoptar la forma de un script o de una utilidad de línea de comandos simple. En otros casos, el código puede utilizarse para integrar el procesamiento de macrodatos en una aplicación o solución de negocio.

### <a name="data-associated-with-azure-hdinsight-clusters"></a>Datos asociados con los clústeres de HDInsight de Azure

La mayoría de los tipos de clúster de HDInsight (Hadoop, HBase, Storm) admiten Data Lake Storage Gen2 como repositorio de almacenamiento de datos. Los clústeres de HDInsight acceden a los datos de Azure Storage Blob (WASB). Para mejorar el rendimiento, puede copiar los datos de WASB en una cuenta de Data Lake Storage Gen2 asociada con el clúster. Puede usar las herramientas siguientes para copiar los datos.

Esta es una lista de herramientas que puede usar para ingerir datos asociados con los clústeres de HDInsight de Azure.

|Herramienta | Guía |
|---|--|
|Apache DistCp | [Uso de DistCp para copiar datos entre Azure Storage Blob y Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|Herramienta AzCopy | [Transferencia de datos con AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10) |
|Azure Data Factory | [Copia de datos con Azure Data Lake Storage Gen2 como origen o destino mediante Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-data-lake-storage-gen2) |

### <a name="data-stored-in-on-premises-or-iaas-hadoop-clusters"></a>Datos almacenados en clústeres locales o de IaaS de Hadoop

Es posible que haya grandes cantidades de datos almacenados en clústeres de Hadoop existentes, de forma local en los equipos mediante HDFS. Los clústeres de Hadoop pueden encontrarse en una implementación local o dentro de un clúster de IaaS en Azure. Podrían existir requisitos para copiar estos datos en Azure Data Lake Storage Gen2 de forma puntual o periódica. Existen diversas opciones que puede usar para lograr esto. A continuación se muestra una lista de alternativas, además de las ventajas y desventajas asociadas.

| Enfoque | Detalles | Ventajas | Consideraciones |
| --- | --- | --- | --- |
| Usar Azure Data Factory (ADF) para copiar datos directamente desde los clústeres de Hadoop hasta Azure Data Lake Storage Gen2 |[ADF admite HDFS como origen de datos](../../data-factory/connector-hdfs.md) |ADF proporciona compatibilidad inmediata con HDFS, así como una supervisión y administración integrales y de primera clase. |Requiere que se implemente Data Management Gateway localmente o en el clúster de IaaS. |
| Usar Distcp para copiar datos de Hadoop en Azure Storage. Después, copiar los datos de Azure Storage en Data Lake Store Gen2 con el mecanismo adecuado. |Puede copiar datos de Azure Storage a Data Lake Store Gen2 por medio de: <ul><li>[Azure Data Factory](../../data-factory/copy-activity-overview.md)</li><li>[Herramienta AzCopy](../common/storage-use-azcopy-v10.md)</li><li>[Apache DistCp ejecutándose en clústeres de HDInsight](data-lake-storage-use-distcp.md)</li></ul> |Puede usar herramientas de código abierto. |Proceso de varios pasos en el que participan varias tecnologías. |

### <a name="really-large-datasets"></a>Conjuntos de datos realmente grandes

Para cargar conjuntos de datos cuyo tamaño oscila en varios terabytes, el uso de los métodos descritos anteriormente puede a veces resultar lento y costoso. En esos casos, puede usar Azure ExpressRoute.  

Azure ExpressRoute permite crear conexiones privadas entre los centros de datos de Azure y la infraestructura de un entorno local. Esto ofrece una opción confiable para transferir grandes cantidades de datos. Para más información, consulte [Información general sobre ExpressRoute](../../expressroute/expressroute-introduction.md).

## <a name="process-the-data"></a>Procesamiento de los datos

Cuando los datos están disponibles en Data Lake Storage Gen2, puede ejecutar un análisis en esos datos mediante las aplicaciones de macrodatos admitidas. 

![Análisis de datos en Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/analyze-data.png "Análisis de datos en Data Lake Storage Gen2")

Esta es una lista de herramientas que puede usar para ejecutar trabajos de análisis de datos en los datos que se almacenan en Data Lake Storage Gen2.

|Herramienta | Guía |
|---|--|
|HDInsight de Azure | [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) |
|Azure Databricks | [Azure Data Lake Storage Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html)<br><br>[Inicio rápido: Análisis de datos en Azure Data Lake Storage Gen2 con Azure Databricks](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-quickstart-create-databricks-account?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)<br><br>[Tutorial: Extracción, transformación y carga de datos mediante Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/databricks-extract-load-sql-data-warehouse?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="visualize-the-data"></a>Visualización de los datos

Utilice el conector de Power BI para crear representaciones visuales de los datos almacenados en Data Lake Storage Gen2. Consulte [Análisis de datos en Azure Data Lake Storage Gen2 con Power BI](https://docs.microsoft.com/power-query/connectors/datalakestorage).

## <a name="download-the-data"></a>Descarga de datos

También puede descargar o mover datos de Azure Data Lake Storage Gen2 en escenarios como los siguientes:

* Mover datos a otros repositorios para interactuar con las canalizaciones de procesamiento de datos existentes. Por ejemplo, puede mover los datos de Data Lake Storage Gen2 a Azure SQL Database o a una instancia de SQL Server.

* Descargar datos en el equipo local para procesarlos en entornos IDE durante la creación de prototipos de aplicaciones.

![Salida datos desde Data Lake Storage Gen2](./media/data-lake-storage-data-scenarios/egress-data.png "Salida datos desde Data Lake Storage Gen2")

Esta es una lista de herramientas que puede usar para descargar datos de Data Lake Storage Gen2.

|Herramienta | Guía |
|---|--|
|Azure Data Factory | [Actividad de copia en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) |
|Apache DistCp | [Uso de DistCp para copiar datos entre Azure Storage Blob y Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-use-distcp) |
|Explorador de Azure Storage|[Uso del Explorador de Azure Storage para administrar directorios, archivos y ACL en Azure Data Lake Storage Gen2](data-lake-storage-explorer.md)|
|Herramienta AzCopy|[Transferencia de datos con AzCopy y Blob Storage](../common/storage-use-azcopy-blobs.md)|
