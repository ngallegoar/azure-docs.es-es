---
title: Sugerencias de rendimiento de Azure Cosmos DB para el SDK de .NET v3
description: Obtenga información sobre las opciones de configuración de cliente para mejorar el rendimiento del SDK de .NET v3 en Azure Cosmos DB.
author: j82w
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/16/2020
ms.author: jawilley
ms.openlocfilehash: 30fdc3c2b75d8ae567acfc612514ab080b929c5f
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85850263"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Sugerencias de rendimiento para Azure Cosmos DB y .NET

> [!div class="op_single_selector"]
> * [.NET SDK v3](performance-tips-dotnet-sdk-v3-sql.md)
> * [SDK de .NET v2](performance-tips.md)
> * [SDK de Java v4](performance-tips-java-sdk-v4-sql.md)
> * [Versión 2 del SDK de Java asincrónico](performance-tips-async-java.md)
> * [SDK de Java v2 sincrónico](performance-tips-java.md)

Azure Cosmos DB es una base de datos distribuida rápida y flexible que se escala sin problemas con una latencia y un rendimiento garantizados. No es necesario realizar cambios de arquitectura importantes ni escribir el código complejo para escalar la base de datos con Azure Cosmos DB. Escalar o reducir verticalmente es tan sencillo como realizar una única llamada API. Para más información, consulte [cómo aprovisionar el rendimiento del contenedor](how-to-provision-container-throughput.md) o [cómo aprovisionar el rendimiento de la base de datos](how-to-provision-database-throughput.md). Sin embargo, dado que se tiene acceso a Azure Cosmos DB a través de llamadas de red, existen optimizaciones del lado cliente que se pueden realizar para lograr el máximo rendimiento cuando se usa el [del SDK de .NET para SQL](https://github.com/Azure/azure-cosmos-dotnet-v3).

Por lo tanto, si está intentando mejorar el rendimiento de la base de datos, tenga en cuenta estas opciones:

## <a name="hosting-recommendations"></a>Hospedando recomendaciones

**En el caso de cargas de trabajo con un uso intensivo de consultas, use Windows de 64 bits en lugar del procesamiento de host de Windows de 32 bits y Linux**

Se recomienda el procesamiento de host de Windows de 64 bits para mejorar el rendimiento. El SDK de SQL incluye un archivo ServiceInterop.dll nativo para analizar y optimizar consultas localmente. ServiceInterop.dll solo se admite en la plataforma Windows x64. En el caso de Linux y otras plataformas no compatibles en las que ServiceInterop.dll no está disponible, se realiza una llamada de red adicional a la puerta de enlace para obtener la consulta optimizada. Los siguientes tipos de aplicaciones utilizan el procesamiento de host de 32 bits de forma predeterminada. Para cambiar el procesamiento de host al procesamiento de 64 bits, siga estos pasos según el tipo de la aplicación:

- En el caso de las aplicaciones ejecutables, puede cambiar el procesamiento de host estableciendo el [destino de la plataforma](https://docs.microsoft.com/visualstudio/ide/how-to-configure-projects-to-target-platforms?view=vs-2019) en **x64** en la ventana **Propiedades del proyecto**, en la pestaña **Compilar**.

- Para proyectos de prueba basados en VSTest, puede cambiar el procesamiento del host seleccionando **Prueba** >  **Configuración de prueba** >  **Arquitectura de procesador predeterminada como X64** en el menú de **Prueba** de Visual Studio.

- En el caso de las aplicaciones Web de ASP.NET implementadas de forma local, puede cambiar el procesamiento de host seleccionando **Usar la versión de 64 bits de IIS Express para proyectos y sitios web** en **Herramientas** > **Opciones** > **Proyectos y Proyectos** > **de Soluciones Web**.

- En el caso de las aplicaciones Web de ASP.NET implementadas en Azure, puede cambiar el procesamiento de host seleccionando la plataforma de **64 bits** en **Configuración de la aplicación** en el Azure Portal.

> [!NOTE] 
> De forma predeterminada, los nuevos proyectos de Visual Studio se establecen en **cualquier CPU**. Se recomienda establecer el proyecto en **x64** para que no cambie a **x86**. Un proyecto establecido para **cualquier CPU** puede cambiar fácilmente a **x86** si se agrega una dependencia de solo x86.<br/>
> ServiceInterop.dll debe estar en la carpeta desde la que se ejecuta la DLL del SDK. Esto solo debe ser un problema si se copian manualmente los archivos DLL o los sistemas de compilación o implementación personalizados.
    
**Activación de la recolección de elementos no utilizados (GC) del lado servidor**

La reducción de la frecuencia de recolección de elementos no utilizados puede ayudar en algunos casos. En .NET, establezca [gcServer](https://docs.microsoft.com/dotnet/core/run-time-config/garbage-collector#flavors-of-garbage-collection) en `true`.

**Escalado horizontal de la carga de trabajo de cliente**

Si va a realizar pruebas en niveles de alto rendimiento (más de 50 000 RU/s), la aplicación del cliente podría convertirse en el cuello de botella debido a que el equipo se limita al uso de la CPU o la red. Si llega a este punto, puede seguir insertando la cuenta de Azure Cosmos DB mediante la escala horizontal de las aplicaciones cliente en varios servidores.

> [!NOTE] 
> Un uso elevado de CPU puede provocar el aumento de la latencia, así como excepciones de tiempo de espera de solicitud.

## <a name="networking"></a>Redes
<a id="direct-connection"></a>

**Directiva de conexión: uso del modo de conexión directa**

La forma en que un cliente se conecta a Azure Cosmos DB tiene importantes implicaciones de rendimiento, especialmente para la latencia observada en el cliente. Existen dos opciones de configuración clave disponibles para configurar la directiva de conexión del cliente: el *modo* de conexión y el *protocolo* de conexión.  Los dos modos disponibles son:

   * Modo directo (valor predeterminado)

     El modo directo es compatible con la conectividad a través del protocolo TCP y es el modo de conectividad predeterminado si utiliza el SDK de [Microsoft.Azure.Cosmos/.NET V3](https://github.com/Azure/azure-cosmos-dotnet-v3). Esto ofrece un mejor rendimiento y requiere menos saltos de red que el modo de puerta de enlace.

   * Modo de puerta de enlace
      
     Si la aplicación se ejecuta dentro de una red corporativa con restricciones de Firewall estrictas, el modo de puerta de enlace es la mejor opción, ya que utiliza el Puerto HTTPS estándar y un único punto de conexión. La desventaja para el rendimiento, sin embargo, es que el modo de puerta de enlace implica un salto de red adicional cada vez que se leen desde o se escriben datos a Azure Cosmos DB. Por tanto, el modo directo ofrece un mejor rendimiento porque hay menos saltos de red. También se recomienda el modo de conexión de puerta de enlace cuando se ejecutan aplicaciones en entornos que tienen un número limitado de conexiones de socket.

     Cuando use el SDK de Azure Functions, especialmente en el [plan de consumo](../azure-functions/functions-scale.md#consumption-plan), tenga en cuenta los [límites actuales en las conexiones](../azure-functions/manage-connections.md). En ese caso, el modo de puerta de enlace podría ser mejor si también está trabajando con otros clientes basados en HTTP dentro de la aplicación Azure Functions.


En el modo de puerta de enlace, Azure Cosmos DB usa el puerto 443 y los puertos 10250, 10255 y 10256 cuando se usa la API de Azure Cosmos DB para MongoDB. El puerto 10250 se asigna a una instancia de MongoDB predeterminada sin replicación geográfica. Los puertos 10255 y 10256 se asignan a la instancia de MongoDB que tiene replicación geográfica.
     
Al usar TCP en modo directo, además de los puertos de puerta de enlace, debe asegurarse de que el intervalo de puertos entre 10000 y 20000 está abierto porque Azure Cosmos DB utiliza puertos TCP dinámicos (cuando se usa el modo directo en [puntos de conexión privados](./how-to-configure-private-endpoints.md), se tiene que abrir el intervalo completo de puertos TCP, de 0 a 65535). Los puertos están abiertos de forma predeterminada para la configuración estándar de la máquina virtual de Azure. Si estos puertos no están abiertos e intenta usar TCP, recibirá un error 503 de servicio no disponible. En esta tabla se muestran los modos de conectividad disponibles para varias API y los puertos de servicio que se usan para cada API:

|Modo de conexión  |Protocolo admitido  |SDK admitidos  |API o puerto de servicio  |
|---------|---------|---------|---------|
|Puerta de enlace  |   HTTPS    |  Todos los SDK    |   SQL (443), MongoDB (10250, 10255, 10256), Tabla (443), Cassandra (10350), Graph (443)    |
|Directo    |     TCP    |  .NET SDK    | Al usar puntos de conexión de servicio o públicos: puertos en el intervalo de 10000 a 20000<br>Al usar puntos de conexión privados: puertos en el intervalo de 0 a 65535 |

Azure Cosmos DB ofrece un modelo de programación RESTful sencillo y abierto sobre HTTPS. Además, ofrece un protocolo TCP eficaz que también es RESTful en su modelo de comunicación y está disponible a través del SDK de cliente de .NET. El protocolo TCP usa TLS para la autenticación inicial y el cifrado del tráfico. Para obtener el mejor rendimiento, utilice el protocolo TCP cuando sea posible.

En el caso del SDK V3, se configura el modo de conexión cuando se crea la instancia de `CosmosClient` en `CosmosClientOptions`. Recuerde que el modo directo es el valor predeterminado.

```csharp
string connectionString = "<your-account-connection-string>";
CosmosClient client = new CosmosClient(connectionString,
new CosmosClientOptions
{
    ConnectionMode = ConnectionMode.Gateway // ConnectionMode.Direct is the default
});
```

Como TCP solo se admite en modo directo, si usa el modo de puerta de enlace, siempre se utiliza el protocolo HTTPS para comunicarse con la puerta de enlace.

:::image type="content" source="./media/performance-tips/connection-policy.png" alt-text="Directiva de conexión de Azure Cosmos DB" border="false":::

**Agotamiento de puertos efímeros**

Si en las instancias detecta un volumen de conexiones alto o un uso elevado de los puertos, compruebe primero que las instancias del cliente son singleton. Es decir, las instancias de cliente deben ser únicas para la duración de la aplicación.

Cuando se ejecuta en el protocolo TCP, el cliente se optimiza para la latencia mediante conexiones de larga duración en lugar de con el protocolo HTTPS, que finaliza las conexiones tras dos minutos de inactividad.

En escenarios en los que tiene acceso disperso y observa un recuento de conexiones superior en comparación con el acceso del modo de puerta de enlace, puede hacer lo siguiente:

* Configure la propiedad [CosmosClientOptions.PortReuseMode](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.portreusemode) en `PrivatePortPool` (en vigor con la versión de Framework >= 4.6.1 y la versión de .NET Core >= 2.0): Esta propiedad permite al SDK usar un pequeño grupo de puertos efímeros para diferentes puntos de conexión de destino de Azure Cosmos DB.
* Configure la propiedad [CosmosClientOptions.IdleConnectionTimeout](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.idletcpconnectiontimeout) para que sea mayor o igual que 10 minutos. Los valores recomendados son entre 20 minutos y 24 horas.

<a id="same-region"></a>

**Para el rendimiento, coloque los clientes en la misma región de Azure**

Cuando sea posible, coloque las aplicaciones que llaman a Azure Cosmos DB en la misma región que la base de datos de Azure Cosmos DB. Esta es una comparación aproximada: las llamadas a Azure Cosmos DB en la misma región se completan entre 1 ms y 2 ms, pero la latencia entre la costa oeste y el este de EE. UU. es superior a 50 ms. Esta latencia puede variar de una solicitud a otra, en función de la ruta realizada por la solicitud a medida que pasa del cliente al límite del centro de recursos de Azure. Para obtener la menor latencia posible, asegúrese de que la aplicación que realiza la llamada se encuentra dentro de la misma región de Azure que el punto de conexión de Azure Cosmos DB aprovisionado. Para obtener una lista de las regiones disponibles, vea [Regiones de Azure](https://azure.microsoft.com/regions/#services).

:::image type="content" source="./media/performance-tips/same-region.png" alt-text="Directiva de conexión de Azure Cosmos DB" border="false":::

   <a id="increase-threads"></a>

**Aumentar el número de subprocesos o tareas**

Como las llamadas a Azure Cosmos DB se realizan a través de la red, es posible que tenga que cambiar el grado de simultaneidad de las solicitudes para que la aplicación cliente dedique un tiempo mínimo a esperar entre solicitudes. Por ejemplo, si usa la [biblioteca TPL](https://msdn.microsoft.com//library/dd460717.aspx) de .NET, cree en el orden de cientos de tareas que leen o escriben en Azure Cosmos DB.

**Habilitación de la redes aceleradas**
 
 Para reducir la latencia y la vibración de la CPU, se recomienda habilitar redes aceleradas en las máquinas virtuales de cliente. Consulte [Creación de una máquina virtual Windows con redes aceleradas](../virtual-network/create-vm-accelerated-networking-powershell.md) o [Creación de una máquina virtual Linux con redes aceleradas](../virtual-network/create-vm-accelerated-networking-cli.md).

## <a name="sdk-usage"></a>Uso del SDK
**Instalación del SDK más reciente**

Los SDK de Azure Cosmos DB se mejoran constantemente para proporcionar el mejor rendimiento. Consulte las páginas del [SDK de Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3) para determinar las mejoras de los SDK y las revisiones más recientes.

**Uso de las API de transmisión**

El [SDK de .NET V3](https://github.com/Azure/azure-cosmos-dotnet-v3) contiene API de secuencia que puede recibir y devolver datos sin serialización. 

Las aplicaciones de nivel intermedio que no consumen respuestas directamente del SDK pero las transmiten a otros niveles de aplicaciones pueden beneficiarse de las API de transmisión. Vea los ejemplos de [administración de elementos](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement) para ver ejemplos de control de transmisión.

**Uso de un cliente de Azure Cosmos DB singleton para aumentar la duración de la aplicación**

Cada instancia de `CosmosClient` es segura para subprocesos y realiza una administración eficaz de la conexión y el almacenamiento en caché de direcciones cuando funciona en modo directo. Para permitir una administración de conexión eficaz y un mejor rendimiento del cliente de SDK, se recomienda usar una sola instancia por `AppDomain` durante la vigencia de la aplicación.

Al trabajar en Azure Functions, las instancias también deben seguir las [instrucciones](../azure-functions/manage-connections.md#static-clients) existentes y mantener una sola instancia.

<a id="max-connection"></a>

**Deshabilitar la respuesta de contenido en operaciones de escritura**

En el caso de cargas de trabajo con cargas útiles de creación intensivas, establezca la opción de solicitud EnableContentResponseOnWrite en false. El servicio ya no devolverá al SDK el recurso creado o actualizado. Normalmente, la aplicación tiene el objeto que se crea, por lo que no necesita que el servicio lo devuelva. Todavía se puede acceder a los valores de encabezado, como Solicitar cargo. Esto puede mejorar el rendimiento, porque el SDK ya no tendrá que asignar memoria ni serializar el cuerpo de la respuesta. Esto también reduce el uso de ancho de banda de red para mejorar más el rendimiento.  

```csharp
ItemRequestOption requestOptions = new ItemRequestOptions() { EnableContentResponseOnWrite = false };
ItemResponse<Book> itemResponse = await this.container.CreateItemAsync<Book>(book, new PartitionKey(book.pk), requestOptions);
// Resource will be null
itemResponse.Resource
```

**Habilitación del modo masivo para optimizar el rendimiento en lugar de la latencia** Habilite el modo masivo para escenarios en los que la carga de trabajo requiere una gran cantidad de rendimiento y la latencia no es tan importante. Vea la [introducción al modo masivo](https://devblogs.microsoft.com/cosmosdb/introducing-bulk-support-in-the-net-sdk) para obtener más información sobre cómo habilitar la característica y los escenarios en los que se debe usar.

**Aumentar System.Net MaxConnections por host al usar el modo de puerta de enlace**

Las solicitudes de Azure Cosmos DB se realizan a través de HTTPS o REST cuando se usa el modo de puerta de enlace. Están sujetos al límite de conexiones predeterminado por nombre de host o dirección IP. Es posible que tenga que establecer `MaxConnections` en un valor superior (de 100 a 1 000) para que la biblioteca de cliente pueda utilizar varias conexiones simultáneas para Azure Cosmos DB. En el SDK de .NET 1.8.0 y versiones posteriores, el valor predeterminado de [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) es 50. Para cambiar el valor, puede establecer [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) en un valor superior.

**Optimizar consultas paralelas para colecciones con particiones**

El SDK de .NET para SQL admite las consultas paralelas, que permiten consultar una colección con particiones en paralelo. Para obtener más información, consulte [ejemplos de código](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs) relacionados para trabajar con los SDK. Las consultas paralelas están diseñadas para proporcionar una mejor latencia de consulta y rendimiento que su homólogo en serie. Las consultas paralelas proporcionan dos parámetros que se pueden adaptar para ajustarse a sus requisitos: 
- `MaxConcurrency` controla el número máximo de particiones que se pueden consultar en paralelo. 
- `MaxBufferedItemCount` controla el número de resultados capturados previamente.

***Grado de optimización de la simultaneidad***

La consulta en paralelo funciona consultando varias particiones en paralelo. Sin embargo, los datos de una partición individual se capturan en serie con respecto a la consulta. Al establecer `MaxConcurrency` en [SDK V3](https://github.com/Azure/azure-cosmos-dotnet-v3) en el número de particiones, tiene la mejor posibilidad de lograr la consulta de mayor rendimiento, siempre y cuando todas las demás condiciones del sistema sigan siendo las mismas. Si no conoce el número de particiones, puede establecer el grado de paralelismo en un número alto. El sistema elegirá el mínimo (número de particiones, entrada proporcionada por el usuario) como el grado de paralelismo.

Las consultas paralelas son las que mayor ventaja ofrecen si los datos se distribuyen de manera uniforme entre todas las particiones con respecto a la consulta. Si la colección con particiones tiene particiones para que todos o la mayoría de los datos devueltos por una consulta se concentren en algunas particiones (una partición es el peor caso), esas particiones producirán cuellos de botella en el rendimiento de la consulta.

***Optimizar MaxBufferedItemCount***
    
Las consultas en paralelo están diseñadas para capturar previamente los resultados mientras el cliente procesa el lote actual de resultados. Esta captura previa ayuda a mejorar la latencia general de una consulta. El parámetro `MaxBufferedItemCount` limita el número de resultados capturados previamente. Establezca `MaxBufferedItemCount` en el número esperado de resultados devueltos (o un número más alto) para permitir que la consulta reciba el máximo beneficio de la captura previa.

La captura previa funciona de la misma manera, independientemente del grado de paralelismo y hay un único búfer para los datos de todas las particiones.  

**Implementación del retroceso según intervalos RetryAfter**

Durante las pruebas de rendimiento, debe aumentar la carga hasta que se limite una pequeña tasa de solicitudes. Si se limitan las solicitudes, la aplicación del cliente debe volver al límite para el intervalo de reintentos especificado por el servidor. Respetar el retroceso garantiza una cantidad mínima de tiempo en espera entre reintentos. 

Para más información, consulte [RetryAfter](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosexception.retryafter?view=azure-dotnet#Microsoft_Azure_Cosmos_CosmosException_RetryAfter).
    
Hay un mecanismo para registrar información de diagnóstico adicional e incidencias de latencia de solución de problemas, como se muestra en el ejemplo siguiente. Puede registrar la cadena de diagnóstico para las solicitudes que tienen una mayor latencia de lectura. La cadena de diagnóstico capturada le ayudará a comprender cuántas veces ha recibido 429 errores para una solicitud determinada.

```csharp
ItemResponse<Book> readItemResponse = await this.cosmosContainer.ReadItemAsync<Book>("ItemId", new PartitionKey("PartitionKeyValue"));
readItemResponse.Diagnostics.ToString(); 
```

**Aumentar el número de subprocesos o tareas**

Consulte [Aumentar el número de subprocesos y tareas](#increase-threads) en la sección redes de este artículo.

## <a name="indexing-policy"></a>Directiva de indexación
 
**Exclusión de rutas de acceso sin utilizar de la indexación para acelerar las escrituras**

La directiva de indexación de Azure Cosmos DB también le permite especificar las rutas de acceso de documentos que se van a incluir o excluir de la indexación mediante rutas de acceso de indexación (IndexingPolicy.IncludedPaths e IndexingPolicy.ExcludedPaths). Si solo se indexan las rutas de acceso necesarias se puede mejorar el rendimiento de escritura, reducir el cargo de RU en las operaciones de escritura y reducir el almacenamiento de índices en los escenarios en los que los patrones de consulta se conocen con antelación. Esto se debe a que los costos de indización se relacionan directamente con el número de rutas de acceso únicas indizadas. Por ejemplo, este código muestra cómo excluir una sección completa de los documentos (un subárbol) de la indexación mediante el carácter comodín "*":

```csharp
var containerProperties = new ContainerProperties(id: "excludedPathCollection", partitionKeyPath: "/pk" );
containerProperties.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
containerProperties.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
Container container = await this.cosmosDatabase.CreateContainerAsync(containerProperties);
```

Para más información, consulte [Directivas de indexación de Azure Cosmos DB](index-policy.md).

## <a name="throughput"></a>Throughput
<a id="measure-rus"></a>

**Medición y optimización del uso menor de unidades de solicitud por segundo**

Azure Cosmos DB ofrece un amplio conjunto de operaciones de base de datos. Estas operaciones incluyen consultas relacionales y jerárquicas con UDF, procedimientos almacenados y desencadenadores que funcionan en los documentos de una colección de base de datos. El costo asociado a cada una de estas operaciones varía en función de la CPU, la E/S y la memoria necesaria para completar la operación. En lugar de pensar y administrar los recursos de hardware, puede pensar en una unidad de solicitud (RU) como una única medida para los recursos necesarios para realizar varias operaciones de base de datos y dar servicio a una solicitud de aplicación.

El rendimiento se aprovisiona en función del número de [unidades de solicitud](request-units.md) establecidas para cada contenedor. El consumo de la unidad de solicitud se evalúa como una tarifa por segundo. Las aplicaciones que superan la frecuencia de unidad de solicitud aprovisionada para su contenedor se limitan hasta que la velocidad cae por debajo del nivel aprovisionado para el contenedor. Si su aplicación requiere un mayor nivel de rendimiento, puede aumentar el rendimiento mediante el aprovisionamiento de unidades de solicitud adicionales.

La complejidad de una consulta afecta a la cantidad de unidades de solicitud que se consumen para una operación. El número de predicados, la naturaleza de los predicados, el número de UDF y el tamaño del conjunto de código de origen influyen en el coste de las operaciones de consulta.

Para medir la sobrecarga de cualquier operación (crear, actualizar o eliminar), inspeccione el encabezado [x-ms-request-charge](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) (o la propiedad equivalente a `RequestCharge` en `ResourceResponse\<T>` o `FeedResponse\<T>` en el SDK de .NET) para medir el número de unidades de solicitud consumidas por las operaciones:

```csharp
// Measure the performance (Request Units) of writes
ItemResponse<Book> response = await container.CreateItemAsync<Book>(myBook, new PartitionKey(myBook.PkValue));
Console.WriteLine("Insert of item consumed {0} request units", response.RequestCharge);
// Measure the performance (Request Units) of queries
FeedIterator<Book> queryable = container.GetItemQueryIterator<ToDoActivity>(queryString);
while (queryable.HasMoreResults)
    {
        FeedResponse<Book> queryResponse = await queryable.ExecuteNextAsync<Book>();
        Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
    }
```             

El cargo de solicitud devuelto en este encabezado es una fracción del rendimiento aprovisionado (es decir, 2 000 RUs por segundo). Por ejemplo, si la consulta anterior devuelve 1 000 documentos de 1 KB, el costo de la operación es 1 000. Por lo tanto, en un segundo, el servidor respeta solo dos solicitudes de este tipo antes de la limitación de velocidad de las solicitudes posteriores. Para más información, consulte [Unidades de solicitud](request-units.md) y la [Calculadora de unidades de solicitud ](https://www.documentdb.com/capacityplanner).
<a id="429"></a>

**Administración de la limitación de velocidad y la tasa de solicitudes demasiado grande**

Cuando un cliente intenta superar la capacidad de proceso reservada para una cuenta, no hay degradación del rendimiento en el servidor y no se usa la capacidad de rendimiento más allá del nivel reservado. El servidor finalizará de forma preventiva la solicitud con RequestRateTooLarge (código de Estado HTTP 429). Devolverá un encabezado [x-ms-retry-after-ms](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) que indica la cantidad de tiempo, en milisegundos, que el usuario debe esperar antes de volver a intentar la solicitud.

```xml
    HTTP Status 429,
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100
```

Los SDK capturan implícitamente esta respuesta, respetan el encabezado retry-after especificado por el servidor y reintentan la solicitud. A menos que varios clientes obtengan acceso a la cuenta al mismo tiempo, el siguiente reintento se realizará correctamente.

Si tiene más de un cliente que funciona acumulativamente de forma sistemática por encima de la tasa de solicitudes, es posible que el número de reintentos predeterminado establecido en 9 internamente por el cliente no sea suficiente. En este caso, el cliente inicia una excepción CosmosException con el código de estado 429 en la aplicación. 

Puede cambiar el número de reintentos predeterminado estableciendo el `RetryOptions` en la instancia de `CosmosClientOptions`. De forma predeterminada, la excepción CosmosException con el código de estado 429 se devuelve tras un tiempo de espera acumulativo de 30 segundos si la solicitud sigue funcionando por encima de la tasa de solicitudes. Este error se devuelve aunque el número de reintentos actual sea inferior al número máximo de reintentos, tanto si el valor actual es el predeterminado de 9 como si es un valor definido por el usuario.

El comportamiento de reintento automatizado ayuda a mejorar la resistencia y la facilidad de uso de la mayoría de las aplicaciones. Pero es posible que no sea el mejor comportamiento al realizar pruebas comparativas de rendimiento, especialmente cuando se mide la latencia. La latencia observada del cliente aumentará si el experimento llega a la limitación del servidor y hace que el SDK del cliente realice reintentos de forma silenciosa. Para evitar aumentos de latencia durante los experimentos de rendimiento, mida el gasto devuelto por cada operación y asegúrese de que las solicitudes funcionan por debajo de la tasa de solicitudes observada. Para más información, consulte [Unidades de solicitud](request-units.md).

**Para un mayor rendimiento, diseñe para documentos más pequeños**

El cargo de la solicitud (es decir, el coste de procesamiento de solicitudes) de una operación determinada se correlaciona directamente con el tamaño del documento. Las operaciones en documentos grandes cuestan más que las operaciones en documentos pequeños.

## <a name="next-steps"></a>Pasos siguientes
Para obtener una aplicación de ejemplo que se usa para evaluar Azure Cosmos DB en escenarios de alto rendimiento en algunos equipos del cliente, consulte [Rendimiento y pruebas de escalado con Azure Cosmos DB](performance-testing.md).

Para más información sobre cómo diseñar la aplicación para escalarla y obtener un alto rendimiento, consulte [Partición y escalado en Azure Cosmos DB](partition-data.md).
