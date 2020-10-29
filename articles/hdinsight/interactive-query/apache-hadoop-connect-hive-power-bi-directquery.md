---
title: Visualización de datos de Interactive Query Hive con Power BI en Azure HDInsight
description: Uso de Microsoft Power BI para visualizar datos de Interactive Query Hive desde Azure HDInsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: how-to
ms.date: 06/17/2019
ms.openlocfilehash: 92e94c911acb701b1ccf8e39636d152cc5bfb575
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92534811"
---
# <a name="visualize-interactive-query-apache-hive-data-with-microsoft-power-bi-using-direct-query-in-hdinsight"></a>Visualización de datos de Interactive Query Apache Hive con Microsoft Power BI mediante consulta directa en HDInsight

En este artículo se describe cómo conectar Microsoft Power BI a clústeres de Interactive Query de Azure HDInsight y cómo visualizar los datos de Apache Hive mediante DirectQuery. En el ejemplo proporcionado se cargan los datos de una tabla de Hive `hivesampletable` en Power BI. La tabla de Hive `hivesampletable` contiene algunos datos de uso de teléfonos móviles. Luego se trazan los datos de uso en un mapamundi:

![Informe de mapa de Power BI en HDInsight](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

Puede aprovechar el [controlador ODBC de Apache Hive](../hadoop/apache-hadoop-connect-hive-power-bi.md) para realizar la importación a través del conector ODBC genérico en Power BI Desktop. Sin embargo, no se recomienda para las cargas de trabajo de BI dada la naturaleza no interactiva del motor de consulta de Hive. El [conector de HDInsight Interactive Query](./apache-hadoop-connect-hive-power-bi-directquery.md) y el [conector de HDInsight Apache Spark](/power-bi/spark-on-hdinsight-with-direct-connect) son las mejores opciones para su rendimiento.

## <a name="prerequisites"></a>Prerequisites
Antes de seguir los pasos de este artículo, debe disponer de los siguientes elementos:

* **Clúster de HDInsight** . El clúster puede ser un clúster de HDInsight con Apache Hive o un clúster de Interactive Query recién lanzado. Para crear clústeres, vea [Crear clúster](../hadoop/apache-hadoop-linux-tutorial-get-started.md).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)** . Puede descargar una copia desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Carga de datos desde HDInsight

La tabla de Hive `hivesampletable` incluye todos los clústeres de HDInsight.

1. Inicie Power BI Desktop.

2. En la barra de menús, vaya a **Inicio** > **Obtener datos** > **Más...** .

    ![Power BI en HDInsight: obtener más datos](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)

3. Desde la ventana **Obtener datos** , escriba **hdinsight** en el cuadro de búsqueda.  

4. En los resultados de la búsqueda, seleccione **HDInsight Interactive Query** y, después, **Conectar** .  Si no ve **HDInsight Interactive Query** , tiene que actualizar Power BI Desktop a la versión más reciente.

5. Seleccione **Continuar** para cerrar el cuadro de diálogo **Conectándose a un servicio externo** .

6. En la ventana **HDInsight Interactive Query** , escriba la información siguiente y luego seleccione **Aceptar** :

    |Propiedad | Value |
    |---|---|
    |Server |Escriba el nombre de clúster, por ejemplo,  *myiqcluster.azurehdinsight.net* .|
    |Base de datos |Escriba **Predeterminada** para este artículo.|
    |Modo Conectividad de datos |Seleccione **DirectQuery** para este artículo.|

    ![Conexión de HDInsight Interactive Query y Power BI con DirectQuery](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. Escriba las credenciales HTTP y, a continuación, seleccione **Conectar** . El nombre de usuario predeterminado es **admin** .

8. En el panel izquierdo, en la ventana del **Navegador** seleccione **hivesampletale** .

9. Seleccione **Carga** desde la ventana principal.

    ![HDInsight Interactive Query y Power BI con hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>Visualización de datos en un mapa

Continúe a partir del último procedimiento.

1. En el panel Visualizaciones, seleccione **Mapa** , el icono del globo. Aparece un mapa genérico en la ventana principal.

    ![HDInsight Power BI personaliza el informe](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)

2. En el panel Campos, seleccione **country** y **devicemake** . Transcurridos unos instantes, un mapa del mundo con los puntos de datos aparece en la ventana principal.

3. Expanda el mapa.

## <a name="next-steps"></a>Pasos siguientes
En este artículo ha aprendido a visualizar datos de HDInsight mediante Microsoft Power BI.  Para más información sobre la visualización de datos, vea los siguientes artículos:

* [Visualización de los datos de Apache Hive con Microsoft Power BI mediante ODBC en Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Conexión de Excel a Hadoop en Azure HDInsight con Microsoft Hive ODBC Driver](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Conexión de Excel a Apache Hadoop con Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Uso de Herramientas de Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carga de datos en HDInsight](./../hdinsight-upload-data.md).