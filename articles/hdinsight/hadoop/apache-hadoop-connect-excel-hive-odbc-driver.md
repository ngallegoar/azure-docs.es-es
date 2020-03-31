---
title: 'Excel y Apache Hadoop con el controlador ODBC: Azure HDInsight'
description: Aprenda a configurar y usar el controlador ODBC de Microsoft Hive para que Excel consulte datos en un clúster de HDInsight desde Microsoft Excel
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 03/02/2020
ms.openlocfilehash: f356009197c0446efa2ea2d7f0e90040229df47b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78251069"
---
# <a name="connect-excel-to-apache-hadoop-in-azure-hdinsight-with-the-microsoft-hive-odbc-driver"></a>Conexión de Excel a Apache Hadoop en Azure HDInsight con el controlador ODBC de Microsoft Hive

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

La solución de macrodatos de Microsoft integra componentes de inteligencia empresarial (BI) de Microsoft con clústeres Apache Hadoop implementados en Azure HDInsight. Un ejemplo de esta integración es la capacidad de conectar Excel al almacenamiento de datos de Hive de un clúster Hadoop en HDInsight usando Microsoft Hive Open Database Connectivity (ODBC) Driver.

También es posible conectar desde Excel los datos asociados con un clúster de HDInsight y otros orígenes de datos, incluidos otros clústeres Hadoop (que no sean de HDInsight), con la utilización del complemento Microsoft Power Query para Excel. Para obtener más información acerca de la instalación y uso de Power Query, consulte [Conexión de Excel a HDInsight con Power Query](../hdinsight-connect-excel-power-query.md).

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar este artículo, debe tener los siguientes elementos:

* Un clúster de Hadoop de HDInsight: Para crear uno, vea [Introducción a HDInsight de Azure](apache-hadoop-linux-tutorial-get-started.md).
* Una estación de trabajo con Office Professional Plus 2010 o posterior, o Excel 2010 o posterior.

## <a name="install-microsoft-hive-odbc-driver"></a>Instalación de Microsoft Hive ODBC Driver

Descargue e instale la versión de [Microsoft Hive ODBC Driver](https://www.microsoft.com/download/details.aspx?id=40886) que coincida con la versión de la aplicación donde va a usar el controlador ODBC.  En este artículo, el controlador se usa para Office Excel.

## <a name="create-apache-hive-odbc-data-source"></a>Creación de un origen de datos de Apache Hive ODBC

En los siguientes pasos se explica cómo crear un origen de datos de Hive ODBC.

1. En Windows, vaya a Inicio > Herramientas administrativas de Windows > Orígenes de datos ODBC (32 bits)/(64 bits).  Se abre la ventana **Administrador de orígenes de datos ODBC**.

    ![Administrador de orígenes de datos OBDC](./media/apache-hadoop-connect-excel-hive-odbc-driver/simbahiveodbc-datasourceadmin1.png "Configuración de un DSN mediante el Administrador de orígenes de datos ODBC")

1. Desde la pestaña **DSN del usuario**, seleccione **Agregar** para abrir la ventana **Crear nuevo origen de datos**.

1. Seleccione **Microsoft Hive ODBC Driver** y, luego, seleccione **Finalizar** para abrir la ventana **Microsoft Hive ODBC Driver DSN Setup** (Configuración de DSN de Microsoft Hive ODBC Driver).

1. Escriba o seleccione los valores siguientes:

   | Propiedad | Descripción |
   | --- | --- |
   |  Data Source Name |Asigne un nombre al origen de datos |
   |  Host(s) |Escriba `HDInsightClusterName.azurehdinsight.net`. Por ejemplo, `myHDICluster.azurehdinsight.net`. Nota: se admite `HDInsightClusterName-int.azurehdinsight.net` siempre y cuando la VM de cliente esté emparejada a la misma red virtual. |
   |  Port |Use **443**. (Este puerto se ha cambiado de 563 a 443). |
   |  Base de datos |Use el **valor predeterminado**. |
   |  Mechanism |Seleccione **Servicio HDInsight de Microsoft Azure**. |
   |  Nombre de usuario |Escriba el nombre de usuario HTTP del clúster de HDInsight. El nombre de usuario predeterminado es **admin**. |
   |  Contraseña |Escriba la contraseña del usuario del clúster de HDInsight. Seleccione la casilla **Save Password (Encrypted)** [Guardar contraseña (cifrada)].|

1. Opcional: Seleccione **Opciones avanzadas...**  

   | Parámetro | Descripción |
   | --- | --- |
   |  Use Native Query |Cuando esta opción está seleccionada, el controlador ODBC NO trata de convertir TSQL en HiveQL. Solo debe usarla si está totalmente seguro de que va a enviar instrucciones de HiveQL puras. Al conectarse a SQL Server o a Azure SQL Database, debe dejar esta opción desactivada. |
   |  Rows fetched per block |Al capturar un gran volumen de registros, es posible que sea necesario ajustar este parámetro para garantizar un rendimiento óptimo. |
   |  Default string column length, Binary column length, Decimal column scale |La longitud y precisión del tipo de datos pueden afectar a la forma en que se devuelven los datos. Pueden dar lugar a que se devuelva información incorrecta debido a la pérdida de precisión o al truncamiento. |

    ![Opciones de configuración avanzada de DSN](./media/apache-hadoop-connect-excel-hive-odbc-driver/hiveodbc-datasource-advancedoptions1.png "Opciones de configuración avanzada de DSN")

1. Seleccione **Probar** para probar el origen de datos. Cuando el origen de datos esté configurado correctamente, el resultado de la prueba mostrará **SUCCESS!** (Correcto).  

1. Seleccione **Aceptar** para cerrar la ventana Probar.  

1. Seleccione **Aceptar** para cerrar la ventana **Microsoft Hive ODBC Driver DSN Setup**.  

1. Seleccione **Aceptar** para cerrar la ventana **Administrador de orígenes de datos ODBC**.  

## <a name="import-data-into-excel-from-hdinsight"></a>Importación de datos en Excel desde HDInsight

En los pasos siguientes se describe cómo importar datos desde una tabla de Hive a un libro de Excel mediante el origen de datos ODBC creado en la sección anterior.

1. Abra un libro de Excel nuevo o existente.

2. Desde la pestaña **Datos**, vaya a **Obtener datos** > **Desde otros orígenes** > **Desde ODBC** para iniciar la ventana **Desde ODBC**.

    ![Open Excel data connection wizard](./media/apache-hadoop-connect-excel-hive-odbc-driver/simbahiveodbc-excel-dataconnection1.png "Open Excel data connection wizard (Abrir el Asistente para la conexión de datos de Excel)") (Abrir el Asistente para la conexión de datos de Excel)

3. De la lista desplegable, seleccione el nombre del origen de datos que creó en la sección anterior y luego seleccione **Aceptar**.

4. Para el primer uso, se abrirá el cuadro de diálogo **Controlador ODBC**. Seleccione **Windows** en el menú izquierdo. Seleccione **Conectar** para abrir la ventana del **Navegador**.

5. Desde **Navegador**, vaya a **HIVE** > **default** > **hivesampletable** y, luego, seleccione **Cargar**. La importación de los datos a Excel tarda un momento.

    ![HDInsight Excel Hive ODBC navigator](./media/apache-hadoop-connect-excel-hive-odbc-driver/hdinsight-hive-odbc-navigator.png "HDInsight Excel Hive ODBC navigator (Navegador de Hive ODBC en Excel para HDInsight)") (Navegador de Hive ODBC en Excel para HDInsight)

## <a name="next-steps"></a>Pasos siguientes

En este artículo se proporciona información sobre cómo usar el controlador ODBC de Microsoft Hive para recuperar datos del servicio HDInsight en Excel. De manera similar, puede recuperar datos del servicio HDInsight en la SQL Database. También es posible cargar datos en un servicio HDInsight. Para obtener más información, consulte:

* [Visualización de datos de Apache Hive con Microsoft Power BI en Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Visualización de datos de Interactive Query Hive con Power BI en Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Uso de Apache Zeppelin para ejecutar consultas de Apache Hive en Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Conexión de Excel a Apache Hadoop con Power Query](apache-hadoop-connect-excel-power-query.md).
* [Conectarse a Azure HDInsight y ejecutar consultas de Apache Hive con Herramientas de Data Lake para Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Uso de Herramientas de Azure HDInsight para Visual Studio Code](../hdinsight-for-vscode.md).
* [Carga de datos en HDInsight](./../hdinsight-upload-data.md).
