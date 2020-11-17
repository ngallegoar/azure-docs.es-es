---
title: Administración del grafo de gemelos con relaciones
titleSuffix: Azure Digital Twins
description: Consulte cómo administrar un grafo de gemelos digitales conectándolos con relaciones.
author: baanders
ms.author: baanders
ms.date: 11/03/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 78e0bfb0af494ecae2865fcc42679b8fcce44916
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2020
ms.locfileid: "94359585"
---
# <a name="manage-a-graph-of-digital-twins-using-relationships"></a>Administración de un grafo de gemelos digitales con relaciones

El corazón de Azure Digital Twins es el [grafo de gemelos](concepts-twins-graph.md) que representa todo el entorno. El grafo de gemelos se compone de gemelos digitales individuales conectados mediante **relaciones**. 

Una vez que tenga una [instancia de Azure Digital Twins](how-to-set-up-instance-portal.md) en funcionamiento y que haya configurado el código de [autenticación](how-to-authenticate-client.md) para la aplicación cliente, puede usar las [**API DigitalTwins**](/rest/api/digital-twins/dataplane/twins) para crear, modificar y eliminar gemelos digitales y sus relaciones en una instancia de Azure Digital Twins. También puede usar el [SDK de .NET ( C# )](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true) o la [CLI de Azure Digital Twins](how-to-use-cli.md).

Este artículo está centrado en la administración de las relaciones y el grafo en conjunto; para trabajar con gemelos digitales individuales, consulte [*Procedimiento: Administración de Digital Twins*](how-to-manage-twin.md).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]
    
[!INCLUDE [visualizing with Azure Digital Twins explorer](../../includes/digital-twins-visualization.md)]

## <a name="create-relationships"></a>Crear relaciones

Las relaciones describen cómo los diferentes gemelos digitales se conectan entre sí, lo que constituye la base del grafo de gemelos.

Las relaciones se crean mediante la llamada a `CreateOrReplaceRelationshipAsync()`. 

Para crear una relación, debe especificar:
* El identificador del gemelo de origen (`srcId` en el ejemplo de código siguiente): identificador del gemelo en el que se origina la relación.
* El identificador del gemelo de destino (`targetId` en el ejemplo de código siguiente): identificador del gemelo al que llega la relación.
* Un nombre de relación (`relName` en el ejemplo de código siguiente): tipo genérico de relación, algo como _contains_ (contiene).
* Un identificador de relación (`relId` en el ejemplo de código siguiente): nombre específico de esta relación, algo como _Relationship1_.

El id. de relación debe ser único dentro del gemelo de origen especificado. No es necesario que sea único globalmente.
Por ejemplo, para el gemelo *foo*, cada id. de relación específico debe ser único. Sin embargo, otro gemelo *bar* puede tener una relación de salida que coincida con el mismo id. de relación de *foo*.

En el ejemplo de código siguiente se muestra cómo crear una relación en la instancia de Azure Digital Twins.

```csharp
public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId, string relName)
        {
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = relName
            };

            try
            {
                string relId = $"{srcId}-{relName}->{targetId}";
                await client.CreateOrReplaceRelationshipAsync<BasicRelationship>(srcId, relId, relationship);
                Console.WriteLine($"Created {relName} relationship successfully");
            }
            catch (RequestFailedException rex)
            {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }
            
        }
```
En el método main, ahora puede llamar a la función `CreateRelationship()` para crear una relación de tipo _contains_ de la siguiente forma: 

```csharp
await CreateRelationship(client, srcId, targetId, "contains");
```
Si desea crear varias relaciones, puede repetir las llamadas al mismo método, con tipos de relación diferentes en el argumento. 

Para obtener más información sobre la clase auxiliar `BasicRelationship`, consulte [*Procedimiento: Uso de las API y los SDK de Azure Digital Twins*](how-to-use-apis-sdks.md#serialization-helpers).

### <a name="create-multiple-relationships-between-twins"></a>Creación de varias relaciones entre gemelos

Las relaciones se pueden clasificar como: 

* Relaciones de salida: relaciones que pertenecen a este gemelo que apuntan hacia afuera para conectarlo con otros gemelos. El método `GetRelationshipsAsync()` se usa para obtener las relaciones de salida de un gemelo.
* Relaciones de entrada: relaciones que pertenecen a otros gemelos que apuntan hacia este gemelo para crear un vínculo "entrante". El método `GetIncomingRelationshipsAsync()` se usa para obtener las relaciones de entrada de un gemelo.

No hay ninguna restricción en el número de relaciones que puede tener entre dos gemelos: puede tener tantas como desee. 

Esto significa que puede expresar varios tipos diferentes de relaciones entre dos gemelos a la vez. Por ejemplo, el *gemelo A* puede tener una relación de *almacenamiento* y una relación de *fabricación* con el *gemelo B*.

También puede crear varias instancias del mismo tipo de relación entre los mismos dos gemelos, si lo desea. En este ejemplo, el *gemelo A* podría tener dos relaciones *almacenadas* diferentes con el *gemelo B*, siempre y cuando las relaciones tengan identificadores de relación diferentes.

## <a name="list-relationships"></a>Enumeración de las relaciones

Para acceder a la lista de relaciones de **salida** de un gemelo determinado del grafo, puede usar el método `GetRelationships()` de la siguiente forma:

```csharp
await client.GetRelationships()
```

Esto devuelve un elemento `Azure.Pageable<T>` o `Azure.AsyncPageable<T>`, en función de si se usa la versión sincrónica o asincrónica de la llamada.

Este es un ejemplo que recupera una lista de relaciones:

```csharp
public static async Task<List<BasicRelationship>> FindOutgoingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            try
            {
                // GetRelationshipsAsync will throw if an error occurs
                AsyncPageable<BasicRelationship> rels = client.GetRelationshipsAsync<BasicRelationship>(dtId);
                List<BasicRelationship> results = new List<BasicRelationship>();
                await foreach (BasicRelationship rel in rels)
                {
                    results.Add(rel);
                    Console.WriteLine($"Found relationship-{rel.Name}->{rel.TargetId}");
                }

                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

```
Ahora puede llamar a este método para ver las relaciones de salida de los gemelos como se muestra a continuación:

```csharp
await FindOutgoingRelationshipsAsync(client, twin_Id);
```
Puede usar las relaciones recuperadas para navegar a otros gemelos del grafo. Para ello, lea el campo `target` de la relación que se devuelve y úselo como id. de la siguiente llamada a `GetDigitalTwin()`.

### <a name="find-incoming-relationships-to-a-digital-twin"></a>Búsqueda de relaciones de entrada para un gemelo digital

Azure Digital Twins también dispone de una API para buscar todas las relaciones de *entrada** para un gemelo determinado. Suele ser útil para la navegación inversa o cuando se elimina un gemelo.

El ejemplo de código anterior se centraba en buscar relaciones de salida desde un gemelo. El ejemplo siguiente está estructurado de manera similar, pero en su lugar busca relaciones de *entrada* para el gemelo.

Tenga en cuenta que las llamadas a `IncomingRelationship` no devuelven todo el cuerpo de la relación.

```csharp
public static async Task<List<IncomingRelationship>> FindIncomingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            try
            {
                // GetRelationshipsAsync will throw an error if a problem occurs
                AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

                List<IncomingRelationship> results = new List<IncomingRelationship>();
                await foreach (IncomingRelationship incomingRel in incomingRels)
                {
                    results.Add(incomingRel);
                    Console.WriteLine($"Found incoming relationship-{incomingRel.RelationshipId}");

                }
                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"*** Error {ex.Status}/{ex.ErrorCode} retrieving incoming relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }
```

Ahora puede llamar a este método para ver las relaciones de entrada de los gemelos como se muestra a continuación:

```csharp
await FindIncomingRelationshipsAsync(client, twin_Id);
```
### <a name="list-all-twin-properties-and-relationships"></a>Enumeración de todas las propiedades y relaciones de un gemelo

Con los métodos anteriores para mostrar las relaciones de salida y de entrada de un gemelo, puede crear un método que imprima la información completa del gemelo, incluidas las propiedades del gemelo y ambos tipos de relaciones. Este es un método de ejemplo, llamado `FetchAndPrintTwinAsync()`, que muestra cómo hacerlo.

```csharp  
private static async Task FetchAndPrintTwinAsync(DigitalTwinsClient client, string twin_Id)
        {
            BasicDigitalTwin twin;
            Response<BasicDigitalTwin> res = client.GetDigitalTwin(twin_Id);
            
            await FindOutgoingRelationshipsAsync(client, twin_Id);
            await FindIncomingRelationshipsAsync(client, twin_Id);

            return;
        }
```

Ahora puede llamar a esta función en el método main de la siguiente manera: 

```csharp
await FetchAndPrintTwinAsync(client, targetId);
```
## <a name="delete-relationships"></a>Eliminar relaciones

El primer parámetro especifica el gemelo de origen (el gemelo en el que se origina la relación). El otro parámetro es el id. de relación. Necesita tanto el id. de gemelo como el id. de relación, ya que los id. de relación solo son únicos dentro del ámbito de un gemelo.

```csharp
private static async Task DeleteRelationship(DigitalTwinsClient client, string srcId, string relId)
        {
            try
            {
                Response response = await client.DeleteRelationshipAsync(srcId, relId);
                await FetchAndPrintTwinAsync(srcId, client);
                Console.WriteLine("Deleted relationship successfully");
            }
            catch (RequestFailedException Ex)
            {
                Console.WriteLine($"Error {Ex.ErrorCode}");
            }
        }
```

Ahora puede llamar a este método para eliminar una relación de la siguiente forma:

```csharp
await DeleteRelationship(client, srcId, relId);
```
## <a name="create-a-twin-graph"></a>Creación de un grafo de gemelos 

En el siguiente fragmento de código ejecutable se usan las operaciones de relación de este artículo para crear un grafo de gemelos a partir de gemelos digitales y relaciones.

### <a name="set-up-the-runnable-sample"></a>Configuración del ejemplo ejecutable

En el fragmento de código se usan las definiciones de modelo [*Room.json*](https://github.com/Azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Room.json) y [*Floor.json*](https://github.com/azure-Samples/digital-twins-samples/blob/master/AdtSampleApp/SampleClientApp/Models/Floor.json) de [*Tutorial: Exploración de Azure Digital Twins con una aplicación cliente de ejemplo*](tutorial-command-line-app.md). Puede usar estos vínculos para ir directamente a los archivos o descargarlos como parte del proyecto de ejemplo completo de un extremo a otro [aquí](/samples/azure-samples/digital-twins-samples/digital-twins-samples/). 

Antes de ejecutar el ejemplo, haga lo siguiente:
1. Descargue los archivos del modelo, colóquelos en el proyecto y reemplace los marcadores de posición `<path-to>` en el código siguiente para indicar al programa dónde encontrarlos.
2. Reemplace el marcador de posición `<your-instance-hostname>` por el nombre de host de la instancia de Azure Digital Twins.
3. Agregue estos paquetes al proyecto:
    ```cmd/sh
    dotnet add package Azure.DigitalTwins.Core --version 1.0.0-preview.3
    dotnet add package Azure.identity
    ```

También necesitará configurar las credenciales locales si desea ejecutar el ejemplo directamente. La siguiente sección le indicará cómo hacerlo.
[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](../../includes/digital-twins-local-credentials-outer.md)]

### <a name="run-the-sample"></a>Ejecución del ejemplo

Después de completar los pasos anteriores, puede ejecutar directamente el siguiente código de ejemplo.

```csharp 
using System;
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
using Azure.DigitalTwins.Core.Serialization;
using System.Text.Json;

namespace minimal
{
    class Program
    {

        public static async Task Main(string[] args)
        {
            Console.WriteLine("Hello World!");

            //Create the Azure Digital Twins client for API calls
            DigitalTwinsClient client = createDTClient();
            Console.WriteLine($"Service client created – ready to go");
            Console.WriteLine();

            //Upload models
            Console.WriteLine($"Upload models");
            Console.WriteLine();
            string dtdl = File.ReadAllText("<path-to>/Room.json");
            string dtdl1 = File.ReadAllText("<path-to>/Floor.json");
            var typeList = new List<string>();
            typeList.Add(dtdl);
            typeList.Add(dtdl1);
            // Upload the models to the service
            await client.CreateModelsAsync(typeList);

            //Create new (Floor) digital twin
            BasicDigitalTwin floorTwin = new BasicDigitalTwin();
            string srcId = "myFloorID";
            floorTwin.Metadata = new DigitalTwinMetadata();
            floorTwin.Metadata.ModelId = "dtmi:example:Floor;1";
            //Floor twins have no properties, so nothing to initialize
            //Create the twin
            await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(srcId, floorTwin);
            Console.WriteLine("Twin created successfully");

            //Create second (Room) digital twin
            BasicDigitalTwin roomTwin = new BasicDigitalTwin();
            string targetId = "myRoomID";
            roomTwin.Metadata = new DigitalTwinMetadata();
            roomTwin.Metadata.ModelId = "dtmi:example:Room;1";
            // Initialize properties
            Dictionary<string, object> props = new Dictionary<string, object>();
            props.Add("Temperature", 35.0);
            props.Add("Humidity", 55.0);
            roomTwin.Contents = props;
            //Create the twin
            await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(targetId, roomTwin);
            
            //Create relationship between them
            await CreateRelationship(client, srcId, targetId, "contains");
            Console.WriteLine();

            //Print twins and their relationships
            Console.WriteLine("--- Printing details:");
            Console.WriteLine("Outgoing relationships from source twin:");
            await FetchAndPrintTwinAsync(srcId, client);
            Console.WriteLine();
            Console.WriteLine("Incoming relationships to target twin:");
            await FetchAndPrintTwinAsync(targetId, client);
            Console.WriteLine("--------");
            Console.WriteLine();

            //Delete the relationship
            Console.WriteLine("Deleting the relationship");
            await DeleteRelationship(client, srcId, $"{srcId}-contains->{targetId}");
            Console.WriteLine();

            //Print twins and their relationships again
            Console.WriteLine("--- Printing details:");
            Console.WriteLine("Outgoing relationships from source twin:");
            await FetchAndPrintTwinAsync(srcId, client);
            Console.WriteLine();
            Console.WriteLine("Incoming relationships to target twin:");
            await FetchAndPrintTwinAsync(targetId, client);
            Console.WriteLine("--------");
            Console.WriteLine();
        }

        private static DigitalTwinsClient createDTClient()
        {
            string adtInstanceUrl = "https://<your-instance-hostname>";
            var credentials = new DefaultAzureCredential();
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
            return client;
        }
        private async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId, string relName)
        {
            // Create relationship between twins
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = relName
            };

            try
            {
                string relId = $"{srcId}-{relName}->{targetId}";
                await client.CreateOrReplaceRelationshipAsync<BasicRelationship>(srcId, relId, relationship);
                Console.WriteLine($"Created {relName} relationship successfully");
            }
            catch (RequestFailedException rex)
            {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }

        }

        private static async Task FetchAndPrintTwinAsync(string twin_Id, DigitalTwinsClient client)
        {
            BasicDigitalTwin twin;
            Response<BasicDigitalTwin> res = client.GetDigitalTwin(twin_Id);
            await FindOutgoingRelationshipsAsync(client, twin_Id);
            await FindIncomingRelationshipsAsync(client, twin_Id);

            return;
        }

        private static async Task<List<BasicRelationship>> FindOutgoingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            
            try
            {
                // GetRelationshipsAsync will throw if an error occurs
                AsyncPageable<BasicRelationship> rels = client.GetRelationshipsAsync<BasicRelationship>(dtId);
                List<BasicRelationship> results = new List<BasicRelationship>();
                await foreach (BasicRelationship rel in rels)
                {
                    results.Add(rel);
                    Console.WriteLine($"Found relationship-{rel.Name}->{rel.TargetId}");
                }

                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"*** Error {ex.Status}/{ex.ErrorCode} retrieving relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

        private static async Task<List<IncomingRelationship>> FindIncomingRelationshipsAsync(DigitalTwinsClient client, string dtId)
        {
            // Find the relationships for the twin
            
            try
            {
                // GetRelationshipsAsync will throw an error if a problem occurs
                AsyncPageable<IncomingRelationship> incomingRels = client.GetIncomingRelationshipsAsync(dtId);

                List<IncomingRelationship> results = new List<IncomingRelationship>();
                await foreach (IncomingRelationship incomingRel in incomingRels)
                {
                    results.Add(incomingRel);
                    Console.WriteLine($"Found incoming relationship-{incomingRel.RelationshipId}");
                }
                return results;
            }
            catch (RequestFailedException ex)
            {
                Console.WriteLine($"**_ Error {ex.Status}/{ex.ErrorCode} retrieving incoming relationships for {dtId} due to {ex.Message}");
                return null;
            }
        }

        private static async Task DeleteRelationship(DigitalTwinsClient client, string srcId, string relId)
        {
            try
            {
                Response response = await client.DeleteRelationshipAsync(srcId, relId);
                await FetchAndPrintTwinAsync(srcId, client);
                Console.WriteLine("Deleted relationship successfully");
            }
            catch (RequestFailedException Ex)
            {
                Console.WriteLine($"Error {Ex.ErrorCode}");
            }
        }
    }
}
```

Esta es la salida de consola del programa anterior: 

:::image type="content" source="./media/how-to-manage-graph/console-output-twin-graph.png" alt-text="Salida de consola que muestra los detalles de los gemelos y las relaciones entrantes y salientes de los gemelos." lightbox="./media/how-to-manage-graph/console-output-twin-graph.png":::

> [!TIP]
> El grafo de gemelos es un concepto de creación de relaciones entre gemelos. Si desea ver la representación visual del grafo de gemelos, consulte la sección [Visualización](how-to-manage-graph.md#visualization) de este artículo. 

### <a name="create-a-twin-graph-from-a-csv-file"></a>Creación de un grafo de gemelos a partir de un archivo .csv

En casos de uso prácticos, las jerarquías de gemelos se crean a menudo a partir de los datos almacenados en una base de datos diferente o quizás en una hoja de cálculo o un archivo .csv. En esta sección se ilustra cómo leer datos de un archivo .csv y crear un grafo gemelo con ellos.

Observe la siguiente tabla de datos, que describe un conjunto de gemelos digitales y las relaciones.

|  Id. de modelo    | Identificador del gemelo (debe ser único) | Nombre de relación  | Identificador del gemelo de destino  | Datos de inicialización del gemelo |
| --- | --- | --- | --- | --- |
| dtmi:example:Floor;1    | Floor1 | contiene | Room1 | |
| dtmi:example:Floor;1    | Floor0 | contains | Room0 | |
| dtmi:example:Room;1    | Room1 | | | {"Temperature": 80} |
| dtmi:example:Room;1    | Room0 | | | {"Temperature": 70} |

Una forma de introducir estos datos en Azure Digital Twins es convertir la tabla en un archivo .csv y escribir código que interprete el archivo en comandos para crear gemelos y relaciones. El siguiente código de ejemplo ilustra la lectura de los datos del archivo .csv y la creación de un grafo gemelo en Azure Digital Twins.

En el código siguiente, el archivo .csv se llama *data.csv* y hay un marcador de posición que representa el **nombre de host** de su instancia de Azure Digital Twins. El ejemplo también usa varios paquetes, que se pueden agregar al proyecto para que faciliten este proceso.

```csharp
using System;
using System.Collections.Generic;
using System.Text.Json;
using System.Threading.Tasks;
using Azure;
using Azure.DigitalTwins.Core;
using Azure.Identity;

namespace creating_twin_graph_from_csv
{
    class Program
    {
        static async Task Main(string[] args)
        {
            List<BasicRelationship> RelationshipRecordList = new List<BasicRelationship>();
            List<BasicDigitalTwin> TwinList = new List<BasicDigitalTwin>();
            List<List<string>> data = ReadData();
            DigitalTwinsClient client = createDTClient();

            // Interpret the CSV file data, by each row
            foreach (List<string> row in data)
            {
                string modelID = row.Count > 0 ? row[0].Trim() : null;
                string srcID = row.Count > 1 ? row[1].Trim() : null;
                string relName = row.Count > 2 ? row[2].Trim() : null;
                string targetID = row.Count > 3 ? row[3].Trim() : null;
                string initProperties = row.Count > 4 ? row[4].Trim() : null;
                Console.WriteLine($"ModelID: {modelID}, TwinID: {srcID}, RelName: {relName}, TargetID: {targetID}, InitData: {initProperties}");
                Dictionary<string, object> props = new Dictionary<string, object>();
                // Parse properties into dictionary (left out for compactness)
                // ...

                // Null check for source and target ID's
                if (srcID != null && srcID.Length > 0 && targetID != null && targetID.Length > 0)
                {
                    BasicRelationship br = new BasicRelationship()
                    {
                        SourceId = srcID,
                        TargetId = targetID,
                        Name = relName
                    };
                    RelationshipRecordList.Add(br);
                }
                BasicDigitalTwin srcTwin = new BasicDigitalTwin();
                srcTwin.Id = srcID;
                srcTwin.Metadata = new DigitalTwinMetadata();
                srcTwin.Metadata.ModelId = modelID;
                srcTwin.Contents = props;
                TwinList.Add(srcTwin);
            }

            // Create digital twins 
            foreach (BasicDigitalTwin twin in TwinList)
            {
                try
                {
                    await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(twin.Id, twin);
                    Console.WriteLine("Twin is created");
                }
                catch (RequestFailedException e)
                {
                    Console.WriteLine($"Error {e.Status}: {e.Message}");
                }
            }
            // Create relationships between the twins
            foreach (BasicRelationship rec in RelationshipRecordList)
            {
                try
                {
                    string relId = $"{rec.SourceId}-{rec.Name}->{rec.TargetId}";
                    await client.CreateOrReplaceRelationshipAsync<BasicRelationship>(rec.SourceId, relId, rec);
                    Console.WriteLine("Relationship is created");
                }
                catch (RequestFailedException e)
                {
                    Console.WriteLine($"Error {e.Status}: {e.Message}");
                }
            }
        }

        // Method to ingest data from the CSV file
        public static List<List<string>> ReadData()
        {
            string path = "<path-to>/data.csv";
            string[] lines = System.IO.File.ReadAllLines(path);
            List<List<string>> data = new List<List<string>>();
            int count = 0;
            foreach (string line in lines)
            {
                if (count++ == 0)
                    continue;
                List<string> cols = new List<string>();
                data.Add(cols);
                string[] columns = line.Split(',');
                foreach (string column in columns)
                {
                    cols.Add(column);
                }
            }
            return data;
        }
        // Method to create the digital twins client
        private static DigitalTwinsClient createDTClient()
        {

            string adtInstanceUrl = "https://<your-instance-hostname>";
            var credentials = new DefaultAzureCredential();
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credentials);
            return client;
        }
    }
}

```
## <a name="manage-relationships-with-cli"></a>Administrar relaciones con la CLI

Los gemelos y sus relaciones también se pueden administrar mediante la CLI de Azure Digital Twins. Los comandos se pueden encontrar en [*Procedimiento: Uso de la CLI de Azure Digital Twins*](how-to-use-cli.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo consultar un grafo de gemelos de Azure Digital Twins:
* [*Conceptos: Lenguaje de consulta*](concepts-query-language.md)
* [*Procedimiento: Consulta del grafo gemelo*](how-to-query-graph.md)