---
title: Uso de almacenes de metadatos externos en Azure HDInsight
description: Use almacenes de metadatos externos con clústeres de Azure HDInsight y procedimientos recomendados.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/02/2020
ms.openlocfilehash: edb2d256d3e5d98c52dbdff1162e0e030ebe2be3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79233528"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Uso de repositorios de metadatos externos en Azure HDInsight

HDInsight permite tomar el control de los datos y metadatos mediante la implementación de soluciones de metadatos y bases de datos de administración clave en almacenes de datos externos. Esta característica está disponible actualmente para [Apache Hive Metastore](#custom-metastore), [Apache Oozie Metastore](#apache-oozie-metastore) y [bases de datos de Apache Ambari](#custom-ambari-db).

Apache Hive Metastore en HDInsight es una parte fundamental de la arquitectura de Apache Hadoop. Metastore es el repositorio del esquema central que otras herramientas de acceso de macrodatos pueden usar, como Apache Spark, Interactive Query (LLAP), Presto o Apache Pig. HDInsight utiliza una base de datos de Azure SQL Database como Hive Metastore.

![Arquitectura del repositorio de metadatos de Hive en HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

Hay dos formas de configurar una tienda de metadatos para los clústeres de HDInsight:

* [Tienda de metadatos predeterminada](#default-metastore)
* [Tienda de metadatos personalizada](#custom-metastore)

## <a name="default-metastore"></a>Tienda de metadatos predeterminada

De forma predeterminada, HDInsight crea una tienda de metadatos con cada tipo de clúster. Pero en vez de esto, puede especificar una tienda de metadatos personalizada. La tienda de metadatos predeterminada incluye las siguientes consideraciones:

* No implica costes adicionales. HDInsight crea una tienda de metadatos con cada tipo de clúster sin ningún coste adicional para el usuario.

* Cada tienda de metadatos predeterminada forma parte del ciclo de vida del clúster. Al eliminar un clúster, también se eliminan la tienda de metadatos y los metadatos correspondientes.

* No puede compartir la metastore predeterminada con otros clústeres.

* La tienda de metadatos predeterminada utiliza una base de datos Azure SQL DB básica, que tiene un límite de cinco DTU (unidad de transacción de base de datos).
Esta tienda de metadatos predeterminada se utiliza normalmente para las cargas de trabajo relativamente sencillas que no requieren varios clústeres y no necesitan que los metadatos se conserven más allá del ciclo de vida del clúster.

## <a name="custom-metastore"></a>Tienda de metadatos personalizada

HDInsight también admite tiendas de metadatos personalizadas, que se recomiendan para clústeres de producción:

* Como tienda de metadatos, se especifica su propia base de datos de Azure SQL Database.

* El ciclo de vida de la metastore no está asociado a un ciclo de vida de los clústeres, por lo que puede crear y eliminar clústeres sin pérdida de metadatos. Los metadatos, como los esquemas de Hive, se conservarán incluso después de eliminar y volver a crear el clúster de HDInsight.

* Una tienda de metadatos personalizada le permite adjuntar varios clústeres y tipos de clústeres a esa tienda de metadatos. Por ejemplo, una tienda de metadatos sencilla puede compartirse a través de clústeres de Interactive Query, Hive y Spark en HDInsight.

* Se paga por el costo de una tienda de metadatos (Azure SQL DB) de acuerdo con el nivel de rendimiento que seleccione.

* Es posible escalar la tienda de metadatos según sea necesario.

* El clúster y la tienda de metadatos externa deben estar hospedados en la misma región.

![Caso de uso de repositorio de metadatos de Hive en HDInsight](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

### <a name="create-and-config-azure-sql-database-for-the-custom-metastore"></a>Creación y configuración de Azure SQL Database para la metastore personalizada

Para configurar una instancia de Hive Metastore personalizada para un clúster de HDInsight, debe crear una instancia de Azure SQL Database o tener ya una.  Para más información, consulte [Inicio rápido: Creación de una base de datos única en Azure SQL DB](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started?tabs=azure-portal).

Para asegurarse de que el clúster de HDInsight puede acceder a la instancia de Azure SQL Database conectada, configure las reglas de firewall de Azure SQL Database para permitir que los servicios y recursos de Azure accedan al servidor.

Para habilitar esta opción en Azure Portal, haga clic en **Establecer el firewall del servidor** y, luego, haga clic en **Activado** en **Permitir que los servicios y recursos de Azure accedan a este servidor** para el servidor o la base de datos de Azure SQL Database. Para más información, consulte [Creación y administración de reglas de firewall de IP.](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#use-the-azure-portal-to-manage-server-level-ip-firewall-rules)

![Botón Establecer el firewall del servidor](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall1.png)

![Permitir el acceso a los servicios de Azure](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall2.png)

### <a name="select-a-custom-metastore-during-cluster-creation"></a>Selección de una tienda de metadatos personalizada durante la creación del clúster

Puede apuntar el clúster a una base de datos de Azure SQL aprovisionada durante la creación del clúster, o puede configurar la base de datos SQL una vez creado el clúster. Esta opción se especifica en **Almacenamiento > Tienda de metadatos** al crear un nuevo clúster de Hadoop, Spark o Hive interactivo desde Azure Portal.

![Repositorio de metadatos de Hive en HDInsight: Azure Portal](./media/hdinsight-use-external-metadata-stores/azure-portal-cluster-storage-metastore.png)

## <a name="hive-metastore-best-practices"></a>Procedimientos recomendados de Hive Metastore

Estos son algunos procedimientos recomendados para Hive Metastore en HDInsight:

* Use una tienda de metadatos personalizada siempre que sea posible para ayudarle a separar los recursos de proceso (el clúster de ejecución) y los metadatos (almacenados en la tienda de metadatos).

* Comience con un nivel S2, que proporciona 50 DTU y 250 GB de almacenamiento. Si percibe un cuello de botella, puede escalar la base de datos.

* Si tiene previsto que varios clústeres de HDInsight tengan acceso a datos separados, use una base de datos independiente para la tienda de metadatos en cada clúster. Si comparte una tienda de metadatos entre varios clústeres de HDInsight, entonces los clústeres utilizan los mismos metadatos y los mismos archivos de datos de usuario subyacentes.

* Realice una copia de la tienda de metadatos personalizada periódicamente. Azure SQL Database genera las copias de seguridad automáticamente, pero el período de tiempo de retención de dichas copias de seguridad varía. Para más información, consulte [más información sobre copias de seguridad automáticas de SQL Database](../sql-database/sql-database-automated-backups.md).

* Coloque la tienda de metadatos y el clúster de HDInsight en la misma región para obtener el mayor rendimiento y los cargos de salida de red más bajos.

* Supervise el rendimiento y la disponibilidad de su tienda de metadatos mediante herramientas de supervisión de Azure SQL Database, como Azure Portal o registros de Azure Monitor.

* Cuando se crea una nueva versión de Azure HDInsight en una base de datos de tienda de metadatos personalizada ya existente, el sistema actualiza el esquema de la tienda de metadatos; esta acción es irreversible sin tener que restaurar la base de datos de la copia de seguridad.

* Si comparte una misma tienda de metadatos entre varios clústeres, asegúrese de que todos ellos cuentan con la misma versión de HDInsight. Diferentes versiones de Hive utilizan diferentes esquemas de base de datos de tienda de metadatos. Por ejemplo, no puede compartir una metastore entre los clústeres de las versiones Hive 2.1 y 3.1.

* En HDInsight 4,0, Spark y Hive usan catálogos independientes para acceder a tablas de SparkSQL o Hive. Una tabla creada por Spark reside en el catálogo de Spark. Una tabla creada por Hive reside en el catálogo de Hive. Esto es diferente a HDInsight 3.6, donde Hive y Spark compartían el catálogo común. La integración de Hive y Spark en HDInsight 4.0 se basa en el conector de Hive Warehouse (HWC). HWC funciona como un puente entre Spark y Hive. [Más información sobre el conector de Hive Warehouse](../hdinsight/interactive-query/apache-hive-warehouse-connector.md).

## <a name="apache-oozie-metastore"></a>Apache Oozie Metastore

Apache Oozie es un sistema de coordinación de flujos de trabajo que administra trabajos de Hadoop.  Oozie es compatible con los trabajos de Hadoop para Apache MapReduce, Pig, Hive y otros.  Oozie utiliza una tienda de metadatos para almacenar los detalles acerca de los flujos de trabajo actuales y completados. Para aumentar el rendimiento al usar Oozie, puede utilizar una base de datos de Azure SQL como almacén de metadatos personalizado. La tienda de metadatos también puede proporcionar acceso a datos de trabajo de Oozie después de eliminar el clúster.

Para obtener instrucciones sobre cómo crear un Metastore de Oozie con Azure SQL Database, consulte este artículo sobre el [uso de Apache Oozie para flujos de trabajo](hdinsight-use-oozie-linux-mac.md).

## <a name="custom-ambari-db"></a>Base de datos de Ambari personalizada

Para usar su propia base de datos externa con Apache Ambari en HDInsight, consulte [Base de datos de Apache Ambari personalizada](hdinsight-custom-ambari-db.md).

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de clústeres en HDInsight con Apache Hadoop, Apache Spark, Apache Kafka, etc.](./hdinsight-hadoop-provision-linux-clusters.md)
