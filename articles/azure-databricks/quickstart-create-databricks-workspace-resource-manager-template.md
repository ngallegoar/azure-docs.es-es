---
title: 'Inicio rápido: Creación de un área de trabajo de Azure Databricks con Azure Resource Manager'
description: En este inicio rápido se muestra cómo usar la plantilla de Azure Resource Manager para crear un área de trabajo de Azure Databricks y, después, crear un clúster de Apache Spark y ejecutar un trabajo de Spark.
services: azure-databricks
ms.service: azure-databricks
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.workload: big-data
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/23/2020
ms.openlocfilehash: d3c3c55a4ce3ee25db01128dcf50bb8763c5829b
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604640"
---
# <a name="quickstart-run-a-spark-job-on-azure-databricks-using-the-azure-resource-manager-template"></a>Inicio rápido: Ejecución de un trabajo de Spark en Azure Databricks con una plantilla de Azure Resource Manager

En este inicio rápido usará una plantilla de Azure Resource Manager para crear un área de trabajo de Azure Databricks con un clúster de Apache Spark. Ejecutará un trabajo en el clúster y usará gráficos personalizados para generar informes en tiempo real de uso gratuito/pagado basado en datos demográficos.

## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/)

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en [Azure Portal](https://portal.azure.com).

> [!Note]
> Este tutorial no puede llevarse a cabo mediante una **suscripción de evaluación gratuita de Azure**.
> Si tiene una cuenta gratuita, vaya a su perfil y cambiar la suscripción a **pago por uso**. Para más información consulte el sitio de [cuentas gratuitas de Azure](https://azure.microsoft.com/free/). Después, [quite el límite de gasto](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit) y [solicite un aumento de la cuota](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) para las vCPU de su región. Cuando crea su área de trabajo de Azure Databricks, puede seleccionar el plan de tarifa de la **Trial (Premium - 14-Days Free DBUs)** para que el área de trabajo acceda a las DBU Premium de Azure Databricks gratis durante 14 días.

## <a name="create-an-azure-databricks-workspace"></a>Creación de un área de trabajo de Azure Databricks

En esta sección, creará un área de trabajo de Azure Databricks mediante una plantilla de Resource Manager.

1. Haga clic en la imagen siguiente para abrir la plantilla en el Portal de Azure.

   [![Implementación en Azure](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-databricks-workspace%2Fazuredeploy.json)

2. Proporcione los valores necesarios para crear el área de trabajo de Azure Databricks.

   ![Creación de un área de trabajo de Azure Databricks con una plantilla de Azure Resource Manager](./media/quickstart-create-databricks-workspace-resource-manager-template/create-databricks-workspace-using-resource-manager-template.png "Creación de un área de trabajo de Azure Databricks con una plantilla de Azure Resource Manager")

   Proporcione los valores siguientes:

   |Propiedad  |Descripción  |
   |---------|---------|
   |**Suscripción**     | En el cuadro desplegable, seleccione la suscripción de Azure.        |
   |**Grupos de recursos**     | Especifique si desea crear un nuevo grupo de recursos o utilizar uno existente. Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. Para más información, consulte [Información general del grupo de recursos de Azure](../azure-resource-manager/management/overview.md). |
   |**Ubicación**     | Seleccione **Este de EE. UU. 2**. Para otras regiones disponibles, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).        |
   |**Workspace name** (Nombre del área de trabajo)     | Proporcione un nombre para el área de trabajo de Databricks        |
   |**Plan de tarifa**     |  Elija entre **Standard** o **Premium**. Para más información sobre estos planes, consulte la [página de precios de Databricks](https://azure.microsoft.com/pricing/details/databricks/).       |

3. Seleccione **I agree to the terms and conditions stated above** (Acepto los términos y condiciones indicados anteriormente) y **Anclar al panel** y haga clic en **Purchase** (Comprar).

4. Se tarda unos minutos en crear el área de trabajo. Durante la creación del área de trabajo, el portal muestra el icono **Enviando implementación para Azure Databricks** a la derecha. Puede que deba desplazarse a la derecha del panel para ver el icono. También hay una barra de progreso que se muestra en la parte superior de la pantalla. Puede ver cualquier área para el progreso.

   ![Icono de implementación de Databricks](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-deployment-tile.png "Icono de implementación de Databricks")

   Cuando se produce un error en una implementación del área de trabajo, esta todavía se crea en un estado de error. Elimine el área de trabajo con errores y cree una nueva que resuelva los errores de implementación. Al eliminar el área de trabajo con errores, también se eliminan el grupo de recursos administrados y los recursos implementados correctamente.

## <a name="create-a-spark-cluster-in-databricks"></a>Creación de un clúster de Spark en Databricks

1. En Azure Portal, vaya al área de trabajo de Databricks que ha creado y, después, haga clic en **Launch Workspace** (Iniciar área de trabajo).

2. Se le redirigirá al portal de Azure Databricks. Desde el portal, haga clic en **Clúster**.

   ![Databricks en Azure](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-on-azure.png "Databricks en Azure")

3. En la página **Nuevo clúster**, proporcione los valores para crear un clúster.

   ![Creación de clústeres de Spark para Databricks en Azure](./media/quickstart-create-databricks-workspace-resource-manager-template/create-databricks-spark-cluster.png "Creación de clústeres de Spark para Databricks en Azure")

   Acepte los demás valores predeterminados, salvo los siguientes:

   * Escriba un nombre para el clúster.
   * Para este artículo, cree un clúster con el entorno en tiempo de ejecución **4.0**.
   * Asegúrese de que selecciona la casilla **Terminate after \_\_ minutes of inactivity** (Terminar después de \_\_ minutos de inactividad). Proporcione una duración (en minutos) para terminar el clúster, si este no se usa.

   Seleccione **Create cluster** (Crear clúster). Una vez que el clúster se está ejecutando, puede asociarle notebooks y ejecutar trabajos de Spark.

Para obtener más información sobre la creación de clústeres, consulte [Create a Spark cluster in Azure Databricks](/azure/databricks/clusters/create) (Creación de un clúster de Spark en Azure Databricks).

## <a name="run-a-spark-sql-job"></a>Ejecución de un trabajo de Spark SQL

Antes de empezar esta sección, debe completar lo siguientes requisitos previos:

* [Cree una cuenta de Azure Blob Storage](../storage/common/storage-account-create.md).
* Descargue un archivo JSON de ejemplo [desde GitHub](https://github.com/Azure/usql/blob/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json).
* Cargue el archivo JSON de ejemplo en la cuenta de Azure Blob Storage que ha creado. Puede usar [Explorador de Microsoft Azure Storage](../vs-azure-tools-storage-manage-with-storage-explorer.md) para cargar archivos.

Realice las tareas siguientes para crear un cuaderno en Databricks, configúrelo para leer los datos de una cuenta de Azure Blob Storage y, a continuación, ejecute un trabajo de Spark SQL en los datos.

1. En el panel izquierdo, haga clic en **Área de trabajo**. Desde la lista desplegable **Área de trabajo**, haga clic en **Crear** y, a continuación, haga clic en **Notebook**.

   ![Creación de un cuaderno en Databricks](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-create-notebook.png "Creación de un cuaderno en Databricks")

2. En el cuadro de diálogo **Creación de notebook**, escriba un nombre, seleccione **Scala** como lenguaje y seleccione el clúster de Spark que creó anteriormente.

   ![Creación de un cuaderno en Databricks](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-notebook-details.png "Creación de un cuaderno en Databricks")

   Haga clic en **Crear**.

3. En este paso, asocie la cuenta de Azure Storage con el clúster de Databricks Spark. Hay dos maneras de hacerlo. Puede montar la cuenta de Azure Storage en el sistema de archivos de Databricks (DBFS) o acceder directamente a la cuenta de Azure Storage desde la aplicación que cree.

   > [!IMPORTANT]
   >En este artículo se utiliza el enfoque para **montar el almacenamiento con DBFS**. Este enfoque garantiza que el almacenamiento montado se asocia con el propio sistema de archivos del clúster. De esa forma, cualquier aplicación que acceda al clúster puede usar también el almacenamiento asociado. El enfoque del acceso directo está limitado a la aplicación desde donde se configura el acceso.
   >
   > Para usar el enfoque del montaje, es preciso crear un clúster de Spark con la versión **4.0** del entorno en tiempo de ejecución de Databricks, que es lo que ha elegido en este artículo.

   En el siguiente fragmento de código, reemplace `{YOUR CONTAINER NAME}`, `{YOUR STORAGE ACCOUNT NAME}` y `{YOUR STORAGE ACCOUNT ACCESS KEY}` por los valores adecuados de su cuenta de Azure Storage. Pegue el fragmento de código siguiente en una celda vacía del notebook y, después, presione MAYÚS + ENTRAR para ejecutar la celda de código.

   * **Montaje de la cuenta de almacenamiento con DBFS (se recomienda)** . En este fragmento de código, la ruta de acceso de la cuenta de Azure Storage se monta en `/mnt/mypath`. Por consiguiente, en todas las futuras ocurrencias en las que acceda a la cuenta de Azure Storage no es preciso que dé la ruta de acceso completa. Puede utilizar `/mnt/mypath`.

          dbutils.fs.mount(
            source = "wasbs://{YOUR CONTAINER NAME}@{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net/",
            mountPoint = "/mnt/mypath",
            extraConfigs = Map("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net" -> "{YOUR STORAGE ACCOUNT ACCESS KEY}"))

   * **Acceso directo a la cuenta de almacenamiento**

          spark.conf.set("fs.azure.account.key.{YOUR STORAGE ACCOUNT NAME}.blob.core.windows.net", "{YOUR STORAGE ACCOUNT ACCESS KEY}")

     Para más información sobre cómo recuperar las claves de acceso de las cuentas de almacenamiento, consulte [Administración de claves de acceso de cuentas de almacenamiento](../storage/common/storage-account-keys-manage.md).

   > [!NOTE]
   > También puede usar Azure Data Lake Store con un clúster de Spark en Azure Databricks. Para obtener instrucciones, consulte [Use Data Lake Store with Azure Databricks](/azure/databricks/data/data-sources/azure/azure-datalake-gen2) (Uso de Data Lake Store con Azure Databricks).

4. Ejecute una instrucción SQL para crear una tabla temporal con datos desde el archivo de datos JSON de ejemplo, **small_radio_json.json**. En el siguiente fragmento de código, reemplace los valores del marcador de posición con el nombre del contenedor y el nombre de la cuenta de almacenamiento. Pegue el fragmento de código en una celda vacía del notebook y, después, presione MAYÚS + ENTRAR. En el fragmento de código, `path` indica la ubicación del archivo JSON de ejemplo que cargó en su cuenta de Azure Storage.

   ```sql
   %sql
   DROP TABLE IF EXISTS radio_sample_data;
   CREATE TABLE radio_sample_data
   USING json
   OPTIONS (
    path "/mnt/mypath/small_radio_json.json"
   )
   ```

   Una vez que el comando se completa correctamente, tiene todos los datos desde el archivo JSON como una tabla en el clúster de Databricks.

   El comando mágico del lenguaje `%sql` le permite ejecutar un código SQL desde el notebook, aunque este sea de otro tipo. Para obtener más información, consulte [Combinación de lenguajes en un notebook](/azure/databricks/notebooks/index).

5. Echemos un vistazo a una instantánea de los datos JSON de ejemplo para entender mejor la consulta que se ejecuta. Pegue el siguiente fragmento de código en una celda vacía y presione **MAYÚS + ENTRAR**.

   ```sql
   %sql
   SELECT * from radio_sample_data
   ```

6. Verá un resultado tabular como se muestra en la siguiente captura de pantalla (solo se ven algunas columnas):

   ![Datos JSON de ejemplo](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-sample-csv-data.png "Datos JSON de ejemplo")

   Entre otros detalles, los datos de ejemplo capturan el género de la audiencia de un canal de radio (nombre de columna, **género**) y si su suscripción es gratis o de pago (nombre de columna, **nivel**).

7. Ahora cree una representación visual de estos datos para mostrar para cada género, cuántos usuarios tienen cuentas gratis y cuántas son de suscriptores pagados. En la parte inferior de la salida tabular, haga clic en el icono **Gráfico de barras** y, a continuación, haga clic en **Opciones de trazado**.

   ![Creación de un gráfico de barras](./media/quickstart-create-databricks-workspace-resource-manager-template/create-plots-databricks-notebook.png "Creación de un gráfico de barras")

8. En **Personalizar trazado**, arrastre y coloque los valores como se muestra en la captura de pantalla.

   ![Personalización del gráfico de barras](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-notebook-customize-plot.png "Personalización del gráfico de barras")

   * Establezca **Claves** en **gender** (género).
   * Establezca **Agrupaciones de serie** en **level** (nivel).
   * Establezca **Valores** en **level** (nivel).
   * Establezca **Agregación** en **COUNT** (recuento).

   Haga clic en **Aplicar**.

9. El resultado muestra la representación visual, como se describe en la siguiente captura de pantalla:

   ![Personalización del gráfico de barras](./media/quickstart-create-databricks-workspace-resource-manager-template/databricks-sql-query-output-bar-chart.png "Personalización del gráfico de barras")

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado el artículo, puede finalizar el clúster. Para ello, desde el área de trabajo de Azure Databricks, en el panel izquierdo, seleccione **Clusters** (Clústeres). En el clúster que desea finalizar, mueva el cursor sobre el botón de puntos suspensivos en la columna **Actions** (Acciones) y seleccione el icono **Terminate** (Finalizar).

![Detención de un clúster de Databricks](./media/quickstart-create-databricks-workspace-resource-manager-template/terminate-databricks-cluster.png "Detención de un clúster de Databricks")

Si no finaliza manualmente el clúster, este se detendrá automáticamente si seleccionó la casilla **Terminate after \_\_ minutes of inactivity** (Finalizar después de \_\_ minutos de inactividad) al crear el clúster. En tal caso, el clúster se detiene automáticamente si ha estado inactivo durante el tiempo especificado.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, creó un clúster de Spark en Azure Databricks y ejecutó un trabajo de Spark utilizando los datos de Azure Storage. También puede mirar [Spark data sources](/azure/databricks/data/data-sources/index) (Orígenes de datos de Spark) para aprender a importar datos desde otros orígenes de datos en Azure Databricks. También puede ver la plantilla de Resource Manager para [crear un área de trabajo de Azure Databricks con dirección VNET personalizada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-databricks-workspace-with-custom-vnet-address). Para la sintaxis y las propiedades de JSON que se usan en una plantilla, consulte la referencia de la plantilla [Microsoft.Databricks/workspaces](/azure/templates/microsoft.databricks/workspaces).

Vaya al siguiente artículo para aprender cómo realizar una operación ETL (extraer, transformar y cargar datos) mediante Azure Databricks.

> [!div class="nextstepaction"]
> [Extracción, transformación y carga de datos mediante Azure Databricks](databricks-extract-load-sql-data-warehouse.md)
