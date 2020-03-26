---
title: 'Tutorial: Realización de operaciones de ETL mediante Azure Databricks'
description: En este tutorial aprenderá a extraer datos de Data Lake Storage Gen2 a Azure Databricks, transformarlos y luego cargarlos en Azure SQL Data Warehouse.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.custom: mvc
ms.topic: tutorial
ms.date: 01/29/2020
ms.openlocfilehash: 8819b79a105b7a654a34e47c5ba9b3d351a1d926
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239412"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-azure-databricks"></a>Tutorial: Extracción, transformación y carga de datos mediante Azure Databricks

En este tutorial, realizará una operación de ETL (extracción, transformación y carga de datos) mediante Azure Databricks. Extraerá datos de Azure Data Lake Storage Gen2 a Azure Databricks, donde ejecutará transformaciones en ellos y, luego, cargará los datos transformados en Azure SQL Data Warehouse.

En los pasos de este tutorial se usa el conector SQL Data Warehouse para Azure Databricks para transferir los datos a Azure Databricks. Este conector usa, a su vez, Azure Blob Storage como almacenamiento temporal para los datos transferidos entre un clúster de Azure Databricks y Azure SQL Data Warehouse.

La siguiente ilustración muestra el flujo de la aplicación:

![Azure Databricks con Data Lake Store y SQL Data Warehouse](./media/databricks-extract-load-sql-data-warehouse/databricks-extract-transform-load-sql-datawarehouse.png "Azure Databricks con Data Lake Store y SQL Data Warehouse")

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Cree un servicio de Azure Databricks.
> * Crear un clúster de Spark en Azure Databricks.
> * Crear un sistema de archivos en la cuenta de Data Lake Storage Gen2.
> * Cargar datos de ejemplo en la cuenta de Azure Data Lake Storage Gen2.
> * Crear una entidad de servicio.
> * Extraer datos de ejemplo de la cuenta de Azure Data Lake Storage Gen2.
> * Transformar datos en Azure Databricks.
> * Cargar datos en Azure SQL Data Warehouse

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!Note]
> Este tutorial no puede llevarse a cabo mediante una **suscripción de evaluación gratuita de Azure**.
> Si tiene una cuenta gratuita, vaya a su perfil y cambiar la suscripción a **pago por uso**. Para más información consulte el sitio de [cuentas gratuitas de Azure](https://azure.microsoft.com/free/). Después, [quite el límite de gasto](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit) y [solicite un aumento de la cuota](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) para las vCPU de su región. Cuando crea su área de trabajo de Azure Databricks, puede seleccionar el plan de tarifa de la **Trial (Premium - 14-Days Free DBUs)** para que el área de trabajo acceda a las DBU Premium de Azure Databricks gratis durante 14 días.
     
## <a name="prerequisites"></a>Prerrequisitos

Complete estas tareas antes de comenzar este tutorial:

* Cree una instancia de Azure SQL Data Warehouse y una regla de firewall de nivel de servidor y conéctese al servidor como administrador. Consulte [Quickstart: Creación de una instancia de Azure SQL Data Warehouse en Azure Portal, y realización de consultas en ella](../synapse-analytics/sql-data-warehouse/create-data-warehouse-portal.md)

* Cree una clave maestra para la instancia de Azure SQL Data Warehouse. Consulte [Crear la clave maestra de una base de datos](https://docs.microsoft.com/sql/relational-databases/security/encryption/create-a-database-master-key).

* Ha creado una cuenta de Azure Blob Storage y, dentro de ella, un contenedor. Además, ha recuperado la clave de acceso para acceder a la cuenta de almacenamiento. Consulte [Quickstart: Carga, descarga y enumeración de blobs con Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md)

* Cree una cuenta de almacenamiento de Azure Data Lake Storage Gen2. Consulte [Quickstart: Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-quickstart-create-account.md).

* Crear una entidad de servicio. Consulte [Cómo: portal para crear una aplicación de Azure AD y una entidad de servicio que puedan acceder a los recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

   Hay un par de cosas que tendrá que hacer cuando realice los pasos de este artículo.

   * Al realizar los pasos de la sección [Asignación de la aplicación a un rol](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application) del artículo, asegúrese de asignar el rol **Colaborador de datos de blobs de almacenamiento** a la entidad de servicio en el ámbito de la cuenta de Data Lake Storage Gen2. Si asigna el rol al grupo de recursos o a la suscripción principales, recibirá errores relacionados con los permisos hasta que esas asignaciones de roles se propaguen a la cuenta de almacenamiento.

      Si prefiere usar una lista de control de acceso (ACL) para asociar la entidad de servicio con un archivo o un directorio específicos, consulte [Control de acceso en Azure Data Lake Storage Gen2](../storage/blobs/data-lake-storage-access-control.md).

   * Al realizar los pasos de la sección [Obtención de valores para iniciar sesión](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) del artículo, pegue los valores de identificador de inquilino, identificador de aplicación y secreto en un archivo de texto. ya que los necesitará pronto.

* Inicie sesión en [Azure Portal](https://portal.azure.com/).

## <a name="gather-the-information-that-you-need"></a>Recopilación de la información que necesita

Asegúrese de completar los requisitos previos de este tutorial.

   Antes de comenzar, debe tener esta información:

   :heavy_check_mark:  El nombre de la base de datos, el nombre del servidor de base de datos, el nombre de usuario y la contraseña de Azure SQL Data Warehouse.

   :heavy_check_mark:  La clave de acceso de su cuenta de Blob Storage.

   :heavy_check_mark:  El nombre de la cuenta de almacenamiento de Data Lake Storage Gen2.

   :heavy_check_mark:  Identificador de inquilino de su suscripción.

   :heavy_check_mark:  Identificador de la aplicación que haya registrado con Azure Active Directory (Azure AD).

   :heavy_check_mark:  La clave de autenticación de la aplicación que ha registrado con Azure AD.

## <a name="create-an-azure-databricks-service"></a>Creación de un servicio de Azure Databricks

En esta sección, va a crear un servicio de Azure Databricks con Azure Portal.

1. En el menú de Azure Portal, seleccione **Crear un recurso**.

    ![Creación de un recurso en Azure Portal](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-on-portal.png)

    A continuación, seleccione **Análisis** > **Azure Databricks**.

    ![Creación de Azure Databricks en Azure Portal](./media/databricks-extract-load-sql-data-warehouse/azure-databricks-resource-create.png)



2. En **Servicio de Azure Databricks**, proporcione los valores siguientes para crear un servicio de Databricks:

    |Propiedad  |Descripción  |
    |---------|---------|
    |**Workspace name** (Nombre del área de trabajo)     | Proporcione un nombre para el área de trabajo de Databricks.        |
    |**Suscripción**     | En el cuadro desplegable, seleccione la suscripción de Azure.        |
    |**Grupos de recursos**     | Especifique si desea crear un nuevo grupo de recursos o utilizar uno existente. Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. Para más información, consulte [Información general del grupo de recursos de Azure](../azure-resource-manager/management/overview.md). |
    |**Ubicación**     | Seleccione **Oeste de EE. UU. 2**.  Para otras regiones disponibles, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).      |
    |**Plan de tarifa**     |  Seleccione **Estándar**.     |

3. La cuenta tarda unos minutos en crearse. Para supervisar el estado de la operación, consulte la barra de progreso en la parte superior.

4. Seleccione **Anclar al panel** y, después, seleccione **Crear**.

## <a name="create-a-spark-cluster-in-azure-databricks"></a>Creación de un clúster de Spark en Azure Databricks

1. En Azure Portal, vaya al servicio de Databricks que ha creado y, después, seleccione **Launch Workspace** (Iniciar área de trabajo).

2. Se le redirigirá al portal de Azure Databricks. En el portal, seleccione **Cluster**.

    ![Databricks en Azure](./media/databricks-extract-load-sql-data-warehouse/databricks-on-azure.png "Databricks en Azure")

3. En la página **Nuevo clúster**, proporcione los valores para crear un clúster.

    ![Creación de clústeres de Spark para Databricks en Azure](./media/databricks-extract-load-sql-data-warehouse/create-databricks-spark-cluster.png "Creación de clústeres de Spark para Databricks en Azure")

4. Rellene los valores para los campos siguientes y acepte los valores predeterminados para el resto de campos:

    * Escriba un nombre para el clúster.

    * Asegúrese de que selecciona la casilla **Terminate after \_\_ minutes of inactivity** (Terminar después de \_\_ minutos de inactividad). Si no se usa el clúster, proporcione una duración (en minutos) para terminar el clúster.

    * Seleccione **Create cluster** (Crear clúster). Después de ejecutar el clúster, puede asociarle cuadernos y ejecutar trabajos de Spark.

## <a name="create-a-file-system-in-the-azure-data-lake-storage-gen2-account"></a>Creación de un sistema de archivos en la cuenta de Azure Data Lake Storage Gen2

En esta sección, creará un cuaderno en el área de trabajo de Azure Databricks y, a continuación, ejecutará fragmentos de código para configurar la cuenta de almacenamiento.

1. En [Azure Portal](https://portal.azure.com), vaya al servicio de Azure Databricks que ha creado y, después, seleccione **Launch Workspace** (Iniciar área de trabajo).

2. A la izquierda, seleccione **Workspace** (Área de trabajo). En la lista desplegable **Workspace** (Área de trabajo), seleccione **Create** > **Notebook** (Crear > Cuaderno).

    ![Creación de un cuaderno en Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-create-notebook.png "Creación de un cuaderno en Databricks")

3. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre para el cuaderno. Seleccione **Scala** como lenguaje y, a continuación, seleccione el clúster de Spark que creó anteriormente.

    ![Proporcionar detalles de un cuaderno en Databricks](./media/databricks-extract-load-sql-data-warehouse/databricks-notebook-details.png "Proporcionar detalles de un cuaderno en Databricks")

4. Seleccione **Crear**.

5. El bloque de código siguiente establece las credenciales predeterminadas de la entidad de servicio de cualquier cuenta de ADLS Gen 2 a la que se accede en la sesión de Spark. El segundo bloque de código anexa el nombre de la cuenta a la configuración para especificar las credenciales para una cuenta específica de ADLS Gen 2.  Copie y pegue cualquiera de estos bloques de código en la primera celda del cuaderno de Azure Databricks.

   **Configuración de la sesión**

   ```scala
   val appID = "<appID>"
   val secret = "<secret>"
   val tenantID = "<tenant-id>"

   spark.conf.set("fs.azure.account.auth.type", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id", "<appID>")
   spark.conf.set("fs.azure.account.oauth2.client.secret", "<secret>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   ```

   **Configuración de la cuenta**

   ```scala
   val storageAccountName = "<storage-account-name>"
   val appID = "<app-id>"
   val secret = "<secret>"
   val fileSystemName = "<file-system-name>"
   val tenantID = "<tenant-id>"

   spark.conf.set("fs.azure.account.auth.type." + storageAccountName + ".dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type." + storageAccountName + ".dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id." + storageAccountName + ".dfs.core.windows.net", "" + appID + "")
   spark.conf.set("fs.azure.account.oauth2.client.secret." + storageAccountName + ".dfs.core.windows.net", "" + secret + "")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint." + storageAccountName + ".dfs.core.windows.net", "https://login.microsoftonline.com/" + tenantID + "/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   dbutils.fs.ls("abfss://" + fileSystemName  + "@" + storageAccountName + ".dfs.core.windows.net/")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")
   ```

6. En este bloque de código, reemplace los valores de marcador de posición `<app-id>`, `<secret>`, `<tenant-id>` y `<storage-account-name>` por los valores que recopiló al completar los requisitos previos de este tutorial. Reemplace el valor del marcador de posición `<file-system-name>` por el nombre que desea dar al sistema de archivos.

   * `<app-id>` y `<secret>` proceden de la aplicación que registró con Active Directory como parte de la creación de una entidad de servicio.

   * `<tenant-id>` procede de su suscripción.

   * `<storage-account-name>` es el nombre de la cuenta de almacenamiento de Azure Data Lake Storage Gen2.

7. Presione las teclas **MAYÚS + ENTRAR** para ejecutar el código de este bloque.

## <a name="ingest-sample-data-into-the-azure-data-lake-storage-gen2-account"></a>Ingesta de datos de ejemplo en la cuenta de Azure Data Lake Storage Gen2

Antes de empezar esta sección, debe completar lo siguientes requisitos previos:

Escriba el código siguiente en una celda del cuaderno:

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

En la celda, presione **MAYÚS + ENTRAR** para ejecutar el código.

Ahora, en una nueva celda debajo de esta, escriba el siguiente código y reemplace los valores que aparecen entre paréntesis por los mismos valores que usó anteriormente:

    dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://" + fileSystemName + "@" + storageAccountName + ".dfs.core.windows.net/")

En la celda, presione **MAYÚS + ENTRAR** para ejecutar el código.

## <a name="extract-data-from-the-azure-data-lake-storage-gen2-account"></a>Extracción de datos de ejemplo de la cuenta de Azure Data Lake Storage Gen2

1. Ahora puede cargar el archivo JSON de ejemplo como trama de datos en Azure Databricks. Pegue el código siguiente en una nueva celda. Reemplace los marcadores de posición que se muestran entre paréntesis por sus valores.

   ```scala
   val df = spark.read.json("abfss://" + fileSystemName + "@" + storageAccountName + ".dfs.core.windows.net/small_radio_json.json")
   ```
2. Presione las teclas **MAYÚS + ENTRAR** para ejecutar el código de este bloque.

3. Ejecute el código siguiente para ver el contenido de la trama de datos:

    ```scala
    df.show()
    ```
   Debe ver una salida similar al siguiente fragmento de código:

   ```output
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   |               artist|     auth|firstName|gender|itemInSession|  lastName|   length|  level|            location|method|    page| registration|sessionId|                song|status|           ts|userId|
   +---------------------+---------+---------+------+-------------+----------+---------+-------+--------------------+------+--------+-------------+---------+--------------------+------+-------------+------+
   | El Arrebato         |Logged In| Annalyse|     F|            2|Montgomery|234.57914| free  |  Killeen-Temple, TX|   PUT|NextSong|1384448062332|     1879|Quiero Quererte Q...|   200|1409318650332|   309|
   | Creedence Clearwa...|Logged In|   Dylann|     M|            9|    Thomas|340.87138| paid  |       Anchorage, AK|   PUT|NextSong|1400723739332|       10|        Born To Move|   200|1409318653332|    11|
   | Gorillaz            |Logged In|     Liam|     M|           11|     Watts|246.17751| paid  |New York-Newark-J...|   PUT|NextSong|1406279422332|     2047|                DARE|   200|1409318685332|   201|
   ...
   ...
   ```

   Ahora ha extraído los datos de Azure Data Lake Storage Gen2 en Azure Databricks.

## <a name="transform-data-in-azure-databricks"></a>Transformación de datos en Azure Databricks

Los datos de ejemplo sin procesar del archivo **small_radio_json.json** capturan la audiencia de una emisora de radio y presentan una variedad de columnas. En esta sección, va a transformar los datos para recuperar solo columnas específicas del conjunto de datos.

1. En primer lugar, recupere solo las columnas **firstName**, **lastName**, **gender**, **location** y **level** de la trama de datos que ha creado.

   ```scala
   val specificColumnsDf = df.select("firstname", "lastname", "gender", "location", "level")
   specificColumnsDf.show()
   ```

   Recibirá una salida como la que se muestra en el fragmento de código siguiente:

   ```output
   +---------+----------+------+--------------------+-----+
   |firstname|  lastname|gender|            location|level|
   +---------+----------+------+--------------------+-----+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX| free|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...| free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Tess|  Townsend|     F|Nashville-Davidso...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |     Liam|     Watts|     M|New York-Newark-J...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   |     Alan|     Morse|     M|Chicago-Napervill...| paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK| paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...| free|
   +---------+----------+------+--------------------+-----+
   ```

2. Puede transformar aún más estos datos para renombrar la columna **level** como **subscription_type**.

   ```scala
   val renamedColumnsDF = specificColumnsDf.withColumnRenamed("level", "subscription_type")
   renamedColumnsDF.show()
   ```

   Recibirá una salida como la que se muestra en el fragmento de código siguiente.

   ```output
   +---------+----------+------+--------------------+-----------------+
   |firstname|  lastname|gender|            location|subscription_type|
   +---------+----------+------+--------------------+-----------------+
   | Annalyse|Montgomery|     F|  Killeen-Temple, TX|             free|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |Gabriella|   Shelton|     F|San Jose-Sunnyval...|             free|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Tess|  Townsend|     F|Nashville-Davidso...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |     Liam|     Watts|     M|New York-Newark-J...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Elijah|  Williams|     M|Detroit-Warren-De...|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   |     Alan|     Morse|     M|Chicago-Napervill...|             paid|
   |   Dylann|    Thomas|     M|       Anchorage, AK|             paid|
   |  Margaux|     Smith|     F|Atlanta-Sandy Spr...|             free|
   +---------+----------+------+--------------------+-----------------+
   ```

## <a name="load-data-into-azure-sql-data-warehouse"></a>Carga de datos en Azure SQL Data Warehouse

En esta sección, cargará los datos transformados en Azure SQL Data Warehouse. Utilice el conector de Azure SQL Data Warehouse para Azure Databricks para cargar directamente una trama de datos como una tabla en SQL Data Warehouse.

Como se mencionó anteriormente, el conector de SQL Data Warehouse usa Azure Blob Storage como almacenamiento temporal para cargar datos entre Azure Databricks y Azure SQL Data Warehouse. Por lo tanto, para comenzar, proporcione la configuración para conectarse a la cuenta de almacenamiento. Ya debe haber creado la cuenta como parte de los requisitos previos de este artículo.

1. Proporcione la configuración para acceder a la cuenta de Azure Storage desde Azure Databricks.

   ```scala
   val blobStorage = "<blob-storage-account-name>.blob.core.windows.net"
   val blobContainer = "<blob-container-name>"
   val blobAccessKey =  "<access-key>"
   ```

2. Especifique una carpeta temporal que usará al mover datos entre Azure Databricks y Azure SQL Data Warehouse.

   ```scala
   val tempDir = "wasbs://" + blobContainer + "@" + blobStorage +"/tempDirs"
   ```

3. Ejecute el siguiente fragmento de código para almacenar las claves de acceso de Azure Blob Storage en la configuración. Esta acción garantiza que no tiene que guardar la clave de acceso en el cuaderno en texto sin formato.

   ```scala
   val acntInfo = "fs.azure.account.key."+ blobStorage
   sc.hadoopConfiguration.set(acntInfo, blobAccessKey)
   ```

4. Proporcione los valores para conectarse a la instancia de Azure SQL Data Warehouse. Debe haber creado una instancia de SQL Data Warehouse como requisito previo. Use el nombre completo del servidor para **dwServer**. Por ejemplo, `<servername>.database.windows.net`.

   ```scala
   //SQL Data Warehouse related settings
   val dwDatabase = "<database-name>"
   val dwServer = "<database-server-name>"
   val dwUser = "<user-name>"
   val dwPass = "<password>"
   val dwJdbcPort =  "1433"
   val dwJdbcExtraOptions = "encrypt=true;trustServerCertificate=true;hostNameInCertificate=*.database.windows.net;loginTimeout=30;"
   val sqlDwUrl = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass + ";$dwJdbcExtraOptions"
   val sqlDwUrlSmall = "jdbc:sqlserver://" + dwServer + ":" + dwJdbcPort + ";database=" + dwDatabase + ";user=" + dwUser+";password=" + dwPass
   ```

5. Ejecute el siguiente fragmento de código para cargar la trama de datos transformada, **renamedColumnsDf**, como una tabla en SQL Data Warehouse. Este fragmento de código crea una tabla denominada **SampleTable** en SQL Database.

   ```scala
   spark.conf.set(
       "spark.sql.parquet.writeLegacyFormat",
       "true")

   renamedColumnsDF.write.format("com.databricks.spark.sqldw").option("url", sqlDwUrlSmall).option("dbtable", "SampleTable")       .option( "forward_spark_azure_storage_credentials","True").option("tempdir", tempDir).mode("overwrite").save()
   ```

   > [!NOTE]
   > Este ejemplo utiliza la marca `forward_spark_azure_storage_credentials`, que hace que SQL Data Warehouse acceda a los datos desde Blob Storage mediante una clave de acceso. Este es el único método admitido de autenticación.
   >
   > Si Azure Blob Storage está restringido para seleccionar redes virtuales, SQL Data Warehouse requiere [Managed Service Identity en lugar de las claves de acceso](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md#impact-of-using-vnet-service-endpoints-with-azure-storage). Esto provocará el error de solicitud no autorizada para realizar operación.

6. Conéctese a la instancia de SQL Database y compruebe que aparece la base de datos denominada **SampleTable**.

   ![Comprobación de la tabla de ejemplo](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table.png "Comprobación de la tabla de ejemplo")

7. Ejecute una consulta select para comprobar el contenido de la tabla. La tabla debe tener los mismos datos que la trama de datos **renamedColumnsDF**.

    ![Comprobación del contenido de la tabla de ejemplo](./media/databricks-extract-load-sql-data-warehouse/verify-sample-table-content.png "Comprobación del contenido de la tabla de ejemplo")

## <a name="clean-up-resources"></a>Limpieza de recursos

Después de terminar el tutorial, puede finalizar el clúster. Desde el área de trabajo de Azure Databricks, seleccione **Clusters** (Clústeres) a la izquierda. Para que el clúster termine, en **Acciones**, apunte a los puntos suspensivos (...) y seleccione el icono **Terminate** (Finalizar).

![Detención de un clúster de Databricks](./media/databricks-extract-load-sql-data-warehouse/terminate-databricks-cluster.png "Detención de un clúster de Databricks")

Si no finaliza manualmente el clúster, este se detendrá automáticamente si seleccionó la casilla **Terminate after \_\_ minutes of inactivity** (Finalizar después de \_\_ minutos de inactividad) al crear el clúster. En tal caso, el clúster se detiene automáticamente si ha estado inactivo durante el tiempo especificado.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a:

> [!div class="checklist"]
> * Creación de un servicio de Azure Databricks
> * Creación de un clúster de Spark en Azure Databricks
> * Creación de un cuaderno en Azure Databricks
> * Extracción de datos desde la cuenta de Data Lake Storage Gen2
> * Transformación de datos en Azure Databricks
> * Carga de datos en Azure SQL Data Warehouse

Vaya al siguiente tutorial para aprender sobre el streaming de datos en tiempo real en Azure Databricks mediante Azure Event Hubs.

> [!div class="nextstepaction"]
>[Transmisión de datos a Azure Databricks con Event Hubs](databricks-stream-from-eventhubs.md)
