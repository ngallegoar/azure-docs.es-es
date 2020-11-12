---
title: Uso de Azure.Search.Documents (versión 11) en .NET
titleSuffix: Azure Cognitive Search
description: Aprenda a crear y administrar objetos de búsqueda en una aplicación de .NET con C# y la biblioteca cliente de Azure.Search.Documents (versión 11). Los fragmentos de código muestran la conexión al servicio, la creación de índices y las consultas.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.devlang: dotnet
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 10/27/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 4b3256591c0aa2536fd42bcdbb2ef339fc1d5c48
ms.sourcegitcommit: 6a902230296a78da21fbc68c365698709c579093
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2020
ms.locfileid: "93356814"
---
# <a name="how-to-use-azuresearchdocuments-in-a-c-net-application"></a>Uso de Azure.Search.Documents en una aplicación de .NET con C#

En este artículo se explica cómo crear y administrar objetos de búsqueda con C# y la biblioteca cliente de [**Azure.Search.Documents**](/dotnet/api/overview/azure/search) (versión 11).

## <a name="about-version-11"></a>Acerca de la versión 11

El SDK de Azure para .NET agrega una nueva biblioteca cliente de [**Azure.Search.Documents**](/dotnet/api/overview/azure/search) del equipo del SDK de Azure que es funcionalmente equivalente a las bibliotecas cliente de [Microsoft.Azure.Search](/dotnet/api/overview/azure/search/client10), pero que emplea enfoques y convenciones comunes, si procede. Algunos ejemplos incluyen la autenticación de la clave [`AzureKeyCredential`](/dotnet/api/azure.azurekeycredential) y [System.Text.Json.Serialization](/dotnet/api/system.text.json.serialization) para la serialización de JSON.

Al igual que con las versiones anteriores, puede usar esta biblioteca para:

+ Crear y administrar índices de búsqueda, orígenes de datos, indizadores, conjuntos de aptitudes y asignaciones de sinónimos.
+ Cargar y administrar documentos de búsqueda en un índice.
+ Ejecutar consultas, todo sin tener que tratar con los detalles de HTTP y JSON.

La biblioteca se distribuye como un solo [paquete NuGet de Azure.Search.Document](https://www.nuget.org/packages/Azure.Search.Documents/), que incluye todas las API que se usan para el acceso mediante programación a un servicio de búsqueda.

La biblioteca de cliente define clases como `SearchIndex`, `SearchField` y `SearchDocument`, además de operaciones como `SearchIndexClient.CreateIndex` y `SearchClient.Search` en las clases `SearchIndexClient` y `SearchClient`. Estas clases están organizadas en los espacios de nombres siguientes:

+ [`Azure.Search.Documents`](/dotnet/api/azure.search.documents)
+ [`Azure.Search.Documents.Indexes`](/dotnet/api/azure.search.documents.indexes)
+ [`Azure.Search.Documents.Indexes.Models`](/dotnet/api/azure.search.documents.indexes.models)
+ [`Azure.Search.Documents.Models`](/dotnet/api/azure.search.documents.models)

Azure.Search.Documents (versión 11) tiene como destino la versión [`2020-06-30` de la API REST de Azure Cognitive Search](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/search/data-plane/Azure.Search/preview/2020-06-30). 

La biblioteca cliente no proporciona [operaciones de administración de servicios](/rest/api/searchmanagement/), como la creación y el escalado de servicios de búsqueda y la administración de claves de API. Si necesita administrar los recursos de búsqueda desde una aplicación .NET, use la biblioteca [Microsoft.Azure.Management.Search](/dotnet/api/overview/azure/search/management) en el SDK de Azure para .NET.

## <a name="upgrade-to-v11"></a>Actualización a la versión 11

Si ha usado la versión anterior del SDK para .NET y le gustaría actualizar a la versión actualmente disponible con carácter general, consulte [Actualización a la versión 11 del SDK de .NET para Azure Cognitive Search](search-dotnet-sdk-migration-version-11.md)

## <a name="sdk-requirements"></a>Requisitos del SDK

+ Visual Studio 2019 o posterior.

+ Su propio servicio Búsqueda cognitiva de Azure. Para usar el SDK, será necesario el nombre del servicio y una o varias claves de API. Si no tiene uno, [cree un servicio en el portal](search-create-service-portal.md).

+ Descargue el [paquete Azure.Search.Documents](https://www.nuget.org/packages/Azure.Search.Documents) mediante **Herramientas** > **Administrador de paquetes NuGet** > **Administrar paquetes NuGet para la solución** en Visual Studio. Busque el nombre de paquete `Azure.Search.Documents`.

El SDK de Azure para .NET se ajusta a [.NET Standard 2.0](/dotnet/standard/net-standard#net-implementation-support); es decir, .NET Framework 4.6.1 y .NET Core 2.0 como requisitos mínimos.

## <a name="example-application"></a>Aplicación de ejemplo

En este artículo "se enseña con el ejemplo", basándose en la muestra de código [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) de GitHub para ilustrar los conceptos básicos de Azure Cognitive Search; específicamente, cómo crear, cargar y consultar un índice de búsqueda.

En el resto de este artículo, supongamos que se trata de un nuevo índice denominado "hotels", que se rellenó con unos pocos documentos y varias consultas que coinciden con los resultados.

El siguiente es el programa principal, que muestra el flujo general:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchIndexClient indexClient = CreateSearchIndexClient(configuration);

    string indexName = configuration["SearchIndexName"];

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteIndexIfExists(indexName, indexClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateIndex(indexName, indexClient);

    SearchClient searchClient = indexClient.GetSearchClient(indexName);

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(searchClient);

    SearchClient indexClientForQueries = CreateSearchClientForQueries(indexName, configuration);

    Console.WriteLine("{0}", "Run queries...\n");
    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

A continuación se muestra una captura de pantalla parcial del resultado, suponiendo que ejecuta esta aplicación con un nombre de servicio válido y claves de API:

:::image type="content" source="media/search-howto-dotnet-sdk/console-output.png" alt-text="Resultados de Console.WriteLine del programa de ejemplo":::

### <a name="client-types"></a>Tipos de clientes

La biblioteca cliente utiliza tres tipos de clientes para varias operaciones: [`SearchIndexClient`](/dotnet/api/azure.search.documents.indexes.searchindexclient) para crear, actualizar o eliminar índices, [`SearchClient`](/dotnet/api/azure.search.documents.searchclient) para cargar o consultar un índice, y [`SearchIndexerClient`](/dotnet/api/azure.search.documents.indexes.searchindexerclient) para trabajar con indizadores y conjuntos de aptitudes. Este artículo se centra en los dos primeros. 

Como mínimo, todos los clientes requieren el nombre del servicio o el punto de conexión y una clave de API. Normalmente, esta información se proporciona en un archivo de configuración, de manera similar a lo que se encuentra en el archivo `appsettings.json` de la [aplicación de ejemplo DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo). Para leer el archivo de configuración, agregue `using Microsoft.Extensions.Configuration;` al programa.

La siguiente instrucción crea el cliente de índice que se usa para crear, actualizar o eliminar los índices. Requiere un punto de conexión de búsqueda y una clave de API de administración.

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceEndPoint = configuration["SearchServiceEndPoint"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(new Uri(searchServiceEndPoint), new AzureKeyCredential(adminApiKey));
    return indexClient;
}
```

La siguiente instrucción crea el cliente de búsqueda que se usa para cargar documentos o ejecutar consultas. `SearchClient` necesita un índice. Necesitará una clave de API de administración para cargar documentos, aunque puede usar una clave de API de consulta para ejecutar las consultas. 

```csharp
string indexName = configuration["SearchIndexName"];

private static SearchClient CreateSearchClientForQueries(string indexName, IConfigurationRoot configuration)
{
    string searchServiceEndPoint = configuration["SearchServiceEndPoint"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchClient searchClient = new SearchClient(new Uri(searchServiceEndPoint), indexName, new AzureKeyCredential(queryApiKey));
    return searchClient;
}
```

> [!NOTE]
> Si especifica una clave no válida para la operación de importación (por ejemplo, una clave de consulta cuando se requiere una clave de administración), el `SearchClient` producirá una `CloudException` con el mensaje de error "Forbidden" (Prohibido) cuando llame a un método de operación con él. Si esto sucede, vuelva a comprobar la clave de API.
>

### <a name="deleting-the-index"></a>Eliminación del índice

En las primeras fases de desarrollo, se recomienda incluir una instrucción [`DeleteIndex`](/dotnet/api/azure.search.documents.indexes.searchindexclient.deleteindex) para eliminar un índice de trabajo en curso; de este modo, podrá volver a crearlo con una definición actualizada. A menudo, el código de ejemplo de Azure Cognitive Search incluye un paso de eliminación para que pueda ejecutar el ejemplo nuevamente.

La línea siguiente realiza una llamada a `DeleteIndexIfExists`:

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteIndexIfExists(indexName, indexClient);
```

Este método usa el `SearchIndexClient` proporcionado para comprobar si existe el índice y, en ese caso, eliminarlo:

```csharp
private static void DeleteIndexIfExists(string indexName, SearchIndexClient indexClient)
{
    try
    {
        if (indexClient.GetIndex(indexName) != null)
        {
            indexClient.DeleteIndex(indexName);
        }
    }
    catch (RequestFailedException e) when (e.Status == 404)
    {
        // Throw an exception if the index name isn't found
        Console.WriteLine("The index doesn't exist. No deletion occurred.");
```

> [!NOTE]
> En el código de ejemplo de este artículo se usan los métodos sincrónicos por simplicidad, pero debe usar los métodos asincrónicos en sus propias aplicaciones para mantener la escalabilidad y la capacidad de respuesta. Por ejemplo, en el método anterior podría usar [`DeleteIndexAsync`](/dotnet/api/azure.search.documents.indexes.searchindexclient.deleteindexasync) en lugar de [`DeleteIndex`](/dotnet/api/azure.search.documents.indexes.searchindexclient.deleteindex).
>

## <a name="create-an-index"></a>Creación de un índice

Puede usar [`SearchIndexClient`](/dotnet/api/azure.search.documents.indexes.searchindexclient) para crear un índice. 

El siguiente método crea un nuevo objeto [`SearchIndex`](/dotnet/api/azure.search.documents.indexes.models.searchindex) con una lista de objetos [`SearchField`](/dotnet/api/azure.search.documents.indexes.models.searchfield) que define el esquema del nuevo índice. Cada campo tiene un nombre, un tipo de datos y varios atributos que definen su comportamiento de búsqueda. 

Los campos se pueden definir a partir de una clase de modelo mediante [`FieldBuilder`](/dotnet/api/azure.search.documents.indexes.fieldbuilder). La clase `FieldBuilder` usa reflexión para crear una lista de objetos `SearchField` para el índice examinando las propiedades públicas y los atributos de la clase de modelo `Hotel` dada. Examinaremos más de cerca la clase `Hotel` más adelante.

```csharp
private static void CreateIndex(string indexName, SearchIndexClient indexClient)
{
    FieldBuilder fieldBuilder = new FieldBuilder();
    var searchFields = fieldBuilder.Build(typeof(Hotel));

    var definition = new SearchIndex(indexName, searchFields);

    indexClient.CreateOrUpdateIndex(definition);
}
```

Además de campos, también podía agregar al índice perfiles de puntuación, proveedores de sugerencias u opciones de CORS (estos parámetros se omiten en el ejemplo para mayor brevedad). Puede encontrar más información sobre el objeto SearchIndex y sus partes constituyentes en la lista de propiedades de [`SearchIndex`](/dotnet/api/azure.search.documents.indexes.models.searchindex), así como en la [referencia de la API REST](/rest/api/searchservice/).

> [!NOTE]
> En caso necesario, siempre puede crear la lista de objetos `Field` directamente en lugar de usar `FieldBuilder`. Por ejemplo, puede que no quiera usar una clase model o que necesite usar una clase model existente que no quiere modificar agregando atributos.
>

### <a name="call-createindex-in-main"></a>Llamada a CreateIndex en Main()

`Main` crea un nuevo índice "hotels" mediante una llamada al método anterior:

```csharp
Console.WriteLine("{0}", "Creating index...\n");
CreateIndex(indexName, indexClient);
```

## <a name="use-a-model-class-for-data-representation"></a>Uso de una clase de modelo para la representación de datos

En el ejemplo DotNetHowTo se usan clases de modelo para las estructuras de datos [Hotel](https://github.com/Azure-Samples/search-dotnet-getting-started/blob/master/DotNetHowTo/DotNetHowTo/Hotel.cs), [Address](https://github.com/Azure-Samples/search-dotnet-getting-started/blob/master/DotNetHowTo/DotNetHowTo/Address.cs) y [Room](https://github.com/Azure-Samples/search-dotnet-getting-started/blob/master/DotNetHowTo/DotNetHowTo/Room.cs). `Hotel` hace referencia a `Address`, un tipo complejo de un solo nivel (un campo de varias partes) y `Room` (una colección de campos de varias partes).

Puede usar estos tipos para crear y cargar el índice, así como para estructurar la respuesta de una consulta:

```csharp
// Use-case: <Hotel> in a field definition
FieldBuilder fieldBuilder = new FieldBuilder();
var searchFields = fieldBuilder.Build(typeof(Hotel));

// Use-case: <Hotel> in a response
private static void WriteDocuments(SearchResults<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.GetResults())
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

### <a name="field-definitions"></a>Definiciones de campo

El modelo de datos de .NET y su esquema de índice correspondiente deben dar cabida a la experiencia de búsqueda que quiera proporcionar al usuario final. Cada objeto de nivel superior en .NET, como un documento de búsqueda en un índice de búsqueda, corresponde a un resultado de búsqueda que aparecería en la interfaz de usuario. Por ejemplo, en una aplicación de búsqueda de alojamiento, puede que los usuarios finales quieran realizar búsquedas por nombre de hotel, por las características del hotel o por las características de una habitación determinada. 

Dentro de cada clase, un campo se define con un tipo de datos y atributos que determinan cómo se usa. El nombre de cada propiedad pública de cada clase se asigna a un campo con el mismo nombre en la definición del índice. 

Eche un vistazo al siguiente fragmento de código que extrae varias definiciones de campo de la clase Hotel. Observe que las estructuras de datos Address y Rooms son tipos de C# con sus propias definiciones de clase (consulte el código de ejemplo si quiere verlas). Ambas son tipos complejos. Para más información, consulte el artículo sobre la [Cómo modelar tipos complejos](search-howto-complex-data-types.md).

```csharp
public partial class Hotel
{
    [SimpleField(IsKey = true, IsFilterable = true)]
    public string HotelId { get; set; }

    [SearchableField(IsSortable = true)]
    public string HotelName { get; set; }

    [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnLucene)]
    public string Description { get; set; }

    [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
    public string Category { get; set; }

    [JsonIgnore]
    public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;

    [SearchableField]
    public Address Address { get; set; }

    public Room[] Rooms { get; set; }
```

#### <a name="choosing-a-field-class"></a>Elección de una clase de campo

Al definir los campos, puede usar la clase base [`SearchField`](/dotnet/api/azure.search.documents.indexes.models.searchfield), o puede usar modelos auxiliares derivados que actúan como "plantillas", con propiedades preconfiguradas.

Exactamente un campo del índice debe servir como clave de documento (`IsKey = true`). Debe ser una cadena y debe identificar de forma exclusiva a cada documento. También es necesario tener `IsHidden = true`, que significa que no puede estar visible en los resultados de la búsqueda.

| Tipo de campo | Descripción y uso |
|------------|-----------------------|
| [`SearchField`](/dotnet/api/azure.search.documents.indexes.models.searchfield) | Clase base, con la mayoría de las propiedades establecidas en NULL, excepto `Name`, porque es necesaria, y `AnalyzerName`, que tiene como valor predeterminado Lucene estándar. |
| [`SimpleField`](/dotnet/api/azure.search.documents.indexes.models.simplefield) | Modelo auxiliar. Puede ser cualquier tipo de datos, no permite búsquedas (se omite en las consultas de búsqueda de texto completo) y se puede recuperar (no está oculto). Otros atributos están desactivados de forma predeterminada, pero se pueden habilitar. Puede usar un `SimpleField` para los identificadores de documento o los campos que se usan solo en filtros, facetas o perfiles de puntuación. En caso de hacerlo, asegúrese de aplicar los atributos necesarios para el escenario, como `IsKey = true` para un identificador de documento. Para más información, consulte [SimpleFieldAttribute.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/src/Indexes/SimpleFieldAttribute.cs) en el código fuente. |
| [`SearchableField`](/dotnet/api/azure.search.documents.indexes.models.searchablefield) | Modelo auxiliar. Debe ser una cadena y siempre permite búsquedas y se puede recuperar. Otros atributos están desactivados de forma predeterminada, pero se pueden habilitar. Ya que este tipo de campo se puede buscar, admite sinónimos y el complemento completo de las propiedades del analizador. Para más información, consulte [SearchableFieldAttribute.cs](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/src/Indexes/SearchableFieldAttribute.cs) en el código fuente. |

Tanto si usa la API básica de `SearchField`, como uno de los modelos auxiliares, debe habilitar explícitamente los atributos de filtro, faceta y ordenación. Por ejemplo, [IsFilterable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfilterable), [IsSortable](/dotnet/api/azure.search.documents.indexes.models.searchfield.issortable)y [IsFacetable](/dotnet/api/azure.search.documents.indexes.models.searchfield.isfacetable) se tienen que atribuir explícitamente, como se muestra en el ejemplo anterior.

#### <a name="adding-field-attributes"></a>Incorporación de atributos de campo

Observe cómo cada campo está decorado con atributos como `IsFilterable`, `IsSortable`, `IsKey` y `AnalyzerName`. Estos atributos se asignan directamente a los [atributos de campo correspondientes de un índice de Búsqueda cognitiva de Azure](/rest/api/searchservice/create-index). La clase `FieldBuilder` usa estas propiedades para construir definiciones de campo para el índice.

#### <a name="field-type-mapping"></a>Asignación de tipos de campo

Los tipos .NET de las propiedades se asignan a los tipos de campo equivalentes de la definición del índice. Por ejemplo, la propiedad de cadena `Category` se asigna al campo `category`, que es de tipo `Edm.String`. Se dan asignaciones de tipos semejantes entre `bool?`, `Edm.Boolean`, `DateTimeOffset?` y `Edm.DateTimeOffset`, etc. 

¿Se ha dado cuenta de la propiedad `SmokingAllowed`?

```csharp
[JsonIgnore]
public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;
```

El atributo `JsonIgnore` de esta propiedad indica a `FieldBuilder` que no la serialice en el índice como un campo.  Se trata de una excelente manera de crear propiedades calculadas del lado cliente que puede usar como aplicaciones auxiliares en su aplicación.  En este caso, la propiedad `SmokingAllowed` refleja si está permitido fumar en algún objeto `Room` de la colección de objetos `Rooms`. Si todos están establecidos en false, quiere decir que no se puede fumar en todo el hotel.

## <a name="load-an-index"></a>Carga de un índice

El siguiente paso en `Main` rellena el índice "hotels" recién creado. Esto se lleva a cabo mediante el método siguiente (parte del código se ha reemplazado por "..." con fines meramente ilustrativos, pero puede consultar la solución de ejemplo completa para ver el código de llenado de datos completo):

```csharp
private static void UploadDocuments(SearchClient searchClient)
{
    IndexDocumentsBatch<Hotel> batch = IndexDocumentsBatch.Create(
        IndexDocumentsAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                HotelName = "Secret Point Motel",
                ...
                Address = new Address()
                {
                    StreetAddress = "677 5th Ave",
                    ...
                },
                Rooms = new Room[]
                {
                    new Room()
                    {
                        Description = "Budget Room, 1 Queen Bed (Cityside)",
                        ...
                    },
                    new Room()
                    {
                        Description = "Budget Room, 1 King Bed (Mountain View)",
                        ...
                    },
                    new Room()
                    {
                        Description = "Deluxe Room, 2 Double Beds (City View)",
                        ...
                    }
                }
            }),
        IndexDocumentsAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                HotelName = "Twin Dome Motel",
                ...
                {
                    StreetAddress = "140 University Town Center Dr",
                    ...
                },
                Rooms = new Room[]
                {
                    new Room()
                    {
                        Description = "Suite, 2 Double Beds (Mountain View)",
                        ...
                    },
                    new Room()
                    {
                        Description = "Standard Room, 1 Queen Bed (City View)",
                        ...
                    },
                    new Room()
                    {
                        Description = "Budget Room, 1 King Bed (Waterfront View)",
                        ...
                    }
                }
            }),
        IndexDocumentsAction.Upload(
            new Hotel()
            {
                HotelId = "3",
                HotelName = "Triple Landscape Hotel",
                ...
                Address = new Address()
                {
                    StreetAddress = "3393 Peachtree Rd",
                    ...
                },
                Rooms = new Room[]
                {
                    new Room()
                    {
                        Description = "Standard Room, 2 Queen Beds (Amenities)",
                        ...
                    },
                    new Room ()
                    {
                        Description = "Standard Room, 2 Double Beds (Waterfront View)",
                        ...
                    },
                    new Room()
                    {
                        Description = "Deluxe Room, 2 Double Beds (Cityside)",
                        ...
                    }
                }
            }
        };

    try
    {
        IndexDocumentsResult result = searchClient.IndexDocuments(batch);
    }
    catch (Exception)
    {
        // Sometimes when your Search service is under load, indexing will fail for some of the documents in
        // the batch. Depending on your application, you can take compensating actions like delaying and
        // retrying. For this simple demo, we just log the failed document keys and continue.
        Console.WriteLine("Failed to index some of the documents: {0}");
    }

    Console.WriteLine("Waiting for documents to be indexed...\n");
    Thread.Sleep(2000);
```

Este método tiene cuatro partes. La primera crea una matriz de tres objetos `Hotel` (cada uno con tres objetos `Room`), que serán los datos de entrada que cargaremos en el índice. Estos datos están incluidos en el código por motivos de simplicidad. En una aplicación real, los datos provendrán normalmente de un origen de datos externo, como una base de datos SQL.

En la segunda parte se crea un [`IndexDocumentsBatch`](/dotnet/api/azure.search.documents.models.indexdocumentsbatch) que contiene los documentos. Especifique la operación que desee aplicar al lote en el momento de su creación, en este caso llamando a [`IndexDocumentsAction.Upload`](/dotnet/api/azure.search.documents.models.indexdocumentsaction.upload). A continuación, el lote se carga en el índice de Búsqueda cognitiva de Azure mediante el método [`IndexDocuments`](/dotnet/api/azure.search.documents.searchclient.indexdocuments).

> [!NOTE]
> En este ejemplo nos limitamos a cargar documentos. Si desea combinar los cambios en los documentos existentes o eliminar documentos, puede crear lotes llamando a `IndexDocumentsAction.Merge`, `IndexDocumentsAction.MergeOrUpload` o `IndexDocumentsAction.Delete`. También puede combinar diferentes operaciones en un único lote llamando a `IndexBatch.New`, que selecciona una colección de objetos de `IndexDocumentsAction`, que indican a Búsqueda cognitiva de Azure que realice una operación determinada en un documento. Puede crear cada `IndexDocumentsAction` con su propia operación llamando al método correspondiente, como `IndexDocumentsAction.Merge`, `IndexAction.Upload`, etc.
> 

La tercera parte de este método es un bloque catch que controla un caso de error importante para la indización. Si su servicio de búsqueda no logra indizar algunos de los documentos del lote, aparece una `IndexBatchException` producida por `IndexDocuments`. Esta excepción puede suceder si se indexan documentos mientras el servicio está sobrecargado. **Recomendamos encarecidamente controlar este caso de forma explícita en el código.** Puede retrasar la indización de los documentos que dieron error y volver a intentarlo; puede crear un registro y continuar, como hace el ejemplo, o puede adoptar otro enfoque según los requisitos de coherencia de datos de la aplicación.

Por último, el método `UploadDocuments` se retrasa durante dos segundos. La indización ocurre de manera asincrónica en el servicio de búsqueda, por lo que la aplicación de ejemplo debe esperar unos momentos para asegurarse de que los documentos estén disponibles para la búsqueda. Retrasos así solo suelen ser necesarios en las pruebas, demostraciones y aplicaciones de ejemplo.

### <a name="call-uploaddocuments-in-main"></a>Llamada a UploadDocuments en Main()

En el fragmento de código siguiente se configura una instancia de [`SearchClient`](/dotnet/api/azure.search.documents.searchclient) mediante el método [`GetSearchClient`](/dotnet/api/azure.search.documents.indexes.searchindexclient.getsearchclient) de indexClient. IndexClient usa una clave de API de administración en las solicitudes, que es necesaria para cargar o actualizar documentos.

Un enfoque alternativo consiste en llamar a `SearchClient` directamente; para ello, se pasa una clave de API de administración en `AzureKeyCredential`.

```csharp
SearchClient searchClient = indexClient.GetSearchClient(indexName);

Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(searchClient);
```

## <a name="run-queries"></a>Ejecutar consultas

En primer lugar, configure un `SearchClient` que lea el punto de conexión de búsqueda y la clave de API de consulta de **appsettings.json** :

```csharp
private static SearchClient CreateSearchClientForQueries(string indexName, IConfigurationRoot configuration)
{
    string searchServiceEndPoint = configuration["SearchServiceEndPoint"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchClient searchClient = new SearchClient(new Uri(searchServiceEndPoint), indexName, new AzureKeyCredential(queryApiKey));
    return searchClient;
}
```

En segundo lugar, defina un método que envíe una solicitud de consulta. 

Cada vez que el método ejecuta una consulta, crea primero un nuevo objeto [`SearchOptions`](/dotnet/api/azure.search.documents.searchoptions). Este objeto se usa para especificar más opciones en la consulta, como el orden, los filtros, la paginación y las facetas. En este método, vamos a establecer la propiedad `Filter`, `Select` y `OrderBy` para diferentes consultas. Para obtener más información acerca de la sintaxis de expresiones de consulta de búsqueda, consulte [Sintaxis de consulta simple](/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).

El siguiente paso consiste en ejecutar la consulta de búsqueda. La búsqueda se ejecuta mediante el método `SearchClient.Search`. Para cada consulta, pase el texto de búsqueda para usarlo como cadena (o `"*"` si no hay ningún texto de búsqueda), además de las opciones de búsqueda creadas anteriormente. También especificamos `Hotel` como el parámetro de tipo para `SearchClient.Search`, lo que indica al SDK que deserialice los documentos de los resultados de búsqueda en objetos de tipo `Hotel`.

```csharp
private static void RunQueries(SearchClient searchClient)
{
    SearchOptions options;
    SearchResults<Hotel> results;

    Console.WriteLine("Query 1: Search for 'motel'. Return only the HotelName in results:\n");

    options = new SearchOptions();
    options.Select.Add("HotelName");

    results = searchClient.Search<Hotel>("motel", options);

    WriteDocuments(results);

    Console.Write("Query 2: Apply a filter to find hotels with rooms cheaper than $100 per night, ");
    Console.WriteLine("returning the HotelId and Description:\n");

    options = new SearchOptions()
    {
        Filter = "Rooms/any(r: r/BaseRate lt 100)"
    };
    options.Select.Add("HotelId");
    options.Select.Add("Description");

    results = searchClient.Search<Hotel>("*", options);

    WriteDocuments(results);

    Console.Write("Query 3: Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    options =
        new SearchOptions()
        {
            Size = 2
        };
    options.OrderBy.Add("LastRenovationDate desc");
    options.Select.Add("HotelName");
    options.Select.Add("LastRenovationDate");

    results = searchClient.Search<Hotel>("*", options);

    WriteDocuments(results);

    Console.WriteLine("Query 4: Search the HotelName field for the term 'hotel':\n");

    options = new SearchOptions();
    options.SearchFields.Add("HotelName");

    //Adding details to select, because "Location" is not supported yet when deserialize search result to "Hotel"
    options.Select.Add("HotelId");
    options.Select.Add("HotelName");
    options.Select.Add("Description");
    options.Select.Add("Category");
    options.Select.Add("Tags");
    options.Select.Add("ParkingIncluded");
    options.Select.Add("LastRenovationDate");
    options.Select.Add("Rating");
    options.Select.Add("Address");
    options.Select.Add("Rooms");

    results = searchClient.Search<Hotel>("hotel", options);

    WriteDocuments(results);
}
```

En tercer lugar, defina un método que escriba la respuesta e imprima cada documento en la consola:

```csharp
private static void WriteDocuments(SearchResults<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.GetResults())
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

### <a name="call-runqueries-in-main"></a>Llamada a RunQueries en Main()

```csharp
SearchClient indexClientForQueries = CreateSearchClientForQueries(indexName, configuration);

Console.WriteLine("{0}", "Running queries...\n");
RunQueries(indexClientForQueries);
```

### <a name="explore-query-constructs"></a>Exploración de las construcciones de consulta

Examinemos más de cerca cada una de las consultas de una en una. Este es el código para ejecutar la primera consulta:

```csharp
options = new SearchOptions();
options.Select.Add("HotelName");

results = searchClient.Search<Hotel>("motel", options);

WriteDocuments(results);
```

En este caso, buscamos en todo el índice la palabra "motel" en cualquier campo incluido en la búsqueda, y solo queremos recuperar los nombres de hotel, según lo especifica la opción `Select`. He aquí los resultados:

```output
Name: Secret Point Motel

Name: Twin Dome Motel
```

En la segunda consulta, use un filtro para seleccionar una habitación con una tarifa nocturna inferior a 100 USD. Devuelva solo el id. y la descripción del hotel en los resultados:

```csharp
options = new SearchOptions()
{
    Filter = "Rooms/any(r: r/BaseRate lt 100)"
};
options.Select.Add("HotelId");
options.Select.Add("Description");

results = searchClient.Search<Hotel>("*", options);
```

La consulta anterior usa una expresión `$filter` de OData, `Rooms/any(r: r/BaseRate lt 100)`, para filtrar los documentos del índice. Aquí se usa [cualquier operador](./search-query-odata-collection-operators.md) para aplicar "BaseRate lt 100" a todos los elementos de la colección de objetos Rooms. Para obtener más información, consulte [Sintaxis de filtro de OData](./query-odata-filter-orderby-syntax.md).

En la tercera consulta, encuentre los dos mejores hoteles que se han renovado más recientemente y muestre el nombre del hotel y la última fecha de renovación. Este es el código: 

```csharp
options =
    new SearchOptions()
    {
        Size = 2
    };
options.OrderBy.Add("LastRenovationDate desc");
options.Select.Add("HotelName");
options.Select.Add("LastRenovationDate");

results = searchClient.Search<Hotel>("*", options);

WriteDocuments(results);
```

En la última consulta, encuentre todos los nombres de hoteles que coincidan con la palabra "hotel":

```csharp
options.Select.Add("HotelId");
options.Select.Add("HotelName");
options.Select.Add("Description");
options.Select.Add("Category");
options.Select.Add("Tags");
options.Select.Add("ParkingIncluded");
options.Select.Add("LastRenovationDate");
options.Select.Add("Rating");
options.Select.Add("Address");
options.Select.Add("Rooms");

results = searchClient.Search<Hotel>("hotel", options);

WriteDocuments(results);
```

En esta sección se concluye esta introducción al SDK de .NET, pero no debe detenerse aquí. En la siguiente sección se sugieren recursos adicionales para obtener más información sobre cómo programar con Azure Cognitive Search.

## <a name="next-steps"></a>Pasos siguientes

+ Consulte la documentación de referencia de la API para aprender sobre [Azure.Search.Documents](/dotnet/api/azure.search.documents) y la [API REST](/rest/api/searchservice/)

+ Consulte otros ejemplos de código basados en Azure.Search.Documents en [ejemplos de DotNet de Azure Search](https://github.com/Azure-Samples/azure-search-dotnet-samples) e [Introducción a las búsquedas con DotNet](https://github.com/Azure-Samples/search-dotnet-getting-started)

+ Consulte las [convenciones de nomenclatura](/rest/api/searchservice/Naming-rules) para conocer las reglas que deben seguir los nombres de diversos objetos

+ Consulte los [tipos de datos admitidos](/rest/api/searchservice/Supported-data-types)