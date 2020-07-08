---
title: Optimización de Apache HBase con Apache Ambari en Azure HDInsight
description: Use la interfaz de usuario web de Apache Ambari para configurar y optimizar Apache HBase.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.date: 05/04/2020
ms.openlocfilehash: d143c9648f84dd0c8b45122cf2271539a0b9d1cf
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086337"
---
# <a name="optimize-apache-hbase-with-apache-ambari-in-azure-hdinsight"></a>Optimización de Apache HBase con Apache Ambari en Azure HDInsight

Apache Ambari es una interfaz web para administrar y supervisar clústeres de HDInsight. Para más información sobre el uso de la interfaz de usuario web de Ambari, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md).

La configuración de Apache HBase se modifica desde la pestaña **HBase Configs** (Configuraciones de HBase). En las secciones siguientes se describen algunas de las opciones de configuración importantes que afectan al rendimiento de HBase.

## <a name="set-hbase_heapsize"></a>Establecer HBASE_HEAPSIZE

El tamaño del montón de HBase especifica la cantidad máxima del montón que se usará en megabytes en los servidores de *regiones* y *maestro*. El valor predeterminado es de 1000 MB. Este valor debe optimizarse para la carga de trabajo del clúster.

1. Para modificarlo, navegue hasta el panel **Advanced HBase-env** (HBase-env avanzado) en la pestaña **Configs** (Configuraciones) de HBase y, a continuación, busque el valor `HBASE_HEAPSIZE`.

1. Cambie el valor predeterminado a 5000 MB.

    !["Tamaño de montón de la memoria de Apache Ambari HBase"](./media/optimize-hbase-ambari/ambari-hbase-heapsize.png)

## <a name="optimize-read-heavy-workloads"></a>Optimizar las cargas de trabajo con mucha actividad de lectura

Las siguientes configuraciones son importantes para mejorar el rendimiento de las cargas de trabajo con mucha actividad de lectura.

### <a name="block-cache-size"></a>Tamaño de la caché de bloques

La caché de bloques es la caché de lectura. Su tamaño lo controla el parámetro `hfile.block.cache.size`. El valor predeterminado es 0,4, que es un 40 por ciento de la memoria total del servidor de regiones. Cuanto mayor sea el tamaño de la caché de bloques, más rápida serán las lecturas aleatorias.

1. Para modificar este parámetro, navegue hasta la pestaña **Configuración** de la pestaña **Configs** (Configuraciones) de HBase y, a continuación, busque **% of RegionServer Allocated to Read Buffers** (% de RegionServer asignado a los búferes de lectura).

    ![Apache HBase: tamaño de la caché de bloques de memoria](./media/optimize-hbase-ambari/hbase-block-cache-size.png)

1. Para cambiar el valor, seleccione el icono **Editar**.

### <a name="memstore-size"></a>Tamaño de Memstore

Todas las ediciones se almacenan en el búfer de memoria, denominado *Memstore*. Este búfer aumenta la cantidad total de datos que se pueden escribir en el disco en una sola operación. También acelera el acceso a las ediciones recientes. El tamaño de Memstore se define mediante los dos parámetros siguientes:

* `hbase.regionserver.global.memstore.UpperLimit`: define el porcentaje máximo del servidor de regiones que puede usar Memstore combinado.

* `hbase.regionserver.global.memstore.LowerLimit`: define el porcentaje mínimo del servidor de regiones que puede usar Memstore combinado.

Para optimizarlo para las lecturas aleatorias, puede reducir los límites superior e inferior de Memstore.

### <a name="number-of-rows-fetched-when-scanning-from-disk"></a>Número de filas recuperadas del examen del disco

El parámetro `hbase.client.scanner.caching` define el número de filas leídas del disco cuando se llama al método `next` en un escáner.  El valor predeterminado es 100. Cuanto mayor sea el número, menos llamadas remotas se realizan del cliente al servidor de regiones, lo que permite exámenes más rápidos. Sin embargo, este valor también aumentará la presión de la memoria en el cliente.

![Apache HBase: número de filas recuperadas](./media/optimize-hbase-ambari/hbase-num-rows-fetched.png)

> [!IMPORTANT]  
> No establezca el valor de modo que el tiempo transcurrido entre la invocación del método siguiente en un escáner sea mayor que el tiempo de espera del escáner. La duración del tiempo de espera del escáner se define mediante la propiedad `hbase.regionserver.lease.period`.

## <a name="optimize-write-heavy-workloads"></a>Optimizar las cargas de trabajo con mucha actividad de escritura

Las siguientes configuraciones son importantes para mejorar el rendimiento de las cargas de trabajo con mucha actividad de escritura.

### <a name="maximum-region-file-size"></a>Tamaño de archivo de región máximo

HBase almacena los datos en un formato de archivo interno denominado *HFile*. La propiedad `hbase.hregion.max.filesize` define el tamaño de un solo archivo HFile para una región.  Una región se divide en dos regiones si la suma de todos los archivos HFile de una región es mayor que este valor.

!["Tamaño de archivo máximo de HRegion de Apache HBase"](./media/optimize-hbase-ambari/hbase-hregion-max-filesize.png)

Cuanto mayor sea el tamaño del archivo de región, menor será el número de divisiones. Puede aumentar el tamaño de archivo para determinar un valor que produzca el máximo rendimiento de escritura.

### <a name="avoid-update-blocking"></a>Evitar el bloqueo de actualizaciones

* La propiedad `hbase.hregion.memstore.flush.size` define el tamaño en el que se va a vaciar Memstore en el disco. El tamaño predeterminado es de 128 MB.

* El multiplicador de bloques de región de HBase se define mediante `hbase.hregion.memstore.block.multiplier`. El valor predeterminado es 4. El máximo permitido es 8 GB.

* HBase bloquea las actualizaciones si Memstore es (`hbase.hregion.memstore.flush.size` * `hbase.hregion.memstore.block.multiplier`) bytes.

    Con los valores predeterminados de tamaño de vaciado y multiplicador de bloque, las actualizaciones se bloquean cuando Memstore tiene un tamaño de 128 * 4 = 512 MB. Para reducir la actualización de recuento de bloqueo, aumente el valor de `hbase.hregion.memstore.block.multiplier`.

![Multiplicador de bloques de regiones de Apache HBase](./media/optimize-hbase-ambari/hbase-hregion-memstore-block-multiplier.png)

## <a name="define-memstore-size"></a>Definir el tamaño de Memstore

El tamaño de Memstore se define mediante los parámetros `hbase.regionserver.global.memstore.UpperLimit` y `hbase.regionserver.global.memstore.LowerLimit`. Establecer estos valores igual reduce las pausas al escribir (lo que también provoca un vaciado más frecuentes) y da como resultado un rendimiento de escritura mayor.

## <a name="set-memstore-local-allocation-buffer"></a>Establecer el búfer de asignación local de Memstore

El uso del búfer de asignación local de Memstore viene determinado por la propiedad `hbase.hregion.memstore.mslab.enabled`. Cuando está habilitado (true), este valor impide la fragmentación del montón durante la operación de escritura intensiva. El valor predeterminado es true.

![hbase.hregion.memstore.mslab.enabled](./media/optimize-hbase-ambari/hbase-hregion-memstore-mslab-enabled.png)

## <a name="next-steps"></a>Pasos siguientes

* [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [API REST de Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Optimización de clústeres](./hdinsight-changing-configs-via-ambari.md)
* [Optimización de Apache Hive](./optimize-hive-ambari.md)
* [Optimización de Apache Pig](./optimize-pig-ambari.md)
