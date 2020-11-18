---
title: Creación de un proveedor de sugerencias
titleSuffix: Azure Cognitive Search
description: Para habilitar acciones de escritura automática en Azure Cognitive Search, puede crear proveedores de sugerencias y formular las solicitudes que invocan los términos de consulta de sugerencias automáticas o de autocompletar.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/10/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: 498934c01970b296c1491e7ccd36ad947324306a
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94445343"
---
# <a name="create-a-suggester-to-enable-autocomplete-and-suggested-results-in-a-query"></a>Creación de un proveedor de sugerencias para habilitar la función autocompletar y los resultados sugeridos en una consulta

En Azure Cognitive Search, la función de "búsqueda al escribir" se habilita mediante una construcción de **proveedor de sugerencias** que se agrega a un [índice de búsqueda](search-what-is-an-index.md). Un proveedor de sugerencias admite dos experiencias: *autocompletar*, que completa una entrada parcial para una consulta de un término completo, y *sugerencias*, que invita a hacer clic en una coincidencia concretar. Autocompletar genera una consulta. Las sugerencias producen un documento coincidente.

En la siguiente captura de pantalla del artículo [Creación de la primera aplicación en C#](tutorial-csharp-type-ahead-and-suggestions.md), se muestran ambas experiencias. Autocompletar anticipa un posible término, agregando a "met" la terminación "ro" por ejemplo. Las sugerencias son resultados de búsquedas en miniatura, donde un campo como el nombre de un hotel representa un documento de búsqueda de hoteles coincidentes del índice. Para obtener sugerencias, puede mostrar cualquier campo que proporcione información descriptiva.

![Comparación visual de Autocompletar y consultas sugeridas](./media/index-add-suggesters/hotel-app-suggestions-autocomplete.png "Comparación visual de Autocompletar y consultas sugeridas")

Puede utilizar estas características de forma conjunta o por separado. Para implementar estos comportamientos en Azure Cognitive Search, hay un componente de consulta e índice. 

+ Agregue un proveedor de sugerencias a una definición de índice de búsqueda. El resto de este artículo se centra en la creación de un proveedor de sugerencias.

+ Llame a una consulta habilitada para el proveedor de sugerencias, en forma de una solicitud de Sugerencia o solicitud de Autocompletar, mediante una de las [API que se enumeran a continuación](#how-to-use-a-suggester).

La compatibilidad con la búsqueda al escribir está habilitada por cada campo para los campos de cadena. Puede implementar ambos comportamientos de escritura automática dentro de la misma solución de búsqueda si desea una experiencia similar a la indicada en la captura de pantalla. El destino de ambas solicitudes son la colección de *documentos* de índice específico y las respuestas se devuelven después de que un usuario haya proporcionado al menos una cadena de entrada de tres caracteres.

## <a name="what-is-a-suggester"></a>¿Qué es un proveedor de sugerencias?

Un proveedor de sugerencias es una estructura de datos interna que admite comportamientos de búsqueda mientras se escribe mediante el almacenamiento de prefijos para buscar coincidencias en consultas parciales. Igual que sucede con los términos tokenizados, los prefijos se almacenan en índices invertidos, uno para cada campo especificado en una colección de campos del proveedor de sugerencias.

## <a name="how-to-create-a-suggester"></a>Creación de un proveedor de sugerencias

Para crear un proveedor de sugerencias, agregue uno a una [definición de índice](/rest/api/searchservice/create-index). Un proveedor de sugerencias obtiene un nombre y una colección de campos en los que está habilitada la experiencia de escritura automática. Y [establezca cada propiedad](#property-reference). El mejor momento para crear un proveedor de sugerencias es durante la definición del campo que va a usarlo.

+ Usar solo campos de cadena

+ Usar el analizador Lucene estándar predeterminado (`"analyzer": null`) o un [analizador de idioma](index-add-language-analyzers.md) (por ejemplo, `"analyzer": "en.Microsoft"`) en el campo

Si intenta crearlo con campos ya existentes, la API no lo permitirá. Los prefijos se generan durante la indexación, cuando se tokenizan los términos parciales de dos o más combinaciones de caracteres junto con los términos completos. Dado que los campos existentes ya están acortados, tendrá que volver a generar el índice si quiere agregarlos a un proveedor de sugerencias. Para más información, consulte [Cómo recompilar un índice de Azure Cognitive Search](search-howto-reindex.md).

### <a name="choose-fields"></a>Elegir los campos

Aunque los proveedores de sugerencias tienen varias propiedades, son principalmente una colección de campos de cadena para los que va a habilitar la experiencia de búsqueda mientras se escribe. Hay un proveedor de sugerencias para cada índice, por lo que la lista de proveedores de sugerencias debe incluir todos los campos que contribuyen al contenido tanto de las sugerencias como de autocompletar.

La ventaja de autocompletar es que tiene un grupo de campos mayor, ya que el contenido adicional tiene más potencial de finalización de términos.

Por su parte, las sugerencias generan mejores resultados cuando la elección de campo es selectiva. Recuerde que la sugerencia es un proxy para un documento de búsqueda, por lo que deseará los campos que mejor se representen como un resultado individual. Los nombres, títulos u otros campos únicos que distinguen entre varias coincidencias son los que mejor funcionan. Si los campos se componen de valores repetitivos, las sugerencias consistirán en resultados idénticos y el usuario no sabrá en cuál hacer clic.

Para satisfacer las experiencias de buscar mientras se escribe, agregue todos los campos que necesita para autocompletar y, después, use **$select**, **$top**, **$filter** y **searchFields** para controlar los resultados de las sugerencias.

### <a name="choose-analyzers"></a>Elección de analizadores

La elección de un analizador determina el modo en que los campos se tokenizan y, posteriormente, se prefijan. Por ejemplo, para una cadena con guiones como "anti-virus", el uso de un analizador de idioma producirá estas combinaciones de tokens: "anti", "virus", "anti-virus". Si usa el analizador de Lucene estándar, la cadena con guiones no existirá. 

Al evaluar analizadores, considere la posibilidad de usar [Analyze Text API](/rest/api/searchservice/test-analyzer) para obtener información sobre cómo se procesan los términos. Una vez creado un índice, puede probar varios analizadores en una cadena para ver la salida de los tokens.

Los campos que utilizan [analizadores personalizados](index-add-custom-analyzers.md) o [predefinidos](index-add-custom-analyzers.md#predefined-analyzers-reference) (con la excepción del estándar de Lucene) no se permiten para evitar resultados deficientes.

> [!NOTE]
> Si necesita solucionar la restricción del analizador, por ejemplo si necesita una palabra clave o un analizador de n-gramas para ciertos escenarios de consulta, debería usar dos campos independientes para el mismo contenido. De esta forma, uno de los campos tendrá un proveedor de sugerencias y el otro se puede configurar con una configuración de analizador personalizada.

## <a name="create-using-the-portal"></a>Creación con el portal

Al usar el asistente para **agregar un índice** o para **importar datos** para crear un índice, tiene la opción de habilitar un proveedor de sugerencias:

1. En la definición del índice, escriba un nombre para el proveedor de sugerencias.

1. En cada definición de campo de los nuevos campos, active una casilla en la columna Proveedor de sugerencias. Solo los campos de cadena tienen disponible una casilla. 

Como se indicó anteriormente, la elección del analizador afecta a la tokenización y al agregado de prefijos. Considere la definición de campo completa al habilitar los proveedores de sugerencias. 

## <a name="create-using-rest"></a>Creación mediante REST

En la API REST, puede agregar proveedores de sugerencias con [Create Index](/rest/api/searchservice/create-index) o [Update Index](/rest/api/searchservice/update-index). 

  ```json
  {
    "name": "hotels-sample-index",
    "fields": [
      . . .
          {
              "name": "HotelName",
              "type": "Edm.String",
              "facetable": false,
              "filterable": false,
              "key": false,
              "retrievable": true,
              "searchable": true,
              "sortable": false,
              "analyzer": "en.microsoft",
              "indexAnalyzer": null,
              "searchAnalyzer": null,
              "synonymMaps": [],
              "fields": []
          },
    ],
    "suggesters": [
      {
        "name": "sg",
        "searchMode": "analyzingInfixMatching",
        "sourceFields": ["HotelName"]
      }
    ],
    "scoringProfiles": [
      . . .
    ]
  }
  ```

## <a name="create-using-net"></a>Creación mediante .NET

En C#, defina un [objeto SearchSuggester](/dotnet/api/azure.search.documents.indexes.models.searchsuggester). `Suggesters` es una colección de un objeto SearchIndex, pero solo puede tomar un elemento. 

```csharp
private static void CreateIndex(string indexName, SearchIndexClient indexClient)
{
    FieldBuilder fieldBuilder = new FieldBuilder();
    var searchFields = fieldBuilder.Build(typeof(Hotel));

    //var suggester = new SearchSuggester("sg", sourceFields = "HotelName", "Category");

    var definition = new SearchIndex(indexName, searchFields);

    var suggester = new SearchSuggester("sg", new[] { "HotelName", "Category"});

    definition.Suggesters.Add(suggester);

    indexClient.CreateOrUpdateIndex(definition);
}
```

## <a name="property-reference"></a>Referencia de propiedades

|Propiedad      |Descripción      |
|--------------|-----------------|
|`name`        | Se especifica en la definición del proveedor de sugerencias, pero también se la llama en una solicitud Autocompletar o Sugerencias. |
|`sourceFields`| Se especifica en la definición del proveedor de sugerencias. Es una lista de uno o más campos en el índice que son el origen del contenido para obtener sugerencias. Los campos deben ser de tipo `Edm.String` y `Collection(Edm.String)`. Si se especifica un analizador en el campo, debe ser un analizador léxico con nombre de [esta lista](/dotnet/api/azure.search.documents.indexes.models.lexicalanalyzername) (no un analizador personalizado).<p/> Como procedimiento recomendado, especifique solo los campos que se prestan a una respuesta adecuada y esperada, ya sea una cadena completa en una barra de búsqueda o una lista desplegable.<p/>Un nombre de hotel es buen candidato porque tiene precisión. Los campos detallados, como las descripciones y los comentarios, son demasiado densos. De forma similar, los campos repetitivos, como las categorías y las etiquetas, son menos eficaces. En los ejemplos, se incluye "categoría" de todos modos para demostrar que puede incluir varios campos. |
|`searchMode`  | Parámetro solo de REST, pero también visible en el portal. Este parámetro no está disponible en el SDK de .NET. Indica la estrategia que se usa para buscar las frases candidatas. El único modo que se admite actualmente es `analyzingInfixMatching`, que actualmente coincide en el principio de un término.|

<a name="how-to-use-a-suggester"></a>

## <a name="use-a-suggester"></a>Uso de un proveedor de sugerencias

El proveedor de sugerencias se utiliza en una consulta. Después de que se crea un proveedor de sugerencias, llame a una de los siguientes API para una experiencia de buscar mientras se escribe:

+ [API REST de sugerencias](/rest/api/searchservice/suggestions)
+ [API REST de autocompletar](/rest/api/searchservice/autocomplete)
+ [Método SuggestAsync](/dotnet/api/azure.search.documents.searchclient.suggestasync)
+ [Método AutocompleteAsync](/dotnet/api/azure.search.documents.searchclient.autocompleteasync)

En una aplicación de búsqueda, el código de cliente debería aprovechar una biblioteca como [jQuery UI Autocomplete](https://jqueryui.com/autocomplete/) para recopilar la consulta parcial y ofrecer la coincidencia. Para más información acerca de esta tarea, consulte [Incorporación de sugerencias o de la función de autocompletar al código de cliente](search-autocomplete-tutorial.md).

El uso de la API se ilustra en la siguiente llamada a la API REST de autocompletar. De este ejemplo se pueden extraer dos conclusiones: En primer lugar, al igual que en todas las consultas, la operación se realiza con una colección de documentos de un índice y la consulta incluye un parámetro **search**, que en este caso proporciona la consulta parcial. En segundo lugar, debe agregar **suggesterName** a la solicitud. Si no se define un proveedor de sugerencias en el índice, se producirá un error en la llamada a autocompletar o sugerencias.

```http
POST /indexes/myxboxgames/docs/autocomplete?search&api-version=2020-06-30
{
  "search": "minecraf",
  "suggesterName": "sg"
}
```

## <a name="sample-code"></a>Código de ejemplo

+ En el ejemplo de [Creación de la primera aplicación en C# (lección 3: Incorporación de buscar mientras se escribe)](tutorial-csharp-type-ahead-and-suggestions.md) se muestran consultas sugeridas, autocompletar y navegación por facetas. Este ejemplo de código se ejecuta en un servicio de espacio aislado de Azure Cognitive Search y se usa un índice de hoteles cargado previamente con un proveedor de sugerencias ya creado, por lo que todo lo que tiene que hacer es presionar F5 para ejecutar la aplicación. No se necesita ninguna suscripción o inicio de sesión.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo se solicita la formulación, se recomienda el siguiente artículo.

> [!div class="nextstepaction"]
> [Incorporación de sugerencias y de la función de autocompletar al código de cliente](search-autocomplete-tutorial.md)