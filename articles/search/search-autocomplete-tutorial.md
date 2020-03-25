---
title: Incorporación de sugerencias y de la función de autocompletar en un cuadro de búsqueda
titleSuffix: Azure Cognitive Search
description: Para habilitar acciones de escritura automática en Azure Cognitive Search, puede crear proveedores de sugerencias y formular las solicitudes que rellenan en un cuadro de búsqueda con los términos o las frases completadas.
manager: nitinme
author: mrcarter8
ms.author: mcarter
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 64c4e65ca7b69c7d61c706b48591ac19be3bfcf5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "72792520"
---
# <a name="add-suggestions-or-autocomplete-to-your-azure-cognitive-search-application"></a>Incorporación de sugerencias o de la función de autocompletar a la aplicación de Azure Cognitive Search

En este artículo, aprenderá a usar [sugerencias](https://docs.microsoft.com/rest/api/searchservice/suggestions) y [la función de autocompletar](https://docs.microsoft.com/rest/api/searchservice/autocomplete) para crear un cuadro de búsqueda eficaz que admita los comportamientos de búsqueda a medida que se escribe.

+ Las *sugerencias* son los resultados sugeridos generados mientras se escribe, donde cada sugerencia es un resultado único del índice que coincide con lo que ha escrito hasta ahora. 

+ La función de *autocompletar* "termina" la palabra o frase que un usuario está escribiendo actualmente. En lugar de devolver los resultados, completa una consulta, que, a continuación, puede ejecutar para devolver resultados. Al igual que con las sugerencias, una palabra o una frase completada en una consulta se basa en una coincidencia en el índice. El servicio no ofrecerá las consultas que devuelvan cero da resultados en el índice.

Puede descargar y ejecutar el código de ejemplo en **DotNetHowToAutocomplete** para evaluar estas características. El código de ejemplo tiene como destino un índice creado previamente y rellenado con los [datos de demostración de NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs). El índice NYCJobs contiene una [construcción del proveedor de sugerencias](index-add-suggesters.md), lo que es un requisito para el uso de sugerencias o la función de autocompletar. Puede usar el índice preparado hospedado en un servicio de espacio aislado o bien [rellenar su propio índice](#configure-app) con un cargador de datos en la solución de muestra NYCJobs. 

La muestra **DotNetHowToAutocomplete** demuestra tanto las sugerencias como la función de autocompletar, en las versiones de lenguaje de C# y de JavaScript. Los programadores de C# pueden ir avanzando mediante una aplicación basada en MVC de ASP.NET que usa el [SDK para .NET de Azure Cognitive Search](https://aka.ms/search-sdk). La lógica para realizar llamadas de consultas sugeridas y de la función de autocompletar puede encontrarse en el archivo HomeController.cs. Los programadores de JavaScript encontrarán la lógica de consulta equivalente en IndexJavaScript.cshtml, lo que incluye llamadas directas a la [API REST de Azure Cognitive Search](https://docs.microsoft.com/rest/api/searchservice/). 

Para ambas versiones de idioma, la experiencia del usuario de front-end se basa en las bibliotecas de la [interfaz de usuario de jQuery](https://jqueryui.com/autocomplete/) y [XDSoft](https://xdsoft.net/jqplugins/autocomplete/). Usamos estas bibliotecas para crear el cuadro de búsqueda que admite tanto las sugerencias como la función de autocompletar. Las entradas que se recopilan en el cuadro de búsqueda se emparejan con las acciones de autocompletar y las sugerencias, como las que se definen en HomeController.cs o IndexJavaScript.cshtml.

En este ejercicio se explican las tareas siguientes:

> [!div class="checklist"]
> * Implementar un cuadro de entrada de búsqueda en JavaScript y emitir solicitudes para las coincidencias sugeridas o los términos autocompletados
> * En C#, definir las sugerencias y las acciones de autocompletar en HomeController.cs
> * En JavaScript, llamar a las API REST directamente para proporcionar la misma funcionalidad

## <a name="prerequisites"></a>Prerequisites

Un servicio Azure Cognitive Search es opcional para este ejercicio porque la solución utiliza un servicio de espacio aislado en vivo que hospeda un índice de demostración de NYCJobs preparado. Si desea ejecutar este ejemplo en su propio servicio de búsqueda, consulte en el [índice de configuración de trabajos de NYC](#configure-app) las instrucciones.

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/), cualquier edición. Se han probado código de ejemplo e instrucciones en la edición Community Edition gratuita.

* Descargue la [muestra DotNetHowToAutoComplete](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowToAutocomplete).

La muestra es muy completa: abarca sugerencias, la función de autocompletar, la navegación por facetas y el almacenamiento en caché del lado cliente. Revise el archivo Léame y los comentarios para obtener una descripción completa de lo que ofrece.

## <a name="run-the-sample"></a>Ejecución del ejemplo

1. Abra el archivo **AutocompleteTutorial.sln** en Visual Studio. La solución contiene un proyecto MVC de ASP.NET con una conexión con el índice de la demostración de trabajos de NYC.

2. Presione F5 para ejecutar el proyecto y cargar la página en el explorador que prefiera.

En la parte superior, verá una opción para seleccionar C# o JavaScript. La opción de C# llama a HomeController desde el explorador y usa el SDK de Azure Cognitive Search para .NET para recuperar los resultados. 

La opción de JavaScript llama a la API REST de Azure Cognitive Search directamente desde el explorador. Por lo general, esta opción tendrá un rendimiento considerablemente mejor porque quita el controlador del flujo. Puede elegir la opción que mejor se adapte a sus necesidades y a sus preferencias de lenguaje. En la página aparecen varios ejemplos de la función Autocompletar con ayuda para cada uno de ellos. En cada ejemplo hay texto de ejemplo recomendado que puede probar.  

Para probar, empiece a escribir algunas letras en cada cuadro de búsqueda para ver lo que sucede.

## <a name="search-box"></a>Cuadro de búsqueda

En las versiones tanto de JavaScript como de C#, la implementación del cuadro de búsqueda es exactamente la misma. 

Abra el archivo **Index.cshtml** en la carpeta \Views\Home para ver el código:

```html
<input class="searchBox" type="text" id="example1a" placeholder="search">
```

Este ejemplo es un cuadro de texto de entrada simple con una clase para establecer el estilo y un identificador al que JavaScript hará referencia, además de un texto de marcador de posición.  La magia está en el código JavaScript incrustado.

El ejemplo del lenguaje C# usa JavaScript en Index.cshtml para aprovechar la [biblioteca de la función de autocompletar de la interfaz de usuario de jQuery](https://jqueryui.com/autocomplete/). Esta biblioteca agrega la experiencia de la función de autocompletar al cuadro de búsqueda; para ello realiza llamadas asincrónicas al controlador de MVC para recuperar las sugerencias. La versión del lenguaje JavaScript está en IndexJavaScript.cshtml. Incluye el siguiente script para la barra de búsqueda, así como llamadas de la API REST a Azure Cognitive Search.

Veamos el código de JavaScript para el primer ejemplo, que llama a la función Autocomplete de la interfaz de usuario de jQuery y pasa una solicitud para obtener sugerencias:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: "/home/suggest?highlights=false&fuzzy=false&",
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

El código anterior se ejecuta en el explorador al cargar la página para configurar la función de autocompletar de la interfaz de usuario de jQuery para el cuadro de entrada de "example1a".  `minLength: 3` garantiza que la recomendación solo se mostrará cuando haya al menos tres caracteres en el cuadro de búsqueda.  El valor de origen es importante:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

La línea anterior indica a la función Autocomplete de la interfaz de usuario de jQuery dónde obtener la lista de elementos para mostrar debajo del cuadro de búsqueda. Puesto que este es un proyecto de MVC, llama a la función Suggest en HomeController.cs, que contiene la lógica para devolver las sugerencias de consulta (más información acerca de Suggest en la sección siguiente). Esta función también pasa algunos parámetros para controlar aspectos destacados, una coincidencia aproximada o un término. La API de JavaScript de la función Autocompletar agrega el parámetro de término.

### <a name="extending-the-sample-to-support-fuzzy-matching"></a>Ampliación del ejemplo para admitir la coincidencia aproximada

La búsqueda aproximada permite obtener resultados basados en coincidencias cercanas incluso si el usuario escribe incorrectamente una palabra en el cuadro de búsqueda. Aunque no es necesario, mejora considerablemente la solidez de una experiencia de escritura anticipada. Para probar esto, cambiemos la línea de asunto para habilitar la coincidencia aproximada.

Cambie la línea siguiente de esto:

```javascript
source: "/home/suggest?highlights=false&fuzzy=false&",
```

a esto:

```javascript
source: "/home/suggest?highlights=false&fuzzy=true&",
```

Presione F5 para iniciar la aplicación.

Pruebe escribir algo como "ejecativo" y observe cómo aparecen resultados para "ejecutivo", incluso si no son coincidencia exacta de las letras que escribió.

### <a name="jquery-autocomplete--backed-by-azure-cognitive-search-autocomplete"></a>Autocomplete de jQuery respaldada por Autocomplete de Azure Cognitive Search

Hasta ahora, la búsqueda de código de la experiencia del usuario se ha centrado en las sugerencias. El bloque de código siguiente muestra la función Autocomplete de la interfaz de usuario de jQuery (línea 91 de index.cshtml) y pasa una solicitud de Autocomplete de Azure Cognitive Search:

```javascript
$(function () {
    // using modified jQuery Autocomplete plugin v1.2.6 https://xdsoft.net/jqplugins/autocomplete/
    // $.autocomplete -> $.autocompleteInline
    $("#example2").autocompleteInline({
        appendMethod: "replace",
        source: [
            function (text, add) {
                if (!text) {
                    return;
                }

                $.getJSON("/home/autocomplete?term=" + text, function (data) {
                    if (data && data.length > 0) {
                        currentSuggestion2 = data[0];
                        add(data);
                    }
                });
            }
        ]
    });

    // complete on TAB and clear on ESC
    $("#example2").keydown(function (evt) {
        if (evt.keyCode === 9 /* TAB */ && currentSuggestion2) {
            $("#example2").val(currentSuggestion2);
            return false;
        } else if (evt.keyCode === 27 /* ESC */) {
            currentSuggestion2 = "";
            $("#example2").val("");
        }
    });
});
```

## <a name="c-example"></a>Ejemplo de C#

Ahora que ya revisamos el código de JavaScript de la página web, veamos el código de control del lado servidor de C# que recupera realmente las coincidencias sugeridas mediante el SDK para .NET de Azure Cognitive Search.

Abra el archivo **HomeController.cs** en el directorio Controllers (Controladores). 

Lo primero que podría apreciar es un método en la parte superior de la clase con el nombre `InitSearch`. Este método crea un cliente del índice HTTP autenticado en el servicio Azure Cognitive Search. Para más información, consulte [Cómo usar Azure Cognitive Search desde una aplicación .NET](https://docs.microsoft.com/azure/search/search-howto-dotnet-sdk).

En la línea 41, observe la función Suggest. Se basa en el método [DocumentsOperationsExtensions.Suggest](/dotnet/api/microsoft.azure.search.documentsoperationsextensions.suggest?view=azure-dotnet).

```csharp
public ActionResult Suggest(bool highlights, bool fuzzy, string term)
{
    InitSearch();

    // Call suggest API and return results
    SuggestParameters sp = new SuggestParameters()
    {
        UseFuzzyMatching = fuzzy,
        Top = 5
    };

    if (highlights)
    {
        sp.HighlightPreTag = "<b>";
        sp.HighlightPostTag = "</b>";
    }

    DocumentSuggestResult resp = _indexClient.Documents.Suggest(term, "sg", sp);

    // Convert the suggest query results to a list that can be displayed in the client.
    List<string> suggestions = resp.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = suggestions
    };
}
```

La función Sugerir toma dos parámetros que determinar si se devuelven resaltados de aciertos o si se usa la coincidencia aproximada además de la entrada del término de búsqueda. El método crea un [objeto SuggestParameters](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.suggestparameters?view=azure-dotnet) que luego se pasa a la API de Suggest. Luego el resultado se convierte en JSON para poder mostrarlo en el cliente.

En la línea 69, observe la función Autocomplete. Se basa en el método [DocumentsOperationsExtensions.Autocomplete](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.autocomplete?view=azure-dotnet).

```csharp
public ActionResult AutoComplete(string term)
{
    InitSearch();
    //Call autocomplete API and return results
    AutocompleteParameters ap = new AutocompleteParameters()
    {
        AutocompleteMode = AutocompleteMode.OneTermWithContext,
        UseFuzzyMatching = false,
        Top = 5
    };
    AutocompleteResult autocompleteResult = _indexClient.Documents.Autocomplete(term, "sg", ap);

    // Conver the Suggest results to a list that can be displayed in the client.
    List<string> autocomplete = autocompleteResult.Results.Select(x => x.Text).ToList();
    return new JsonResult
    {
        JsonRequestBehavior = JsonRequestBehavior.AllowGet,
        Data = autocomplete
    };
}
```

La función Autocomplete toma la entrada del término de búsqueda. El método crea un [objeto AutoCompleteParameters](https://docs.microsoft.com/rest/api/searchservice/autocomplete). Luego el resultado se convierte en JSON para poder mostrarlo en el cliente.

(Opcional) Agregue un punto de interrupción al inicio de la función Sugerir y recorra el código. Observe la respuesta que devuelve el SDK y cómo se convierte al resultado que el método devuelve.

Los otros ejemplos de la página siguen el mismo patrón para agregar el resaltado de los aciertos y las facetas para admitir el almacenamiento en caché del lado cliente de los resultados de la función de autocompletar. Revise cada uno de estos elementos para saber cómo funcionan y cómo puede aprovecharlos en la experiencia de búsqueda.

## <a name="javascript-example"></a>Ejemplo de JavaScript

Una implementación de Javascript de la función de autocompletar y sugerencias llama a la API REST con un identificador URI como el origen para especificar el índice y la operación. 

Para revisar la implementación de JavaScript, abra **IndexJavaScript.cshtml**. Tenga en cuenta que también se usa la función Autocomplete de la interfaz de usuario de jQuery para el cuadro de búsqueda, se recopilan las entradas de los términos de búsqueda y se realizan llamadas asincrónicas a Azure Cognitive Search para recuperar las coincidencias sugeridas o los términos completados. 

Veamos el código de JavaScript del primer ejemplo:

```javascript
$(function () {
    $("#example1a").autocomplete({
        source: function (request, response) {
        $.ajax({
            type: "POST",
            url: suggestUri,
            dataType: "json",
            headers: {
                "api-key": searchServiceApiKey,
                "Content-Type": "application/json"
            },
            data: JSON.stringify({
                top: 5,
                fuzzy: false,
                suggesterName: "sg",
                search: request.term
            }),
                success: function (data) {
                    if (data.value && data.value.length > 0) {
                        response(data.value.map(x => x["@@search.text"]));
                    }
                }
            });
        },
        minLength: 3,
        position: {
            my: "left top",
            at: "left-23 bottom+10"
        }
    });
});
```

Si compara este ejemplo con el anterior en el que se llama al controlador de inicio, verá varias similitudes.  La configuración de la función Autocompletar para `minLength` y `position` es exactamente igual. 

Aquí el cambio importante es el origen. En lugar de llamar al método Suggest en el controlador de inicio, se crea una solicitud REST en una función de JavaScript y se ejecuta mediante Ajax. Luego la respuesta se procesa en estado "correcto" y se usa como el origen.

Las llamadas de REST utilizan identificadores URI para especificar si se realiza una llamada a la API [Autocomplete](https://docs.microsoft.com/rest/api/searchservice/autocomplete) o [Suggestions](https://docs.microsoft.com/rest/api/searchservice/suggestions). Los siguientes URI están en las líneas 9 y 10, respectivamente.

```javascript
var suggestUri = "https://" + searchServiceName + ".search.windows.net/indexes/" + indexName + "/docs/suggest?api-version=" + apiVersion;
var autocompleteUri = "https://" + searchServiceName + ".search.windows.net/indexes/" + indexName + "/docs/autocomplete?api-version=" + apiVersion;
```

En la línea 148, encontrará un script que llama al `autocompleteUri`. La primera llamada a `suggestUri` está en la línea 39.

> [!Note]
> La realización de llamadas de REST al servicio en JavaScript se ofrece aquí como una demostración práctica de la API REST, pero no debe interpretarse como un procedimiento recomendado ni como una recomendación. La inclusión de una clave de API y el punto de conexión en un script abre el servicio a los ataques de denegación de servicio a cualquier persona que pueda leer esos valores fuera del script. Aunque es seguro usar JavaScript con fines de aprendizaje, en los índices que se hospedan en el servicio gratuito, se recomienda usar Java o C# para las operaciones de indexación y consulta del código de producción. 

<a name="configure-app"></a>

## <a name="configure-nycjobs-to-run-on-your-service"></a>Configuración de NYCJobs para ejecutarse en el servicio

Hasta ahora, ha estado utilizando el índice de la demostración NYCJobs hospedado. Si desea la visibilidad completa de todo el código, incluido el índice, siga estas instrucciones para crear y cargar el índice en su propio servicio de búsqueda.

1. [Cree un servicio Azure Cognitive Search](search-create-service-portal.md) o [busque uno existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este ejemplo. 

   > [!Note]
   > Si va a usar el servicio gratuito Azure Cognitive Search, está limitado a tres índices. El cargador de datos de NYCJobs crea dos índices. Asegúrese de que haya espacio en el servicio para aceptar los nuevos índices.

1. Descargue el código de ejemplo [NYCJobs](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

1. En la carpeta DataLoader del código de ejemplo NYCJobs, abra el archivo **DataLoader.sln** en Visual Studio.

1. Agregue la información de la conexión para el servicio Azure Cognitive Search. Abra el archivo App.config dentro del proyecto DataLoader y cambie los valores de appSettings TargetSearchServiceName y TargetSearchServiceApiKey para reflejar el servicio Azure Cognitive Search y la clave de API del servicio Azure Cognitive Search. Esta información puede encontrarse en Azure Portal.

1. Presione F5 para iniciar la aplicación, crear dos índices e importar los datos de muestra de NYCJob.

1. Abra **AutocompleteTutorial.sln** y edite el archivo Web.config en el proyecto **AutocompleteTutorial**. Cambie los valores SearchServiceName y SearchServiceApiKey por otros válidos para el servicio de búsqueda.

1. Presione F5 para ejecutar la aplicación. Se abre la aplicación web de ejemplo en el explorador predeterminado. La experiencia es idéntica a la versión de espacio aislado, solo el índice y los datos se hospedan en su servicio.

## <a name="next-steps"></a>Pasos siguientes

En este ejemplo se muestran los pasos básicos para crear un cuadro de búsqueda que admita las sugerencias y la función de autocompletar. Se explicó cómo crear una aplicación MVC de ASP.NET y usar la API REST o el SDK de Azure Cognitive Search para .NET a fin de recuperar las sugerencias.

En el siguiente paso, intente integrar las sugerencias y la función de autocompletar en la experiencia de búsqueda. Los siguientes artículos de referencia deberían serle de ayuda.

> [!div class="nextstepaction"]
> [Autocomplete REST API (API REST de la función Autocompletar)](https://docs.microsoft.com/rest/api/searchservice/autocomplete)
> [Suggestions REST API (API REST de Sugerencias)](https://docs.microsoft.com/rest/api/searchservice/suggestions)
> [Facets index attribute on a Create Index REST API (Atributo de índice de facetas en una API REST de creación de índice)](https://docs.microsoft.com/rest/api/searchservice/create-index)

