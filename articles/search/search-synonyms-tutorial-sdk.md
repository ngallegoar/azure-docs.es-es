---
title: Ejemplo de sinónimos de C#
titleSuffix: Azure Cognitive Search
description: En este ejemplo de C#, obtendrá información sobre cómo agregar la característica de sinónimos a un índice de Azure Cognitive Search. Un mapa de sinónimos es una lista de términos equivalentes. Los campos que incluyen compatibilidad con sinónimos amplían las consultas para incluir el término proporcionado por el usuario y todos los sinónimos relacionados.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/05/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 4b97b223ac180df7f8eb07ad8eaab66847f50776
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "93423001"
---
# <a name="example-add-synonyms-for-azure-cognitive-search-in-c"></a>Ejemplo: Adición de sinónimos para Azure Cognitive Search en C#

Los sinónimos amplían una consulta realizando coincidencias con términos que se consideran equivalentes semánticamente al término de entrada. Por ejemplo, podría interesarle que "coche" obtenga coincidencias con documentos que contengan los términos "automóvil" o "vehículo". 

En Azure Cognitive Search, los sinónimos se definen en un *mapa de sinónimos* a través de *reglas de asignación* que permiten asociar términos equivalentes. En este ejemplo se explican los pasos esenciales para agregar y usar sinónimos con un índice existente.

En este ejemplo, aprenderá a:

> [!div class="checklist"]
> * Cree un mapa de sinónimos mediante la [clase SynonymMap](/dotnet/api/azure.search.documents.indexes.models.synonymmap). 
> * Establezca la [propiedad SynonymMapsName](/dotnet/api/azure.search.documents.indexes.models.searchfield.synonymmapnames) en los campos que deben admitir la expansión de consultas a través de sinónimos.

Puede consultar un campo habilitado para sinónimos como lo haría normalmente. No hay ninguna sintaxis de consulta adicional necesaria para acceder a los sinónimos.

Puede crear varios mapas de sinónimos, publicarlos como un recurso de todo el servicio disponible para todos los índices y, a continuación, hacer referencia a cuál desea usar en el nivel de campo. En el momento de la consulta, además de buscar un índice, Azure Cognitive Search realiza una búsqueda en un mapa de sinónimos, si se especifica uno en los campos usados en la consulta.

> [!NOTE]
> Los sinónimos se pueden crear mediante programación, pero no en el portal.

## <a name="prerequisites"></a>Requisitos previos

Los requisitos del tutorial incluyen los siguientes:

* [Visual Studio](https://www.visualstudio.com/downloads/)
* [Servicio Azure Cognitive Search](search-create-service-portal.md)
* [Paquete Azure.Search.Documents](https://www.nuget.org/packages/Azure.Search.Documents/)

Si no está familiarizado con la biblioteca cliente de .NET, consulte [Uso de Azure Cognitive Search en .NET](search-howto-dotnet-sdk.md).

## <a name="sample-code"></a>Código de ejemplo

Puede encontrar el código fuente completo de la aplicación de ejemplo usada en este ejemplo en [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToSynonyms).

## <a name="overview"></a>Información general

Las consultas de antes y después se usan para mostrar el valor de los sinónimos. En este ejemplo, una aplicación de ejemplo ejecuta consultas y devuelve los resultados en un índice de ejemplo denominado "hotels", que contiene dos documentos. En primer lugar, la aplicación ejecuta las consultas de búsqueda con términos y frases que no aparecen en el índice. En segundo lugar, el código habilita la característica de sinónimos y, a continuación, vuelve a emitir las mismas consultas. En esta ocasión, devuelve resultados en función de las coincidencias de la asignación de sinónimos. 

El código siguiente muestra el flujo general.

```csharp
static void Main(string[] args)
{
   SearchIndexClient indexClient = CreateSearchIndexClient();

   Console.WriteLine("Cleaning up resources...\n");
   CleanupResources(indexClient);

   Console.WriteLine("Creating index...\n");
   CreateHotelsIndex(indexClient);

   SearchClient searchClient = indexClient.GetSearchClient("hotels");

   Console.WriteLine("Uploading documents...\n");
   UploadDocuments(searchClient);

   SearchClient searchClientForQueries = CreateSearchClientForQueries();

   RunQueriesWithNonExistentTermsInIndex(searchClientForQueries);

   Console.WriteLine("Adding synonyms...\n");
   UploadSynonyms(indexClient);

   Console.WriteLine("Enabling synonyms in the test index...\n");
   EnableSynonymsInHotelsIndexSafely(indexClient);
   Thread.Sleep(10000); // Wait for the changes to propagate

   RunQueriesWithNonExistentTermsInIndex(searchClientForQueries);

   Console.WriteLine("Complete.  Press any key to end application...\n");

   Console.ReadKey();
}
```

## <a name="before-queries"></a>Consultas "antes de"

En `RunQueriesWithNonExistentTermsInIndex`, se emiten consultas de búsqueda con "five star", "internet" y "economy AND hotel".

```csharp
Console.WriteLine("Search the entire index for the phrase \"five star\":\n");
results = searchClient.Search<Hotel>("\"five star\"", searchOptions);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'internet':\n");
results = searchClient.Search<Hotel>("internet", searchOptions);
WriteDocuments(results);

Console.WriteLine("Search the entire index for the terms 'economy' AND 'hotel':\n");
results = searchClient.Search<Hotel>("economy AND hotel", searchOptions);
WriteDocuments(results);
```

Ninguno de los dos documentos indexados contienen los términos, por lo que obtenemos el siguiente resultado del primer comando `RunQueriesWithNonExistentTermsInIndex`: **ningún documento coincide**.

## <a name="enable-synonyms"></a>Habilitación de sinónimos

Después de ejecutar las consultas "antes de", el código de ejemplo habilita los sinónimos. La habilitación de sinónimos es un proceso de dos pasos. En primer lugar, se definen y cargan las reglas de sinónimos. En segundo lugar, se configuran los campos para usarlos. El proceso se describe en `UploadSynonyms` y `EnableSynonymsInHotelsIndex`.

1. Agregue un mapa de sinónimos al servicio de búsqueda. En `UploadSynonyms`, definimos cuatro reglas en nuestro mapa de sinónimos 'desc synonymmap' y lo cargamos en el servicio.

   ```csharp
   private static void UploadSynonyms(SearchIndexClient indexClient)
   {
      var synonymMap = new SynonymMap("desc-synonymmap", "hotel, motel\ninternet,wifi\nfive star=>luxury\neconomy,inexpensive=>budget");

      indexClient.CreateOrUpdateSynonymMap(synonymMap);
   }
   ```

1. Configure los campos de búsqueda para utilizar el mapa de sinónimos en la definición del índice. En `AddSynonymMapsToFields`, habilitaremos los sinónimos en dos campos `category` y `tags` estableciendo la propiedad `SynonymMapNames` en el nombre del mapa de sinónimos recién cargado.

   ```csharp
   private static SearchIndex AddSynonymMapsToFields(SearchIndex index)
   {
      index.Fields.First(f => f.Name == "category").SynonymMapNames.Add("desc-synonymmap");
      index.Fields.First(f => f.Name == "tags").SynonymMapNames.Add("desc-synonymmap");
      return index;
   }
   ```

   Al agregar un mapa de sinónimos, no son necesarias las recompilaciones de índices. Puede agregar un mapa de sinónimos a su servicio y luego modificar las definiciones de campo existentes en los índices para usar el nuevo mapa de sinónimos. La adición de nuevos atributos no influye en la disponibilidad de los índices. Lo mismo se aplica a la deshabilitación de sinónimos para un campo. Simplemente establezca la propiedad `SynonymMapNames` en una lista vacía.

   ```csharp
   index.Fields.First(f => f.Name == "category").SynonymMapNames.Add("desc-synonymmap");
   ```

## <a name="after-queries"></a>Consultas "después de"

Después cargar el mapa de sinónimos y de actualizar el índice para que use el mapa de sinónimos, la segunda llamada a `RunQueriesWithNonExistentTermsInIndex` genera lo siguiente:

```
Search the entire index for the phrase "five star":

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the term 'internet':

Name: Fancy Stay        Category: Luxury        Tags: [pool, view, wifi, concierge]

Search the entire index for the terms 'economy' AND 'hotel':

Name: Roach Motel       Category: Budget        Tags: [motel, budget]
```

La primera consulta busca el documento a partir de la regla `five star=>luxury`. La segunda consulta amplía la búsqueda utilizando `internet,wifi` y la tercera usando `hotel, motel` y `economy,inexpensive=>budget` en la búsqueda de coincidencia en los documentos.

Agregar sinónimos cambia totalmente la experiencia de búsqueda. En este ejemplo, las consultas originales no devolvieron resultados significativos, aunque los documentos en nuestro índice eran relevantes. Al habilitar sinónimos, podemos ampliar un índice para incluir los términos de uso común, sin realizar cambios en los datos subyacentes en el índice.

## <a name="clean-up-resources"></a>Limpieza de recursos

La manera más rápida de borrar el contenido después de un ejemplo es eliminar el grupo de recursos que contiene el servicio Azure Cognitive Search. Para eliminar de forma definitiva todo lo que contenga el grupo de recursos, elimine el grupo. En el portal, el nombre del grupo de recursos está en la página Información general de cada servicio Azure Cognitive Search.

## <a name="next-steps"></a>Pasos siguientes

En este ejemplo se ha mostrado la característica de sinónimos en código C# para crear y publicar reglas de asignación y, después, llamar a la asignación de sinónimos en una consulta. Puede encontrar información adicional en la documentación de referencia de [.NET SDK](/dotnet/api/overview/azure/search.documents-readme) y [API REST](/rest/api/searchservice/).

> [!div class="nextstepaction"]
> [Uso de sinónimos en Azure Cognitive Search](search-synonyms.md)