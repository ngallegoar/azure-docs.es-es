---
title: Conexión de Excel a Apache Hadoop con Power Query en Azure HDInsight
description: Vea cómo aprovechar las ventajas de los componentes de inteligencia empresarial y usar Power Query para Excel a fin de acceder a los datos almacenados en Hadoop en HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 12/17/2019
ms.openlocfilehash: acb5171048b5a12310b6c0af5d652b7e615a71f5
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087816"
---
# <a name="connect-excel-to-apache-hadoop-by-using-power-query"></a>Conexión de Excel a Apache Hadoop con Power Query

Una de las características clave de la solución para Big Data de Microsoft es la integración de los componentes de Microsoft Business Intelligence (BI) con los clústeres de Apache Hadoop en Azure HDInsight. Un ejemplo importante es la capacidad de conectar Excel a la cuenta de Azure Storage que contiene los datos asociados a su clúster de Hadoop mediante el complemento de Microsoft Power Query para Excel. En este artículo se describen las pautas para configurar y usar Power Query para consultar los datos asociados con un clúster de Hadoop administrado con HDInsight.

## <a name="prerequisites"></a>Prerrequisitos

* Un clúster de Apache Hadoop en HDInsight. Consulte [Introducción a HDInsight en Linux](./apache-hadoop-linux-tutorial-get-started.md).
* Una estación de trabajo que ejecute Windows 10, 7, Windows Server 2008 R2 o un sistema operativo posterior.
* Office 2016, Office Profesional Plus 2013, Office 365 ProPlus, Excel 2013 Standalone u Office Professional Plus 2010.

## <a name="install-microsoft-power-query"></a>Instalar Microsoft Power Query

Power Query puede importar datos ofrecidos o generados por un trabajo de Hadoop ejecutado en un clúster de HDInsight.

En Excel 2016, Power Query se ha integrado en la cinta de Datos en la sección Obtener y transformar. Para versiones anteriores de Excel, descargue Microsoft Power Query para Excel desde el [Centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?LinkID=286689) e instálelo.

## <a name="import-hdinsight-data-into-excel"></a>Importación de datos de HDInsight a Excel

El complemento de Power Query para Excel facilita la importación de datos desde el clúster de HDInsight hasta Excel, donde se pueden usar herramientas de BI como PowerPivot y Power Map para la inspección, el análisis y la presentación de los datos.

1. Inicie Excel.

1. Cree un libro vacío.

1. Siga los pasos siguientes en función de la versión de Excel:

   * Excel 2016

     * Seleccione > **Datos** > **Obtener datos** > **Desde Azure** > **Desde Azure HDInsight (HDFS)** .

       ![HDI.PowerQuery.SelectHdiSource.2016](./media/apache-hadoop-connect-excel-power-query/powerquery-selecthdisource-excel2016.png)

   * Excel 2013/2010

     * Seleccione **Power Query** > **Desde Azure** > **Desde Microsoft Azure HDInsight**.

       ![HDI.PowerQuery.SelectHdiSource](./media/apache-hadoop-connect-excel-power-query/powerquery-selecthdisource.png)

       **Nota:** Si no ve el menú **Power Query**, vaya a **Archivo** > **Opciones** > **Complementos** y seleccione **Complementos COM** en la lista desplegable **Administrar** situada en la parte inferior de la página. Elija el botón **Go...** y compruebe que la casilla del complemento de Power Query para Excel esté activada.

       **Nota:** Power Query también permite importar datos de HDFS mediante la opción **Desde otros orígenes**.

1. En el cuadro de diálogo **Azure HDInsight (HDFS)** , en el cuadro de texto **URL o nombre de cuenta**, escriba el nombre de la cuenta de Azure Blob Storage asociada con el clúster. Después, seleccione **Aceptar**. Esta puede ser una cuenta de almacenamiento predeterminada o una cuenta de almacenamiento vinculada.  El formato es `https://StorageAccountName.blob.core.windows.net/`.

1. En **Clave de cuenta**, escriba la clave de la cuenta de Blob Storage y, a continuación, seleccione **Conectar**. (Solo tiene que escribir la información de la cuenta la primera vez que tenga acceso a este almacén).

1. En el panel del **navegador** situado a la izquierda del Editor de consultas, haga doble clic en el nombre del contenedor de Blob Storage asociado al clúster. De forma predeterminada, el nombre del contenedor es el mismo que el del clúster.

1. Localice el archivo **HiveSampleData.txt** en la columna **Nombre** (la ruta de acceso de la carpeta es **../hive/warehouse/hivesampletable/** ) y, a continuación, seleccione **Binario** a la izquierda de HiveSampleData.txt. HiveSampleData.txt incluye todo el clúster. Opcionalmente, puede utilizar su propio archivo.

    ![Datos de importación de Power Query de Excel para HDI](./media/apache-hadoop-connect-excel-power-query/powerquery-importdata.png)

1. Si quiere, puede cambiar el nombre de las columnas. Cuando esté listo, elija **Cerrar y cargar**.  Los datos se han cargado en el libro:

    ![Tabla importada de Power Query de Excel para HDI](./media/apache-hadoop-connect-excel-power-query/powerquery-importedtable.png)

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a usar Power Query para recuperar datos de HDInsight en Excel. Del mismo modo, puede recuperar datos de HDInsight en Azure SQL Database. También se pueden cargar los datos en HDInsight. Para más información, vea los siguientes artículos:

* [Visualización de datos de Apache Hive con Microsoft Power BI en Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Visualización de datos de Interactive Query Hive con Power BI en Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Conexión de Excel a Hadoop en Azure HDInsight con Microsoft Hive ODBC Driver](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Uso de Herramientas de Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carga de datos en HDInsight](./../hdinsight-upload-data.md).
