---
title: Introducción a HDInsight 4.0 (Azure)
description: Comparación entre las características, limitaciones y recomendaciones de actualización de HDInsight 3.6 y HDInsight 4.0.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/22/2019
ms.openlocfilehash: 0463e3297bbb2fda50adfeefaa89f0a7a3ef8b0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72901525"
---
# <a name="azure-hdinsight-40-overview"></a>Introducción a Azure HDInsight 4.0

Azure HDInsight es uno de los servicios más populares entre los clientes de empresa para el análisis de código abierto de Apache Hadoop y Apache Spark en Azure. HDInsight 4.0 es una distribución de nube de componentes de Apache Hadoop. Este artículo proporciona información sobre la última versión de Azure HDInsight y cómo actualizarla.

## <a name="whats-new-in-hdinsight-40"></a>Novedades de HDInsight 4.0

### <a name="apache-hive-30-and-llap"></a>Apache Hive 3.0 y LLAP

El procesamiento analítico de baja latencia (LLAP) de Apache Hive usa servidores de consultas persistentes y almacenamiento en memoria caché para ofrecer resultados rápidos de consultas de SQL realizadas en datos de un almacenamiento en la nube remoto. Este tipo de procesamiento de Hive aprovecha un conjunto de demonios persistentes que ejecutan fragmentos de consultas de Hive. La ejecución de consultas en LLAP es similar a la de Hive sin LLAP, con las tareas de trabajo ejecutándose en los demonios de LLAP en lugar de en contenedores.

Entre las ventajas del procesamiento LLAP de Hive se pueden citar las siguientes:

* Capacidad para realizar análisis profundos en SQL como combinaciones complejas, subconsultas, funciones basadas en ventanas, ordenación, funciones definidas por el usuario y agregaciones complejas, sin sacrificar en rendimiento y escalabilidad.

* Consultas interactivas en datos en el mismo almacenamiento en el que se preparan los datos, eliminando la necesidad de mover los datos de un almacenamiento a otro motor para el procesamiento analítico.

* El almacenamiento de resultados de consulta en la memoria caché permite que se puedan reutilizar los resultados procesados previamente, lo cual supone un ahorro en el tiempo y los recursos que se usan para ejecutar las tareas de clúster necesarias para la consulta.

### <a name="hive-dynamic-materialized-views"></a>Vistas materializadas dinámicas de Hive

Hive admite ahora vistas materializadas dinámicas, o procesamiento previo de los resúmenes correspondientes, que se usan para acelerar el procesamiento de las consultas en los almacenes de datos. Las vistas materializadas pueden almacenarse de forma nativa en Hive y pueden utilizar sin problemas la aceleración de LLAP.

### <a name="hive-transactional-tables"></a>Tablas transaccionales de Hive

HDI 4.0 incluye Apache Hive 3, lo que requiere el cumplimiento con la atomicidad, coherencia, aislamiento y durabilidad (ACID) de las tablas transaccionales que residen en el almacén de Hive. Se puede administrar y acceder a las tablas compatibles con ACID y a los datos de estas mediante Hive. Los datos de las tablas CRUD (crear, recuperar, actualizar y eliminar) deben tener el formato de archivo con columnas y filas optimizadas (ORC). Las tablas de solo inserción admiten todos los formatos de archivos.

* ACID v2 tiene mejoras de rendimiento en el formato de almacenamiento y el motor de ejecución.

* ÁCIDO está habilitado de forma predeterminada para permitir la compatibilidad total de las actualizaciones de datos.

* Las funcionalidades mejoradas de ACID le permiten actualizar y eliminar en el nivel de fila.

* No hay sobrecarga de rendimiento.

* La creación de depósitos no es necesaria.

* Spark puede leer y escribir en las tablas ACID de Hive mediante Hive Warehouse Connector.

Más información sobre [Apache Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/hive-overview/content/hive_whats_new_in_this_release_hive.html).

### <a name="apache-spark"></a>Spark de Apache

Apache Spark obtiene tablas actualizables y transacciones ACID con Hive Warehouse Connector. Hive Warehouse Connector le permite registrar las tablas transaccionales de Hive como tablas externas en Spark para acceder a una funcionalidad transaccional completa. Las versiones anteriores solo admitían la manipulación de particiones de tablas. Hive Warehouse Connector también admite tramas de datos de streaming para hacer streaming de lecturas y escrituras desde Spark a las tablas de Hive transaccionales y de streaming.

Los ejecutores de Spark se pueden conectar directamente a los demonios de LLAP de Hive para recuperar y actualizar datos de una forma transaccional, lo que permite a Hive mantener el control de los datos.

Apache Spark en HDInsight 4.0 admite los siguientes escenarios:

* Ejecute el aprendizaje del modelo de Machine Learning sobre la misma tabla transaccional usada para los informes.
* Use transacciones ACID para agregar de forma segura columnas del aprendizaje automático de Spark a una tabla de Hive.
* Ejecute un trabajo de streaming de Spark en la fuente de cambios desde una tabla de streaming de Hive.
* Cree archivos ORC directamente desde un trabajo de Spark Structured Streaming.

Ya no tiene que preocuparse por acceder accidentalmente a tablas transaccionales de Hive directamente desde Spark, lo cual provocaba resultados incoherentes y datos duplicados o dañados. En HDInsight 4.0, las tablas de Spark y Hive se mantienen en distintas instancias de Metastore. Use Hive Data Warehouse Connector para registrar explícitamente tablas transaccionales de Hive como tablas externas de Spark.

Más información sobre [Apache Spark](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/spark-overview/content/analyzing_data_with_apache_spark.html).

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie 4.3.1 se incluye en HDI 4.0 con los cambios siguientes:

* Oozie ya no ejecuta acciones de Hive. La CLI de Hive se ha eliminado y se ha reemplazado por BeeLine.

* Puede excluir las dependencias no deseadas de ShareLib incluyendo un patrón de exclusión en el archivo **job.properties**.

Más información sobre [Apache Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/patch_oozie.html).

## <a name="how-to-upgrade-to-hdinsight-40"></a>Actualización a HDInsight 4.0

Como en cualquier otra versión principal, es importante probar concienzudamente los componentes antes de implementar la versión más reciente en un entorno de producción. HDInsight 4.0 está disponible para comenzar el proceso de actualización, pero HDInsight 3.6 es la opción predeterminada para evitar contratiempos accidentales.

No hay ninguna ruta de actualización a HDInsight 4.0 que se admita desde versiones anteriores de HDInsight. Como han cambiado los formatos de metastore y de datos de blob, HDInsight 4.0 no es compatible con versiones anteriores. Es importante que mantenga su nuevo entorno de HDInsight 4.0 separado del entorno de producción actual. Si implementa HDInsight 4.0 en el entorno actual, metastore se actualizará y esta acción no se podrá deshacer.  

## <a name="limitations"></a>Limitaciones

* HDInsight 4.0 no admite MapReduce para Apache Hive. En su lugar, use Apache Tez. Más información sobre [Apache Tez](https://tez.apache.org/).
* HDInsight 4.0 no es compatible con Apache Storm.
* Hive View ya no está disponible en HDInsight 4.0.
* No se admite el intérprete de shell de Apache Zeppelin en clústeres de Spark e Interactive Query.
* No puede *deshabilitar* LLAP en un clúster LLAP de Spark. Solo puede desactivarlo.
* Azure Data Lake Storage Gen2 no puede guardar cuadernos de Jupyter en un clúster de Spark.

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de HDInsight de Azure](index.yml)
* [Notas de la versión](hdinsight-release-notes.md)
