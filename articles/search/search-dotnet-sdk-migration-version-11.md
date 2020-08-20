---
title: Actualización a la versión 11 del SDK de .NET
titleSuffix: Azure Cognitive Search
description: Migración del código a la versión 11 del SDK de .NET para Azure Cognitive Search a partir de versiones anteriores. Obtenga información sobre las novedades y los cambios de código necesarios.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 08/05/2020
ms.openlocfilehash: 03d40dcaeaefe01fecbc201cf28dc20c8634af9d
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87926678"
---
# <a name="upgrade-to-azure-cognitive-search-net-sdk-version-11"></a>Actualización a la versión 11 del SDK de .NET para Azure Cognitive Search

Si usa la versión 10.0 o anterior del [SDK de .NET](https://docs.microsoft.com/dotnet/api/overview/azure/search), este artículo lo ayudará a actualizar a la versión 11.

La versión 11 es una biblioteca cliente totalmente rediseñada que ha publicado el equipo de desarrollo de Azure SDK (las versiones anteriores fueron creadas por el equipo de desarrollo de Azure Cognitive Search). La biblioteca se ha rediseñado para que sea más coherente con otras bibliotecas cliente de Azure, tomando una dependencia de [Azure.Core](https://docs.microsoft.com/dotnet/api/azure.core) y [System.Text.Json](https://docs.microsoft.com/dotnet/api/system.text.json) e implementando enfoques conocidos para las tareas comunes.

Algunas de las principales diferencias que observará en la nueva versión son estas:

+ Un paquete y una biblioteca en lugar de varios
+ Un nuevo nombre de paquete: `Azure.Search.Documents` en lugar de `Microsoft.Azure.Search`
+ Tres clientes en lugar de dos: `SearchClient`, `SearchIndexClient` y `SearchIndexerClient`
+ Diferencias de nomenclatura en una serie de API y pequeñas diferencias estructurales que simplifican algunas tareas

## <a name="package-and-library-consolidation"></a>Consolidación de paquetes y bibliotecas

La versión 11 consolida varios paquetes y bibliotecas en uno solo. Después de la migración, tendrá menos bibliotecas para administrar.

+ [Paquete Azure.Search.Documents](https://www.nuget.org/packages/Azure.Search.Documents/)

+ [Referencia de API de la biblioteca cliente](https://docs.microsoft.com/dotnet/api/overview/azure/search.documents-readme?view=azure-dotnet)

## <a name="client-differences"></a>Diferencias de cliente

En la siguiente tabla se relacionan las bibliotecas cliente de las dos versiones.

| Ámbito de operaciones | Microsoft.Azure.Search&nbsp;(v. 10) | Azure.Search.Documents&nbsp;(v. 11) |
|---------------------|------------------------------|------------------------------|
| Cliente usado para las consultas y para rellenar un índice. | [SearchIndexClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexclient) | [SearchClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient) |
| Cliente usado para índices, analizadores y mapas de sinónimos. | [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) | [SearchIndexClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexclient) |
| Cliente usado para indizadores, orígenes de datos y conjuntos de aptitudes. | [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) | [SearchIndexerClient (**nueva**)](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexerclient) |

> [!Important]
> `SearchIndexClient` está en ambas versiones, pero no admite las mismas cosas. En la versión 10, `SearchIndexClient` crea índices y otros objetos. En la versión 11, `SearchIndexClient` funciona con índices ya creados. Para evitar confusiones al actualizar el código, tenga en cuenta el orden en que se actualizan las referencias de cliente. Si sigue la secuencia de [pasos de actualización](#UpgradeSteps), debería poder mitigar cualquier problema de reemplazo de cadenas.

<a name="naming-differences"></a>

## <a name="naming-and-other-api-differences"></a>Nomenclatura y otras diferencias de API

Además de las diferencias de cliente (indicadas anteriormente y que, por tanto, se omitieron aquí), se ha cambiado el nombre de varias API y, en algunos casos, se han rediseñado. A continuación se resumen las diferencias en los nombres de clase. Esta lista no es exhaustiva, pero agrupa los cambios de API por tarea, lo que puede resultar útil para las revisiones en bloques de código específicos. Para ver una lista de actualizaciones de API, consulte el [registro de cambios](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) de `Azure.Search.Documents` en GitHub.

### <a name="authentication-and-encryption"></a>Autenticación y cifrado

| Versión 10 | Equivalente en la versión 11 |
|------------|-----------------------|
| [SearchCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchcredentials) | [AzureKeyCredential](https://docs.microsoft.com/dotnet/api/azure.azurekeycredential) |
| `EncryptionKey` (estaba en el [SDK en versión preliminar](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) como una característica disponible con carácter general) | [SearchResourceEncryptionKey](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchresourceencryptionkey) |

### <a name="indexes-analyzers-synonym-maps"></a>Índices, analizadores y mapas de sinónimos

| Versión 10 | Equivalente en la versión 11 |
|------------|-----------------------|
| [Index](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.index) | [SearchIndex](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindex) |
| [Campo](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.field) | [SearchField](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchfield) |
| [DataType](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datatype) | [SearchFieldDataType](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchfielddatatype) |
| [ItemError](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.itemerror) | [SearchIndexerError](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexererror) |
| [Analyzer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzer) | [LexicalAnalyzer](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.lexicalanalyze) (también, de `AnalyzerName` a `LexicalAnalyzerName`) |
| [AnalyzeRequest](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzerequest) | [AnalyzeTextOptions](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.analyzetextoptions) |
| [StandardAnalyzer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.standardanalyzer) | [LuceneStandardAnalyzer](https://docs.microsoft.com//dotnet/api/azure.search.documents.indexes.models.lucenestandardanalyzer) |
| [StandardTokenizer](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.standardtokenizer) | [LuceneStandardTokenizer](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.lucenestandardtokenizer) (también, de `StandardTokenizerV2` a `LuceneStandardTokenizerV2`) |
| [TokenInfo](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.tokeninfo) | [AnalyzedTokenInfo](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.analyzedtokeninfo) |
| [Tokenizador](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.tokenizer) | [LexicalTokenizer](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.lexicaltokenizer) (también, de `TokenizerName` a `LexicalTokenizerName`) |
| [SynonymMap.Format](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.synonymmap.format) | Ninguno. Se eliminan las referencias a `Format`. |

Se han optimizado las definiciones de campo: [SearchableField](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchablefield), [SimpleField](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.simplefield) y [ComplexField](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.complexfield) son nuevas API para crear definiciones de campo.

### <a name="indexers-datasources-skillsets"></a>Indizadores, orígenes de datos y conjuntos de aptitudes

| Versión 10 | Equivalente en la versión 11 |
|------------|-----------------------|
| [Indizador](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexer) | [SearchIndexer](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexer) |
| [DataSource](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource) | [SearchIndexerDataSourceConnection](https://docs.microsoft.com//dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourceconnection) |
| [Habilidad](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.skill) | [SearchIndexerSkill](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexerskill) |
| [Conjunto de aptitudes](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.skillset) | [SearchIndexerSkillset](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexerskillse) |
| [DataSourceType](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasourcetype) | [SearchIndexerDataSourceType](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.models.searchindexerdatasourcetype) |

### <a name="data-import"></a>Importación de datos

| Versión 10 | Equivalente en la versión 11 |
|------------|-----------------------|
| [IndexAction](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction) | [IndexDocumentsAction](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.indexdocumentsaction) |
| [IndexBatch](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch) | [IndexDocumentsBatch](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.indexdocumentsbatch) |

### <a name="query-definitions-and-results"></a>Definiciones de consulta y resultados

| Versión 10 | Equivalente en la versión 11 |
|------------|-----------------------|
| [DocumentSearchResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1) | [SearchResult](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.searchresult-1) o [SearchResults](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.searchresults-1), en función de si el resultado está en un único documento o en varios. |
| [DocumentSuggestResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsuggestresult-1) | [SuggestResults](https://docs.microsoft.com/dotnet/api/azure.search.documents.models.suggestresults-1) |
| [SearchParameters](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters) |  [SearchOptions](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchoptions)  |

<a name="WhatsNew"></a>

## <a name="whats-in-version-11"></a>Novedades de la versión 11

Cada versión de una biblioteca cliente de Azure Cognitive Search tiene como destino su versión correspondiente de la API REST. La API REST se considera fundamental para el servicio, con SDK individuales que encapsulan una versión de la API REST. Como desarrollador de .NET, puede ser útil revisar la [documentación de la API REST](https://docs.microsoft.com/rest/api/searchservice/) para obtener más información sobre objetos u operaciones específicos.

La versión 11 tiene como destino el [servicio de búsqueda 2020-06-30](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/search/data-plane/Azure.Search/preview/2020-06-30/searchservice.json). Dado que la versión 11 también es una nueva biblioteca cliente creada desde cero, la mayor parte del trabajo de desarrollo se ha centrado en la equivalencia con la versión 10, y queda aún pendiente la compatibilidad con algunas características de la API REST.

La versión 11 es totalmente compatible con los siguientes objetos y operaciones:

+ Creación y administración de índices
+ Creación y administración de mapas de sinónimos
+ Todos los tipos de consulta y sintaxis (excepto los filtros geoespaciales)
+ Objetos de indizadores y operaciones para indexar orígenes de datos de Azure, incluidos los orígenes de datos y los conjuntos de habilidades

### <a name="pending-features"></a>Características pendientes

Las siguientes características de la versión 10 aún no están disponibles en la 11. Si usa estas características, desactive la migración hasta que se admitan.

+ Tipos geoespaciales
+ [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder) (aunque puede usar [esta solución alternativa](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/tests/Samples/FieldBuilder/FieldBuilder.cs)).
+ [Almacén de conocimiento](knowledge-store-concept-intro.md)

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Pasos para actualizar

Los pasos siguientes lo ayudarán a comenzar a realizar una migración de código recorriendo el primer conjunto de tareas necesarias, especialmente en lo que respecta a las referencias de cliente.

1. Instale el paquete [Azure.Search.Documents](https://www.nuget.org/packages/Azure.Search.Documents/) haciendo clic con el botón derecho en las referencias del proyecto y seleccione "Administrar paquetes NuGet..." en Visual Studio.

1. Reemplace las directivas "using" de Microsoft.Azure.Search por lo siguiente:

   ```csharp
   using Azure;
   using Azure.Search.Documents;
   using Azure.Search.Documents.Indexes;
   using Azure.Search.Documents.Indexes.Models;
   using Azure.Search.Documents.Models;
   ```

1. Reemplace [SearchCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchcredentials) por [AzureKeyCredential](https://docs.microsoft.com/dotnet/api/azure.azurekeycredential).

1. Actualice las referencias de cliente para los objetos relacionados con los indizadores. Si usa indexadores, orígenes de usuario o conjuntos de habilidades, cambie las referencias de cliente a [SearchIndexerClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.indexes.searchindexerclient). Este cliente es nuevo en la versión 11 y no tiene antecedente.

1. Actualice las referencias de cliente para las consultas y la importación de datos. Las instancias de [SearchIndexClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient) deben cambiarse a [SearchClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchclient). Para evitar confusión con los nombres, asegúrese de detectar todas las instancias antes de continuar con el siguiente paso.

1. Actualice las referencias de cliente en los objetos index, indexer, synonym map y analyzer. Las instancias de [SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient) deben cambiarse a [SearchIndexClient](https://docs.microsoft.com/dotnet/api/azure.search.documents.searchindexclient). 

1. En la medida de lo posible, actualice las clases, los métodos y las propiedades para usar las API de la nueva biblioteca. La sección [Nomenclatura y otras diferencias de API](#naming-differences) es un buen sitio para empezar, pero también puede revisar el [registro de cambios](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md).

   Si tiene problemas para encontrar API equivalentes, se recomienda registrar un problema en [https://github.com/MicrosoftDocs/azure-docs/issues](https://github.com/MicrosoftDocs/azure-docs/issues) para que podamos mejorar la documentación o investigar el problema.

1. Recompilar la solución. Después de corregir las advertencias o los errores de compilación, puede realizar cambios en la aplicación para aprovechar las ventajas de la [nueva funcionalidad](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-11"></a>Cambios importantes en la versión 11

Dado los cambios radicales producidos en las bibliotecas y las API, una actualización a la versión 11 no es algo trivial y constituye un cambio importante en el sentido de que el código ya no será compatible con la versión 10 y anteriores. Para ver todas las diferencias, vea el [registro de cambios](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/search/Azure.Search.Documents/CHANGELOG.md) de `Azure.Search.Documents`.

En lo que respecta a las versiones del servicio, al pasar de la 10 a la 11 se introducen los siguientes cambios de comportamiento: 

+ El [algoritmo de clasificación BM25](index-ranking-similarity.md) reemplaza el algoritmo de clasificación anterior por una tecnología más reciente. Los servicios nuevos usarán este algoritmo de manera automática. En el caso de los servicios existentes, debe establecer los parámetros para usar el algoritmo nuevo.

+ Los [resultados ordenados](search-query-odata-orderby.md) de los valores NULL han cambiado en esta versión y aparecen en primer lugar si la ordenación es `asc` y en el último si la ordenación es `desc`. Si ha escrito código para controlar cómo se ordenan los valores NULL, debe revisar y, posiblemente, quitar ese código si ya no es necesario.

## <a name="next-steps"></a>Pasos siguientes

+ [Paquete Azure.Search.Documents](https://www.nuget.org/packages/Azure.Search.Documents/)
+ [Ejemplos en GitHub](https://github.com/azure/azure-sdk-for-net/tree/Azure.Search.Documents_11.0.0/sdk/search/Azure.Search.Documents/samples)
+ [Referencia de la API de Azure.Search.Document](https://docs.microsoft.com/dotnet/api/overview/azure/search.documents-readme?view=azure-dotnet)