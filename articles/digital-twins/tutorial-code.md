---
title: 'Tutorial: Programación de una aplicación cliente'
titleSuffix: Azure Digital Twins
description: Tutorial para escribir el código mínimo para una aplicación cliente mediante el SDK de .NET (C#).
author: baanders
ms.author: baanders
ms.date: 11/02/2020
ms.topic: tutorial
ms.service: digital-twins
ms.openlocfilehash: 3225fff1c82822dee990804f934ada86068841e8
ms.sourcegitcommit: 58f12c358a1358aa363ec1792f97dae4ac96cc4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/03/2020
ms.locfileid: "93280281"
---
# <a name="tutorial-coding-with-the-azure-digital-twins-apis"></a>Tutorial: Programación con las API de Azure Digital Twins

Es habitual que los desarrolladores que trabajan con Azure Digital Twins escriban una aplicación que interactúe con su instancia del servicio Azure Digital Twins. Este tutorial orientado al desarrollador proporciona una introducción a la programación para el servicio Azure Digital Twins, mediante el [SDK de Azure Digital Twins para .NET (C#)](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true). Le guía a través de la escritura de una aplicación cliente de consola paso a paso en C#, empezando desde cero.

> [!div class="checklist"]
> * Configuración del proyecto
> * Introducción al código del proyecto   
> * Ejemplo con todo el código
> * Limpieza de recursos
> * Pasos siguientes

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se usa la línea de comandos para configurar el proyecto y trabajar en él. Por lo tanto, puede usar cualquier editor de código para realizar los ejercicios.

Lo que necesita para comenzar:
* Cualquier editor de código
* **.NET Core 3.1** en la máquina de desarrollo. Esta versión del SDK de .NET Core se puede descargar para varias plataformas desde la página de [descarga de .NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1).

### <a name="prepare-an-azure-digital-twins-instance"></a>Preparación de una instancia de Azure Digital Twins

[!INCLUDE [Azure Digital Twins: instance prereq](../../includes/digital-twins-prereq-instance.md)]

[!INCLUDE [Azure Digital Twins: local credentials prereq (outer)](../../includes/digital-twins-local-credentials-outer.md)]

## <a name="set-up-project"></a>Configuración del proyecto

Una vez que esté listo para trabajar con la instancia de Azure Digital Twins, empiece a configurar el proyecto de aplicación cliente. 

Abra un símbolo del sistema u otra ventana de la consola en la máquina y cree un directorio de proyecto vacío donde quiera almacenar el trabajo durante este tutorial. Asigne al directorio el nombre que desee (por ejemplo, *DigitalTwinsCodeTutorial* ).

Vaya al nuevo directorio.

Una vez en el directorio del proyecto, **cree un proyecto de aplicación de consola .NET vacío**. En la ventana de comandos, puede ejecutar el siguiente comando para crear un proyecto de C# mínimo para la consola:

```cmd/sh
dotnet new console
```

Se crean varios archivos dentro del directorio, incluido uno llamado *Program.cs* en el que escribirá la mayor parte del código.

Mantenga abierta la ventana de comandos, ya que la seguirá usando durante todo el tutorial.

A continuación, **agregue dos dependencias al proyecto** , que necesitará para trabajar con Azure Digital Twins. Puede usar los vínculos siguientes para desplazarse a los paquetes NuGet, en donde encontrará los comandos de la consola (incluida la CLI de .NET) para agregar la versión más reciente de cada paquete al proyecto.
* [**Azure.DigitalTwins.Core**](https://www.nuget.org/packages/Azure.DigitalTwins.Core). Este es el paquete del [SDK de Azure Digital Twins para .NET](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true). 
* [**Azure.Identity**](https://www.nuget.org/packages/Azure.Identity). Esta biblioteca proporciona herramientas para facilitar la autenticación en Azure.

## <a name="get-started-with-project-code"></a>Introducción al código del proyecto

En esta sección, comenzará a escribir el código del nuevo proyecto de aplicación para trabajar con Azure Digital Twins. Las acciones que se van a realizar son:
* Autenticación en el servicio
* Carga de un modelo
* Almacenamiento en caché de los errores
* Creación de los gemelos digitales
* Creación de las relaciones
* Consulta de los gemelos digitales

También hay una sección en la que se muestra el código completo al final del tutorial. Puede usarla como referencia para comprobar el programa a medida que avanza.

Para empezar, abra el archivo *Program.cs* en cualquier editor de código. Verá una plantilla de código mínimo parecida a esta:

```csharp
using System;

namespace DigitalTwinsCodeTutorial
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

En primer lugar, agregue algunas líneas `using` en la parte superior del código para extraer las dependencias necesarias.

```csharp
using Azure.DigitalTwins.Core;
using Azure.Identity;
```

Luego, agregará código a este archivo para rellenar alguna funcionalidad. 

### <a name="authenticate-against-the-service"></a>Autenticación en el servicio

Lo primero que tendrá que hacer la aplicación es autenticarse en el servicio Azure Digital Twins. Después, puede crear una clase de cliente de servicio para acceder a las funciones del SDK.

Para realizar la autenticación, necesita el *nombre de host* de la instancia de Azure Digital Twins.

En *Program.cs* , pegue el siguiente código debajo de la línea impresa "Hello, World!" en el método `Main`. Establezca el valor de `adtInstanceUrl` en el *nombre de host* de la instancia de Azure Digital Twins.

```csharp
string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-hostName>"; 
var credential = new DefaultAzureCredential();
DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
Console.WriteLine($"Service client created – ready to go");
```

Guarde el archivo. 

En la ventana de comandos, ejecute el código con este comando: 

```cmd/sh
dotnet run
```

Se restaurarán las dependencias en la primera ejecución y, luego, se ejecutará el programa. 
* Si no se produce ningún error, el programa devolverá el mensaje *Service client created - ready to go* (Se ha creado el servicio de cliente: listo para continuar).
* Cómo aún no hay ningún control de errores en este proyecto, si hay algún problema, verá que el código inicia una excepción.

### <a name="upload-a-model"></a>Carga de un modelo

Azure Digital Twins carece de vocabulario de dominio intrínseco. Los tipos de elementos del entorno que puede representar en Azure Digital Twins los define el usuario mediante **modelos**. Los [modelos](concepts-models.md) son parecidos a las clases de los lenguajes de programación orientados a objetos en el sentido de que proporcionan plantillas definidas por el usuario para [gemelos digitales](concepts-twins-graph.md), las cuales se siguen. Más adelante también se crean instancias de estas plantillas. Se escriben en un lenguaje similar a JSON denominado **lenguaje de definición de gemelos digitales (DTDL)** .

El primer paso para crear una solución de Azure Digital Twins es definir al menos un modelo en un archivo DTDL.

En el directorio en el que creó el proyecto, cree un archivo *.json* llamado *SampleModel.json*. Pegue el siguiente cuerpo de archivo: 

```json
{
  "@id": "dtmi:example:SampleModel;1",
  "@type": "Interface",
  "displayName": "SampleModel",
  "contents": [
    {
      "@type": "Relationship",
      "name": "contains"
    },
    {
      "@type": "Property",
      "name": "data",
      "schema": "string"
    }
  ],
  "@context": "dtmi:dtdl:context;2"
}
```

> [!TIP]
> Si va a usar Visual Studio en este tutorial, es posible que quiera seleccionar el archivo JSON recién creado y establecer la propiedad *Copiar en el directorio de salida* del inspector de propiedad en *Copiar si es más reciente* o *Copia siempre*. De esta forma, Visual Studio podrá encontrar el archivo JSON con la ruta de acceso predeterminada cuando ejecute el programa con **F5** durante el resto del tutorial.

> [!TIP] 
> Hay un [ejemplo de validador de DTDL](/samples/azure-samples/dtdl-validator/dtdl-validator) independiente del lenguaje que puede usar para comprobar los documentos del modelo y asegurarse de que el DTDL sea válido. Se basa en la biblioteca del analizador de DTDL, sobre la que puede leer más en [*Procedimiento: Análisis y validación de modelos*](how-to-parse-models.md).

Después, agregue más código a *Program.cs* para cargar el modelo que acaba de crear en la instancia de Azure Digital Twins.

Primero, agregue algunas instrucciones `using` en la parte superior del archivo:

```csharp
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
```

A continuación, prepárese para usar los métodos asincrónicos del SDK de servicio de C#; para ello, cambie la firma del método `Main` y así permitir la ejecución asincrónica. 

```csharp
static async Task Main(string[] args)
```

> [!NOTE]
> El uso de `async` no es estrictamente necesario, ya que el SDK también proporciona versiones sincrónicas de todas las llamadas. En este tutorial se practica con `async`.

A continuación, viene el primer bit de código que interactúa con el servicio Azure Digital Twins. Este código carga el archivo DTDL que creó desde el disco y, luego, lo carga en la instancia del servicio Azure Digital Twins. 

Pegue el código siguiente en el código de autorización que agregó anteriormente.

```csharp
Console.WriteLine();
Console.WriteLine($"Upload a model");
var typeList = new List<string>();
string dtdl = File.ReadAllText("SampleModel.json");
typeList.Add(dtdl);
// Upload the model to the service
await client.CreateModelsAsync(typeList);
```

En la ventana de comandos, ejecute el programa con este comando: 

```cmd/sh
dotnet run
```
En la salida se imprimirá "Upload a model" (Cargar un modelo), lo que indica que se ha llegado al código, pero aún no hay ninguna salida para indicar si la carga se realizó correctamente o no.

Para agregar una instrucción de impresión que muestre que todos los modelos se han cargado correctamente en la instancia, agregue el código siguiente justo después de la sección anterior:

```csharp
// Read a list of models back from the service
Console.WriteLine("Models uploaded to the instance:");
AsyncPageable<DigitalTwinsModelData> modelDataList = client.GetModelsAsync();
await foreach (DigitalTwinsModelData md in modelDataList)
{
    Console.WriteLine($"{md.Id}");
}
```

**Antes de volver a ejecutar el programa para probar este nuevo código** recuerde que la última vez que ejecutó el programa ya cargó el modelo. Azure Digital Twins no permite cargar un mismo modelo dos veces, por lo que si intenta volver a cargar un modelo, el programa debería generar una excepción.

Teniendo esto en cuenta, ejecute de nuevo el programa con este comando en la ventana de comandos:

```cmd/sh
dotnet run
```

El programa iniciará una excepción. Al intentar cargar un modelo que ya se ha cargado, el servicio devuelve un error de "solicitud incorrecta" a través de la API REST. Como resultado, el SDK de cliente de Azure Digital Twins inicia a su vez una excepción por cada servicio que devuelva un código que no sea correcto. 

La siguiente sección trata de excepciones como esta y como abordarlas en el código.

### <a name="catch-errors"></a>Almacenamiento en caché de los errores

Para evitar que el programa se bloquee, puede agregar código de excepción alrededor del código de carga del modelo. Encapsule la llamada al cliente existente `await client.CreateModelsAsync(typeList)` en un controlador try/catch, como este:

```csharp
try {
    await client.CreateModelsAsync(typeList);
} catch (RequestFailedException rex) {
    Console.WriteLine($"Load model: {rex.Status}:{rex.Message}");
}
```

Si ahora ejecuta el programa con `dotnet run` en la ventana de comandos, verá que recibe un código de error. La salida del código de creación del modelo muestra este error:

:::image type="content" source= "media/tutorial-code/model-error.png" alt-text="Salida del programa, que muestra el mensaje &quot;409:Service request failed. Status: 409 (Conflict).&quot; [409: error de solicitud de servivio. Estado: 409 (conflicto)], seguido de una impresión del error que indica que dtmi:example: SampleModel;1 ya existe":::

A partir de este punto, en el tutorial se encapsulan todas las llamadas a los métodos de servicio de los controladores try/catch.

### <a name="create-digital-twins"></a>Creación de gemelos digitales

Ahora que ha cargado un modelo en Azure Digital Twins, puede usar esta definición de modelo para crear **gemelos digitales**. Los [gemelos digitales](concepts-twins-graph.md) son instancias de un modelo y representan las entidades del entorno empresarial, como los sensores de una granja, las salas de un edificio o las luces de un coche. En esta sección se crean algunos gemelos digitales basados en el modelo que cargó anteriormente.

Agregue el código siguiente al final del método `Main` para crear e inicializar tres gemelos digitales basados en este modelo.

```csharp
// Initialize twin data
BasicDigitalTwin twinData = new BasicDigitalTwin();
twinData.Metadata.ModelId = "dtmi:example:SampleModel;1";
twinData.Contents.Add("data", $"Hello World!");

string prefix="sampleTwin-";
for(int i=0; i<3; i++) {
    try {
        twinData.Id = $"{prefix}{i}";
        await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(twinData.Id, twinData);
        Console.WriteLine($"Created twin: {prefix}{i}");
    } catch(RequestFailedException rex) {
        Console.WriteLine($"Create twin error: {rex.Status}:{rex.Message}");  
    }
}
```

En la ventana de comandos, ejecute el programa con `dotnet run`. En la salida, busque los mensajes de impresión de que se han creado *sampleTwin-0* , *sampleTwin-1* y *sampleTwin-2*. 

Luego, Vuelva a ejecutar el programa. 

Tenga en cuenta que no se produce ningún error cuando los gemelos se crean la segunda vez, aunque ya existen después de la primera ejecución. A diferencia de la creación de modelos, la creación de gemelos es, en el nivel de REST, una llamada *PUT* con semántica *upsert*. Esto significa que si ya existe un gemelo, un intento de volver a crear el mismo gemelo reemplazará al gemelo original. No aparece ningún error.

### <a name="create-relationships"></a>Crear relaciones

A continuación, puede crear **relaciones** entre estos gemelos que ha creado para conectarlos en un **gráfico de gemelos**. Los [gráficos de gemelos](concepts-twins-graph.md) se usan para representar un entorno completo.

Agregue un **nuevo método estático** a la clase `Program`, debajo del método `Main`: (ahora el código tiene dos métodos):

```csharp
public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId)
{
    var relationship = new BasicRelationship
    {
        TargetId = targetId,
        Name = "contains"
    };

    try
    {
        string relId = $"{srcId}-contains->{targetId}";
        await client.CreateOrReplaceRelationshipAsync(srcId, relId, relationship);
        Console.WriteLine("Created relationship successfully");
    }
    catch (RequestFailedException rex) {
        Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
    }
}
```

A continuación, agregue el siguiente código al final del método `Main` para llamar al método `CreateRelationship` y utilizar el código que acaba de escribir:

```csharp
// Connect the twins with relationships
await CreateRelationship(client, "sampleTwin-0", "sampleTwin-1");
await CreateRelationship(client, "sampleTwin-0", "sampleTwin-2");
```

En la ventana de comandos, ejecute el programa con `dotnet run`. En la salida, busque instrucciones de impresión que digan que las dos relaciones se han creado correctamente.

Tenga en cuenta que Azure Digital Twins no permite crear una relación si ya existe otra con el mismo identificador, por lo que, si ejecuta el programa varias veces, observará excepciones en la creación de la relación. Este código almacena en caché las excepciones y las omite. 

### <a name="list-relationships"></a>Enumeración de las relaciones

El siguiente código que va a agregar le permite ver la lista de relaciones que ha creado.

Agregue el **nuevo método** siguiente a la clase `Program`:

```csharp
public async static Task ListRelationships(DigitalTwinsClient client, string srcId)
{
    try {
        AsyncPageable<BasicRelationship> results = client.GetRelationshipsAsync<BasicRelationship>(srcId);
        Console.WriteLine($"Twin {srcId} is connected to:");
        await foreach (BasicRelationship rel in results)
        {
            Console.WriteLine($" -{rel.Name}->{rel.TargetId}");
        }
    } catch (RequestFailedException rex) {
        Console.WriteLine($"Relationship retrieval error: {rex.Status}:{rex.Message}");   
    }
}
```

Luego, agregue el siguiente código al final del método `Main` para llamar al código `ListRelationships`:

```csharp
//List the relationships
await ListRelationships(client, "sampleTwin-0");
```

En la ventana de comandos, ejecute el programa con `dotnet run`. Debería ver una relación de todas las relaciones que ha creado en una instrucción de salida similares a esta:

:::image type="content" source= "media/tutorial-code/list-relationships.png" alt-text="Salida del programa, que muestra el mensaje &quot;Twin sampleTwin-0 is connected to: contains->sampleTwin-1, -contains->sampleTwin-2&quot;":::

### <a name="query-digital-twins"></a>Consulta de los gemelos digitales

Una de las principales características de Azure Digital Twins es la posibilidad de [consultar](concepts-query-language.md) el gráfico de gemelos de forma fácil y eficaz para responder a las preguntas sobre el entorno.

La última sección del código que se va a agregar en este tutorial ejecuta una consulta en la instancia de Azure Digital Twins. La consulta usada en este ejemplo devuelve todos los gemelos digitales de la instancia.

Agregue esta instrucción `using` para permitir el uso de la clase `JsonSerializer` para ayudar a presentar la información de los gemelos digitales:

```csharp
using System.Text.Json;
```

Luego, agregue el siguiente código al final del método `Main`:

```csharp
// Run a query for all twins   
string query = "SELECT * FROM digitaltwins";
AsyncPageable<BasicDigitalTwin> result = client.QueryAsync<BasicDigitalTwin>(query);

await foreach (BasicDigitalTwin twin in result)
{
    Console.WriteLine(JsonSerializer.Serialize(twin));
    Console.WriteLine("---------------");
}
```

En la ventana de comandos, ejecute el programa con `dotnet run`. En la salida verá todos los gemelos digitales de esta instancia.

## <a name="complete-code-example"></a>Finalización del ejemplo de código

En este punto del tutorial, tiene una aplicación cliente completa, capaz de llevar a cabo acciones básicas en Azure Digital Twins. Como referencia, a continuación, se muestra el código completo del programa en *Program.cs* :

```csharp
using System;
using Azure.DigitalTwins.Core;
using Azure.Identity;
using System.Threading.Tasks;
using System.IO;
using System.Collections.Generic;
using Azure;
using System.Text.Json;

namespace minimal
{
    class Program
    {
        static async Task Main(string[] args)
        {
            Console.WriteLine("Hello World!");
            
            string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-hostName>"; 
            
            var credential = new DefaultAzureCredential();
            DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
            Console.WriteLine($"Service client created – ready to go");

            Console.WriteLine();
            Console.WriteLine($"Upload a model");
            var typeList = new List<string>();
            string dtdl = File.ReadAllText("SampleModel.json");
            typeList.Add(dtdl);

            // Upload the model to the service
            try {
                await client.CreateModelsAsync(typeList);
            } catch (RequestFailedException rex) {
                Console.WriteLine($"Load model: {rex.Status}:{rex.Message}");
            }
            // Read a list of models back from the service
            Console.WriteLine("Models uploaded to the instance:");
            AsyncPageable<DigitalTwinsModelData> modelDataList = client.GetModelsAsync();
            await foreach (DigitalTwinsModelData md in modelDataList)
            {
                Console.WriteLine($"{md.Id}");
            }

            // Initialize twin data
            BasicDigitalTwin twinData = new BasicDigitalTwin();
            twinData.Metadata.ModelId = "dtmi:example:SampleModel;1";
            twinData.Contents.Add("data", $"Hello World!");
            
            string prefix="sampleTwin-";
            for(int i=0; i<3; i++) {
                try {
                    twinData.Id = $"{prefix}{i}";
                    await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>(twinData.Id, twinData);
                    Console.WriteLine($"Created twin: {prefix}{i}");
                } catch(RequestFailedException rex) {
                    Console.WriteLine($"Create twin error: {rex.Status}:{rex.Message}");  
                }
            }

            // Connect the twins with relationships
            await CreateRelationship(client, "sampleTwin-0", "sampleTwin-1");
            await CreateRelationship(client, "sampleTwin-0", "sampleTwin-2");

            //List the relationships
            await ListRelationships(client, "sampleTwin-0");

            // Run a query for all twins   
            string query = "SELECT * FROM digitaltwins";
            AsyncPageable<BasicDigitalTwin> result = client.QueryAsync<BasicDigitalTwin>(query);
            
            await foreach (BasicDigitalTwin twin in result)
            {
                Console.WriteLine(JsonSerializer.Serialize(twin));
                Console.WriteLine("---------------");
            }
        }

        public async static Task CreateRelationship(DigitalTwinsClient client, string srcId, string targetId)
        {
            var relationship = new BasicRelationship
            {
                TargetId = targetId,
                Name = "contains"
            };
        
            try
            {
                string relId = $"{srcId}-contains->{targetId}";
                await client.CreateOrReplaceRelationshipAsync(srcId, relId, relationship);
                Console.WriteLine("Created relationship successfully");
            }
            catch (RequestFailedException rex) {
                Console.WriteLine($"Create relationship error: {rex.Status}:{rex.Message}");
            }
        }
        
        public async static Task ListRelationships(DigitalTwinsClient client, string srcId)
        {
            try {
                AsyncPageable<BasicRelationship> results = client.GetRelationshipsAsync<BasicRelationship>(srcId);
                Console.WriteLine($"Twin {srcId} is connected to:");
                await foreach (BasicRelationship rel in results)
                {
                    Console.WriteLine($" -{rel.Name}->{rel.TargetId}");
                }
            } catch (RequestFailedException rex) {
                Console.WriteLine($"Relationship retrieval error: {rex.Status}:{rex.Message}");   
            }
        }

    }
}
```
## <a name="clean-up-resources"></a>Limpieza de recursos
 
La instancia usada en este tutorial se puede reutilizar en [*Tutorial: Exploración de los conceptos básicos con una aplicación cliente de ejemplo*](tutorial-command-line-app.md). Si tiene previsto continuar con el siguiente tutorial, puede mantener la instancia de Azure Digital Twins que configuró aquí.
 
[!INCLUDE [digital-twins-cleanup-basic.md](../../includes/digital-twins-cleanup-basic.md)]

Por último, elimine la carpeta del proyecto que creó en la máquina local.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado una aplicación cliente de consola .NET desde cero. Escribió código para ella de forma que realizara las acciones básicas en una instancia de Azure Digital Twins.

Continúe con el siguiente tutorial para explorar lo que puede hacer con esta aplicación cliente de ejemplo: 

> [!div class="nextstepaction"]
> [*Tutorial: Exploración de los conceptos básicos con una aplicación cliente de ejemplo*](tutorial-command-line-app.md)
