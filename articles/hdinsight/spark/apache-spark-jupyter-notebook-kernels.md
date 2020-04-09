---
title: Kernels para Jupyter Notebook en clústeres Spark en Azure HDInsight
description: Obtenga información sobre los kernels de PySpark, PySpark3 y Spark que puede usar con el cuaderno de Jupyter Notebook disponible con clústeres Spark en Azure HDInsight.
keywords: Jupyter Notebook en Spark, Spark en Jupyter
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 03/20/2020
ms.openlocfilehash: a04b8fee31ffa5280bc8ad0fca35495bb87e0e8a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064463"
---
# <a name="kernels-for-jupyter-notebook-on-apache-spark-clusters-in-azure-hdinsight"></a>Kernels para Jupyter Notebook en clústeres Apache Spark en Azure HDInsight

Los clústeres de HDInsight Spark proporcionan kernels que se pueden utilizar con el cuaderno de Jupyter Notebook en [Azure Spark](./apache-spark-overview.md) para probar las aplicaciones. Un kernel es un programa que ejecuta e interpreta el código. Estos son los tres kernels:

- **PySpark**: para aplicaciones escritas en Python2.
- **PySpark3**: para aplicaciones escritas en Python3.
- **Spark**: para aplicaciones escritas en Scala.

En este artículo, aprenderá a usar estos kernels y las ventajas de utilizarlos.

## <a name="prerequisites"></a>Prerrequisitos

Un clúster de Apache Spark en HDInsight. Para obtener instrucciones, vea [Creación de clústeres Apache Spark en HDInsight de Azure](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Creación de un cuaderno de Jupyter Notebook en clústeres Spark de HDInsight

1. En [Azure Portal](https://portal.azure.com/), seleccione el clúster Spark.  Consulte [Enumeración y visualización de clústeres](../hdinsight-administer-use-portal-linux.md#showClusters) para obtener instrucciones. Se abre la vista **Introducción**.

2. Desde la vista **Introducción**, en el cuadro **Paneles de clúster**, seleccione **Jupyter Notebook**. Cuando se le pida, escriba las credenciales del clúster.

    ![Jupyter Notebook en Apache Spark](./media/apache-spark-jupyter-notebook-kernels/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Jupyter Notebook en Spark")
  
   > [!NOTE]  
   > También puede comunicarse con el cuaderno de Jupyter Notebook del clúster Spark si abre la siguiente dirección URL en el explorador. Reemplace **CLUSTERNAME** por el nombre del clúster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

3. Seleccione **Nuevo** y, después, seleccione **Pyspark**, **PySpark3** o **Spark** para crear un cuaderno. Utilice el kernel de Spark para las aplicaciones de Scala, kernel PySpark para aplicaciones de Python2 y kernel PySpark para3 aplicaciones de Python3.

    ![Kernels para Jupyter Notebook en Spark](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "Kernels para Jupyter Notebook en Spark")

4. Se abre un cuaderno con el kernel que seleccionó.

## <a name="benefits-of-using-the-kernels"></a>Ventajas de utilizar los kernels

Estas son algunas ventajas de usar los kernels nuevo con el cuaderno de Jupyter Notebook en clústeres Spark de HDInsight.

- **Contextos preestablecidos**. Gracias a los kernels de **PySpark**, **PySpark3** o **Spark**, no necesita establecer de forma explícita los contextos de Spark o Hive para poder empezar a trabajar con las aplicaciones, ya que están disponibles de forma predeterminada. Estos contextos son:

  - **sc** : para el contexto Spark
  - **sqlContext** : para el contexto Hive

    Por tanto, **no** tiene que ejecutar instrucciones como la siguiente para definir los contextos:

         sc = SparkContext('yarn-client')
         sqlContext = HiveContext(sc)

    En su lugar, puede utilizar directamente los contextos preestablecidos en la aplicación.

- **Instrucciones mágicas de celda**. El kernel de PySpark proporciona algunas "instrucciones mágicas" predefinidas, que son comandos especiales que se pueden llamar con `%%` (por ejemplo, `%%MAGIC` `<args>`). El comando mágico debe ser la primera palabra de una celda de código y permitir varias líneas de contenido. La palabra mágica debe ser la primera palabra en la celda. Si se agrega algo antes del comando mágico, incluso comentarios, se producirá un error.     Para obtener más información sobre instrucciones mágicas, vaya [aquí](https://ipython.readthedocs.org/en/stable/interactive/magics.html).

    La siguiente tabla muestra las diferentes instrucciones mágicas disponibles a través de los kernels.

   | Instrucción mágica | Ejemplo | Descripción |
   | --- | --- | --- |
   | help |`%%help` |Genera una tabla de todas las instrucciones mágicas disponibles con el ejemplo y la descripción |
   | info |`%%info` |Produce información de sesión del punto de conexión actual de Livy. |
   | CONFIGURAR |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Configura los parámetros para crear una sesión. El marcador de fuerza (-f) es obligatorio si ya se ha creado una sesión, lo que garantiza que la sesión se elimina y se vuelve a crear. Consulte [Livy's POST /sessions Request Body (Cuerpo de la solicitud de sesiones o POST de Livy)](https://github.com/cloudera/livy#request-body) para obtener una lista de parámetros válidos. Los parámetros deben pasarse como una cadena JSON y deben estar en la siguiente línea después de la instrucción mágica, como se muestra en la columna de ejemplo. |
   | sql |`%%sql -o <variable name>`<br> `SHOW TABLES` |Ejecuta una consulta de Hive en el sqlContext. Si se pasa el parámetro `-o` , el resultado de la consulta se conserva en el contexto %%local de Python como trama de datos [Pandas](https://pandas.pydata.org/) . |
   | local |`%%local`<br>`a=1` |Todo el código de las líneas siguientes se ejecuta localmente. El código debe ser código Python2 válido, independientemente del kernel que se utilice. Por tanto, aunque haya seleccionado los kernels **PySpark3** o **Spark** al crear el cuaderno, si usa el comando mágico `%%local` en una celda, esta solo debe tener código Python2 válido. |
   | logs |`%%logs` |Genera los registros de la sesión actual de Livy. |
   | delete |`%%delete -f -s <session number>` |Elimina una sesión específica del punto de conexión actual de Livy. No se puede eliminar la sesión iniciada en el propio kernel. |
   | cleanup |`%%cleanup -f` |Elimina todas las sesiones del punto de conexión actual de Livy, incluida la sesión de este cuaderno. La marca force -f es obligatoria. |

   > [!NOTE]  
   > Además de las instrucciones mágicas agregadas mediante el kernel PySpark, también puede utilizar las [instrucciones integradas de IPython](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), como `%%sh`. Puede usar la instrucción mágica `%%sh` para ejecutar scripts y el bloque de código en el nodo principal del clúster.

- **Visualización automática**. El kernel Pyspark visualiza automáticamente el resultado de las consultas de Hive y SQL. Puede elegir entre diferentes tipos de visualizaciones, como tabla, circular, línea, área o barra.

## <a name="parameters-supported-with-the-sql-magic"></a>Parámetros compatibles con la instrucción mágica %%sql

El comando mágico `%%sql` es compatible con distintos parámetros que se pueden usar para controlar el tipo de resultado que se obtiene al ejecutar consultas. En la tabla siguiente se muestra el resultado.

| Parámetro | Ejemplo | Descripción |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Use este parámetro para conservar el resultado de la consulta en el contexto %%local de Python como trama de datos [Pandas](https://pandas.pydata.org/) . El nombre de la variable de la trama de datos es el nombre de variable que especifique. |
| -q |`-q` |Úselo para desactivar visualizaciones de la celda. Si no quiere visualizar de forma automática el contenido de una celda y simplemente quiere capturarlo como una trama de datos, use `-q -o <VARIABLE>`. Si quiere desactivar visualizaciones sin capturar los resultados (por ejemplo, para ejecutar una consulta de SQL, como una instrucción `CREATE TABLE`), use `-q` sin especificar un argumento `-o`. |
| -M |`-m <METHOD>` |Donde **METHOD** puede ser **take** o **sample** (el valor predeterminado es **take**). Si el método es **take**, el kernel toma elementos de la parte superior del conjunto de datos de resultados especificado por MAXROWS (se describe más adelante en esta tabla). Si el método es **sample**, el kernel toma como muestra los elementos del conjunto de datos de forma aleatoria en función del parámetro `-r`, que se describe a continuación en esta tabla. |
| -r |`-r <FRACTION>` |Aquí **FRACTION** es un número de punto flotante entre 0,0 y 1,0. Si el método sample de la consulta SQL es `sample`, el kernel muestrea de forma aleatoria la fracción especificada de los elementos del conjunto de resultados establecido. Por ejemplo, si ejecuta una consulta SQL con los argumentos `-m sample -r 0.01`, el 1 % de las filas del resultados se muestrean de forma aleatoria. |
| -n |`-n <MAXROWS>` |**MAXROWS** es un valor entero. El kernel limita el número de filas de salida a **MAXROWS**. Si **MAXROWS** tiene un valor negativo (por ejemplo, **-1**), no se limita el número de filas del conjunto de resultados. |

**Ejemplo**:

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

La instrucción anterior hace lo siguiente:

- Selecciona todos los registros de **hivesampletable**.
- Dado que se usa -q, desactiva la visualización automática.
- Dado que se usa `-m sample -r 0.1 -n 500` , toma como muestra de forma aleatoria el 10 % de las filas de hivesampletable y limita el tamaño del conjunto de resultados a 500 filas.
- Por último, dado que se ha usado `-o query2` , también guarda el resultado en una trama de datos denominada **query2**.

## <a name="considerations-while-using-the-new-kernels"></a>Consideraciones al utilizar los kernels nuevos

Sea cual sea el kernel que se use, dejar los cuadernos en ejecución consumirá los recursos del clúster.  Con estos kernels, dado que los contextos están preestablecidos, salir simplemente de los cuadernos no elimina el contexto y, por tanto, los recursos de clúster siguen estando en uso. Una práctica recomendada es utilizar la opción **Close and Halt** (Cerrar y detener) del menú **File** (Archivo) del cuaderno cuando haya terminado de usar el cuaderno, que termina el contexto y, después, sale del cuaderno.

## <a name="where-are-the-notebooks-stored"></a>Almacenamiento de los cuadernos

Si el clúster usa Azure Storage como la cuenta de almacenamiento predeterminada, las instancias de Jupyter Notebook se guardan en la cuenta de almacenamiento de la carpeta **/HdiNotebooks**.  Es posible acceder a los cuadernos, los archivos de texto y las carpetas que se crean en Jupyter desde la cuenta de almacenamiento.  Por ejemplo, si usa Jupyter para crear una carpeta **myfolder** y un cuaderno **myfolder/mynotebook.ipynb**, puede acceder a él en `/HdiNotebooks/myfolder/mynotebook.ipynb` dentro de la cuenta de almacenamiento.  También ocurre lo contrario, es decir, si carga un cuaderno directamente en la cuenta de almacenamiento en `/HdiNotebooks/mynotebook1.ipynb`, se ve también desde Jupyter.  Los cuadernos permanecen en la cuenta de almacenamiento incluso después de que se elimine el clúster.

> [!NOTE]  
> Los clústeres de HDInsight con Azure Data Lake Storage como almacenamiento predeterminado no almacenan los cuadernos en un almacenamiento asociado.

La forma de guardar los cuadernos en la cuenta de almacenamiento es compatible con [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html). Por lo tanto, si se usa SSH en el clúster, puede usar comandos de administración de archivos como se muestra en el siguiente fragmento de código:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                   # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it's visible from Jupyter

Independientemente de si el clúster usa Azure Storage o Azure Data Lake Storage como cuenta de almacenamiento predeterminada, los cuadernos también se guardan en el nodo principal del clúster en `/var/lib/jupyter`.

## <a name="supported-browser"></a>Explorador compatible

Los cuadernos de Jupyter Notebook que se ejecutan en clústeres Spark de HDInsight solo son compatibles con Google Chrome.

## <a name="feedback"></a>Comentarios

El nuevo kernel está en la fase de evolución y se desarrollará con el tiempo. También podría significar que las API podrían cambiar a medida que estos kernels maduran. Agradecemos cualquier comentario que tenga al utilizar estos nuevos kernels. Esto resulta muy útil para dar forma a la versión final de estos kernels. Puede dejar sus comentarios la sección **Comentarios** al final de este artículo.

## <a name="see-also"></a>Consulte también

- [Información general: Apache Spark en Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Escenarios

- [Apache Spark con BI: Análisis de datos interactivos con Spark en HDInsight con las herramientas de BI](apache-spark-use-bi-tools.md)
- [Apache Spark con Machine Learning: uso de Apache Spark en HDInsight para analizar la temperatura de edificios con los datos del sistema de acondicionamiento de aire](apache-spark-ipython-notebook-machine-learning.md)
- [Apache Spark con Machine Learning: uso de Spark en HDInsight para predecir los resultados de la inspección de alimentos](apache-spark-machine-learning-mllib-ipython.md)
- [Análisis de registros de un sitio web mediante Apache Spark en HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Creación y ejecución de aplicaciones

- [Crear una aplicación independiente con Scala](apache-spark-create-standalone-application.md)
- [Ejecución de trabajos de forma remota en un clúster de Apache Spark mediante Apache Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Herramientas y extensiones

- [Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para crear y enviar aplicaciones de Spark Scala](apache-spark-intellij-tool-plugin.md)
- [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Apache Spark applications remotely](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md) (Uso del complemento de herramientas de HDInsight para IntelliJ IDEA para depurar aplicaciones de Apache Spark de forma remota)
- [Uso de cuadernos de Apache Zeppelin con un clúster de Apache Spark en HDInsight](apache-spark-zeppelin-notebook.md)
- [Uso de paquetes externos con cuadernos de Jupyter Notebook](apache-spark-jupyter-notebook-use-external-packages.md)
- [Instalación de un cuaderno de Jupyter Notebook en el equipo y conexión al clúster de Apache Spark en HDInsight de Azure](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Administrar recursos

- [Administración de recursos para el clúster Apache Spark en HDInsight de Azure](apache-spark-resource-manager.md)
- [Track and debug jobs running on an Apache Spark cluster in HDInsight (Seguimiento y depuración de trabajos que se ejecutan en un clúster de Apache Spark en HDInsight)](apache-spark-job-debugging.md)
