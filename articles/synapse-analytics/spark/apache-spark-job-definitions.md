---
title: 'Tutorial: Apache Spark para Azure Synapse Analytics: Definición de trabajo de Apache Spark para Synapse'
description: 'Tutorial: Uso de Azure Synapse Analytics para crear definiciones de trabajos de Spark y enviarlas a un grupo de Apache Spark para Azure Synapse Analytics.'
author: hrasheed-msft
ms.author: jejiang
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: tutorial
ms.date: 04/15/2020
ms.openlocfilehash: 5fc9dffaa73d195c842381b6682a00e9834c0fe7
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/19/2020
ms.locfileid: "83587942"
---
# <a name="tutorial-use-azure-synapse-analytics-to-create-apache-spark-job-definitions-for-synapse-spark-pools"></a>Tutorial: Uso de Azure Synapse Analytics para crear definiciones de trabajos de Spark y enviarlas a un grupo de Synapse Spark

En este tutorial se muestra cómo usar Azure Synapse Analytics para crear definiciones de trabajos de Spark y enviarlas a un grupo de Synapse Spark. Puede usar el complemento de varias maneras:

* Desarrollar y enviar una definición de trabajo de Spark en un grupo de Synapse Spark.
* Ver los detalles del trabajo después del envío.

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Desarrollar y enviar una definición de trabajo de Spark en un grupo de Synapse Spark.
> * Ver los detalles del trabajo después del envío.

## <a name="prerequisites"></a>Prerrequisitos

* Un área de trabajo de Azure Synapse Analytics. Para obtener instrucciones, consulte [Creación de un área de trabajo de Azure Synapse Analytics](../../machine-learning/how-to-manage-workspace.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#create-a-workspace).

## <a name="get-started"></a>Introducción

Antes de enviar una definición de trabajo de Spark, debe ser el propietario de los datos del blob de almacenamiento del sistema de archivos de ADLS Gen2 con el que desea trabajar. Si no lo es, debe agregar el permiso manualmente.

### <a name="scenario-1-add-permission"></a>Escenario 1: Adición de permisos

1. Abra [Microsoft Azure](https://ms.portal.azure.com) y, a continuación, abra la cuenta de almacenamiento.

2. Haga clic en **Contenedores** y, a continuación, cree un **sistema de archivos**. En este tutorial se usa `sparkjob`.

    ![Clic en el botón Enviar para enviar la definición de trabajo de Spark](./media/apache-spark-job-definitions/open-azure-container.png)

    ![Cuadro de diálogo de envío de Spark](./media/apache-spark-job-definitions/create-new-filesystem.png)

3. Abra `sparkjob`, haga clic en **Access Control (IAM)** , haga clic en **Agregar** y seleccione **Agregar asignación de roles**.

    ![Clic en el botón Enviar para enviar la definición de trabajo de Spark](./media/apache-spark-job-definitions/add-role-assignment-01.png)

    ![Clic en el botón Enviar para enviar la definición de trabajo de Spark](./media/apache-spark-job-definitions/add-role-assignment-02.png)

4. Haga clic en **Asignaciones de roles**, escriba el nombre de usuario y, a continuación, compruebe el rol de usuario.

    ![Clic en el botón Enviar para enviar la definición de trabajo de Spark](./media/apache-spark-job-definitions/verify-user-role.png)

### <a name="scenario-2-prepare-folder-structure"></a>Escenario 2: Preparación de la estructura de carpetas

Antes de enviar una definición de trabajo de Spark, debe cargar los archivos en ADLS Gen2 y preparar la estructura de carpetas allí. Usaremos el nodo de almacenamiento de Synapse Studio para almacenar los archivos.

1. Abra [Azure Synapse Analytics](https://web.azuresynapse.net/).

2. Haga clic en **Datos**, seleccione **Cuentas de almacenamiento** y cargue los archivos correspondientes en el sistema de archivos de ADLS Gen2. Se admiten Scala, Java, .NET y Python. En este tutorial se usa el ejemplo de la ilustración a modo de demostración, pero puede cambiar la estructura del proyecto como desee.

    ![Establecer el valor de la definición de trabajo de Spark](./media/apache-spark-job-definitions/prepare-project-structure.png)

## <a name="create-a-spark-job-definition"></a>Creación de una definición de trabajo de Spark

1. Abra [Azure Synapse Analytics](https://web.azuresynapse.net/)y seleccione **Develop** (Desarrollar).

2. Seleccione **Spark job definitions** (Definiciones de trabajo de Spark) en el panel izquierdo.

3. Haga clic en el nodo **Actions** (Acciones) situado a la derecha de "Spark job definitions" (Definiciones de trabajo de Spark).

     ![Creación de una definición de trabajo de Spark](./media/apache-spark-job-definitions/create-new-definition-01.png)

4. En la lista desplegable **Actions** (Acciones), seleccione **New Spark job definition** (Nueva definición de trabajo de Spark).

     ![Creación de una definición de trabajo de Spark](./media/apache-spark-job-definitions/create-new-definition-02.png)

5. En la ventana New Spark job definition (Nueva definición de trabajo de Spark), seleccione el lenguaje y, a continuación, proporcione la siguiente información:  

   * En **Language** (Lenguaje), seleccione **Spark(Scala)** .

    |  Propiedad   | Descripción   |  
    | ----- | ----- |  
    |Job definition name (Nombre de la definición de trabajo)| Proporcione un nombre para la definición de trabajo de Spark.  En este tutorial se usa `job definition sample`. Este nombre se puede actualizar en cualquier momento hasta que se publique.|  
    |Main definition file (Archivo de definición principal)| Archivo principal usado para el trabajo. Seleccione un archivo JAR del almacenamiento. Puede seleccionar **Upload file** (Cargar archivo) para cargar un archivo en una cuenta de almacenamiento. |
    |Nombre de clase principal| Identificador completo o clase principal que se encuentra en el archivo de definición principal.|
    |Command-line arguments (Argumentos de la línea de comandos)| Argumentos opcionales del trabajo.|
    |Reference files (Archivos de referencia)| Archivos adicionales usados como referencia en el archivo de definición principal. Puede seleccionar **Upload file** (Cargar archivo) para cargar un archivo en una cuenta de almacenamiento.|
    |Grupo de Spark| El trabajo se enviará al grupo de Spark seleccionado.|
    |Spark version (Versión de Spark)| Versión de Spark que se ejecuta en el grupo de Spark.|
    |Ejecutores| Número de ejecutores que se van a proporcionar en el grupo de Spark especificado para el trabajo.|
    |Executor size (Tamaño del ejecutor)| Número de núcleos y memoria que se van a usar para los ejecutores proporcionados en el grupo de Spark especificado para el trabajo.|  
    |Driver size (Tamaño del controlador)| Número de núcleos y memoria que se van a usar para el controlador proporcionado en el grupo de Spark especificado para el trabajo.|

    ![Establecer el valor de la definición de trabajo de Spark](./media/apache-spark-job-definitions/create-scala-definition.png)

   * En **Language** (Lenguaje), seleccione **PySpark(Python)** .

    |  Propiedad   | Descripción   |  
    | ----- | ----- |  
    |Job definition name (Nombre de la definición de trabajo)| Proporcione un nombre para la definición de trabajo de Spark.  En este tutorial se usa `job definition sample`. Este nombre se puede actualizar en cualquier momento hasta que se publique.|  
    |Main definition file (Archivo de definición principal)| Archivo principal usado para el trabajo. Seleccione un archivo PY en el almacenamiento. Puede seleccionar **Upload file** (Cargar archivo) para cargar un archivo en una cuenta de almacenamiento.|
    |Command-line arguments (Argumentos de la línea de comandos)| Argumentos opcionales del trabajo.|
    |Reference files (Archivos de referencia)| Archivos adicionales usados como referencia en el archivo de definición principal. Puede seleccionar **Upload file** (Cargar archivo) para cargar un archivo en una cuenta de almacenamiento.|
    |Grupo de Spark| El trabajo se enviará al grupo de Spark seleccionado.|
    |Spark version (Versión de Spark)| Versión de Spark que se ejecuta en el grupo de Spark.|
    |Ejecutores| Número de ejecutores que se van a proporcionar en el grupo de Spark especificado para el trabajo.|
    |Executor size (Tamaño del ejecutor)| Número de núcleos y memoria que se van a usar para los ejecutores proporcionados en el grupo de Spark especificado para el trabajo.|  
    |Driver size (Tamaño del controlador)| Número de núcleos y memoria que se van a usar para el controlador proporcionado en el grupo de Spark especificado para el trabajo.|

    ![Establecer el valor de la definición de trabajo de Spark](./media/apache-spark-job-definitions/create-py-definition.png)

   * En **Language** (Lenguaje), seleccione **.NET Spark(C#/F#)** .

    |  Propiedad   | Descripción   |  
    | ----- | ----- |  
    |Job definition name (Nombre de la definición de trabajo)| Proporcione un nombre para la definición de trabajo de Spark.  En este tutorial se usa `job definition sample`. Este nombre se puede actualizar en cualquier momento hasta que se publique.|  
    |Main definition file (Archivo de definición principal)| Archivo principal usado para el trabajo. Seleccione un archivo ZIP que contenga la aplicación de .NET para Spark (es decir, el archivo ejecutable principal, los archivos DLL que contienen las funciones definidas por el usuario y otros archivos necesarios) del almacenamiento. Puede seleccionar **Upload file** (Cargar archivo) para cargar un archivo en una cuenta de almacenamiento.|
    |Main executable file (Archivo ejecutable principal)| Archivo ejecutable principal del archivo ZIP de la definición principal.|
    |Command-line arguments (Argumentos de la línea de comandos)| Argumentos opcionales del trabajo.|
    |Reference files (Archivos de referencia)| Archivos adicionales necesarios para que los nodos de trabajo ejecuten la aplicación de .NET para Spark y que no se incluyen en el archivo ZIP de la definición principal (es decir, archivos JAR dependientes, archivos DLL adicionales de funciones definidas por el usuario y otros archivos de configuración). Puede seleccionar **Upload file** (Cargar archivo) para cargar un archivo en una cuenta de almacenamiento.|
    |Grupo de Spark| El trabajo se enviará al grupo de Spark seleccionado.|
    |Spark version (Versión de Spark)| Versión de Spark que se ejecuta en el grupo de Spark.|
    |Ejecutores| Número de ejecutores que se van a proporcionar en el grupo de Spark especificado para el trabajo.|
    |Executor size (Tamaño del ejecutor)| Número de núcleos y memoria que se van a usar para los ejecutores proporcionados en el grupo de Spark especificado para el trabajo.|  
    |Driver size (Tamaño del controlador)| Número de núcleos y memoria que se van a usar para el controlador proporcionado en el grupo de Spark especificado para el trabajo.|

    ![Establecer el valor de la definición de trabajo de Spark](./media/apache-spark-job-definitions/create-net-definition.png)

6. Seleccione **Publish** (Publicar) para guardar la definición de trabajo de Spark.

    ![Publicación de una definición de trabajo de Spark](./media/apache-spark-job-definitions/publish-net-definition.png)

## <a name="submit-a-spark-job-definition"></a>Envío de una definición de trabajo de Spark

Después de crear una definición de trabajo de Spark, puede enviarla a un grupo de Synapse Spark. Asegúrese de que ha realizado los pasos descritos en **Introducción** antes de probar los ejemplos de esta parte.

### <a name="scenario-1-submit-spark-job-definition"></a>Escenario 1: Envío de una definición de trabajo de Spark

1. Haga clic en una ventana de definición de trabajo de Spark para abrirla.

      ![Abrir definición de trabajo de Spark para enviar ](./media/apache-spark-job-definitions/open-spark-definition.png)

2. Haga clic en el icono **Submit** (Enviar) para enviar el proyecto al grupo de Spark seleccionado. Puede hacer clic en la pestaña **Spark monitoring URL** (Dirección URL de supervisión de Spark) para ver la consulta del registro de la aplicación Spark.

    ![Clic en el botón Enviar para enviar la definición de trabajo de Spark](./media/apache-spark-job-definitions/submit-spark-definition.png)

    ![Cuadro de diálogo de envío de Spark](./media/apache-spark-job-definitions/submit-definition-result.png)

### <a name="scenario-2-view-spark-job-running-progress"></a>Escenario 2: Visualización del progreso de ejecución del trabajo de Spark

1. Haga clic en **Monitor** (Supervisar) y, a continuación, seleccione la opción **Spark applications** (Aplicaciones Spark). Podrá ver la aplicación Spark enviada.

    ![Ver aplicación Spark](./media/apache-spark-job-definitions/view-spark-application.png)

2. A continuación, haga clic en la aplicación Spark para mostrar la ventana **Log Query** (Consulta del registro). Puede ver el progreso de la ejecución del trabajo desde **Log Query** (Consulta del registro).

    ![Ver consulta del registro de la aplicación Spark](./media/apache-spark-job-definitions/view-job-log-query.png)

### <a name="scenario-3-check-output-file"></a>Escenario 3: Comprobación del archivo de salida

 1. Haga clic en **Data** (Datos) y, a continuación, seleccione **Storage accounts** (Cuentas de almacenamiento). Después de una ejecución correcta, puede ir al almacenamiento de ADLS Gen2 y comprobar que se han generado las salidas.

    ![Ver archivo de salida](./media/apache-spark-job-definitions/view-output-file.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha mostrado cómo usar Azure Synapse Analytics para crear definiciones de trabajos de Spark y enviarlas a un grupo de Synapse Spark. A continuación, puede usar Azure Synapse Analytics para crear conjuntos de datos de Power BI y administrar los datos de Power BI. 

- [Conectarse a los datos en Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-quickstart-connect-to-data)
- [Visualización con Power BI](../sql-data-warehouse/sql-data-warehouse-get-started-visualize-with-power-bi.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
