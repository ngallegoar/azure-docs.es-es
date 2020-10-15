---
title: Migración de datos existentes a la cuenta de Table API en Azure Cosmos DB
description: Aprenda a migrar o importar datos locales o en la nube a una cuenta de Table API de Azure en Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: tutorial
ms.date: 12/07/2017
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 0023308c74d58b1c94bf13fcb47ffb8aa7ade1d6
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85557626"
---
# <a name="migrate-your-data-to-azure-cosmos-db-table-api-account"></a>Migración de los datos a una cuenta de Table API de Azure Cosmos DB

En este tutorial se proporcionan instrucciones sobre cómo importar datos para su uso con [Table API](table-introduction.md) de Azure Cosmos DB. Si tiene datos almacenados en Azure Table Storage, puede usar la Herramienta de migración de datos o AzCopy para importar los datos en Table API de Azure Cosmos DB. Si tiene datos almacenados en una cuenta de Table API de Azure Cosmos DB (versión preliminar), debe usar la Herramienta de migración de datos para migrar los datos. 

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Importación de datos con la Herramienta de migración de datos
> * Importación de datos con AzCopy
> * Migración de Table API (versión preliminar) a Table API 

## <a name="prerequisites"></a>Requisitos previos

* **Aumento del rendimiento:** la duración de la migración de datos depende de la cantidad de rendimiento configurado para un solo contenedor o un conjunto de contenedores. Asegúrese de aumentar el rendimiento para migraciones de datos más grandes. Después de haber completado la migración, reduzca el rendimiento para ahorrar costos. Para más información acerca de cómo aumentar el rendimiento en Azure Portal, consulte Niveles de rendimiento y planes de tarifa de Azure Cosmos DB.

* **Creación de recursos de Azure Cosmos DB:** antes de comenzar la migración de datos, cree previamente todas las tablas desde Azure Portal. Si va a migrar a una cuenta de Azure Cosmos DB que tiene un rendimiento de nivel de base de datos, asegúrese de proporcionar una clave de partición al crear las tablas de Azure Cosmos DB.

## <a name="data-migration-tool"></a>Herramienta de migración de datos

La Herramienta de migración de datos de Azure Cosmos DB de línea de comandos (dt.exe) puede usarse para importar los datos de Azure Table Storage existentes a una cuenta con disponibilidad general de Table API o para migrar los datos desde una cuenta de Table API (versión preliminar) a una cuenta con disponibilidad general de Table API. Otros orígenes no se admiten actualmente. La herramienta de migración de datos basada en la interfaz de usuario (dtui.exe) no se admite actualmente para las cuentas de la API Table. 

Para realizar una migración de datos de tablas, complete las tareas siguientes:

1. Descargue la herramienta de migración de [GitHub](https://github.com/azure/azure-documentdb-datamigrationtool).
2. Ejecute `dt.exe` usando los argumentos de la línea de comandos para su escenario. `dt.exe` toma un comando con el formato siguiente:

   ```bash
    dt.exe [/<option>:<value>] /s:<source-name> [/s.<source-option>:<value>] /t:<target-name> [/t.<target-option>:<value>] 
   ```

Las opciones admitidas para este comando son:

* **/ErrorLog:** Opcional. Nombre del archivo CSV al que se redirigirán los errores de transferencia de datos.
* **/OverwriteErrorLog:** Opcional. Sobrescribir archivo de registro de errores.
* **/ProgressUpdateInterval:** Opcional, el valor predeterminado es 00:00:01. Intervalo de tiempo para actualizar el progreso de la transferencia de datos en pantalla.
* **/ErrorDetails:** Opcional, el valor predeterminado es Ninguno. Especifica que se debe mostrar información de error detallada para los siguientes errores: Ninguno, Crítico, Todos.
* **/EnableCosmosTableLog:** Opcional. Dirija el registro a una cuenta de Cosmos Table. Si se establece, el valor predeterminado es la cadena de conexión de la cuenta de destino, a menos que también se proporcione/CosmosTableLogConnectionString. Esto resulta útil si se ejecutan varias instancias de DT simultáneamente.
* **/CosmosTableLogConnectionString:** Opcional. Cadena de conexión para dirigir el registro a una cuenta de Cosmos Table remota.

### <a name="command-line-source-settings"></a>Configuración del origen de la línea de comandos

Utilice las siguientes opciones de origen al definir la versión preliminar de Table API o Azure Table Storage como origen de la migración.

* **/s:AzureTable:** Lee datos de Azure Table Storage.
* **/s.ConnectionString:** Cadena de conexión para el punto de conexión de tabla. Se puede recuperar desde Azure Portal.
* **/s.LocationMode:** Opcional, el valor predeterminado es PrimaryOnly. Especifica el modo de ubicación que se va a usar al conectarse a Azure Table Storage: PrimaryOnly, PrimaryThenSecondary, SecondaryOnly, SecondaryThenPrimary.
* **/s.Table:** Nombre de la tabla de Azure.
* **/s.InternalFields:** Se establece en Todos para la migración de tablas, ya que RowKey y PartitionKey son campos necesarios para la importación.
* **/s.Filter:** Opcional. Cadena de filtro que se va a aplicar.
* **/s.Projection:** Opcional. Lista de columnas para seleccionar.

Para recuperar la cadena de conexión de origen al importar desde Azure Table Storage, abra Azure Portal y haga clic en **Cuentas de almacenamiento** > **Cuenta** > **Claves de acceso** y, a continuación, utilice el botón Copiar para copiar la **cadena de conexión**.

:::image type="content" source="./media/table-import/storage-table-access-key.png" alt-text="Captura de pantalla de opciones de origen de HBase":::

Para recuperar la cadena de conexión de origen al importar desde una cuenta de Table API de Azure Cosmos DB (versión preliminar), abra Azure Portal, haga clic en **Azure Cosmos DB** > **Cuenta** > **Cadena de conexión** y, a continuación, utilice el botón Copiar para copiar la **cadena de conexión**.

:::image type="content" source="./media/table-import/cosmos-connection-string.png" alt-text="Captura de pantalla de opciones de origen de HBase":::

[Comando de Azure Table Storage de ejemplo](#azure-table-storage)

[Comando de Table API de Azure Cosmos DB (versión preliminar) de ejemplo](#table-api-preview)

### <a name="command-line-target-settings"></a>Configuración del destino de la línea de comandos

Utilice las siguientes opciones de destino al definir Table API de Azure Cosmos DB como destino de la migración.

* **/t: TableAPIBulk:** Carga datos en Azure CosmosDB Table en lotes.
* **/t.ConnectionString:** Cadena de conexión para el punto de conexión de tabla
* **/t.TableName:** Especifica el nombre de la tabla en la que se va a escribir.
* **/t.Overwrite:** Opcional, el valor predeterminado es falso. Especifica si se deben sobrescribir los valores existentes.
* **/t.MaxInputBufferSize:** Opcional, el valor predeterminado es 1 GB. Estimación aproximada de bytes de entrada que se almacenarán en búfer antes de vaciar los datos en el receptor.
* **/t.Throughput:** Opcional, los valores predeterminados de servicio si no se especifican. Especifica el rendimiento que se va a configurar para la tabla.
* **/t.MaxBatchSize:** Opcional, el valor predeterminado es 2 MB. Especificar el tamaño del lote en bytes.

<a id="azure-table-storage"></a>
### <a name="sample-command-source-is-azure-table-storage"></a>Comando de ejemplo: El origen es el almacenamiento de Azure Table

A continuación se muestra un ejemplo de línea de comandos para importar desde Azure Table Storage a Table API:

```bash
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Table storage account name>;AccountKey=<Account Key>;EndpointSuffix=core.windows.net /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

<a id="table-api-preview"></a>
### <a name="sample-command-source-is-azure-cosmos-db-table-api-preview"></a>Comando de ejemplo: El origen es Table API de Azure Cosmos DB (versión preliminar)

A continuación se muestra un ejemplo de línea de comandos para importar desde la versión preliminar de Table API a Table API con disponibilidad general:

```bash
dt /s:AzureTable /s.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Table API preview account name>;AccountKey=<Table API preview account key>;TableEndpoint=https://<Account Name>.documents.azure.com; /s.Table:<Table name> /t:TableAPIBulk /t.ConnectionString:DefaultEndpointsProtocol=https;AccountName=<Azure Cosmos DB account name>;AccountKey=<Azure Cosmos DB account key>;TableEndpoint=https://<Account name>.table.cosmosdb.azure.com:443 /t.TableName:<Table name> /t.Overwrite
```

## <a name="migrate-data-by-using-azcopy"></a>Migración de datos mediante AzCopy

La utilidad de línea de comandos AzCopy es la otra opción para migrar datos desde Azure Table Storage a Table API de Azure Cosmos DB. Para usar AzCopy, primero exporte los datos tal y como se describe en [Exportación de datos desde Table Storage](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#export-data-from-table-storage) y luego impórtelos a Azure Cosmos DB, según se describe en [Table API de Azure Cosmos DB](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#import-data-into-table-storage).

Al realizar la importación en Azure Cosmos DB, consulte el ejemplo siguiente. Tenga en cuenta que el valor /Dest utiliza cosmosdb, no el básico.

Comando de importación de ejemplo:

```bash
AzCopy /Source:C:\myfolder\ /Dest:https://myaccount.table.cosmosdb.windows.net/mytable1/ /DestKey:key /Manifest:"myaccount_mytable_20140103T112020.manifest" /EntityOperation:InsertOrReplace
```

## <a name="migrate-from-table-api-preview-to-table-api"></a>Migración de Table API (versión preliminar) a Table API

> [!WARNING]
> Si desea disfrutar inmediatamente de las ventajas de las tablas disponibles con carácter general, migre las tablas existentes de la versión preliminar tal y como se especifica en esta sección; en caso contrario, en las próximas semanas, realizaremos migraciones automáticas para los clientes de la versión preliminar existentes. No obstante, tenga en cuenta que las tablas de la versión preliminar migradas automáticamente tendrán ciertas restricciones que las recién creadas no sufrirán.

Table API ya está disponible ahora con carácter general (GA). Existen diferencias entre la versión preliminar y las versiones de tablas con disponibilidad general, tanto en el código que se ejecuta en la nube como en el que lo hace en el cliente. Por lo tanto, no se recomienda intentar mezclar un cliente SDK de la versión preliminar con una cuenta de Table API con disponibilidad general y viceversa. Los clientes de la versión preliminar de Table API que deseen seguir utilizando sus tablas existentes pero en un entorno de producción, tienen que migrar desde la versión preliminar al entorno con disponibilidad general o bien esperar a la migración automática. Si espera a la migración automática, se le notificarán las restricciones de las tablas migradas. Tras la migración, podrá crear nuevas tablas en su cuenta actual sin restricciones (solo las tablas migradas las tendrán).

Para migrar desde Table API (versión preliminar) a Table API disponible con carácter general:

1. Cree una nueva cuenta de Azure Cosmos DB y establezca su tipo de API en Azure Table, como se describe en [Creación de una cuenta de base de datos](create-table-dotnet.md#create-a-database-account).

2. Cambie los clientes para que usen una versión con disponibilidad general de los [SDK de Table API](table-sdk-dotnet.md).

3. Migre los datos de cliente desde las tablas de la versión preliminar a las tablas con disponibilidad general mediante la Herramienta de migración de datos. Las instrucciones sobre cómo utilizar la Herramienta de migración de datos para este propósito se describen en [Herramienta de migración de datos](#data-migration-tool). 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido cómo:

> [!div class="checklist"]
> * Importar datos con la Herramienta de migración de datos
> * Importar datos con AzCopy
> * Migración de Table API (versión preliminar) a Table API

Ahora puede pasar al siguiente tutorial y aprender a consultar los datos mediante Table API de Azure Cosmos DB. 

> [!div class="nextstepaction"]
>[Consulta de datos](../cosmos-db/tutorial-query-table.md)
