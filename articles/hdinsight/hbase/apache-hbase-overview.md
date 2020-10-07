---
title: ¿Qué es Apache HBase in Azure HDInsight?
description: Una introducción a Apache HBase en HDInsight, una base de datos NoSQL basada en Hadoop. Obtenga información acerca de los casos de uso y compare HBase con otros clústeres de Hadoop.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: hdinsightactive,hdiseo17may2017,seoapr2020
ms.date: 04/20/2020
ms.openlocfilehash: afbf9aff09999a34a84d55634a868250fbb6d1ff
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "82188967"
---
# <a name="what-is-apache-hbase-in-azure-hdinsight"></a>¿Qué es Apache HBase in Azure HDInsight?

[Apache HBase](https://hbase.apache.org/) es una base de datos NoSQL de código abierto construida sobre Apache Hadoop y modelada según [Google BigTable](https://cloud.google.com/bigtable/). HBase proporciona acceso aleatorio y fuerte coherencia para grandes cantidades de datos en una base de datos sin esquemas. La base de datos está organizada por familias de columnas.

Desde la perspectiva del usuario, HBase es similar a una base de datos. Los datos se almacenan en las filas y las columnas de una tabla, mientras que los datos de una fila se agrupan por familia de columnas. HBase es una base de datos sin esquema. Las columnas y los tipos de datos pueden estar sin definir antes de usarlos. El código abierto se escala linealmente para controlar petabytes de datos en miles de nodos. Puede basarse en la redundancia de datos, el procesamiento por lotes y otras características proporcionadas por aplicaciones distribuidas en el entorno de Hadoop.

## <a name="how-is-apache-hbase-implemented-in-azure-hdinsight"></a>¿Cómo se implementa Apache HBase en Azure HDInsight?

HBase de HDInsight se ofrece como un clúster administrado que se integra en el entorno de Azure. Los clústeres están configurados para almacenar datos directamente en [Azure Storage](./../hdinsight-hadoop-use-blob-storage.md), que proporciona una baja latencia y una mayor elasticidad en las opciones de costo y rendimiento. Esta propiedad permite a los clientes crear sitios web interactivos que funcionan con grandes conjuntos de datos, crear servicios que almacenan datos de sensores y telemetría de millones de puntos de conexión y analizar estos datos con trabajos de Hadoop. HBase y Hadoop son buenos puntos de partida para proyectos de macrodatos de Azure. Los servicios pueden permitir que las aplicaciones en tiempo real trabajen con grandes conjuntos de datos.

La implementación de HDInsight usa la arquitectura de escalado horizontal de HBase para proporcionar el particionamiento automático de las tablas, una fuerte coherencia para lecturas y escrituras y la conmutación por error automática. Se mejora el rendimiento mediante el almacenamiento en caché de memoria para lecturas y streaming de alto rendimiento para escrituras. Se puede crear un clúster de HBase dentro de la red virtual. Para obtener detalles, consulte [Creación de clústeres de HDInsight en Azure Virtual Network](./apache-hbase-provision-vnet.md).

## <a name="how-is-data-managed-in-hdinsight-hbase"></a>¿Cómo se administran los datos en HBase de HDInsight?

En HBase, los datos pueden administrarse mediante los comandos `create`, `get`, `put` y `scan` del shell de HBase. Los datos se escriben en la base de datos mediante `put` y se leen con `get`. El comando de `scan` se usa para obtener datos de varias filas de una tabla. Los datos también se pueden administrar usando la API de C# de HBase, que proporciona una biblioteca de cliente en la parte superior de la API de REST de HBase. Asimismo, una base de datos de HBase se puede consultar usando [Apache Hive](https://hive.apache.org/). Para obtener una introducción a estos modelos de programación, consulte [Introducción a un ejemplo de Apache HBase con Apache Hadoop en HDInsight](./apache-hbase-tutorial-get-started-linux.md). También existen coprocesadores, que permiten el procesamiento de datos en los nodos que hospedan la base de datos.

> [!NOTE]  
> Thrift no es compatible con HBase en HDInsight.

## <a name="use-cases-for-apache-hbase"></a>casos de uso de Apache HBase

El caso de uso canónico para el que se creó BigTable y, por extensión, HBase, fue la búsqueda web. Los motores de búsqueda crean índices que asignan términos a las páginas web que los contienen. Sin embargo, hay muchos otros casos de uso a los que se adapta HBase, varios de los cuales se muestran en esta sección.

|Escenario |Descripción |
|---|---|
|almacén de pares clave-valor|HBase se puede usar como almacén de pares clave-valor y es adecuado para la administración de sistemas de mensajes. Facebook usa HBase para su sistema de mensajería y es ideal para almacenar y administrar comunicaciones de Internet. WebTable usa HBase para buscar y administrar tablas extraídas de páginas web.|
|datos del sensor|HBase es útil para capturar datos que se recopilan cada vez más a partir de diversas fuentes. Estos datos incluyen análisis social y series temporales. También lo es para mantener actualizados los paneles interactivos con tendencias y contadores, y para administrar los sistemas de registro de auditoría. Algunos ejemplos son el terminal del comerciante de Bloomberg y Open Time Series Database (OpenTSDB). OpenTSDB almacena y proporciona acceso a las métricas recopiladas sobre el estado de los sistemas de servidor.|
|consulta en tiempo real|[Apache Phoenix](https://phoenix.apache.org/) es un motor de consultas SQL para HBase Apache. Se obtiene acceso a él como controlador de JDBC y permite consultar y administrar tablas de HBase usando SQL.|
|HBase como plataforma|Las aplicaciones pueden ejecutarse en de HBase usándolo como almacén de datos. Entre los ejemplos se incluyen Phoenix, OpenTSDB, `Kiji` y Titan. Las aplicaciones también pueden integrarse con HBase. Algunos ejemplos son: [Apache Hive](https://hive.apache.org/), Apache Pig, [Solr](https://lucene.apache.org/solr/), Apache Storm, Apache Flume, [Apache Impala](https://impala.apache.org/), Apache Spark, `Ganglia` y Apache Drill.|

## <a name="next-steps"></a>Pasos siguientes

* [Introducción al uso de Apache HBase con Apache Hadoop en HDInsight](./apache-hbase-tutorial-get-started-linux.md)
* [Create HDInsight clusters on Azure Virtual Network](./apache-hbase-provision-vnet.md) (Creación de clústeres de HDInsight en Azure Virtual Network)
* [Configuración de la replicación de Apache HBase en HDInsight](apache-hbase-replication.md)
