---
title: 'Visualización de datos de Apache Hive con Power BI: Azure HDInsight'
description: Aprenda a usar Microsoft Power BI para visualizar datos de Hive procesados por Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/24/2020
ms.openlocfilehash: 973b8a49c4a849f5a50fb3ab72321746a5ca06a7
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86083481"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Visualización de los datos de Apache Hive con Microsoft Power BI mediante ODBC en Azure HDInsight

Aprenda a conectar Microsoft Power BI Desktop a Azure HDInsight mediante ODBC y a visualizar los datos de Apache Hive.

> [!IMPORTANT]
> Puede aprovechar el controlador ODBC de Hive para importar a través del conector ODBC genérico en Power BI Desktop. Sin embargo, no se recomienda para las cargas de trabajo de BI dada la naturaleza no interactiva del motor de consulta de Hive. El [conector de HDInsight Interactive Query](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) y el [conector de HDInsight Spark ](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) son mejores soluciones para su rendimiento.

En este artículo, cargaremos los datos de una tabla de Hive (`hivesampletable`) en Power BI. La tabla de Hive contiene algunos datos de uso de teléfonos móviles. Luego se trazan los datos de uso en un mapamundi:

![Informe de mapa de Power BI en HDInsight](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

La información también se aplica al nuevo tipo de clúster de [consulta interactiva](../interactive-query/apache-interactive-query-get-started.md). Para saber cómo conectarse a HDInsight Interactive Query mediante DirectQuery, consulte [Visualización de datos de Interactive Query Hive con Power BI mediante DirectQuery en Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).

## <a name="prerequisites"></a>Prerrequisitos

Antes de seguir los pasos de este artículo, debe disponer de los siguientes elementos:

* Clúster de HDInsight. El clúster puede ser un clúster de HDInsight con Hive o un clúster de consulta interactiva recién lanzado. Para crear clústeres, vea [Crear clúster](apache-hadoop-linux-tutorial-get-started.md).

* [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/). Puede descargar una copia desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Creación de un origen de datos de Hive ODBC

Vea [Creación de un origen de datos de Hive ODBC](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Carga de datos desde HDInsight

La tabla de Hive **hivesampletable** incluye todos los clústeres de HDInsight.

1. Inicie Power BI Desktop.

1. En el menú superior, vaya a **Inicio** > **Obtener datos** > **Más...**

    ![abrir datos de Power BI en Excel para HDInsight](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)

1. En el cuadro de diálogo **Obtener datos**, seleccione **Otro** a la izquierda, **ODBC** a la derecha y, luego, **Conectar** en la parte inferior.

1. En el cuadro de diálogo **Desde ODBC**, seleccione en la lista desplegable el nombre del origen de datos que creó en la sección anterior. Después, seleccione **Aceptar**.

1. Para el primer uso, se abrirá el cuadro de diálogo **Controlador ODBC**. Seleccione **Predeterminado o personalizado** en el menú de la izquierda. Seleccione **Conectar** para abrir el **Navegador**.

1. En el cuadro de diálogo **Navegador**, expanda **ODBC > HIVE > Predeterminado**, seleccione **hivesampletable** y, después, seleccione **Cargar**.

## <a name="visualize-data"></a>Visualización de datos

Continúe a partir del último procedimiento.

1. En el panel Visualizaciones, seleccione **Mapa**, que es un icono de globo.

    ![HDInsight Power BI personaliza el informe](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)

1. En el panel **Campos**, seleccione **country** y **devicemake**. Puede ver los datos trazados en el mapa.

1. Expanda el mapa.

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a visualizar datos de HDInsight mediante Power BI.  Para más información, vea los siguientes artículos:

* [Conexión de Excel a Hadoop en Azure HDInsight con Microsoft Hive ODBC Driver](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Conexión de Excel a Apache Hadoop con Power Query](apache-hadoop-connect-excel-power-query.md).
* [Visualización de datos de Interactive Query Apache Hive con Microsoft Power BI mediante consulta directa](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md)
