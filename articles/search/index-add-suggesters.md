---
title: Incorporación de consultas de escritura automática a un índice
titleSuffix: Azure Cognitive Search
description: Para habilitar acciones de escritura automática en Azure Cognitive Search, puede crear proveedores de sugerencias y formular las solicitudes que invocan los términos de consulta de sugerencias automáticas o de autocompletar.
manager: nitinme
author: Brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: a312068d5c8c574e7b069263cf37e3b855810e4b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "72790101"
---
# <a name="add-suggesters-to-an-index-for-typeahead-in-azure-cognitive-search"></a>Incorporación de proveedores de sugerencias a un índice de escritura automática en Azure Cognitive Search

En Azure Cognitive Search, la funcionalidad de escritura anticipada o "buscar mientras escribe" se basa en una construcción de **proveedor de sugerencias** que se agrega a un [índice de búsqueda](search-what-is-an-index.md). Se trata de una lista de uno o más campos en los que quiere habilitar la escritura anticipada.

Un proveedor de sugerencias admite dos variantes de escritura anticipada: *autocompletar*, que completa el término o la frase que escribe, y *sugerencias*, que devuelve una corta lista de documentos coincidentes.  

En la siguiente captura de pantalla, en el ejemplo [Creación de la primera aplicación en C#](tutorial-csharp-type-ahead-and-suggestions.md), se ilustra la escritura anticipada. Autocompletar anticipa lo que el usuario podría escribir en el cuadro de búsqueda. La entrada real es "gem", que autocompletar termina con "elo". Esta entrada se resuelve como "gemelo" en cuanto posible término de búsqueda. Las sugerencias se visualizan en la lista desplegable. Para obtener sugerencias, puede exponer cualquier parte de un documento que mejor describa el resultado. En este ejemplo, las sugerencias son nombres de hoteles. 

![Comparación visual de Autocompletar y consultas sugeridas](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Comparación visual de Autocompletar y consultas sugeridas")

Para implementar estos comportamientos en Azure Cognitive Search, hay un componente de consulta e índice. 

+ En el índice, agregue un proveedor de sugerencias a un índice. Puede usar el portal, la [API REST](https://docs.microsoft.com/rest/api/searchservice/create-index) o el [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). El resto de este artículo se centra en la creación de un proveedor de sugerencias. 

+ En la solicitud de consulta, llame a una de las [API enumeradas a continuación](#how-to-use-a-suggester).

La compatibilidad de búsqueda como al escribir está habilitada por cada campo. Puede implementar ambos comportamientos de escritura automática dentro de la misma solución de búsqueda si desea una experiencia similar a la indicada en la captura de pantalla. El destino de ambas solicitudes son la colección de *documentos* de índice específico y las respuestas se devuelven después de que un usuario haya proporcionado al menos una cadena de entrada de tres caracteres.

## <a name="create-a-suggester"></a>Creación de un proveedor de sugerencias

Aunque un proveedor de sugerencias tiene varias propiedades, es principalmente una colección de campos para los que va a habilitar la experiencia de escritura automática. Por ejemplo, una aplicación de viajes desea habilitar la búsqueda con escritura anticipada de destinos, ciudades y atracciones. Por lo tanto, estos tres campos deberían estar en la colección de campos.

Para crear un proveedor de sugerencias, agregue uno a un esquema de índice. Puede tener un solo proveedor de sugerencias para cada índice (en concreto, un proveedor de sugerencias de la colección de proveedores de sugerencias). 

### <a name="when-to-create-a-suggester"></a>Cuándo crear un proveedor de sugerencias

El mejor momento para crear un proveedor de sugerencias es durante la creación de la propia definición de campo.

Si intenta crearlo con campos ya existentes, la API no lo permitirá. El texto de escritura anticipada se crea durante la indexación, cuando se acortan los términos parciales de dos o más combinaciones de caracteres junto con los términos completos. Dado que los campos existentes ya están acortados, tendrá que volver a generar el índice si quiere agregarlos a un proveedor de sugerencias. Para más información sobre la indexación, consulte [Cómo recompilar un índice de Azure Cognitive Search](search-howto-reindex.md).

### <a name="create-using-the-rest-api"></a>Creación del proveedor de sugerencias mediante la API REST

En la API REST, puede agregar proveedores de sugerencias con [Create Index](https://docs.microsoft.com/rest/api/searchservice/create-index) o [Update Index](https://docs.microsoft.com/rest/api/searchservice/update-index). 

  ```json
  {
    "name": "hotels",
    "fields": [
      . . .
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["hotelName", "category"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```


### <a name="create-using-the-net-sdk"></a>Creación del proveedor de sugerencias mediante el SDK de .NET

En C#, defina un [objeto Suggester](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggester?view=azure-dotnet). `Suggesters` es una colección, pero puede tomar solamente un elemento. 

```csharp
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
        Fields = FieldBuilder.BuildForType<Hotel>(),
        Suggesters = new List<Suggester>() {new Suggester()
            {
                Name = "sg",
                SourceFields = new string[] { "HotelId", "Category" }
            }}
    };

    serviceClient.Indexes.Create(definition);

}
```

### <a name="property-reference"></a>Referencia de propiedades

|Propiedad      |Descripción      |
|--------------|-----------------|
|`name`        |El nombre del proveedor de sugerencias.|
|`searchMode`  |La estrategia que se usa para buscar las frases candidatas. El único modo que se admite actualmente es `analyzingInfixMatching`, que establece una correspondencia flexible de frases al principio o en medio de las oraciones.|
|`sourceFields`|Una lista de uno o más campos que son el origen del contenido para obtener sugerencias. Los campos deben ser de tipo `Edm.String` y `Collection(Edm.String)`. Si se especifica un analizador en el campo, debe ser un analizador con nombre de [esta lista](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername?view=azure-dotnet) (no un analizador personalizado).<p/>Como procedimiento recomendado, especifique solo los campos que se prestan a una respuesta adecuada y esperada, ya sea una cadena completa en una barra de búsqueda o una lista desplegable.<p/>Un nombre de hotel es buen candidato porque tiene precisión. Los campos detallados, como las descripciones y los comentarios, son demasiado densos. De forma similar, los campos repetitivos, como las categorías y las etiquetas, son menos eficaces. En los ejemplos, se incluye "categoría" de todos modos para demostrar que puede incluir varios campos. |

### <a name="analyzer-restrictions-for-sourcefields-in-a-suggester"></a>Restricciones del analizador para sourceFields en un proveedor de sugerencias

Azure Cognitive Search analiza el contenido del campo para habilitar las consultas sobre términos individuales. Los proveedores de sugerencias requieren que se indexen los prefijos además de los términos completos, lo cual requiere un análisis adicional de los campos de origen. Las configuraciones de los analizadores personalizados pueden combinar cualquiera de los distintos tokenizadores y filtros, a menudo de formas que harían imposible la generación de los prefijos necesarios para las sugerencias. Por esta razón, Azure Cognitive Search impide que los campos con analizadores personalizados se incluyan en un proveedor de sugerencias.

> [!NOTE] 
>  Si necesita solucionar la limitación anterior, use dos campos distintos para el mismo contenido. De esta forma, uno de los campos tendrá un proveedor de sugerencias y el otro se puede configurar con una configuración de analizador personalizada.

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester-in-a-query"></a>Uso de un proveedor de sugerencias en una consulta

Después de crear un proveedor de sugerencias, llame a la API adecuada de la lógica de consultas para invocar la característica. 

+ [API REST de sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) 
+ [API REST de autocompletar](https://docs.microsoft.com/rest/api/searchservice/autocomplete) 
+ [Método SuggestWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.suggestwithhttpmessagesasync?view=azure-dotnet)
+ [Método AutocompleteWithHttpMessagesAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations.autocompletewithhttpmessagesasync?view=azure-dotnet&viewFallbackFrom=azure-dotnet)

El uso de la API se ilustra en la siguiente llamada a la API REST de autocompletar. De este ejemplo se pueden extraer dos conclusiones: En primer lugar, al igual que con todas las consultas, la operación se realiza en la colección de documentos de un índice. En segundo lugar, puede agregar parámetros de consulta. Aunque muchos parámetros de consulta son comunes a ambas API, la lista es diferente para cada una.

```http
GET https://[service name].search.windows.net/indexes/[index name]/docs/autocomplete?[query parameters]  
api-key: [admin or query key]
```

Si no se define un proveedor de sugerencias en el índice, se producirá un error en la llamada a autocompletar o sugerencias.

## <a name="sample-code"></a>Código de ejemplo

+ En el ejemplo de [Creación de la primera aplicación en C#](tutorial-csharp-type-ahead-and-suggestions.md) se muestra una construcción de proveedor de sugerencias, consultas sugeridas, autocompletar y navegación por facetas. Este ejemplo de código se ejecuta en un servicio de espacio aislado de Azure Cognitive Search y usa un índice de hoteles cargado previamente, por lo que todo lo que tiene que hacer es presionar F5 para ejecutar la aplicación. No se necesita ninguna suscripción o inicio de sesión.

+ [DotNetHowToAutocomplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete) es otro ejemplo más antiguo que contiene tanto código de C# como de Java. También se muestra una construcción de proveedor de sugerencias, consultas sugeridas, autocompletar y navegación por facetas. Este ejemplo de código usa los datos de ejemplo [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs) hospedados. 


## <a name="next-steps"></a>Pasos siguientes

Se recomienda el siguiente ejemplo para ver cómo se formulan las solicitudes.

> [!div class="nextstepaction"]
> [Ejemplos de sugerencias y de autocompletar](search-autocomplete-tutorial.md) 
