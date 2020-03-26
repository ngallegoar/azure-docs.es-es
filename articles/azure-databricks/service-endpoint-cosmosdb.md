---
title: 'Tutorial: Implementación de Azure Databricks con un punto de conexión de Cosmos DB'
description: En este tutorial se describe cómo implementar Azure Databricks en una red virtual con un punto de conexión de servicio habilitado para Cosmos DB.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: tutorial
ms.date: 04/17/2019
ms.openlocfilehash: 4ac8c01e986cf1f3158c615a0791ba476e5bf1bb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "74706162"
---
# <a name="tutorial-implement-azure-databricks-with-a-cosmos-db-endpoint"></a>Tutorial: Implementar Azure Databricks con un punto de conexión de Cosmos DB

En este tutorial se describe cómo implementar un entorno de Databricks que tenga una red virtual insertada, con un punto de conexión de servicio habilitado para Cosmos DB.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear un área de trabajo de Azure Databricks en una red virtual
> * Crear un punto de conexión de servicio de Cosmos DB
> * Crear una cuenta de Cosmos DB e importar datos
> * Crear un clúster de Azure Databricks
> * Realizar una consulta de Cosmos DB desde un cuaderno de Azure Databricks

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar, haga lo siguiente:

* Cree un [área de trabajo de Azure Databricks en una red virtual](quickstart-create-databricks-workspace-vnet-injection.md).

* Descargue el [conector Spark](https://search.maven.org/remotecontent?filepath=com/microsoft/azure/azure-cosmosdb-spark_2.4.0_2.11/1.3.4/azure-cosmosdb-spark_2.4.0_2.11-1.3.4-uber.jar).

* Descargue los datos de ejemplo desde el sitio web de la agencia [NOAA National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/) (Administración Nacional Oceánica y Atmosférica). Seleccione un estado o área y seleccione **Search** (Buscar). En la página siguiente, acepte los valores predeterminados y seleccione **Search** (Buscar). A continuación, seleccione **CSV Download** (Descarga de CSV) en el lado izquierdo de la página para descargar los resultados.

* Descargue el valor [binario compilado previamente](https://aka.ms/csdmtool) de la herramienta de migración de datos de Azure Cosmos DB.

## <a name="create-a-cosmos-db-service-endpoint"></a>Crear un punto de conexión de servicio de Cosmos DB

1. Una vez que haya implementado un área de trabajo de Azure Databricks en una red virtual, vaya a la red virtual en [Azure Portal](https://portal.azure.com). Tenga en cuenta las subredes públicas y privadas que se crearon mediante la implementación de Databricks.

   ![Subredes de la red virtual](./media/service-endpoint-cosmosdb/virtual-network-subnets.png)

2. Seleccione la *subred pública* y cree un punto de conexión de servicio de Cosmos DB. A continuación, **guarde** la operación.
   
   ![Agregar un punto de conexión de servicio de Cosmos DB](./media/service-endpoint-cosmosdb/add-cosmosdb-service-endpoint.png)

## <a name="create-a-cosmos-db-account"></a>Creación de una cuenta de Cosmos DB

1. Abra Azure Portal. En la parte superior izquierda de la pantalla, seleccione **Create a resource > Databases > Azure Cosmos DB** (Crear un recurso > Bases de datos > Azure Cosmos DB).

2. Rellene los **detalles de la instancia** en la pestaña **Basics** (Datos básicos) con la configuración siguiente:

   |Configuración|Value|
   |-------|-----|
   |Subscription|*su suscripción*|
   |Grupo de recursos|*su grupo de recursos*|
   |Nombre de cuenta|db-vnet-service-endpoint|
   |API|Core (SQL)|
   |Location|Oeste de EE. UU.|
   |Redundancia geográfica|Disable|
   |Escrituras de varias regiones|Habilitar|

   ![Agregar un punto de conexión de servicio de Cosmos DB](./media/service-endpoint-cosmosdb/create-cosmosdb-account-basics.png)

3. Seleccione la pestaña **Network** (Red) y configure su red virtual. 

   a. Elija la red virtual que creó como un requisito previo y, a continuación, seleccione la *subred pública*. Tenga en cuenta que la *subred privada* tiene la nota *"Falta el punto de conexión de Microsoft AzureCosmosDB"* . Esto es porque solo se habilita el punto de conexión de servicio de Cosmos DB en la *subred pública*.

   b. Asegúrese de tener habilitada la opción **Allow access from Azure portal** (Permitir acceso desde Azure Portal). Esta configuración le permite obtener acceso a su cuenta de Cosmos DB desde Azure Portal. Si esta opción está configurada en **Deny** (Denegar), surgirán errores cuando intente acceder a su cuenta. 

   > [!NOTE]
   > Esto no es necesario para este tutorial, pero también puede habilitar la opción*Allow access from my IP* (Permitir el acceso desde mi IP) si quiere obtener acceso a su cuenta de Cosmos DB desde la máquina local. Por ejemplo, si se conecta a su cuenta con el SDK de Cosmos DB, debe habilitar esta configuración. Si está deshabilitada, se crearán errores de tipo "Acceso denegado".

   ![Configuración de red de la cuenta de Cosmos DB](./media/service-endpoint-cosmosdb/create-cosmosdb-account-network.png)

4. Seleccione **Review + Create** (Revisar y crear) y **Create** (Crear) para crear su cuenta de Cosmos DB en la red virtual.

5. Una vez que haya creado su cuenta de Cosmos DB, vaya a **Keys** (Claves) en **Settings** (Configuración). Copie la cadena de conexión principal y guárdela en un editor de texto para usarla más tarde.

    ![Página de claves de la cuenta de Cosmos DB](./media/service-endpoint-cosmosdb/cosmos-keys.png)

6. Seleccione **Data Explorer** (Explorador de datos) y **New Collection** (Nueva colección) para agregar una nueva base de datos y una colección a su cuenta de Cosmos DB.

    ![Nueva colección de Cosmos DB](./media/service-endpoint-cosmosdb/new-collection.png)

## <a name="upload-data-to-cosmos-db"></a>Cargar datos en Cosmos DB

1. Abra la versión de la interfaz gráfica de la [herramienta de migración de datos para Cosmos DB](https://aka.ms/csdmtool), **Dtui.exe**.

    ![Herramienta de migración de datos de Cosmos DB](./media/service-endpoint-cosmosdb/cosmos-data-migration-tool.png)

2. En la pestaña **Source Information** (Información de origen), seleccione **CSV File(s)** (Archivos CSV) en la lista desplegable **Import from** (Importar desde). A continuación, seleccione **Add Files** (Agregar archivos) y agregue el archivo CSV de Storm Data que descargó como requisito previo.

    ![Información de origen de la herramienta de migración de datos de Cosmos DB](./media/service-endpoint-cosmosdb/cosmos-source-information.png)

3. En la pestaña **Target Information** (Información de destino), escriba la cadena de conexión. Por tanto, el formato de la cadena de conexión es `AccountEndpoint=<URL>;AccountKey=<key>;Database=<database>`. AccountEndpoint y AccountKey se incluyen en la cadena de conexión principal que guardó en la sección anterior. Agregue `Database=<your database name>` al final de la cadena de conexión y seleccione **Verify** (Comprobar). A continuación, agregue el nombre de la colección y la clave de partición.

    ![Información de destino de la herramienta de migración de datos de Cosmos DB](./media/service-endpoint-cosmosdb/cosmos-target-information.png)

4. Seleccione **Next** (Siguiente) hasta llegar a la página de resumen. Seleccione **Import** (Importar).

## <a name="create-a-cluster-and-add-library"></a>Crear un clúster y agregar una biblioteca

1. En [Azure Portal](https://portal.azure.com), vaya al servicio de Azure Databricks y seleccione **Launch Workspace** (Iniciar área de trabajo).

   ![Iniciar el área de trabajo de Databricks](./media/service-endpoint-cosmosdb/launch-workspace.png)

2. Cree un clúster nuevo. Elija un nombre de clúster y acepte los valores predeterminados restantes.

   ![Configuración del nuevo clúster](./media/service-endpoint-cosmosdb/create-cluster.png)

3. Una vez creado el clúster, vaya a la página correspondiente y seleccione la pestaña **Libraries** (Bibliotecas). Seleccione **Install New** (Instalar nuevo) y cargue el archivo JAR del conector de Spark para instalar la biblioteca.

    ![Instalar la biblioteca del conector de Spark](./media/service-endpoint-cosmosdb/install-cosmos-connector-library.png)

    Puede comprobar que la biblioteca se haya instalado en la pestaña **Libraries** (Bibliotecas).

    ![Pestaña Libraries (Bibliotecas) del clúster de Databricks](./media/service-endpoint-cosmosdb/installed-library.png)

## <a name="query-cosmos-db-from-a-databricks-notebook"></a>Consulta de Cosmos DB desde un cuaderno de Azure Databricks

1. Vaya al área de trabajo de Azure Databricks y cree un nuevo cuaderno de Python.

    ![Crear un cuaderno de Databricks](./media/service-endpoint-cosmosdb/new-python-notebook.png)

2. Ejecute el siguiente código de Python para establecer la configuración de la conexión de Cosmos DB. Cambie el **punto de conexión**, la **clave maestra**, la **base de datos** y la **colección**  en consecuencia.

    ```python
    connectionConfig = {
      "Endpoint" : "https://<your Cosmos DB account name.documents.azure.com:443/",
      "Masterkey" : "<your Cosmos DB primary key>",
      "Database" : "<your database name>",
      "preferredRegions" : "West US 2",
      "Collection": "<your collection name>",
      "schema_samplesize" : "1000",
      "query_pagesize" : "200000",
      "query_custom" : "SELECT * FROM c"
    }
    ```

3. Use el siguiente código de Python para cargar los datos y crear una vista temporal.

    ```python
    users = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**connectionConfig).load()
    users.createOrReplaceTempView("storm")
    ```

4. Use el siguiente comando magic para ejecutar una instrucción SQL que devuelva datos.

    ```python
    %sql
    select * from storm
    ```

    Ha conectado con éxito el área de trabajo de Databricks que tiene insertada una red virtual a un recurso de Cosmos DB con el punto de conexión de servicio habilitado. Para obtener más información sobre cómo conectarse a Cosmos DB, consulte [Azure Cosmos DB Connector for Apache Spark](https://github.com/Azure/azure-cosmosdb-spark) (Conector de Azure Cosmos DB para Apache Spark).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos, el área de trabajo de Azure Databricks y todos los recursos relacionados. Si elimina el trabajo evitará que se generen facturas innecesarias. Si planea usar el área de trabajo de Azure Databricks en el futuro, puede detener el clúster y reiniciarlo más tarde. Si no va a seguir usando esta área de trabajo de Azure Databricks, elimine todos los recursos creados en este tutorial mediante los siguientes pasos:

1. En el menú de la izquierda de Azure Portal, haga clic en **Grupos de recursos** y en el nombre del grupo de recursos que creó.

2. En la página del grupo de recursos, seleccione **Eliminar**, escriba el nombre del recurso que quiere eliminar en el cuadro de texto y seleccione **Eliminar** de nuevo.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial implementó un área de trabajo de Azure Databricks en una red virtual y usó el conector Spark de Cosmos DB para consultar los datos de Cosmos DB de Databricks. Para obtener más información sobre cómo trabajar con Azure Databricks en una red virtual, consulte el tutorial para usar SQL Server con Azure Databricks.

> [!div class="nextstepaction"]
> [Tutorial: Consultar un contenedor de Docker de Linux para SQL Server en una red virtual mediante un cuaderno de Azure Databricks](vnet-injection-sql-server.md)
