---
title: Administración de dependencias de aplicaciones Spark en Azure HDInsight
description: En este artículo se proporciona una introducción a la administración de dependencias de Spark en un clúster de HDInsight Spark para aplicaciones PySpark y Scala.
author: yanancai
ms.author: yanacai
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: how-to
ms.date: 09/09/2020
ms.openlocfilehash: dafb4485ae9b10d89fa36bd790dcf3a799054de3
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90064086"
---
# <a name="manage-spark-application-dependencies"></a>Administración de dependencias de aplicaciones Spark

En este artículo, obtendrá información sobre cómo administrar las dependencias de las aplicaciones Spark que se ejecutan en HDInsight. Abarcamos Scala y PySpark en el ámbito de clúster y aplicación Spark.

Use los vínculos rápidos para saltar a la sección correspondiente en función de su caso de usuario:
* [Configuración de dependencias JAR de trabajos de Spark mediante Jupyter Notebook](#use-jupyter-notebook)
* [Configuración de dependencias JAR de trabajos de Spark mediante Azure Toolkit for IntelliJ](#use-azure-toolkit-for-intellij)
* [Configuración de dependencias JAR para el clúster Spark](#jar-libs-for-cluster)
* [Administración segura de dependencias de JAR](#safely-manage-jar-dependencies)
* [Configuración de paquetes Python de trabajos de Spark mediante Jupyter Notebook](#use-jupyter-notebook-1)
* [Administración segura de paquetes Python para el clúster Spark](#python-packages-for-cluster)

## <a name="jar-libs-for-one-spark-job"></a>Bibliotecas JAR para un trabajo de Spark
### <a name="use-jupyter-notebook"></a>Uso de Jupyter Notebook
Cuando se inicia una sesión de Spark en Jupyter Notebook en el kernel de Spark para Scala, puede configurar paquetes desde:

* El [repositorio Maven](https://search.maven.org/) o los paquetes para obtener paquetes aportados por la comunidad en [Spark Packages](https://spark-packages.org/).
* Archivos JAR en el almacenamiento principal del clúster.

Utilizará la instrucción mágica `%%configure` para configurar el cuaderno para usar un paquete externo. En los cuadernos que utilizan paquetes externos, asegúrese de invocar la instrucción mágica `%%configure` en la primera celda de código. Esto garantiza que el kernel se configure para utilizar el paquete antes de iniciar la sesión.

>
>[!IMPORTANT]  
>Si se olvida de configurar el kernel en la primera celda, puede utilizar el parámetro `%%configure` con el parámetro `-f`, pero ello reiniciará la sesión y se perderá todo el trabajo.

**Ejemplo de paquetes del repositorio de Maven o Spark Packages**

Tras encontrar el paquete del repositorio Maven, recopile los valores de **GroupId**, **ArtifactId** y **Version**. Concatene los tres valores separados por dos puntos ( **:** ).

   ![Concatene esquemas de paquetes](./media/apache-spark-manage-dependencies/spark-package-schema.png "Concatene esquemas de paquetes")

Asegúrese de que los valores recopilados coinciden con el clúster. En este caso, estamos usando el paquete de conector de Spark Cosmos DB para Scala 2.11 y Spark 2.3 para el clúster Spark de HDInsight 3.6. Si no está seguro, ejecute `scala.util.Properties.versionString` en la celda de código del kernel de Spark para consultar la versión de Scala del clúster. Ejecute `sc.version` para consultar la versión de Spark del clúster.

```
%%configure { "conf": {"spark.jars.packages": "com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.3.3" }}
```

**Ejemplo de archivos JAR almacenados en el almacenamiento principal**

Use el [esquema de URI](../hdinsight-hadoop-linux-information.md#URI-and-scheme) para los archivos JAR en el almacenamiento principal de sus clústeres. Sería `wasb://` para Azure Storage, `abfs://` para Azure Data Lake Storage Gen2 o `adl://` para Azure Data Lake Storage Gen1. Si la transferencia segura está habilitada para Azure Storage o Data Lake Storage Gen2, el URI será `wasbs://` o `abfss://`. Consulte [Transferencia segura](../../storage/common/storage-require-secure-transfer.md).

Use una lista separada por comas de rutas de acceso de archivos JAR para varios archivos JAR; se permiten los patrones globales. Los archivos JAR incluyen en las rutas de clases de controlador y ejecutor.

```
%%configure { "conf": {"spark.jars": "wasb://mycontainer@mystorageaccount.blob.core.windows.net/libs/azure-cosmosdb-spark_2.3.0_2.11-1.3.3.jar" }}
```

Después de configurar los paquetes externos, puede ejecutar la opción Importar en la celda de código para comprobar si los paquetes se colocaron correctamente.

```scala
import com.microsoft.azure.cosmosdb.spark._
```

### <a name="use-azure-toolkit-for-intellij"></a>Uso de Azure Toolkit for IntelliJ
El [complemento Azure Toolkit for IntelliJ](./apache-spark-intellij-tool-plugin.md) proporciona una experiencia de interfaz de usuario para enviar una aplicación Spark Scala a un clúster de HDInsight. Proporciona las propiedades `Referenced Jars` y `Referenced Files` para configurar las rutas de acceso de las bibliotecas JAR al enviar la aplicación Spark. Consulte más detalles sobre [cómo usar el complemento Azure Toolkit for IntelliJ para HDInsight](./apache-spark-intellij-tool-plugin.md#run-a-spark-scala-application-on-an-hdinsight-spark-cluster).

![Cuadro de diálogo de envío de Spark](./media/apache-spark-intellij-tool-plugin/hdi-submit-spark-app-02.png)

## <a name="jar-libs-for-cluster"></a>Bibliotecas JAR para clúster
En algunos casos, se recomienda configurar las dependencias JAR en el nivel de clúster para que todas las aplicaciones se puedan configurar con las mismas dependencias de manera predeterminada. El enfoque consiste en agregar las rutas de acceso de archivos JAR a la ruta de acceso de clase del controlador y ejecutor de Spark.

1. Ejecute los ejemplos de acciones de script siguientes para copiar los archivos JAR del almacenamiento principal `wasb://mycontainer@mystorageaccount.blob.core.windows.net/libs/*` al sistema de archivos local `/usr/libs/sparklibs` del clúster. Este paso es necesario cuando Linux usa `:` para separar la lista de rutas de acceso de clase, pero HDInsight solo admite las rutas de acceso de almacenamiento con un esquema parecido a `wasb://`. La ruta de acceso de almacenamiento remoto no funcionará correctamente si se agrega directamente a la ruta de acceso de clase.

    ```bash
    sudo mkdir -p /usr/libs/sparklibs
    sudo hadoop fs -copyToLocal wasb://mycontainer@mystorageaccount.blob.core.windows.net/libs/*.* /usr/libs/sparklibs
    ```

2. Cambie la configuración del servicio Spark de Ambari para actualizar la ruta de acceso de clase. Vaya a **Ambari > Spark > Configs > Custom Spark2-defaults** (Ambari > Spark > Configuraciones > Valore predeterminados de Spark2 personalizados). **Agregue la propiedad** como se indica a continuación. Use `:` para separar las rutas de acceso si necesita agregar más de una. Se permiten los patrones globales.

    ```
    spark.driver.extraClassPath=/usr/libs/sparklibs/*
    spark.executor.extraClassPath=/usr/libs/sparklibs/*
    ```

   ![Cambie la configuración predeterminada de Spark](./media/apache-spark-manage-dependencies/change-spark-default-config.png "Cambie la configuración predeterminada de Spark")

3. Guarde las configuraciones modificadas y reinicie los servicios afectados.

   ![Reinicie los servicios afectados](./media/apache-spark-manage-dependencies/restart-impacted-services.png "Reinicie los servicios afectados")

Puede automatizar los pasos con [acciones de script](../hdinsight-hadoop-customize-cluster-linux.md). La acción de script para [agregar bibliotecas personalizadas de Hive](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh) es una buena referencia. Al cambiar las configuraciones del servicio Spark, asegúrese de usar las API de Ambari en lugar de modificar directamente los archivos de configuración. 

## <a name="safely-manage-jar-dependencies"></a>Administración segura de dependencias de JAR
El clúster de HDInsight tiene dependencias JAR integradas y de vez en cuando se realizan actualizaciones para estas versiones de JAR. Para evitar conflictos de versión entre los archivos JAR integrados y los archivos JAR que se proporcionan como referencia, considere la posibilidad de [sombrear las dependencias de la aplicación](./safely-manage-jar-dependency.md).

## <a name="python-packages-for-one-spark-job"></a>Paquetes Python para un trabajo de Spark
### <a name="use-jupyter-notebook"></a>Uso de Jupyter Notebook
El kernel de PySpark de Jupyter Notebook para HDInsight no admite la instalación de paquetes Python directamente desde el repositorio de paquetes de PyPi o Anaconda. Si tiene dependencias `.zip`, `.egg` o `.py` y quiere hacer referencia a ellas para una sesión de Spark, siga estos pasos:

1. Ejecute los ejemplos de acciones de script siguientes para copiar los archivos `.zip`, `.egg` o `.py` del almacenamiento principal `wasb://mycontainer@mystorageaccount.blob.core.windows.net/libs/*` al sistema de archivos local `/usr/libs/pylibs` del clúster. Este paso es necesario cuando Linux usa `:` para separar la lista de rutas de acceso de búsqueda, pero HDInsight solo admite las rutas de acceso de almacenamiento con un esquema parecido a `wasb://`. La ruta de acceso de almacenamiento remoto no funcionará correctamente cuando se use `sys.path.insert`.

    ```bash
    sudo mkdir -p /usr/libs/pylibs
    sudo hadoop fs -copyToLocal wasb://mycontainer@mystorageaccount.blob.core.windows.net/libs/*.* /usr/libs/pylibs
    ```

2. En el cuaderno, ejecute el código siguiente en una celda de código con el kernel de PySpark:

   ```python
   import sys
   sys.path.insert(0, "/usr/libs/pylibs/pypackage.zip")
   ```

3. Ejecute `import` para comprobar si los paquetes se incluyeron correctamente.  

## <a name="python-packages-for-cluster"></a>Paquetes de Python para el clúster
Puede instalar paquetes de Python desde Anaconda en el clúster al usar el comando Conda a través de acciones de script. Los paquetes instalados se encuentran en el nivel de clúster y se aplican a todas las aplicaciones. 

El clúster de Spark de HDInsight tiene dos instalaciones de Python integradas, Anaconda Python 2.7 y Anaconda Python 3.5. Para obtener más información sobre la configuración predeterminada de Python para los servicios y sobre cómo instalar de forma segura paquetes de Python externos sin interrumpir el clúster, consulte más detalles en [Administración segura de las dependencias de Python para el clúster](./apache-spark-python-package-installation.md).
