---
title: Extracción, transformación y carga de datos (ETL) a escala en Azure HDInsight
description: Obtenga información acerca de cómo se usa la extracción, transformación y carga en HDInsight con Apache Hadoop.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/28/2020
ms.openlocfilehash: ee989ccbb2e441256bec71781c538c7761fc7b88
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82232247"
---
# <a name="extract-transform-and-load-etl-at-scale"></a>Extracción, transformación y carga de datos (ETL) a escala

Extracción, transformación y carga de datos (ETL) es el proceso por el que se obtienen los datos de diversos orígenes. Estos se recopilan en una ubicación estándar, se limpian y se procesan. En última instancia, se cargan en un almacén de datos de los que se puede consultar. Los procesos ETL heredados importan los datos, los limpian localmente y, a continuación, los almacenan en un motor de datos relacionales. Con HDInsight, una gran variedad de componentes del entorno de Apache Hadoop admiten ETL a escala.

Esta canalización puede resumir el uso de HDInsight en el proceso ETL:

![Información general de ETL de HDInsight a escala](./media/apache-hadoop-etl-at-scale/hdinsight-etl-at-scale-overview.png)

En las siguientes secciones se explica cada una de las fases de ETL y sus componentes asociados.

## <a name="orchestration"></a>Orquestación

La orquestación se expande a lo largo de todas las fases de la canalización de ETL. Los trabajos de ETL en HDInsight a menudo implican varios productos diferentes que funcionan con dependencia el uno del otro.  Puede usar Hive para limpiar alguna parte de los datos, mientras Pig limpia otra.  Puede usar Azure Data Factory para cargar datos en Azure SQL Database desde Azure Data Lake Store.

La orquestación es necesaria para ejecutar el trabajo pertinente en el momento adecuado.

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie es un sistema de coordinación de flujos de trabajo que administra trabajos de Hadoop. Oozie se ejecuta dentro de un clúster de HDInsight y se integra con la pila de Hadoop. Oozie es compatible con los trabajos de Hadoop para Apache Hadoop MapReduce, Apache Pig, Apache Hive y Apache Sqoop. También puede usarse para programar trabajos específicos de un sistema, como scripts de shell o programas Java.

Para obtener más información, consulte [Uso de Apache Oozie con Apache Hadoop para definir y ejecutar un flujo de trabajo en HDInsight](../hdinsight-use-oozie-linux-mac.md). Consulte también [Uso de una canalización de datos](../hdinsight-operationalize-data-pipeline.md).

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory proporciona funcionalidades de orquestación como plataforma como servicio. Es un servicio de integración de datos basado en la nube que le permite crear flujos de trabajo controlados por datos en la nube. Flujos de trabajo para orquestar y automatizar tanto el movimiento de datos como la transformación de datos.

Con Azure Data Factory, puede:

1. Crear y programar flujos de trabajo controlados por datos (denominados canalizaciones) que ingieren datos de distintos almacenes de datos.
2. Procesar y transformar los datos mediante servicios de proceso, como Azure HDInsight Hadoop. O Spark, Azure Data Lake Analytics, Azure Batch y Azure Machine Learning.
3. Publicación de datos de salida en almacenes de datos como Azure SQL Data Warehouse para que las aplicaciones de inteligencia empresarial los consuman.

Para obtener más información sobre Azure Data Factory, consulte la [documentación](../../data-factory/introduction.md).

## <a name="ingest-file-storage-and-result-storage"></a>Ingesta de almacenamiento de archivos y de resultados

Normalmente, los archivos de datos de origen se cargan en una ubicación de Azure Storage o Azure Data Lake Storage. Los archivos pueden estar en cualquier formato, pero son normalmente archivos planos, como CSV.

### <a name="azure-storage"></a>Azure Storage

Azure Storage tiene objetivos de adaptabilidad específicos. Consulte [Objetivos de escalabilidad y rendimiento de Blob Storage](../../storage/blobs/scalability-targets.md). Para los nodos más analíticos, Azure Storage se escala mejor cuando trabaja con muchos archivos pequeños.  Azure Storage garantiza el mismo rendimiento, independientemente del tamaño de los archivos (siempre y cuando se encuentren dentro de sus límites).  Esto garantiza que puede almacenar terabytes y seguir obteniendo un rendimiento coherente, tanto si usa un subconjunto de los datos como su totalidad.

Azure Storage tiene varios tipos diferentes de blobs.  Un *blob en anexos* es una opción ideal para almacenar los registros web o los datos del sensor.  

Varios blobs pueden distribuirse en distintos servidores para escalar horizontalmente su acceso. Sin embargo, un solo servidor puede enviar a un único blob. Aunque los blobs pueden agruparse lógicamente en contenedores de blob, esta agrupación no afecta a las particiones.

Azure Storage también tiene una capa de API de WebHDFS para el almacenamiento de blobs.  Todos los servicios de HDInsight pueden acceder a los archivos de Azure Blob Storage para la limpieza y procesamiento de datos. De forma similar al modo en que esos servicios usarían el sistema de archivos distribuido de Hadoop (HDFS).

Normalmente, los datos se ingieren en Azure Storage mediante PowerShell, el SDK de Azure Storage o AZCopy.

### <a name="azure-data-lake-storage"></a>Azure Data Lake Storage

Azure Data Lake Storage (ADLS) es un repositorio administrado de hiperescala para datos de análisis que es compatible con HDFS.  ADLS usa un paradigma de diseño similar al de HDFS. Además, ofrece adaptabilidad ilimitada tanto para la capacidad total como para el tamaño de los archivos individuales. ADLS es adecuado para trabajar con archivos grandes, ya que un archivo de gran tamaño se puede almacenar en varios nodos.  La creación de particiones de datos de ADLS se realiza en segundo plano.  Obtiene un rendimiento masivo para ejecutar trabajos de análisis con miles de ejecutores simultáneos que leen y escriben cientos de terabytes de datos con gran eficacia.

Normalmente, los datos se ingieren en ADLS mediante Azure Data Factory. O bien, los SDK de ADLS, el servicio de AdlCopy, Apache DistCp o Apache Sqoop.  El uso de un servicio u otro depende en gran medida de dónde se encuentran dichos datos.  Si los datos están actualmente en un clúster de Hadoop existente, podría usar Apache DistCp, AdlCopy Service o Azure Data Factory.  En el caso de los datos en Azure Blob Storage, puede usar el SDK de .NET de Azure Data Lake Storage, Azure PowerShell o Azure Data Factory.

ADLS también está optimizado para la ingesta de eventos con Azure Event Hubs o Apache Storm.

#### <a name="considerations-for-both-storage-options"></a>Consideraciones para ambas opciones de almacenamiento

Para cargar los conjuntos de datos en el intervalo de terabytes, la latencia de red puede ser un problema serio, especialmente si los datos provienen de una ubicación local.  En tales casos, puede utilizar las opciones siguientes:

* Azure ExpressRoute:  Azure ExpressRoute permite crear conexiones privadas entre los centros de datos de Azure y la infraestructura local. Estas conexiones ofrecen una opción confiable para transferir grandes cantidades de datos. Para obtener más información, consulte la [documentación de Azure ExpressRoute](../../expressroute/expressroute-introduction.md).

* Carga "sin conexión" de los datos. Puede usar el [servicio Azure Import/Export](../../storage/common/storage-import-export-service.md) para enviar unidades de disco duro con sus datos a un centro de datos de Azure. Los datos se cargan primero a Blobs de Azure Storage. Después, puede usar Azure Data Factory o la herramienta AdlCopy para copiar datos desde Azure Storage Blob a Data Lake Storage.

### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse

Azure SQL Data Warehouse es una excelente opción para almacenar resultados preparados.  Azure HDInsight puede usarse para hacer esos servicios para Azure SQL DW.

Azure SQL Data Warehouse (SQL DW) es un almacén de bases de datos relacionales optimizado para cargas de trabajo de análisis.  Azure SQL DW se escala según las tablas con particiones.  La tablas se pueden dividir en particiones entre varios nodos.  Los nodos de Azure SQL DW se seleccionan en el momento de la creación.  Se pueden escalar a posteriori, pero es un proceso activo que puede requerir el movimiento de datos. Para más información, consulte [SQL Data Warehouse: administración del proceso](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).

### <a name="apache-hbase"></a>HBase Apache

Apache HBase es un almacén de par clave-valor disponible en Azure HDInsight.  Apache HBase es una base de datos NoSQL de código abierto que se compila en Hadoop y se modela después de Google BigTable. HBase proporciona un acceso aleatorio y eficaz y una enorme coherencia para grandes cantidades de datos no estructurados y semiestructurados. Estos datos se encuentran en una base de datos sin esquemas organizada por familias de columnas.

Los datos se almacenan en las filas de una tabla, mientras que los datos de una fila se agrupan por familia de columnas. HBase es una base de datos sin esquema. No es necesario definir las columnas y los tipos de datos almacenados en ellas antes de usarlos. El código abierto se escala linealmente para controlar petabytes de datos en miles de nodos. HBase puede basarse en la redundancia de datos, el procesamiento por lotes y otras características proporcionadas por aplicaciones distribuidas en el entorno de Hadoop.

Es un destino excelente para los datos del sensor y de registro para su posterior análisis.

La adaptabilidad de HBase depende del número de nodos del clúster de HDInsight.

### <a name="azure-sql-database-and-azure-database"></a>Azure SQL Database y Azure Database

Azure ofrece tres bases de datos relacionales diferentes como plataforma como servicio (PaaS).

* [Azure SQL Database](../../sql-database/sql-database-technical-overview.md) es una implementación de Microsoft SQL Server. Para obtener más información sobre el rendimiento, consulte [Ajuste del rendimiento en Azure SQL Database](../../sql-database/sql-database-performance-guidance.md).
* [Azure Database for MySQL](../../mysql/overview.md) es una implementación de MySQL de Oracle.
* [Azure Database for PostgreSQL](../../postgresql/quickstart-create-server-database-portal.md) es una implementación de PostgreSQL.

Estos productos se escalan verticalmente, lo que significa que se escalan mediante la adición de más CPU y memoria.  También puede optar por usar los discos premium con los productos para mejorar el rendimiento de E/S.

## <a name="azure-analysis-services"></a>Azure Analysis Services

Azure Analysis Services (AAS) es un motor de datos analíticos que se usa para la ayuda para la toma de decisiones y el análisis empresarial. AAS proporciona los datos analíticos para los informes empresariales y las aplicaciones cliente, como Power BI. Así como para Excel, los informes de Reporting Services y otras herramientas de visualización de datos.

Para que los cubos de análisis se escalen, se deben cambiar los niveles de cada cubo individual.  Para obtener más información, consulte [Precios de Azure Analysis Services](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="extract-and-load"></a>Extracción y carga

Una vez que los datos existan en Azure, puede usar muchos servicios para extraerlos y cargarlos en otros productos.  HDInsight admite Sqoop y Flume.

### <a name="apache-sqoop"></a>Apache Sqoop

Apache Sqoop es una herramienta que se ha diseñado para transferir datos eficazmente entre orígenes de datos estructurados, semiestructurados y no estructurados.

Sqoop usa MapReduce para importar y exportar los datos y para proporcionar tolerancia a errores y una operación paralela.

### <a name="apache-flume"></a>Apache Flume

`Apache Flume` es un servicio distribuido, confiable y disponible para recopilar, agregar y mover grandes cantidades de datos de registro de forma eficaz. Flume tiene una arquitectura flexible basada en los flujos de datos de streaming. Flume es sólido y tolerante a errores con mecanismos de confiabilidad ajustables y numerosos mecanismos de conmutación por error y recuperación. Flume utiliza un modelo de datos extensible simple que permite la aplicación de análisis en línea.

No se puede usar Flume Apache con Azure HDInsight.  En una instalación de Hadoop local se puede usar Flume para enviar datos a Azure Storage Blob o Azure Data Lake Storage.  Para obtener más información, consulte [Using Apache Flume with HDInsight (Uso de Apache Flume con HDInsight)](https://web.archive.org/web/20190217104751/https://blogs.msdn.microsoft.com/bigdatasupport/2014/03/18/using-apache-flume-with-hdinsight/).

## <a name="transform"></a>Transformación

Una vez que existan los datos en la ubicación elegida, deberá limpiarlos, combinarlos o prepararlos para un patrón de uso específico.  Hive, Pig y Spark SQL son buenas opciones para ese tipo de trabajo.  Todos se admiten en HDInsight.

## <a name="next-steps"></a>Pasos siguientes

* [Uso de Apache Hive como herramienta ETL](apache-hadoop-using-apache-hive-as-an-etl-tool.md)
* [Uso de Data Lake Storage Gen2 con clústeres de Azure HDInsight](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Traslado de datos de Azure SQL Database a tablas de Apache Hive](./apache-hadoop-use-sqoop-mac-linux.md)
