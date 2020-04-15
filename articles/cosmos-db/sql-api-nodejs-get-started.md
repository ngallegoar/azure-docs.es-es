---
title: Tutorial de Node.js para la API de SQL para Azure Cosmos DB
description: Un tutorial de Node.js en el que se muestra cómo conectar con Azure Cosmos DB y realizar consultas en él mediante SQL API
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 08/06/2019
ms.author: dech
Customer intent: As a developer, I want to build a Node.js console application to access and manage SQL API account resources in Azure Cosmos DB, so that customers can better use the service.
ms.openlocfilehash: a67c00dab33272120097fde75fcf56f24f9f532b
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411344"
---
# <a name="tutorial-build-a-nodejs-console-app-with-the-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>Tutorial: Creación de una aplicación de consola de Node.js con el SDK de JavaScript para administrar los datos de SQL API de Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Como desarrollador, puede que tenga aplicaciones que usan datos de documentos NoSQL. Puede usar una cuenta de SQL API en Azure Cosmos DB para almacenar datos de documentos y acceder a dichos datos. En este tutorial se muestra cómo crear una aplicación de consola Node.js para crear recursos de Azure Cosmos DB y consultarlos.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Crear una cuenta de Azure Cosmos DB y conectarse a ella.
> * Configurar la aplicación.
> * Crear una base de datos.
> * Cree un contenedor.
> * Agregar elementos al contenedor.
> * Realizar operaciones básicas sobre los elementos, el contenedor y la base de datos.

## <a name="prerequisites"></a>Prerrequisitos 

Asegúrese de que dispone de los siguientes recursos:

* Una cuenta de Azure activa. Si no tiene una, puede registrarse para obtener una [prueba gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) versión 6.0.0 o superior.

## <a name="create-azure-cosmos-db-account"></a>Creación de una cuenta de Azure Cosmos DB

Vamos a crear una cuenta de Azure Cosmos DB. Si ya tiene una cuenta que desea usar, puede ir directamente a [Configuración de la aplicación de Node.js](#SetupNode). Si usa el emulador de Azure Cosmos DB, siga los pasos que se indican en [Emulador de Azure Cosmos DB](local-emulator.md) para configurar el emulador y vaya directamente a [Configuración de la aplicación de Node.js](#SetupNode). 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="set-up-your-nodejs-application"></a><a id="SetupNode"></a>Configuración de la aplicación de Node.js

Antes de empezar a escribir código para compilar la aplicación, puede compilar el marco de la aplicación. Ejecute estos pasos para configurar la aplicación de Node.js que tiene el código del marco:

1. Abra su terminal favorito.
2. Busque la carpeta o el directorio donde desea guardar la aplicación de Node.js.
3. Cree dos archivos de JavaScript vacíos con los siguientes comandos:

   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```

   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```

4. Cree e inicialice un archivo `package.json`. Use el comando siguiente:
   * ```npm init -y```

5. Instale el módulo @azure/cosmos mediante npm. Use el comando siguiente:
   * ```npm install @azure/cosmos --save```

## <a name="set-your-apps-configurations"></a><a id="Config"></a>Configuración de las opciones de la aplicación

Ahora que existe la aplicación, se debe asegurar de que se puede comunicar con Azure Cosmos DB. Si actualiza ciertas opciones de configuración, tal como se muestra en los pasos siguiente, puede establecer que la aplicación se comunique con Azure Cosmos DB:

1. Abra ```config.js``` en el editor de texto que prefiera.

1. Copie y pegue el siguiente fragmento de código y defina las propiedades ```config.endpoint``` y ```config.key``` para el identificador URI del punto de conexión de Azure Cosmos DB y la clave principal. Ambas opciones de configuración se encuentran en [Azure Portal](https://portal.azure.com).

   ![Captura de pantalla de obtención de claves en Azure Portal][keys]

   ```javascript
   // ADD THIS PART TO YOUR CODE
   var config = {}

   config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
   config.key = "~your primary key here~";
   ``` 

1. Copie y pegue los datos de ```database```, ```container``` y ```items``` en el objeto ```config``` siguiente, donde se definen las propiedades de ```config.endpoint``` y ```config.key```. Si ya dispone de los datos que quiere almacenar en la base de datos, puede usar la herramienta de migración de datos de Azure Cosmos DB, en lugar de definir los datos aquí. El archivo config.js debe tener el siguiente código:

   :::code language="javascript" source="~/cosmosdb-nodejs-get-started/config.js":::

   El SDK de JavaScript usa los términos genéricos de *contenedor* y *elemento*. Un contenedor puede ser una colección, un grafo o una tabla. Un elemento puede ser un documento, un vértice o borde, o una fila, y es el contenido que hay en un contenedor. 
   
   El código `module.exports = config;` se usa para exportar el objeto ```config```, con el fin de que pueda hacer referencia a él en el archivo ```app.js```.

## <a name="connect-to-an-azure-cosmos-db-account"></a><a id="Connect"></a>Conexión a una cuenta de Azure Cosmos DB

1. Abra el archivo ```app.js``` vacío en el editor de texto. Copie y pegue el código siguiente para importar el módulo ```@azure/cosmos``` y el módulo ```config``` que acaba de crear.

   ```javascript
   // ADD THIS PART TO YOUR CODE
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   ```

1. Copie y pegue el código para usar las propiedades ```config.endpoint``` y ```config.key``` que guardó anteriormente para crear un nuevo CosmosClient.

   ```javascript
   const config = require('./config');

   // ADD THIS PART TO YOUR CODE
   const endpoint = config.endpoint;
   const key = config.key;

   const client = new CosmosClient({ endpoint, key });
   ```
   
> [!Note]
> Si se conecta al **emulador de Cosmos DB**, deshabilite la comprobación de TLS para el proceso de los nodos:
>   ```
>   process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";
>   const client = new CosmosClient({ endpoint, key });
>   ```

Ahora que tiene el código necesario para inicializar el cliente de Azure Cosmos DB, se explicará cómo usar los recursos de este servicio.

## <a name="create-a-database"></a>Crear una base de datos

1. Copie y pegue el código siguiente para establecer el identificador de la base de datos y el identificador del contenedor. Dichos identificadores son la forma en que el cliente de Azure Cosmos DB encuentra la base de datos y el contenedor correctos.

   ```javascript
   const client = new CosmosClient({ endpoint, key });

   // ADD THIS PART TO YOUR CODE
   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };
   ```

   Para crear una base de datos se pueden usar las funciones `createIfNotExists` o create de la clase **Databases**. Una base de datos es el contenedor lógico de elementos con particiones en contenedores. 

2. Copie y pegue los métodos **createDatabase** y **readDatabase** en el archivo app.js debajo de la definición de ```databaseId``` y ```containerId```. La función **createDatabase** creará una base de datos con el identificador ```FamilyDatabase```, que se especifica desde el objeto ```config``` si aún no existe. La función **readDatabase** leerá la definición de la base de datos para asegurarse de que la base de datos existe.

   ```javascript
   /**
    * Create the database if it does not exist
    */
   async function createDatabase() {
       const { database } = await client.databases.createIfNotExists({ id: databaseId });
       console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
      const { resource: databaseDefinition } = await client.database(databaseId).read();
      console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }
   ```

3. Copie y pegue el siguiente código debajo de donde definió las funciones **createDatabase** y **readDatabase** para agregar la función auxiliar **exit** que imprimirá el mensaje de salida. 

   ```javascript
   // ADD THIS PART TO YOUR CODE
   function exit(message) {
      console.log(message);
      console.log('Press any key to exit');
      process.stdin.setRawMode(true);
      process.stdin.resume();
      process.stdin.on('data', process.exit.bind(process, 0));
   };
   ```

4. Copie y pegue el siguiente código debajo de donde definió la función **exit** para llamar a las funciones **createDatabase** y **readDatabase**.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
   ```

   En este momento, el código de ```app.js``` debería ser como este:

   ```javascript
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');

   const endpoint = config.endpoint;
   const key = config.key;

   const client = new CosmosClient({ endpoint, key });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };

    /**
    * Create the database if it does not exist
    */
    async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { resource: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error) }`) });
   ```

5. En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

   ```bash 
   node app.js
   ```

## <a name="create-a-container"></a><a id="CreateContainer"></a>Creación de un contenedor

A continuación, cree un contenedor dentro de la cuenta de Azure Cosmos DB para poder almacenar los datos y consultarlos. 

> [!WARNING]
> La creación de un contenedor tiene implicaciones en los precios. Visite la [página de precios](https://azure.microsoft.com/pricing/details/cosmos-db/) para saber qué esperar.

Para crear un contenedor se pueden usar las funciones `createIfNotExists` o create de la clase **Containers**.  Un contenedor se compone de elementos (que en el caso de SQL API son documentos JSON) y un registro de aplicaciones JavaScript asociado.

1. Copie y pegue las funciones **createContainer** y **readContainer** debajo de la función **readDatabase** en el archivo app.js. La función **createContainer** creará un contenedor con ```containerId```, que se especifica desde el objeto ```config``` si aún no existe. La función **readContainer** leerá la definición del contenedor para comprobar que el contenedor existe.

   ```javascript
   /**
   * Create the container if it does not exist
   */

   async function createContainer() {

    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId, partitionKey }, { offerThroughput: 400 });
    console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
    * Read the container definition
   */
   async function readContainer() {
      const { resource: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
   }
   ```

1. Copie y pegue el código debajo de la llamada a **readDatabase** para ejecutar las funciones **createContainer** y **readContainer**.

   ```javascript
   createDatabase()
     .then(() => readDatabase())

     // ADD THIS PART TO YOUR CODE
     .then(() => createContainer())
     .then(() => readContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

   En este momento, el código de ```app.js``` debería ser como este:

   ```javascript
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');

   const endpoint = config.endpoint;
   const key = config.key;

   const client = new CosmosClient({ endpoint, key });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   const partitionKey = { kind: "Hash", paths: ["/Country"] };

   /**
   * Create the database if it does not exist
   */
   async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
     console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Create the container if it does not exist
   */

   async function createContainer() {

    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId, partitionKey }, { offerThroughput: 400 });
    console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
    * Read the container definition
   */
   async function readContainer() {
      const { resource: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

   ```bash 
   node app.js
   ```

## <a name="create-an-item"></a><a id="CreateItem"></a>Creación de un elemento

Para crear un elemento se puede usar la función create de la clase **Items**. Cuando se usa SQL API, los elementos se proyectan como documentos, que son contenido JSON (arbitrario) definido por el usuario. Ahora puede insertar un elemento en Azure Cosmos DB.

1. Copie y pegue la función **createFamilyItem** debajo de la función **readContainer**. La función **createFamilyItem** crea los elementos que contienen los datos JSON guardados en el objeto ```config```. Vamos a asegurarnos de que no existe un elemento con el mismo identificador antes de crearlo.

   ```javascript
   /**
   * Create family item
   */
   async function createFamilyItem(itemBody) {
      const { item } = await client.database(databaseId).container(containerId).items.upsert(itemBody);
      console.log(`Created family item with id:\n${itemBody.id}\n`);
   };
   ```

1. Copie y pegue el código de debajo de la llamada a **readContainer** para ejecutar la función **createFamilyItem**.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

   ```bash 
   node app.js
   ```


## <a name="query-azure-cosmos-db-resources"></a><a id="Query"></a>Consulta de los recursos de Azure Cosmos DB

Azure Cosmos DB admite consultas enriquecidas en los documentos JSON que se almacenan en los contenedores. El código de ejemplo siguiente muestra una consulta que se puede ejecutar en los documentos de un contenedor.

1. Copie y pegue la función **queryContainer** debajo de la función **createFamilyItem** en el archivo app.js. Azure Cosmos DB admite consultas del tipo SQL tal y como se muestra a continuación.

   ```javascript
   /**
   * Query the container using SQL
    */
   async function queryContainer() {
     console.log(`Querying container:\n${config.container.id}`);

     // query to return all children in a family
     const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { resources } = await client.database(databaseId).container(containerId).items.query(querySpec).fetchAll();
    for (var queryResult of resources) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
   };
   ```

1. Copie y pegue el código de debajo de las llamadas en **createFamilyItem** para ejecutar la función **queryContainer**.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))

     // ADD THIS PART TO YOUR CODE
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. En el terminal, busque el archivo ```app.js``` y ejecute el comando:

   ```bash 
   node app.js
   ```


## <a name="replace-an-item"></a><a id="ReplaceItem"></a>Reemplazo de un elemento
Azure Cosmos DB admite el reemplazo del contenido de los elementos.

1. Copie y pegue la función **replaceFamilyItem** debajo de la función **queryContainer** en el archivo app.js. Observe que hemos cambiado la propiedad "grade" de un elemento secundario del valor anterior de 5 a 6.

   ```javascript
   // ADD THIS PART TO YOUR CODE
   /**
   * Replace the item by ID.
   */
   async function replaceFamilyItem(itemBody) {
      console.log(`Replacing item:\n${itemBody.id}\n`);
      // Change property 'grade'
      itemBody.children[0].grade = 6;
     const { item } = await client.database(databaseId).container(containerId).item(itemBody.id, itemBody.Country).replace(itemBody);
   };
   ```

1. Copie y pegue el código de debajo de la llamada en **queryContainer** para ejecutar la función **replaceFamilyItem**. Además, vuelva a agregar el código para llamar a **queryContainer** para comprobar que el elemento ha cambiado correctamente.

   ```javascript
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     .then(() => queryContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => replaceFamilyItem(config.items.Andersen))
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. En el terminal, busque el archivo ```app.js``` y ejecute el comando:

   ```bash 
   node app.js
   ```


## <a name="delete-an-item"></a><a id="DeleteItem"></a>Eliminación de un elemento

Azure Cosmos DB admite la eliminación de elementos JSON.

1. Copie y pegue la función **deleteFamilyItem** debajo de la función **replaceFamilyItem**.

   ```javascript
   /**
   * Delete the item by ID.
   */
   async function deleteFamilyItem(itemBody) {
     await client.database(databaseId).container(containerId).item(itemBody.id, itemBody.Country).delete(itemBody);
      console.log(`Deleted item:\n${itemBody.id}\n`);
   };
   ```

1. Copie y pegue el código de debajo de la llamada a la segunda función **queryContainer** para ejecutar la función **deleteFamilyItem**.

   ```javascript
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer
      ())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
      .then(() => deleteFamilyItem(config.items.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

   ```bash 
   node app.js
   ```


## <a name="delete-the-database"></a><a id="DeleteDatabase"></a>Eliminación de la base de datos

La eliminación de la base de datos creada quitará la base de datos y todos los recursos secundarios (contenedores, elementos, etc.).

1. Copie y pegue la función **cleanup** debajo de la función **deleteFamilyItem** para quitar la base de datos y todos sus recursos secundarios.

   ```javascript
   /**
   * Cleanup the database and container on completion
   */
   async function cleanup() {
     await client.database(databaseId).delete();
   }
   ```

1. Copie y pegue el código de debajo de la llamada a **deleteFamilyItem** para ejecutar la función **cleanup**.

   ```javascript
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())
      .then(() => deleteFamilyItem(config.items.Andersen))

      // ADD THIS PART TO YOUR CODE
      .then(() => cleanup())
      // ENDS HERE

      .then(() => { exit(`Completed successfully`); })
      .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

## <a name="run-your-nodejs-application"></a><a id="Run"></a>Ejecución de la aplicación de Node.js

En conjunto, el código debe ser similar a este:

:::code language="javascript" source="~/cosmosdb-nodejs-get-started/app.js":::

En el terminal, busque el archivo ```app.js``` y ejecute el comando: 

```bash 
node app.js
```

Ahora debería ver la salida de la aplicación GetStarted. La salida debe coincidir con el texto del ejemplo siguiente.

   ```
    Created database:
    FamilyDatabase

    Reading database:
    FamilyDatabase

    Created container:
    FamilyContainer

    Reading container:
    FamilyContainer

    Created family item with id:
    Anderson.1

    Created family item with id:
    Wakefield.7

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing item:
    Anderson.1

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleted item:
    Anderson.1

    Completed successfully
    Press any key to exit
   ```

## <a name="get-the-complete-nodejs-tutorial-solution"></a><a id="GetSolution"></a>Obtención de la solución completa del tutorial de Node.js 

Si no dispuso de tiempo para completar los pasos de este tutorial, o simplemente desea descargar el código, puede obtenerlo de [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ). 

Para ejecutar la solución de introducción que contiene todo el código de este artículo, necesitará lo siguiente: 

* Una [cuenta de Azure Cosmos DB][create-account]. 
* La solución [Getting Started](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started) disponible en GitHub. 

Instale las dependencias del proyecto mediante npm. Use el comando siguiente: 

* ```npm install``` 

Luego, en el archivo ```config.js```, actualice los valores config.endpoint y config.key como describe en [Paso 3: Configuración de las opciones de la aplicación](#Config).  

Luego, en el terminal, busque el archivo ```app.js``` y ejecute el comando:  

```bash  
node app.js 
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite estos recursos, podrá eliminar el grupo de recursos, la cuenta de Azure Cosmos DB y todos los recursos relacionados. Para hacerlo, seleccione el grupo de recursos que usó en la cuenta de Azure Cosmos DB, seleccione **Eliminar** y confirme el nombre del grupo de recursos que se va a eliminar.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Supervisión de una cuenta de Azure Cosmos DB](monitor-accounts.md)

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png
