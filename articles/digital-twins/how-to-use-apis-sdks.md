---
title: Uso de las API y los SDK de Azure Digital Twins
titleSuffix: Azure Digital Twins
description: Vea cómo trabajar con las API de Azure Digital Twins, incluido con el SDK.
author: baanders
ms.author: baanders
ms.date: 06/04/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: a09563e787573f3cb3dd510229822a0b0fbcb6c9
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93395893"
---
# <a name="use-the-azure-digital-twins-apis-and-sdks"></a>Uso de las API y los SDK de Azure Digital Twins

Azure Digital Twins incluye las **API de plano de control** y las **API de plano de datos** para administrar su instancia y sus elementos. 
* Las API de plano de control son las API de [Azure Resource Manager (ARM)](../azure-resource-manager/management/overview.md) y cubren las operaciones de administración de recursos como la creación y eliminación de la instancia. 
* Las API de plano de datos son las API de Azure Digital Twins y se usan para operaciones de administración de datos, como la administración de modelos, gemelos y el grafo.

En este artículo se proporciona información general sobre las API disponibles y los métodos para interactuar con ellas. Puede usar las API REST directamente con las instancias de Swagger asociadas o mediante un SDK.

## <a name="overview-control-plane-apis"></a>Información general: API de plano de control

Las API del plano de control son las API de [ARM](../azure-resource-manager/management/overview.md) que se usan para administrar la instancia de Azure Digital Twins como un todo, por lo que cubren operaciones como la creación o eliminación de toda la instancia. También se usarán para crear y eliminar puntos de conexión.

La versión más reciente de la API de plano de control es _**2020-10-31**_.

Para usar las API de plano de control:
* Para llamar a las API directamente, haga referencia al Swagger más reciente en la [carpeta Swagger del plano de control](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/resource-manager/Microsoft.DigitalTwins). Este repositorio también incluye una carpeta de ejemplos que muestran el uso.
* Actualmente puede acceder a los SDK para las API de control en...
  - [ **.NET (C#)** ](https://www.nuget.org/packages/Microsoft.Azure.Management.DigitalTwins/) ([referencia [generada automáticamente]](/dotnet/api/overview/azure/digitaltwins/management?view=azure-dotnet&preserve-view=true)) ([código fuente](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Microsoft.Azure.Management.DigitalTwins))
  - [**Java**](https://search.maven.org/artifact/com.microsoft.azure.digitaltwins.v2020_10_31/azure-mgmt-digitaltwins/1.0.0/jar) ([referencia [generada automáticamente]](/java/api/overview/azure/digitaltwins/management?preserve-view=true&view=azure-java-preview)) ([código fuente](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/digitaltwins/mgmt-v2020_10_31))
  - [**JavaScript**](https://www.npmjs.com/package/@azure/arm-digitaltwins) ([código fuente](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/digitaltwins/arm-digitaltwins))
  - [**Python**](https://pypi.org/project/azure-mgmt-digitaltwins/) ([código fuente](https://github.com/Azure/azure-sdk-for-python/tree/release/v3/sdk/digitaltwins/azure-mgmt-digitaltwins))
  - [**Go**](https://godoc.org/github.com/Azure/azure-sdk-for-go/services/digitaltwins/mgmt/2020-10-31/digitaltwins) ([código fuente](https://github.com/Azure/azure-sdk-for-go/tree/master/services/digitaltwins/mgmt/2020-10-31/digitaltwins))

También puede ejercitar las API de plano de control interactuando con Azure Digital Twins mediante [Azure Portal](https://portal.azure.com) y la [CLI](how-to-use-cli.md).

## <a name="overview-data-plane-apis"></a>Información general: API de plano de datos

Las API de plano de datos son las API de Azure Digital Twins que se usan para administrar los elementos de la instancia de Azure Digital Twins. Incluyen operaciones como la creación de rutas, la carga de modelos, la creación de relaciones y la administración de gemelos. Se pueden dividir ampliamente en las siguientes categorías:
* **DigitalTwinModels**: la categoría DigitalTwinModels contiene las API para administrar los [modelos](concepts-models.md) en una instancia de Azure Digital Twins. Las actividades de administración incluyen la carga, validación, recuperación y eliminación de modelos creados en DTDL.
* **DigitalTwins**: la categoría DigitalTwins contiene las API que permiten a los desarrolladores crear, modificar y eliminar [gemelos digitales](concepts-twins-graph.md) y sus relaciones en una instancia de Azure Digital Twins.
* **Query**: la categoría Query permite a los desarrolladores [buscar conjuntos de gemelos digitales en el grafo de gemelos](how-to-query-graph.md) entre las relaciones.
* **Event Routes**: la categoría Event Routes contiene las API para [enrutar datos](concepts-route-events.md), en el sistema y a los servicios de bajada.

La versión más actual de la API de plano de datos es _**2020-10-31**_.

Para usar las API de plano de datos:
* Para llamar directamente a las API...
   - haga referencia al Swagger más reciente en la [carpeta Swagger del plano de datos](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/digitaltwins/data-plane/Microsoft.DigitalTwins). Este repositorio también incluye una carpeta de ejemplos que muestran el uso. 
   - consulte la [documentación de referencia de la API](/rest/api/azure-digitaltwins/).
* Puede usar el **SDK de .NET (C#)** . Para usar el SDK de .NET...
   - puede ver y agregar el paquete desde NuGet: [Azure.DigitalTwins.Core](https://www.nuget.org/packages/Azure.DigitalTwins.Core). 
   - puede ver la [documentación de referencia del SDK](/dotnet/api/overview/azure/digitaltwins/client?view=azure-dotnet&preserve-view=true).
   - puede encontrar el código fuente del SDK, incluida una carpeta de ejemplos, en GitHub: [Biblioteca de cliente de Digital Twins de Azure IoT para .NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core). 
   - puede ver información detallada y ejemplos de uso si continúa con la sección [*SDK de .NET (C#) (plano de datos)* ](#net-c-sdk-data-plane) de este artículo.
* Puede usar el **SDK de Java**. Para usar el SDK de Java…
   - puede ver e instalar el paquete desde Maven: [`com.azure:azure-digitaltwins-core`](https://search.maven.org/artifact/com.azure/azure-digitaltwins-core/1.0.0/jar)
   - puede ver la [documentación de referencia del SDK](/java/api/overview/azure/digitaltwins/client?preserve-view=true&view=azure-java-stable)
   - puede encontrar el código fuente del SDK en GitHub: [Biblioteca cliente de Digital Twins de Azure IoT para Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/digitaltwins/azure-digitaltwins-core)
* Puede usar el **SDK de JavaScript**. Para usar el SDK de JavaScript...
   - puede ver e instalar el paquete desde npm: [Biblioteca cliente de Azure Digital Twins Core para JavaScript](https://www.npmjs.com/package/@azure/digital-twins-core).
   - puede ver la [documentación de referencia del SDK](/javascript/api/@azure/digital-twins-core/?branch=master&view=azure-node-latest&preserve-view=true).
   - puede encontrar el código fuente del SDK en GitHub: [Biblioteca cliente de Azure Digital Twins Core para JavaScript](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/digitaltwins/digital-twins-core).
* Puede usar el **SDK para Python**. Para usar el SDK para Python...
   - puede ver e instalar el paquete desde PyPi: [Biblioteca cliente de Azure Digital Twins Core para Python](https://pypi.org/project/azure-digitaltwins-core/).
   - puede ver la [documentación de referencia del SDK](/python/api/azure-digitaltwins-core/azure.digitaltwins.core?view=azure-python&preserve-view=true).
   - puede encontrar el código fuente del SDK en GitHub: [Biblioteca cliente de Azure Digital Twins Core para Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/digitaltwins/azure-digitaltwins-core)
* Puede generar un SDK para otro lenguaje mediante el uso de REST. Siga las instrucciones de [*Procedimiento: SDK personalizados para Azure Digital Twins con AutoRest*](how-to-create-custom-sdks.md).

También puede probar las API de plano de fecha interactuando con Azure Digital Twins a través de la [CLI](how-to-use-cli.md).

## <a name="net-c-sdk-data-plane"></a>SDK de .NET (C#) (plano de datos)

El SDK de .NET (C#) para Azure Digital Twins forma parte del SDK de Azure para .NET. Es de código abierto y se basa en las API del plano de datos de Azure Digital Twins.

> [!NOTE]
> Para más información sobre el diseño del SDK, consulte los [principios de diseño generales para los SDK de Azure](https://azure.github.io/azure-sdk/general_introduction.html) y las [directrices de diseño de .NET](https://azure.github.io/azure-sdk/dotnet_introduction.html) específicas.

Para usar el SDK, incluya el paquete NuGet **Azure.DigitalTwins.Core** con el proyecto. También necesitará la versión más reciente del paquete **Azure.Identity**.

* En Visual Studio, puede agregar paquetes con el administrador de paquetes NuGet (al que se accede a través de *Herramientas > Administrador de paquetes NuGet > Administrar paquetes NuGet para la solución*). 
* Con la herramienta de línea de comandos de .NET, puede ejecutar:

    ```cmd/sh
    dotnet add package Azure.DigitalTwins.Core --version 1.0.0-preview.3
    dotnet add package Azure.identity
    ```

Para obtener un tutorial detallado sobre el uso de las API en la práctica, consulte [*Tutorial: Programación de una aplicación cliente*](tutorial-code.md). 

### <a name="net-sdk-usage-examples"></a>Ejemplos de uso del SDK de .NET

Estos son algunos ejemplos de código que ilustran el uso del SDK de .NET.

Autenticación en el servicio:

```csharp
// Authenticate against the service and create a client
string adtInstanceUrl = "https://<your-Azure-Digital-Twins-instance-hostName>";
var credential = new DefaultAzureCredential();
DigitalTwinsClient client = new DigitalTwinsClient(new Uri(adtInstanceUrl), credential);
```

[!INCLUDE [Azure Digital Twins: local credentials note](../../includes/digital-twins-local-credentials-note.md)] 

Carga de un modelo y enumeración de modelos:

```csharp
// Upload a model
var typeList = new List<string>();
string dtdl = File.ReadAllText("SampleModel.json");
typeList.Add(dtdl);
try {
    await client.CreateModelsAsync(typeList);
} catch (RequestFailedException rex) {
    Console.WriteLine($"Load model: {rex.Status}:{rex.Message}");
}
// Read a list of models back from the service
AsyncPageable<DigitalTwinsModelData> modelDataList = client.GetModelsAsync();
await foreach (DigitalTwinsModelData md in modelDataList)
{
    Console.WriteLine($"Type name: {md.DisplayName}: {md.Id}");
}
```

Creación y consulta de gemelos:

```csharp
// Initialize twin metadata
BasicDigitalTwin updateTwinData = new BasicDigitalTwin();

twinData.Id = $"firstTwin";
twinData.Metadata.ModelId = "dtmi:com:contoso:SampleModel;1";
twinData.Contents.Add("data", "Hello World!");
try {
    await client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>("firstTwin", updateTwinData);
} catch(RequestFailedException rex) {
    Console.WriteLine($"Create twin error: {rex.Status}:{rex.Message}");  
}
 
// Run a query    
AsyncPageable<string> result = client.QueryAsync("Select * From DigitalTwins");
await foreach (string twin in result)
{
    // Use JSON deserialization to pretty-print
    object jsonObj = JsonSerializer.Deserialize<object>(twin);
    string prettyTwin = JsonSerializer.Serialize(jsonObj, new JsonSerializerOptions { WriteIndented = true });
    Console.WriteLine(prettyTwin);
    // Or use BasicDigitalTwin for convenient property access
    BasicDigitalTwin btwin = JsonSerializer.Deserialize<BasicDigitalTwin>(twin);
}
```

Consulte [*Tutorial: Programación de una aplicación cliente*](tutorial-code.md) para obtener un tutorial de este código de aplicación de ejemplo. 

También puede encontrar más ejemplos en el [repositorio de GitHub para el SDK de .NET (C#)](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/digitaltwins/Azure.DigitalTwins.Core/samples).

#### <a name="serialization-helpers"></a>Asistentes de serialización

Las aplicaciones auxiliares de serialización son funciones auxiliares disponibles en el SDK para crear o deserializar rápidamente datos gemelos para el acceso a información básica. Dado que los métodos principales del SDK devuelven los datos gemelos como JSON de forma predeterminada, puede resultar útil usar estas clases auxiliares para reducir aún más los datos gemelos.

Las clases de asistente disponibles son:
* `BasicDigitalTwin`: Representa los datos principales de un gemelo digital.
* `BasicRelationship`: Representa los datos principales de una relación.
* `UpdateOperationUtility`: Representa la información de revisión de JSON usada en las llamadas de actualización.
* `WriteableProperty`: Representa los metadatos de la propiedad.

##### <a name="deserialize-a-digital-twin"></a>Deserialización de un gemelo digital

Siempre puede deserializar los datos gemelos mediante la biblioteca JSON que prefiera, como `System.Test.Json` o `Newtonsoft.Json`. Para el acceso básico a un gemelo, las clases de asistentes lo hacen un poco más conveniente.

```csharp
Response<BasicDigitalTwin> twin = client.GetDigitalTwin(twin_id);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
```

La clase de asistente de `BasicDigitalTwin` también proporciona acceso a las propiedades definidas en el gemelo, a través de un `Dictionary<string, object>`. Para enumerar las propiedades del gemelo, puede usar:

```csharp
Response<BasicDigitalTwin> twin = client.GetDigitalTwin(twin_id);
Console.WriteLine($"Model id: {twin.Metadata.ModelId}");
foreach (string prop in twin.Contents.Keys)
{
    if (twin.Contents.TryGetValue(prop, out object value))
        Console.WriteLine($"Property '{prop}': {value}");
}
```

##### <a name="create-a-digital-twin"></a>Creación de un gemelo digital

Con la clase `BasicDigitalTwin`, puede preparar los datos para crear una instancia gemela:

```csharp
BasicDigitalTwin twin = new BasicDigitalTwin();
twin.Metadata = new DigitalTwinMetadata();
twin.Metadata.ModelId = "dtmi:example:Room;1";
// Initialize properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("Temperature", 25.0);
twin.Contents = props;

client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>("myNewRoomID", twin);
```

El código anterior es equivalente a la siguiente variante "manual":

```csharp
Dictionary<string, object> meta = new Dictionary<string, object>()
{
    { "$model", "dtmi:example:Room;1"}
};
Dictionary<string, object> twin = new Dictionary<string, object>()
{
    { "$metadata", meta },
    { "Temperature", 25.0 }
};
client.CreateOrReplaceDigitalTwinAsync<BasicDigitalTwin>("myNewRoomID", twin);
```

##### <a name="deserialize-a-relationship"></a>Deserialización de una relación

Siempre puede deserializar los datos de la relación en un tipo de su elección. Para obtener acceso básico a una relación, utilice el tipo `BasicRelationship`.

```csharp
BasicRelationship res = client.GetRelationship<BasicRelationship>(twin_id, rel_id);
Console.WriteLine($"Relationship Name: {rel.Name}");
```

La clase de asistente `BasicRelationship` también proporciona acceso a las propiedades definidas en la relación, mediante un elemento `IDictionary<string, object>`. Para enumerar las propiedades, puede usar:

```csharp
BasicRelationship res = client.GetRelationship<BasicRelationship>(twin_id, rel_id);
Console.WriteLine($"Relationship Name: {rel.Name}");
foreach (string prop in rel.Contents.Keys)
{
    if (twin.Contents.TryGetValue(prop, out object value))
        Console.WriteLine($"Property '{prop}': {value}");
}
```

##### <a name="create-a-relationship"></a>Creación de una relación

Con la clase `BasicRelationship`, también puede preparar los datos para crear relaciones en una instancia gemela:

```csharp
BasicRelationship rel = new BasicRelationship();
rel.TargetId = "myTargetTwin";
rel.Name = "contains"; // a relationship with this name must be defined in the model
// Initialize properties
Dictionary<string, object> props = new Dictionary<string, object>();
props.Add("active", true);
rel.Properties = props;
client.CreateOrReplaceRelationshipAsync("mySourceTwin", "rel001", rel);
```

##### <a name="create-a-patch-for-twin-update"></a>Creación de una revisión para la actualización gemela

Las llamadas de actualización de los gemelos y las relaciones usan la estructura de [revisión de JSON](http://jsonpatch.com/). Para crear listas de operaciones de revisión de JSON, puede usar `JsonPatchDocument`, tal como se muestra a continuación.

```csharp
var updateTwinData = new JsonPatchDocument();
updateTwinData.AppendAddOp("/Temperature", 25.0);
updateTwinData.AppendAddOp("/myComponent/Property", "Hello");
// Un-set a property
updateTwinData.AppendRemoveOp("/Humidity");

client.UpdateDigitalTwin("myTwin", updateTwinData);
```

## <a name="general-apisdk-usage-notes"></a>Notas generales sobre el uso de la API o del SDK

> [!NOTE]
> Tenga en cuenta que Azure Digital Twins no admite actualmente el **uso compartido de recursos entre orígenes (CORS)** . Para más información sobre las estrategias de impacto y resolución, consulte la sección [*Uso compartido de recursos entre orígenes (CORS)*](concepts-security.md#cross-origin-resource-sharing-cors) de *Conceptos: Seguridad para las soluciones de Azure Digital Twins*.

En la lista siguiente se proporcionan detalles adicionales y directrices generales para usar las API y los SDK.

* Para usar el SDK, cree una instancia de la clase `DigitalTwinsClient`. El constructor requiere credenciales que se pueden obtener con una variedad de métodos de autenticación en el paquete de `Azure.Identity`. Para más información sobre `Azure.Identity`, consulte la [documentación del espacio de nombres](/dotnet/api/azure.identity?preserve-view=true&view=azure-dotnet). 
* Es posible que encuentre útil `InteractiveBrowserCredential` al comenzar, pero hay muchas otras opciones, incluidas las credenciales para la [identidad administrada](/dotnet/api/azure.identity.interactivebrowsercredential?preserve-view=true&view=azure-dotnet), que probablemente va a usar para autenticar las [funciones de Azure configuradas con MSI](../app-service/overview-managed-identity.md?tabs=dotnet) en Azure Digital Twins. Para más información sobre `InteractiveBrowserCredential`, consulte la [documentación de la clase](/dotnet/api/azure.identity.interactivebrowsercredential?preserve-view=true&view=azure-dotnet).
* Todas las llamadas de API de servicio se exponen como funciones miembro en la clase `DigitalTwinsClient`.
* Todas las funciones de servicio se encuentran en versiones sincrónicas y asincrónicas.
* Todas las funciones de servicio inician una excepción en cualquier estado de devolución de 400 o superior. Asegúrese de ajustar las llamadas en una sección `try` y de capturar al menos `RequestFailedExceptions`. Para más información sobre este tipo de excepción, consulte [este artículo](/dotnet/api/azure.requestfailedexception?preserve-view=true&view=azure-dotnet).
* La mayoría de los métodos de servicio devuelven `Response<T>` o (`Task<Response<T>>` para las llamadas asincrónicas), donde `T` es la clase de objeto de devolución para la llamada de servicio. La clase [`Response`](/dotnet/api/azure.response-1?preserve-view=true&view=azure-dotnet) encapsula la devolución del servicio y presenta los valores devueltos en su campo `Value`.  
* Los métodos de servicio con los resultados paginados devuelven `Pageable<T>` o `AsyncPageable<T>` como resultados. Para más información sobre la clase `Pageable<T>`, consulte [este artículo](/dotnet/api/azure.pageable-1?preserve-view=true&view=azure-dotnet-preview); para más información sobre `AsyncPageable<T>`, consulte [este artículo](/dotnet/api/azure.asyncpageable-1?preserve-view=true&view=azure-dotnet-preview).
* Puede recorrer en iteración los resultados paginados mediante un bucle `await foreach`. Para más información sobre este proceso, consulte [este artículo](/archive/msdn-magazine/2019/november/csharp-iterating-with-async-enumerables-in-csharp-8).
* El SDK subyacente es `Azure.Core`. Consulte la [documentación del espacio de nombres de Azure](/dotnet/api/azure?preserve-view=true&view=azure-dotnet-preview) como referencia sobre los tipos y la infraestructura del SDK.

Los métodos de servicio devuelven objetos fuertemente tipados siempre que sea posible. Sin embargo, dado que Azure Digital Twins se basa en los modelos configurados de forma personalizada por el usuario en tiempo de ejecución (a través de los modelos DTDL cargados en el servicio), muchas API de servicio toman y devuelven datos gemelos en formato JSON.

## <a name="monitor-api-metrics"></a>Supervisión de las métricas de API

Las métricas de API, como las solicitudes, la latencia y la tasa de errores, se pueden ver en [Azure Portal](https://portal.azure.com/). 

En la página principal del portal, busque la instancia de Azure Digital Twins para ver sus detalles. Seleccione la opción **Métricas** en el menú de la instancia de Azure Digital Twins para abrir la página *Métricas*.

:::image type="content" source="media/troubleshoot-metrics/azure-digital-twins-metrics.png" alt-text="Captura de pantalla que muestra la página de métricas de Azure Digital Twins":::

Desde aquí, puede ver las métricas de la instancia y crear vistas personalizadas.

## <a name="next-steps"></a>Pasos siguientes

Vea cómo usar las API para configurar una instancia de Azure Digital Twins y realizar la autenticación:
* [*Procedimiento: Configuración de una instancia y autenticación*](how-to-set-up-instance-cli.md)

O bien, siga los pasos para crear una aplicación cliente como la que se usa en este tutorial:
* [*Tutorial: Programación de una aplicación cliente*](tutorial-code.md)