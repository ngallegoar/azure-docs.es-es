---
title: 'Inicio rápido: Creación de un clúster de Spark en HDInsight mediante Azure Portal'
description: Este inicio rápido muestra cómo usar Azure Portal para crear un clúster de Apache Spark en Azure HDInsight y ejecutar una consulta SQL de Spark.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/25/2020
ms.openlocfilehash: c22136e58ddf35ca28ab2251b3476ce2a80dbc4a
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2020
ms.locfileid: "92539282"
---
# <a name="quickstart-create-apache-spark-cluster-in-azure-hdinsight-using-azure-portal"></a>Inicio rápido: Creación de un clúster de Apache Spark en Azure HDInsight mediante Azure Portal

En este inicio rápido, se usa Azure Portal para crear un clúster de Apache Spark en Azure HDInsight. Luego se crea un cuaderno de Jupyter Notebook y se usa para ejecutar consultas de Spark SQL en tablas de Apache Hive. Azure HDInsight es un servicio administrado, de espectro completo y de análisis de código abierto para empresas. El marco de Apache Spark para HDInsight permite un análisis de datos y un procesamiento de clústeres rápidos mediante el procesamiento en memoria. Jupyter Notebook permite interactuar con los datos, combinar código con texto Markdown y realizar visualizaciones simples.

Para una explicación detallada de las configuraciones disponibles, consulte el artículo sobre la [configuración de clústeres en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md). Para más información sobre el uso del portal para crear clústeres, consulte el artículo sobre la [creación de clústeres en el portal](../hdinsight-hadoop-create-linux-clusters-portal.md).

Si usa varios clústeres juntos, querrá crear una red virtual y, si está usando un clúster de Spark, también querrá usar Hive Warehouse Connector. Para más información, consulte [Planificación de una red virtual para Azure HDInsight](../hdinsight-plan-virtual-network-deployment.md) e [Integración de Apache Spark y Apache Hive con el conector de Hive Warehouse](../interactive-query/apache-hive-warehouse-connector.md).

> [!IMPORTANT]  
> La facturación de los clústeres de HDInsight se prorratea por minuto, tanto si se usan como si no. Por consiguiente, es aconsejable eliminar el clúster al terminar de usarlo. Para obtener más información, consulte la sección [Limpieza de recursos](#clean-up-resources) de este artículo.

## <a name="prerequisites"></a>Prerrequisitos

Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="create-an-apache-spark-cluster-in-hdinsight"></a>Creación de un clúster de Apache Spark en HDInsight

Azure Portal se usa para crear un clúster de HDInsight que utiliza blobs de Azure Storage Blob como almacenamiento de clústeres. Para más información sobre el uso de Data Lake Storage Gen2, consulte [Guía de inicio rápido: Configuración de clústeres en HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).

1. En el menú superior, seleccione **+ Crear un recurso** .

    ![Creación de recursos en Azure Portal](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-create-resource.png "Creación de un recurso en Azure Portal")

1. Seleccione **Analytics** > **Azure HDInsight** para ir a la página **Crear clúster de HDInsight** .

1. En la pestaña **Básico** , especifique la siguiente información:

    |Propiedad  |Descripción  |
    |---------|---------|
    |Subscription  | En la lista desplegable, seleccione la suscripción de Azure que se usa para el clúster. |
    |Resource group | En la lista desplegable, seleccione el grupo de recursos existente o seleccione **Crear** .|
    |Nombre del clúster | Escriba un nombre único global.|
    |Region   | En la lista desplegable, seleccione una región donde crear el clúster. |
    |Tipo de clúster| Seleccione Seleccionar tipo de clúster para abrir una lista. En ella, seleccione **Spark** .|
    |Versión del clúster|Este campo se rellenará automáticamente con la versión predeterminada una vez que se haya seleccionado el tipo de clúster.|
    |Nombre de usuario de inicio de sesión del clúster| Escriba el nombre de usuario de inicio de sesión del clúster.  El nombre predeterminado es **admin** . Use esta cuenta para iniciar sesión en Jupyter Notebook más adelante en la guía de inicio rápido. |
    |Contraseña de inicio de sesión de clúster| Escriba la contraseña de inicio de sesión del clúster. |
    |Nombre de usuario de Secure Shell (SSH)| Escriba el nombre de usuario de SSH. El nombre de usuario de SSH usado para este inicio rápido es **sshuser** . De manera predeterminada, esta cuenta comparte la contraseña con la cuenta de *nombre de usuario de inicio de sesión del clúster* . |

    ![La captura de pantalla muestra el clúster Crear HD Insight con la pestaña Aspectos básicos seleccionada.](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-basics-spark.png "Creación de configuraciones básicas de clústeres de Spark en HDInsight")

    Seleccione **Siguiente: Almacenamiento>>** para continuar en la página **Almacenamiento** .

1. En **Almacenamiento** , proporcione los valores siguientes:

    |Propiedad  |Descripción  |
    |---------|---------|
    |Tipo de almacenamiento principal|Use el valor predeterminado **Azure Storage** .|
    |Método de selección|Use el valor predeterminado **Seleccionar de la lista** .|
    |Cuenta de almacenamiento principal|Use el valor que se rellena automáticamente.|
    |Contenedor|Use el valor que se rellena automáticamente.|

    ![La captura de pantalla muestra el clúster Crear HD Insight con la pestaña Almacenamiento seleccionada.](./media/apache-spark-jupyter-spark-sql-use-portal/azure-portal-cluster-storage.png "Creación de configuraciones básicas de clústeres de Spark en HDInsight")

    Seleccione **Revisar y crear** para continuar.

1. Seleccione **Revisar y crear** y, después, **Crear** . La creación del clúster tarda aproximadamente 20 minutos. El clúster debe crearse para poder pasar a la siguiente sesión.

Si surge algún problema al crear clústeres de HDInsight, podría deberse a que no tiene los permisos adecuados para hacerlo. Para más información, consulte [Requisitos de control de acceso](../hdinsight-hadoop-customize-cluster-linux.md#access-control).

## <a name="create-a-jupyter-notebook"></a>Creación de un cuaderno de Jupyter

Jupyter Notebook es un entorno de cuaderno interactivo que admite varios lenguajes de programación. El cuaderno le permite interactuar con los datos, combinar código con el texto de marcado y realizar visualizaciones básicas.

1. En un explorador web, vaya a `https://CLUSTERNAME.azurehdinsight.net/jupyter`, donde `CLUSTERNAME` es el nombre del clúster. Cuando se le solicite, escriba las credenciales de inicio de sesión del clúster.

1. Seleccione **Nuevo** > **PySpark** para crear un cuaderno.

   ![Creación de un cuaderno de Jupyter Notebook para ejecutar consultas Spark SQL interactivas](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Creación de un cuaderno de Jupyter Notebook para ejecutar consultas Spark SQL interactivas")

   Se crea y se abre un nuevo cuaderno con el nombre Untitled(Untitled.pynb).

## <a name="run-apache-spark-sql-statements"></a>Ejecución de instrucciones de Apache Spark SQL

SQL (Lenguaje de consulta estructurado) es el lenguaje más común y ampliamente utilizado en la consulta y definición de datos. Spark SQL funciona como una extensión de Apache Spark para procesar datos estructurados, mediante la conocida sintaxis de SQL.

1. Compruebe que el kernel esté preparado. El kernel está preparado cuando aparece un círculo vacío junto al nombre del kernel en el cuaderno. Un círculo sólido indica que el kernel está ocupado.

    ![La captura de pantalla muestra una ventana de Jupyter con un indicador de PySpark.](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "Consulta de Hive en HDInsight")

    Al iniciar el cuaderno por primera vez, el kernel realiza tareas en segundo plano. Espere a que el kernel esté preparado.

1. Pegue el código siguiente en una celda vacía y presione **MAYÚS + ENTRAR** para ejecutar el código. El comando muestra las tablas Hive del clúster:

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    Si se utiliza un cuaderno de Jupyter Notebook con un clúster de HDInsight, se obtiene un valor de `sqlContext` preestablecido que puede usar para ejecutar consultas de Hive mediante Spark SQL. `%%sql` indica a Jupyter Notebook que use el valor de `sqlContext` preestablecido para ejecutar la consulta de Hive. La consulta recupera las 10 primeras filas de una tabla de Hive ( **hivesampletable** ) que se incluye de forma predeterminada en todos los clústeres de HDInsight. Se tardan unos 30 segundos en obtener los resultados. El resultado tendrá una apariencia similar a la siguiente:

    ![La captura de pantalla muestra una ventana de Jupyter para el cuaderno creado en este inicio rápido.](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-get-started-hive-query.png "Consulta de Hive en HDInsight")

    Cada vez que se ejecuta una consulta en Jupyter, el título de la ventana del explorador web muestra el estado **(Busy)** (Ocupado) junto con el título del cuaderno. También verá un círculo sólido junto al texto **PySpark** en la esquina superior derecha.

1. Ejecute otra consulta para ver los datos en `hivesampletable`.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    Debe actualizar la pantalla para mostrar la salida de la consulta.

    ![Salida de una consulta de Hive en HDInsight](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-spark-get-started-hive-query-output.png "Salida de una consulta de Hive en HDInsight")

1. En el menú **File** (Archivo) del cuaderno, seleccione **Close and Halt** (Cerrar y detener). Al cerrar el cuaderno, se liberan los recursos de clúster.

## <a name="clean-up-resources"></a>Limpieza de recursos

HDInsight guarda los datos en Azure Storage o Azure Data Lake Storage, por lo que puede eliminar de manera segura un clúster si no se está usando. Los clústeres de HDInsight se cobran aunque no se estén usando. Como en muchas ocasiones los cargos por el clúster son mucho más elevados que los cargos por el almacenamiento, desde el punto de vista económico tiene sentido eliminar clústeres cuando no se usen. Si tiene previsto pasar inmediatamente al tutorial de [Pasos siguientes](#next-steps), es aconsejable que no elimine el clúster.

Vuelva a Azure Portal y seleccione **Eliminar** .

![Azure Portal: eliminación de un clúster de HDInsight](./media/apache-spark-jupyter-spark-sql-use-portal/hdinsight-azure-portal-delete-cluster.png "Eliminación de un clúster de HDInsight")

También puede seleccionar el nombre del grupo de recursos para abrir la página del grupo de recursos y, a continuación, seleccionar **Eliminar grupo de recursos** . Al eliminar el grupo de recursos, se eliminan tanto el clúster de HDInsight como la cuenta de almacenamiento predeterminada.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha aprendido a crear un clúster de Apache Spark en HDInsight y a ejecutar una consulta básica de Spark SQL. Pase al siguiente tutorial, donde aprenderá a usar un clúster de HDInsight para ejecutar consultas interactivas en datos de ejemplo.

> [!div class="nextstepaction"]
> [Ejecución de consultas interactivas en Apache Spark](./apache-spark-load-data-run-query.md)