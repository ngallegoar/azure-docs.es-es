---
title: 'Herramientas de Apache Hive y Data Lake para Visual Studio: Azure HDInsight'
description: Aprenda a usar las herramientas de Data Lake para Visual Studio a fin de ejecutar consultas de Apache Hive con Apache Hadoop en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive
ms.date: 11/27/2019
ms.openlocfilehash: c8645ae9cb901b9fc95f00665d73e223a24fda63
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86076460"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Ejecución de las consultas de Apache Hive mediante las herramientas de Data Lake para Visual Studio

Aprenda a usar las herramientas de Data Lake para Visual Studio a fin de realizar consultas en Apache Hive. Las herramientas de Data Lake permiten crear, enviar y supervisar fácilmente consultas de Hive en Apache Hadoop en Azure HDInsight.

## <a name="prerequisites"></a>Prerrequisitos

* Un clúster de Apache Hadoop en HDInsight. Para más información sobre la creación de este elemento, consulte [Creación de clústeres de Apache Hadoop en Azure HDInsight con plantillas de Resource Manager](./apache-hadoop-linux-tutorial-get-started.md).

* [Visual Studio](https://visualstudio.microsoft.com/vs/). En los pasos que se describen en este artículo, se utiliza Visual Studio 2019.

* Herramientas de HDInsight para Visual Studio o Azure Data Lake Tools for Visual Studio. Para más información acerca de cómo instalar y configurar las herramientas, consulte [Instalación de herramientas de Data Lake para Visual Studio](apache-hadoop-visual-studio-tools-get-started.md#install-data-lake-tools-for-visual-studio).

## <a name="run-apache-hive-queries-using-the-visual-studio"></a>Ejecución de consultas de Apache Hive con Visual Studio

Para crear y ejecutar consultas de Hive tiene dos opciones:

* Crear consultas ad hoc.
* Crear una aplicación de Hive.

### <a name="create-an-ad-hoc-hive-query"></a>Creación de una consulta ad hoc de Hive

Las consultas ad hoc se pueden ejecutar en el modo **Lotes** o **Interactivo**.

1. Inicie **Visual Studio** y seleccione **Continuar sin código**.

2. En el **Explorador de servidores**, haga clic con el botón derecho en **Azure**, seleccione **Conectar a la suscripción de Microsoft Azure...** y complete el proceso de inicio de sesión.

3. Expanda **HDInsight**, haga clic con el botón derecho en el clúster donde quiere ejecutar la consulta y seleccione **Escribir una consulta de Hive**.

4. Especifique esta consulta de Hive:

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. Seleccione **Execute**(Ejecutar). El valor predeterminado del modo de ejecución es **Interactivo**.

    ![Ejecución de una consulta interactiva de Hive, Visual Studio](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. Para ejecutar la misma consulta en el modo **Lotes**, cambie la lista desplegable de **Interactivo** a **Lotes**. El botón de ejecución cambia de **Ejecutar** a **Enviar**.

    ![Envío de consultas por lotes de Hive, Visual Studio](./media/apache-hadoop-use-hive-visual-studio/visual-studio-batch-query.png)

    El editor de Hive es compatible con IntelliSense. Data Lake Tools para Visual Studio es compatible con la carga de metadatos remotos cuando se edita un script de Hive. Por ejemplo, si escribe `SELECT * FROM`, IntelliSense enumera todos los nombres de tabla sugeridos. Cuando se especifica un nombre de tabla, IntelliSense enumera los nombres de columna. Las herramientas admiten casi todas las instrucciones DML de Hive, subconsultas y UDF integradas. IntelliSense solo sugiere los metadatos del clúster que se seleccionan en la barra de herramientas de HDInsight.

7. En la barra de herramientas de consulta (el área situada debajo de la pestaña de consulta y encima del texto de la consulta), seleccione **Enviar** o haga clic en la flecha desplegable situada junto a **Enviar** y elija **Opciones avanzadas**. Si selecciona la última opción,

8. Si selecciona la opción de envío avanzado, configure las opciones **Nombre del trabajo**, **Argumentos**, **Configuraciones adicionales** y **Estado de directorio** en el cuadro de diálogo **Enviar script**. Después, seleccione **Enviar**.

    ![Cuadro de diálogo Enviar script, consulta de Hive en HDInsight Hadoop](./media/apache-hadoop-use-hive-visual-studio/vs-tools-submit-jobs-advanced.png)

### <a name="create-a-hive-application"></a>Crear una aplicación de Hive

Para ejecutar una consulta de Hive creando una aplicación de Hive, siga estos pasos:

1. Abra **Visual Studio**.

2. En la ventana **Inicio**, seleccione **Crear un proyecto**.

3. En la ventana **Crear un nuevo proyecto**, en el cuadro **Buscar plantillas**, escriba *Hive*. A continuación, elija **Aplicación de Hive** y seleccione **Siguiente**.

4. En la ventana **Configure su nuevo proyecto**, escriba el **nombre del proyecto**, seleccione o cree una **ubicación** para el nuevo proyecto y haga clic en **Crear**.

5. Abra el archivo **Script.hql** creado con este proyecto y péguelo en las siguientes instrucciones de HiveQL:

    ```hql
    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Estas instrucciones realizan las acciones siguientes:

    * `DROP TABLE`: Elimina la tabla, si existe.

    * `CREATE EXTERNAL TABLE`: crea una nueva tabla "externa" en Hive. Las tablas externas solo almacenan la definición de Tabla en Hive. (Los datos permanecen en la ubicación original).

        > [!NOTE]  
        > Las tablas externas deben utilizarse si espera que los datos subyacentes se actualicen mediante un origen externo, como un trabajo de MapReduce o un servicio de Azure.
        >
        > La eliminación de una tabla externa **no** elimina los datos, solamente la definición de tabla.

    * `ROW FORMAT`: indica a Hive cómo se da formato a los datos. En este caso, los campos de cada registro se separan mediante un espacio.

    * `STORED AS TEXTFILE LOCATION`: indica a Hive que los datos deben almacenarse en el directorio *example/data* como texto.

    * `SELECT`: selecciona todas las filas en las que la columna `t4` contiene el valor `[ERROR]`. Esta instrucción devuelve el valor `3` porque hay tres filas que contienen este valor.

    * `INPUT__FILE__NAME LIKE '%.log'`: indica a Hive que solo debe devolver datos de los archivos que terminan por .log. Esta cláusula limita la búsqueda al archivo *sample.log* que contiene los datos.

6. En la barra de herramientas del archivo de consulta (que tiene una apariencia similar a la de la barra de herramientas de consultas ad hoc), seleccione el clúster de HDInsight que desea usar para esta consulta. A continuación, cambie **Interactivo** a **Lote** (si es necesario) y seleccione **Enviar** para ejecutar las instrucciones como un trabajo de Hive.

   El **resumen del trabajo de Hive** aparecerá y mostrará información sobre el trabajo en ejecución. Use el vínculo **Actualizar** para actualizar la información del trabajo, hasta que el **estado del trabajo** cambie a **Completado**.

   ![Resumen del trabajo de Hive completado, aplicación de Hive, Visual Studio](./media/apache-hadoop-use-hive-visual-studio/hdinsight-job-summary.png)

7. Seleccione **Salida del trabajo** para ver el resultado de este trabajo. Muestra `[ERROR] 3`, que es el valor que devuelve esta consulta.

### <a name="additional-example"></a>Ejemplo adicional

El ejemplo siguiente se basa en la tabla `log4jLogs` creada en el procedimiento anterior, [Creación de una aplicación de Hive](#create-a-hive-application).

1. En el **Explorador de servidores**, haga clic con el botón derecho en el clúster y seleccione **Escribir una consulta de Hive**.

2. Especifique esta consulta de Hive:

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Estas instrucciones realizan las acciones siguientes:

    * `CREATE TABLE IF NOT EXISTS`: Crea una tabla, en caso de que no exista todavía. Como no se utiliza la palabra clave `EXTERNAL`, esta instrucción crea una tabla interna. Las tablas internas se guardan en el almacenamiento de datos de Hive y Hive las administra.

        > [!NOTE]  
        > A diferencia de las tablas `EXTERNAL`, la eliminación de una tabla interna también eliminará los datos subyacentes.

    * `STORED AS ORC`: almacena los datos en *formato de columnas de filas optimizadas* (ORC). ORC es un formato altamente optimizado y eficiente para almacenar datos de Hive.

    * `INSERT OVERWRITE ... SELECT`: selecciona filas de la tabla `log4jLogs` que contienen `[ERROR]` y luego inserta los datos en la tabla `errorLogs`.

3. Si es necesario, cambie **Interactivo** por **Lote** y seleccione **Enviar**.

4. Para comprobar si el trabajo creó la tabla, vaya al **Explorador de servidores** y expanda **Azure** > **HDInsight**. Expanda el clúster de HDInsight y después **Bases de datos de Hive** > **predeterminado**. La tabla **errorLogs** y la tabla **log4jLogs** aparecen en la lista.

## <a name="next-steps"></a>Pasos siguientes

Como puede ver, las herramientas de HDInsight para Visual Studio proporcionan una manera sencilla de trabajar con consultas de Hive en HDInsight.

* Para más información sobre Hive en HDInsight, consulte [¿Qué son Apache Hive y HiveQL en Azure HDInsight?](hdinsight-use-hive.md).

* Para más información sobre otras formas en que puede trabajar con Hadoop en HDInsight, consulte [Uso de MapReduce con Apache Hadoop en HDInsight](hdinsight-use-mapreduce.md).

* Para más información sobre las herramientas de HDInsight para Visual Studio, consulte [Uso de Herramientas de Azure Data Lake para Visual Studio para conectarse a Azure HDInsight y ejecutar consultas de Apache Hive](apache-hadoop-visual-studio-tools-get-started.md).
