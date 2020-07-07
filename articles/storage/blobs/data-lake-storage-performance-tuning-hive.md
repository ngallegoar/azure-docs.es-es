---
title: 'Ajustar rendimiento: Hive, HDInsight y Azure Data Lake Storage Gen2 | Microsoft Docs'
description: Guía para la optimización del rendimiento de Hive en Azure Data Lake Storage Gen2
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 9a54565f320ae45a4a8297a40027c5e6b3b25202
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84465973"
---
# <a name="tune-performance-hive-hdinsight--azure-data-lake-storage-gen2"></a>Ajustar rendimiento: Hive, HDInsight y Azure Data Lake Storage Gen2

La configuración predeterminada se ha establecido para proporcionar un buen rendimiento en muchos casos de uso diferentes.  Para las consultas de uso intensivo de E/S, Hive se puede optimizar para obtener un mejor rendimiento con Azure Data Lake Storage Gen2.  

## <a name="prerequisites"></a>Prerrequisitos

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
* **Una cuenta de Data Lake Storage Gen2**. Para obtener instrucciones sobre cómo crear una, consulte [Guía de inicio rápido: Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2](data-lake-storage-quickstart-create-account.md)
* **Clúster de Azure HDInsight** con acceso a una cuenta de Data Lake Storage Gen2. Consulte [Use Azure Data Lake Storage Gen2 with Azure HDInsight clusters](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2) (Uso de Azure Data Lake Storage Gen2 con clústeres de Azure HDInsight)
* **Ejecución de Hive en HDInsight**.  Para obtener información sobre cómo ejecutar trabajos de Hive en HDInsight, vea [Uso de Hive en HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive).
* **Guía para la optimización del rendimiento en Data Lake Storage Gen2**.  Para conocer los conceptos generales sobre rendimiento, consulte [Guía para la optimización del rendimiento en Data Lake Storage Gen2](data-lake-storage-performance-tuning-guidance.md).

## <a name="parameters"></a>Parámetros

Estos son los parámetros de configuración más importantes que se deben ajustar para mejorar el rendimiento de Data Lake Storage Gen2:

* **hive.tez.container.size**: la cantidad de memoria usada en cada tarea.

* **tez.grouping.min-size**: tamaño mínimo de cada asignador.

* **tez.grouping.min-size**: tamaño mínimo de cada asignador.

* **hive.exec.reducer.bytes.per.reducer**: tamaño de cada reductor.

**hive.tez.container.size**: el tamaño del contenedor determina cuánta memoria está disponible para cada tarea.  Esta es la entrada principal para controlar la simultaneidad en Hive.  

**tez.grouping.min-size**: este parámetro permite establecer el tamaño mínimo de cada asignador.  Si el número de asignadores que Tez elige es inferior al valor de este parámetro, Tez usará el valor establecido aquí.

**tez.grouping.max-size**: el parámetro le permite establecer el tamaño máximo de cada asignador.  Si el número de asignadores que elige Tez es superior al valor de este parámetro, Tez usará entonces el valor establecido aquí.

**hive.exec.reducer.bytes.per.reducer**: este parámetro establece el tamaño de cada reductor.  De forma predeterminada, cada reductor tiene 256 MB.  

## <a name="guidance"></a>Guía

**Set hive.exec.reducer.bytes.per.reducer**: el valor predeterminado funciona bien cuando los datos están descomprimidos.  En el caso de los datos que están comprimidos, debe reducir el tamaño del reductor.  

**Set hive.tez.container.size**: en cada nodo, la memoria se especifica mediante yarn.nodemanager.resource.memory-mb y se debe establecer correctamente en el clúster de HDI de forma predeterminada.  Para más información sobre la configuración de la memoria adecuada en YARN, consulte esta [entrada](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

Las cargas de trabajo que hacen un uso intensivo de E/S pueden beneficiarse de un mayor paralelismo disminuyendo el tamaño del contenedor de Tez. Con ello el usuario tiene más contenedores, lo que aumenta la simultaneidad.  Sin embargo, algunas consultas de Hive requieren una cantidad significativa de memoria (p. ej. MapJoin).  Si la tarea no tiene suficiente memoria, obtendrá una excepción de memoria insuficiente durante el tiempo de ejecución.  Si recibe excepciones de memoria insuficiente, debe aumentar la memoria.   

El número de tareas simultáneas en ejecución o con paralelismo estará enlazado por la cantidad total de memoria YARN.  El número de contenedores YARN determina cuántas tareas simultáneas se pueden ejecutar.  Para buscar la memoria YARN por nodo, puede ir a Ambari.  Vaya a YARN y vea la pestaña Configs (Configuraciones).  En esta ventana se muestra el tamaño de la memoria de YARN.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
La clave para mejorar el rendimiento mediante Data Lake Storage Gen2 es aumentar la simultaneidad tanto como sea posible.  Tez calcula automáticamente el número de tareas que se deben crear por lo que no es necesario establecerlo.   

## <a name="example-calculation"></a>Cálculo de ejemplo

Supongamos que tiene un clúster D14 de 8 nodos.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="further-information-on-hive-tuning"></a>Información adicional sobre la optimización de Hive

Estos son algunos blogs que le ayudará a optimizar las consultas de Hive:
* [Optimizar consultas de Hive para Hadoop en HDInsight](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Troubleshooting Hive query performance](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/) (Solución de problemas de rendimiento de consultas de Hive)
* [Ignite talk on optimize Hive on HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25) (Acalorada charla sobre la optimización de Hive en HDInsight)
