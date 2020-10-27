---
title: Enlaces de entrada de Azure Table Storage para Azure Functions
description: Descubra cómo usar los enlaces de entrada de Azure Table Storage en Azure Functions.
author: craigshoemaker
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python
ms.openlocfilehash: 7f5db2a2df7314c89f2ebba8e7e54ebe24126386
ms.sourcegitcommit: 93329b2fcdb9b4091dbd632ee031801f74beb05b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92098156"
---
# <a name="azure-table-storage-input-bindings-for-azure-functions"></a>Enlaces de entrada de Azure Table Storage para Azure Functions

Use el enlace de entrada de Azure Table Storage para leer una tabla de una cuenta de Azure Storage.

## <a name="example"></a>Ejemplo

# <a name="c"></a>[C#](#tab/csharp)

### <a name="one-entity"></a>Una entidad

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que lee una única fila de la tabla. Para cada mensaje que se envíe a la cola, se desencadenará la función.

El valor "{queueTrigger}" de clave de fila indica que la clave de fila procede de la cadena del mensaje en la cola.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        ILogger log)
    {
        log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### <a name="cloudtable"></a>CloudTable

`CloudTable` solo se admite en los [entornos de ejecución de Functions v2 y superiores](functions-versions.md).

Use un parámetro del método `CloudTable` para leer la tabla mediante Azure Storage SDK. Este es un ejemplo de una función que consulta una tabla de registros de Azure Functions:

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.Cosmos.Table;
using System;
using System.Threading.Tasks;

namespace FunctionAppCloudTable2
{
    public class LogEntity : TableEntity
    {
        public string OriginalName { get; set; }
    }
    public static class CloudTableDemo
    {
        [FunctionName("CloudTableDemo")]
        public static async Task Run(
            [TimerTrigger("0 */1 * * * *")] TimerInfo myTimer, 
            [Table("AzureWebJobsHostLogscommon")] CloudTable cloudTable,
            ILogger log)
        {
            log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

            TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
                TableQuery.CombineFilters(
                    TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
                        "FD2"),
                    TableOperators.And,
                    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
                        "t")));

            // Execute the query and loop through the results
            foreach (LogEntity entity in 
                await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
            {
                log.LogInformation(
                    $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
            }
        }
    }
}
```

Para más información sobre cómo usar CloudTable, Consulte [Introducción a Azure Table Storage](../cosmos-db/tutorial-develop-table-dotnet.md).

Si intenta enlazar a `CloudTable` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](./functions-bindings-storage-table.md#azure-storage-sdk-version-in-functions-1x).

### <a name="iqueryable"></a>IQueryable

`IQueryable` solo se admite en el [entorno de ejecución de Functions v1](functions-versions.md).

En el ejemplo siguiente se muestra una [función de C#](functions-dotnet-class-library.md) que lee varias filas de la tabla, donde la clase `MyPoco` se deriva de `TableEntity`.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        ILogger log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

### <a name="one-entity"></a>Una entidad

En el ejemplo siguiente se muestra un enlace de entrada de la tabla en un archivo *function.json* y código de [script de C#](functions-reference-csharp.md) que usa el enlace. La función usa un desencadenador de cola para leer una fila de tabla única. 

El archivo *function.json* especifica un valor `partitionKey` y un valor `rowKey`. El valor `rowKey` "{queueTrigger}" de clave de fila indica que la clave de fila procede de la cadena del mensaje en la cola.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

Este es el código de script de C#:

```csharp
public static void Run(string myQueueItem, Person personEntity, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    log.LogInformation($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="cloudtable"></a>CloudTable

`IQueryable` no se admite en el entorno en tiempo de ejecución de Functions para las [versiones 2.x y posteriores)](functions-versions.md). Una alternativa consiste en usar un parámetro del método `CloudTable` para leer la tabla mediante Azure Storage SDK. Este es un ejemplo de una función que consulta una tabla de registros de Azure Functions:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 */1 * * * *"
    },
    {
      "name": "cloudTable",
      "type": "table",
      "connection": "AzureWebJobsStorage",
      "tableName": "AzureWebJobsHostLogscommon",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, CloudTable cloudTable, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

    TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
            "FD2"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
            "a")));

    // Execute the query and loop through the results
    foreach (LogEntity entity in 
    await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
    {
        log.LogInformation(
            $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
    }
}

public class LogEntity : TableEntity
{
    public string OriginalName { get; set; }
}
```

Para más información sobre cómo usar CloudTable, Consulte [Introducción a Azure Table Storage](../cosmos-db/tutorial-develop-table-dotnet.md).

Si intenta enlazar a `CloudTable` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](./functions-bindings-storage-table.md#azure-storage-sdk-version-in-functions-1x).

### <a name="iqueryable"></a>IQueryable

En el ejemplo siguiente se muestra un enlace de entrada de la tabla en un archivo *function.json* y código de [script de C#](functions-reference-csharp.md) que usa el enlace. La función lee las entidades de una clave de partición que se especifica en un mensaje en la cola.

Este es el archivo *function.json* :

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

El código de script de C# agrega una referencia al SDK de Azure Storage con el fin de que el tipo de entidad pueda derivar de `TableEntity`:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Extensions.Logging;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.LogInformation($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

En el ejemplo siguiente se muestra un enlace de entrada de la tabla en un archivo *function.json* y el código de [JavaScript](functions-reference-node.md) que usa el enlace. La función usa un desencadenador de cola para leer una fila de tabla única. 

El archivo *function.json* especifica un valor `partitionKey` y un valor `rowKey`. El valor `rowKey` "{queueTrigger}" de clave de fila indica que la clave de fila procede de la cadena del mensaje en la cola.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

En la sección de [configuración](#configuration) se explican estas propiedades.

Este es el código de JavaScript:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

# <a name="python"></a>[Python](#tab/python)

Única fila de tabla 

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "messageJSON",
      "type": "table",
      "tableName": "messages",
      "partitionKey": "message",
      "rowKey": "{id}",
      "connection": "AzureWebJobsStorage",
      "direction": "in"
    },
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ],
      "route": "messages/{id}"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ],
  "disabled": false
}
```

```python
import json

import azure.functions as func

def main(req: func.HttpRequest, messageJSON) -> func.HttpResponse:

    message = json.loads(messageJSON)
    return func.HttpResponse(f"Table row: {messageJSON}")
```

# <a name="java"></a>[Java](#tab/java)

En el ejemplo siguiente se muestra una función desencadenada por HTTP que devuelve una lista de objetos Person que se encuentran en una partición especificada del almacenamiento Table. En el ejemplo, la clave de partición se extrae de la ruta http y los elementos tableName y connection proceden de la configuración de la función. 

```java
public class Person {
    private String PartitionKey;
    private String RowKey;
    private String Name;

    public String getPartitionKey() { return this.PartitionKey; }
    public void setPartitionKey(String key) { this.PartitionKey = key; }
    public String getRowKey() { return this.RowKey; }
    public void setRowKey(String key) { this.RowKey = key; }
    public String getName() { return this.Name; }
    public void setName(String name) { this.Name = name; }
}

@FunctionName("getPersonsByPartitionKey")
public Person[] get(
        @HttpTrigger(name = "getPersons", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="persons/{partitionKey}") HttpRequestMessage<Optional<String>> request,
        @BindingName("partitionKey") String partitionKey,
        @TableInput(name="persons", partitionKey="{partitionKey}", tableName="%MyTableName%", connection="MyConnectionString") Person[] persons,
        final ExecutionContext context) {

    context.getLogger().info("Got query for person related to persons with partition key: " + partitionKey);

    return persons;
}
```

La anotación TableInput también puede extraer los enlaces del cuerpo JSON de la solicitud, como se muestra en el ejemplo siguiente.

```java
@FunctionName("GetPersonsByKeysFromRequest")
public HttpResponseMessage get(
        @HttpTrigger(name = "getPerson", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="query") HttpRequestMessage<Optional<String>> request,
        @TableInput(name="persons", partitionKey="{partitionKey}", rowKey = "{rowKey}", tableName="%MyTableName%", connection="MyConnectionString") Person person,
        final ExecutionContext context) {

    if (person == null) {
        return request.createResponseBuilder(HttpStatus.NOT_FOUND)
                    .body("Person not found.")
                    .build();
    }

    return request.createResponseBuilder(HttpStatus.OK)
                    .header("Content-Type", "application/json")
                    .body(person)
                    .build();
}
```

En los ejemplos siguientes se usa el filtro para buscar personas con un nombre concreto en una tabla de Azure y se limita el número de posibles coincidencias a 10 resultados.

```java
@FunctionName("getPersonsByName")
public Person[] get(
        @HttpTrigger(name = "getPersons", methods = {HttpMethod.GET}, authLevel = AuthorizationLevel.FUNCTION, route="filter/{name}") HttpRequestMessage<Optional<String>> request,
        @BindingName("name") String name,
        @TableInput(name="persons", filter="Name eq '{name}'", take = "10", tableName="%MyTableName%", connection="MyConnectionString") Person[] persons,
        final ExecutionContext context) {

    context.getLogger().info("Got query for person related to persons with name: " + name);

    return persons;
}
```

---

## <a name="attributes-and-annotations"></a>Atributos y anotaciones

# <a name="c"></a>[C#](#tab/csharp)

 Para [bibliotecas de clases de C#](functions-dotnet-class-library.md), utilice los siguientes atributos para configurar un enlace de entrada de la tabla:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  El constructor del atributo toma el nombre de la tabla, la clave de partición y la clave de fila. El atributo se puede usar en un parámetro `out` o en el valor devuelto de la función, como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Puede establecer la propiedad `Connection` para especificar la cuenta de almacenamiento que se usará, tal como se muestra en el ejemplo siguiente:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Para ver un ejemplo completo, consulte el ejemplo de C#.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Proporciona otra manera de especificar la cuenta de almacenamiento que se debe usar. El constructor toma el nombre de una configuración de aplicación que contiene una cadena de conexión de almacenamiento. El atributo se puede aplicar en el nivel de clase, método o parámetro. En el ejemplo siguiente se muestran el nivel de clase y de método:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

La cuenta de almacenamiento que se debe usar se determina en el orden siguiente:

* La propiedad `Connection` del atributo `Table`.
* El atributo `StorageAccount` aplicado al mismo parámetro que el atributo `Table`.
* El atributo `StorageAccount` aplicado a la función.
* El atributo `StorageAccount` aplicado a la clase.
* La cuenta de almacenamiento predeterminada de la aplicación de función (configuración de aplicación "AzureWebJobsStorage").

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

El script de C# no admite atributos.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript no admite atributos.

# <a name="python"></a>[Python](#tab/python)

Python no admite atributos.

# <a name="java"></a>[Java](#tab/java)

En la [biblioteca en tiempo de ejecución de funciones de Java](/java/api/overview/azure/functions/runtime), utilice la anotación `@TableInput` en los parámetros cuyo valor provendría del almacenamiento de la tabla.  Esta anotación se puede usar con tipos nativos de Java, POJO o valores que aceptan valores NULL mediante `Optional<T>`.

---

## <a name="configuration"></a>Configuración

En la siguiente tabla se explican las propiedades de configuración de enlace que se definen en el archivo *function.json* y el atributo `Table`.

|Propiedad de function.json | Propiedad de atributo |Descripción|
|---------|---------|----------------------|
|**type** | N/D | Se debe establecer en `table`. Esta propiedad se establece automáticamente cuando se crea el enlace en Azure Portal.|
|**direction** | N/D | Se debe establecer en `in`. Esta propiedad se establece automáticamente cuando se crea el enlace en Azure Portal. |
|**name** | N/D | Nombre de la variable que representa la tabla o la entidad en el código de la función. | 
|**tableName** | **TableName** | Nombre de la tabla.| 
|**partitionKey** | **PartitionKey** |Opcional. Clave de partición de la entidad de tabla que se va a leer. Consulte la sección acerca del [uso](#usage) para obtener información acerca de cómo usar esta propiedad.| 
|**rowKey** |**RowKey** | Opcional. Clave de fila de la entidad de tabla que se va a leer. Consulte la sección acerca del [uso](#usage) para obtener información acerca de cómo usar esta propiedad.| 
|**take** |**Take** | Opcional. Número máximo de entidades que se van a leer en JavaScript. Consulte la sección acerca del [uso](#usage) para obtener información acerca de cómo usar esta propiedad.| 
|**filter** |**Filter** | Opcional. Expresión de filtro de OData en la entrada de la tabla en JavaScript. Consulte la sección acerca del [uso](#usage) para obtener información acerca de cómo usar esta propiedad.| 
|**connection** |**Connection** | El nombre de una configuración de aplicación que contiene la cadena de conexión de almacenamiento que se usará para este enlace. El valor de configuración puede ser el nombre de una configuración de aplicación con el prefijo "AzureWebJobs" o un nombre de cadena de conexión. Por ejemplo, si el nombre de la configuración es "AzureWebJobsMyStorage", puede especificar "MyStorage" aquí. El entorno de ejecución de Functions buscará automáticamente una configuración de aplicación denominada "AzureWebJobsMyStorage". Si deja `connection` vacía, el entorno en tiempo de ejecución de Functions usa la cadena de conexión de almacenamiento predeterminada en la configuración de aplicación que se denomina `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Uso

# <a name="c"></a>[C#](#tab/csharp)

* **Leer una fila en**

  Establezca `partitionKey` y `rowKey`. Obtenga acceso a los datos de la tabla mediante un parámetro de método `T <paramName>`. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json* . `T` suele ser un tipo que implementa `ITableEntity` o deriva de `TableEntity`. Las propiedades `filter` y `take` no se utilizan en este escenario.

* **Leer una o varias filas**

  Obtenga acceso a los datos de la tabla mediante un parámetro de método `IQueryable<T> <paramName>`. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json* . `T` debe ser un tipo que implementa `ITableEntity` o deriva de `TableEntity`. Puede usar métodos `IQueryable` para realizar cualquier operación de filtrado necesaria. Las propiedades `partitionKey`, `rowKey`, `filter` y `take` no se utilizan en este escenario.  

  > [!NOTE]
  > `IQueryable` no es compatible con el [entorno de tiempo de ejecución de Functions v2](functions-versions.md). Una alternativa consiste en [usar un parámetro del método CloudTable](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) para leer la tabla mediante Azure Storage SDK. Si intenta enlazar a `CloudTable` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](./functions-bindings-storage-table.md#azure-storage-sdk-version-in-functions-1x).

# <a name="c-script"></a>[Script de C#](#tab/csharp-script)

* **Leer una fila en**

  Establezca `partitionKey` y `rowKey`. Obtenga acceso a los datos de la tabla mediante un parámetro de método `T <paramName>`. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json* . `T` suele ser un tipo que implementa `ITableEntity` o deriva de `TableEntity`. Las propiedades `filter` y `take` no se utilizan en este escenario.

* **Leer una o varias filas**

  Obtenga acceso a los datos de la tabla mediante un parámetro de método `IQueryable<T> <paramName>`. En script de C#, `paramName` es el valor especificado en la propiedad `name` de *function.json* . `T` debe ser un tipo que implementa `ITableEntity` o deriva de `TableEntity`. Puede usar métodos `IQueryable` para realizar cualquier operación de filtrado necesaria. Las propiedades `partitionKey`, `rowKey`, `filter` y `take` no se utilizan en este escenario.  

  > [!NOTE]
  > `IQueryable` no es compatible con el [entorno de tiempo de ejecución de Functions v2](functions-versions.md). Una alternativa consiste en [usar un parámetro del método CloudTable](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) para leer la tabla mediante Azure Storage SDK. Si intenta enlazar a `CloudTable` y obtiene un mensaje de error, asegúrese de que tiene una referencia a [la versión correcta del SDK de Storage](./functions-bindings-storage-table.md#azure-storage-sdk-version-in-functions-1x).

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Establezca las propiedades `filter` y `take`. No establezca `partitionKey` o `rowKey`. Obtenga acceso a la entidad (o entidades) de la tabla de entradas mediante `context.bindings.<BINDING_NAME>`. Los objetos deserializados tienen las propiedades `RowKey` y `PartitionKey`.

# <a name="python"></a>[Python](#tab/python)

Los datos de la tabla se pasan a la función como una cadena JSON. Anule la serialización del mensaje mediante una llamada a `json.loads` como se muestra en el ejemplo de [entrada](#example).

# <a name="java"></a>[Java](#tab/java)

El atributo [TableInput](/java/api/com.microsoft.azure.functions.annotation.tableinput) da acceso a la fila de tabla que desencadenó la función.

---

## <a name="next-steps"></a>Pasos siguientes

* [Escritura de datos de Table Storage desde una función](./functions-bindings-storage-table-output.md)
