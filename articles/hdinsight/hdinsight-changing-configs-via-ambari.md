---
title: Optimización de clústeres con Apache Ambari en Azure HDInsight
description: Use la interfaz de usuario web de Apache Ambari para configurar y optimizar clústeres de Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 05/04/2020
ms.openlocfilehash: 52eb1b6c89ff02cb44fe731c2463ab02c284f26c
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086456"
---
# <a name="optimize-clusters-with-apache-ambari-in-azure-hdinsight"></a>Optimización de clústeres con Apache Ambari en Azure HDInsight

HDInsight proporciona clústeres de Apache Hadoop para aplicaciones de procesamiento de datos a gran escala. La administración, la supervisión y la optimización de estos clústeres complejos de varios nodos pueden ser un desafío. Apache Ambari es una interfaz web para administrar y supervisar clústeres de HDInsight Linux.

Para una introducción al uso de la interfaz de usuario web de Ambari, consulte [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md).

Inicie sesión en Ambari en `https://CLUSTERNAME.azurehdidnsight.net` con las credenciales del clúster. La pantalla inicial muestra un panel de información general.

![Panel del usuario de Apache Ambari](./media/hdinsight-changing-configs-via-ambari/apache-ambari-dashboard.png)

La interfaz de usuario web de Ambari se usa para administrar hosts, servicios, alertas, configuraciones y vistas. Ambari no se puede usar para crear un clúster de HDInsight o actualizar los servicios. Tampoco puede administrar pilas y versiones, retirar o dar de alta hosts o agregar servicios al clúster.

## <a name="manage-your-clusters-configuration"></a>Administrar la configuración del clúster

Las opciones de configuración ayudan a optimizar un servicio determinado. Para modificar los valores de configuración de un servicio, seleccione el servicio en la barra lateral de **Servicios** (a la izquierda). A continuación, vaya a la pestaña **Configs** (Configuraciones) de la página de detalles del servicio.

![Barra lateral de servicios de Apache Ambari](./media/hdinsight-changing-configs-via-ambari/ambari-services-sidebar.png)

## <a name="modify-namenode-java-heap-size"></a>Modificar el tamaño del montón de Java NameNode

El tamaño del montón de Java NameNode depende de muchos factores tales como la carga en el clúster. También del número de archivos y el número de bloques. El tamaño predeterminado de 1 GB funciona bien con la mayoría de los clústeres, aunque algunas cargas de trabajo pueden requerir más o menos memoria.

Para modificar el tamaño del montón de Java NameNode:

1. Seleccione **HDFS** de la barra lateral Servicios y navegue hasta la pestaña **Configs** (Configuraciones).

    ![Configuración HDFS de Apache Ambari](./media/hdinsight-changing-configs-via-ambari/ambari-apache-hdfs-config.png)

1. Busque el valor **NameNode Java heap size** (Tamaño del montón de Java NameNode). También puede usar el cuadro de texto de **filtro** para escribir y buscar un valor determinado. Seleccione el icono de **lápiz** junto al nombre del valor.

    ![Tamaño del montón de Java NameNode de Apache Ambari](./media/hdinsight-changing-configs-via-ambari/ambari-java-heap-size.png)

1. Escriba el nuevo valor en el cuadro de texto y, a continuación, presione **ENTRAR** para guardar el cambio.

    ![Editar el tamaño del montón de Java NameNode 1 de Ambari](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edit1.png)

1. El tamaño del montón de Java NameNode ha cambiado de 1 GB a 2 GB.

    ![Tamaño del montón de Java NameNode editado 2](./media/hdinsight-changing-configs-via-ambari/java-heap-size-edited.png)

1. Para guardar los cambios, haga clic en el botón **Guardar** verde de la parte superior de la pantalla de configuración.

    !["Guardado de configuraciones de Apache Ambari"](./media/hdinsight-changing-configs-via-ambari/ambari-save-changes1.png)

## <a name="next-steps"></a>Pasos siguientes

* [Administración de clústeres de HDInsight con la interfaz de usuario web de Apache Ambari](hdinsight-hadoop-manage-ambari.md)
* [API REST de Apache Ambari](hdinsight-hadoop-manage-ambari-rest-api.md)
* [Optimización de Apache HBase](./optimize-hbase-ambari.md)
* [Optimización de Apache Hive](./optimize-hive-ambari.md)
* [Optimización de Apache Pig](./optimize-pig-ambari.md)
