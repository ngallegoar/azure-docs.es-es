---
title: Optimización de Apache Pig con Apache Ambari en Azure HDInsight
description: Use la interfaz de usuario web de Apache Ambari para configurar y optimizar Apache Pig.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 05/04/2020
ms.openlocfilehash: d6f20780ccd90d83631ce07411820fb8c9280c3e
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86084093"
---
# <a name="optimize-apache-pig-with-apache-ambari-in-azure-hdinsight"></a>Optimización de Apache Pig con Apache Ambari en Azure HDInsight

Apache Ambari es una interfaz web para administrar y supervisar clústeres de HDInsight. Para más información sobre el uso de la interfaz de usuario web de Ambari, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md).

Las propiedades de Apache Pig se pueden modificar desde la interfaz de usuario web de Ambari para optimizar las consultas de Pig. La modificación de las propiedades de Pig desde Ambari directamente modifica las propiedades de Pig en el archivo `/etc/pig/2.4.2.0-258.0/pig.properties`.

1. Para modificar las propiedades de Pig, navegue hasta la pestaña **Configs** (Configuraciones) de Pig y, a continuación, expanda el panel **Advanced pig-properties** (Propiedades de Pig avanzadas).

1. Busque, quite la marca de comentario y cambie el valor de la propiedad que quiera modificar.

1. Seleccione **Guardar** en la parte superior derecha de la ventana para guardar el nuevo valor. Algunas propiedades pueden requerir un reinicio del servicio.

    ![Propiedades avanzadas de Apache Pig](./media/optimize-pig-ambari/advanced-pig-properties.png)

> [!NOTE]  
> Cualquier configuración de nivel de sesión reemplaza los valores de propiedad en el archivo `pig.properties`.

## <a name="tune-execution-engine"></a>Optimizar el motor de ejecución

Existen dos motores de ejecución disponibles para ejecutar scripts de Pig: MapReduce y Tez. Tez es un motor optimizado y es mucho más rápido que MapReduce.

1. Para modificar el motor de ejecución, en el panel **Advanced pig-properties** (Propiedades de Pig avanzadas), busque la propiedad `exectype`.

1. El valor predeterminado es **MapReduce**. Cámbielo a **Tez**.

## <a name="enable-local-mode"></a>Habilitar el modo local

De manera similar a Hive, el modo local se usa para acelerar los trabajos con cantidades de datos relativamente más pequeñas.

1. Para habilitar el modo local, establezca `pig.auto.local.enabled` en **true**. El valor predeterminado es false.

1. Los trabajos con un tamaño de datos de entrada inferior al valor de propiedad `pig.auto.local.input.maxbytes` se consideran trabajos pequeños. El valor predeterminado es 1 GB.

## <a name="copy-user-jar-cache"></a>Copiar la caché de archivos JAR usuario

Pig copia los archivos JAR que requieren los formatos UDF en una caché distribuida para estar disponibles para los nodos de tarea. Estos archivos JAR no cambian con frecuencia. Si está habilitado, el valor `pig.user.cache.enabled` permite colocar archivos JAR en una caché para volver a usarlos para trabajos ejecutados por el mismo usuario. Este valor da como resultado un aumento menor del rendimiento de trabajo.

1. Para habilitarla, establezca `pig.user.cache.enabled` en true. El valor predeterminado es false.

1. Para establecer la ruta de acceso base de los archivos JAR en caché, establezca `pig.user.cache.location` en la ruta de acceso base. El valor predeterminado es `/tmp`.

## <a name="optimize-performance-with-memory-settings"></a>Optimizar el rendimiento con la configuración de memoria

La configuración de memoria siguiente puede ayudar a optimizar el rendimiento de los scripts de Pig.

* `pig.cachedbag.memusage`: Cantidad máxima de memoria que se asigna a un contenedor. Un contenedor es una colección de tuplas. Una tupla es un conjunto ordenado de campos y un campo es un fragmento de datos. Si los datos de un contenedor están fuera de la memoria dada, se vuelcan en el disco. El valor predeterminado es 0,2, que representa el 20 por ciento de memoria disponible. Esta memoria se comparte entre todos los contenedores en una aplicación.

* `pig.spill.size.threshold`: los contenedores que superan este umbral de tamaño de volcado (en bytes) se vuelcan en el disco. El valor predeterminado es de 5 MB.

## <a name="compress-temporary-files"></a>Comprimir archivos temporales

Pig genera archivos temporales durante la ejecución del trabajo. Comprimir los archivos temporales da como resultado un aumento del rendimiento al leer o escribir archivos en el disco. La configuración siguiente se puede usar para comprimir archivos temporales.

* `pig.tmpfilecompression`: si es true, habilita la compresión de archivos temporales. El valor predeterminado es false.

* `pig.tmpfilecompression.codec`: códec de compresión que se usará para comprimir archivos temporales. Los códecs de compresión recomendados son LZO y Snappy para usos menores de la CPU.

## <a name="enable-split-combining"></a>Habilitar la combinación de divisiones

Si se habilita esta opción, los archivos pequeños se combinan para menos tareas de asignación. Este valor mejora la eficiencia de los trabajos con muchos archivos pequeños. Para habilitarla, establezca `pig.noSplitCombination` en true. El valor predeterminado es false.

## <a name="tune-mappers"></a>Optimizar asignadores

El número de asignadores se controla mediante la modificación de la propiedad `pig.maxCombinedSplitSize`. Esta propiedad especifica el tamaño de los datos que se van a procesar mediante una tarea de asignación única. El valor predeterminado es el tamaño de bloque predeterminado del sistema de archivos. Aumentar este valor produce una disminución del número de tareas del asignador.

## <a name="tune-reducers"></a>Optimizar reductores

El número de reductores se calcula en función del parámetro `pig.exec.reducers.bytes.per.reducer`. El parámetro especifica el número de bytes procesados por reductor, que de manera predeterminada es 1 GB. Para limitar el número máximo de reductores, establezca la propiedad `pig.exec.reducers.max`, cuyo valor predeterminado es 999.

## <a name="next-steps"></a>Pasos siguientes

* [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [API REST de Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Optimización de clústeres](./hdinsight-changing-configs-via-ambari.md)
* [Optimización de Apache HBase](./optimize-hbase-ambari.md)
* [Optimización de Apache Hive](./optimize-hive-ambari.md)
