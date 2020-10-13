---
title: Tutorial de C# sobre el uso de facetas para facilitar la navegación
titleSuffix: Azure Cognitive Search
description: Continúe desde "Resultados de paginación" para agregar la navegación por facetas. Aprenda cómo usar facetas para restringir una búsqueda fácilmente.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 10/01/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: ab15af07c5f63d375d8fdb4fc38e0853e207a0be
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2020
ms.locfileid: "91667289"
---
# <a name="tutorial-add-faceted-navigation-using-the-net-sdk"></a>Tutorial: Incorporación de la navegación por facetas mediante el SDK de .NET

Las facetas permiten la navegación autodirigida, ya que proporcionan un conjunto de vínculos para filtrar los resultados. En este tutorial, se coloca una estructura de navegación por facetas en el lado izquierdo de la página, con etiquetas y texto en el que se puede hacer clic para recortar los resultados.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Establecer las propiedades del modelo en _IsFacetable_
> * Agregar navegación por facetas a la aplicación

## <a name="overview"></a>Información general

Las facetas se basan en los campos de un índice de búsqueda. Una solicitud de consulta que incluye facet=[cadena] proporciona el campo para la faceta. Es habitual incluir varias facetas, como `&facet=category&facet=amenities`, separadas cada una por un carácter de Y comercial (&). La implementación de una estructura de navegación por facetas requiere que especifique facetas y filtros. El filtro se usa en un evento de clic para restringir los resultados. Por ejemplo, al hacer clic en "budget" (presupuesto), se filtran los resultados en función de ese criterio.

En este tutorial se amplía el proyecto de paginación creado en el tutorial [Incorporación de la paginación a los resultados de búsqueda](tutorial-csharp-paging.md) tutorial.

En el proyecto siguiente se puede encontrar una versión finalizada del código de este tutorial:

* [4-add-facet-navigation (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/4-add-facet-navigation)

## <a name="prerequisites"></a>Prerrequisitos

* Solución [2a-add-paging (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/2a-add-paging). Este proyecto puede ser su propia versión, que se creó a partir del tutorial anterior o de una copia de GitHub.

Este tutorial se ha actualizado para usar el paquete [Azure.Search.Documents (versión 11)](https://www.nuget.org/packages/Azure.Search.Documents/). Para obtener una versión anterior del SDK de .NET, consulte el [código de ejemplo de Microsoft.Azure.Search (versión 10)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v10).

## <a name="set-model-properties-as-isfacetable"></a>Establecer las propiedades del modelo en IsFacetable

Para que una propiedad de modelo se encuentre en una búsqueda por facetas, deben estar etiquetada con **IsFacetable**.

1. Consulte la clase **Hotel**. Los objetos **Category** y **Tags**, por ejemplo, se etiquetan como **IsFacetable**, pero **HotelName** y **Description** no. 

    ```cs
    public partial class Hotel
    {
        [SimpleField(IsFilterable = true, IsKey = true)]
        public string HotelId { get; set; }

        [SearchableField(IsSortable = true)]
        public string HotelName { get; set; }

        [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.EnLucene)]
        public string Description { get; set; }

        [SearchableField(AnalyzerName = LexicalAnalyzerName.Values.FrLucene)]
        [JsonPropertyName("Description_fr")]
        public string DescriptionFr { get; set; }

        [SearchableField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
        public string Category { get; set; }

        [SearchableField(IsFilterable = true, IsFacetable = true)]
        public string[] Tags { get; set; }

        [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
        public bool? ParkingIncluded { get; set; }

        [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
        public DateTimeOffset? LastRenovationDate { get; set; }

        [SimpleField(IsFilterable = true, IsSortable = true, IsFacetable = true)]
        public double? Rating { get; set; }

        public Address Address { get; set; }

        [SimpleField(IsFilterable = true, IsSortable = true)]
        public GeographyPoint Location { get; set; }

        public Room[] Rooms { get; set; }
    }
    ```

1. Como parte de este tutorial, no se cambiará ninguna etiquete, por lo que debe cerrar el archivo hotel.cs sin cambiar nada.

    > [!Note]
    > Una búsqueda por facetas producirá un error si un campo solicitado en la búsqueda no se etiqueta correctamente.

## <a name="add-facet-navigation-to-your-app"></a>Agregar navegación por facetas a la aplicación

En este ejemplo, vamos a permitir al usuario seleccionar una categoría de un hotel o un servicio de las listas de vínculos que se muestran a la izquierda de los resultados. Para empezar, el usuario escribir el texto que se va a buscar y, después, restringirá progresivamente los resultados de la búsqueda mediante la selección de una categoría o servicio.

El trabajo de la controladora es pasar las listas de facetas a la vista. Para mantener las selecciones del usuario a medida que avanza la búsqueda, usamos el almacenamiento temporal como mecanismo para conservar los datos.

![Usar la navegación por facetas para limitar una búsqueda de "pool"](./media/tutorial-csharp-create-first-app/azure-search-facet-nav.png)

### <a name="add-filter-strings-to-the-searchdata-model"></a>Agregar cadenas de filtro al modelo SearchData

1. Abra el archivo SearchData.cs y agregue las propiedades de cadena a la clase **SearchData** para almacenar las cadenas de filtro de faceta.

    ```cs
    public string categoryFilter { get; set; }
    public string amenityFilter { get; set; }
    ```

### <a name="add-the-facet-action-method"></a>Agregar el método de acción de faceta

El controlador principal necesita una nueva acción, **Facet**, y actualizaciones tanto a sus acciones **Index** y **Page**existentes, como al método **RunQueryAsync** .

<!-- 1. Open the home controller file, and add the **using** statement, to enable the **List&lt;string&gt;** construct.

    ```cs
    using System.Collections.Generic; 
    ```-->

1. Reemplace el método de acción **Index(SearchData model)** .

    ```cs
    public async Task<ActionResult> Index(SearchData model)
    {
        try
        {
            // Ensure the search string is valid.
            if (model.searchText == null)
            {
                model.searchText = "";
            }

            // Make the search call for the first page.
            await RunQueryAsync(model, 0, 0, "", "").ConfigureAwait(false);
        }
        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "1" });
        }

        return View(model);
    }
    ```

1. Reemplace el método de acción **PageAsync(SearchData model)** .

    ```cs
    public async Task<ActionResult> PageAsync(SearchData model)
    {
        try
        {
            int page;

            // Calculate the page that should be displayed.
            switch (model.paging)
            {
                case "prev":
                    page = (int)TempData["page"] - 1;
                    break;

                case "next":
                    page = (int)TempData["page"] + 1;
                    break;

                default:
                    page = int.Parse(model.paging);
                    break;
            }

            // Recover the leftMostPage.
            int leftMostPage = (int)TempData["leftMostPage"];

            // Recover the filters.
            string catFilter = TempData["categoryFilter"].ToString();
            string ameFilter = TempData["amenityFilter"].ToString();

            // Recover the search text.
            model.searchText = TempData["searchfor"].ToString();

            // Search for the new page.
            await RunQueryAsync(model, page, leftMostPage, catFilter, ameFilter);
        }

        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "2" });
        }
        return View("Index", model);
    }
    ```

1. Agregue un método de acción **FacetAsync(SearchData model)** , que se activará cuando el usuario haga clic en un vínculo de faceta. El modelo contendrá un filtro de búsqueda de categorías o servicios. Agréguelo después de la acción **PageAsync**.

    ```cs
    public async Task<ActionResult> FacetAsync(SearchData model)
    {
        try
        {
            // Filters set by the model override those stored in temporary data.
            string catFilter;
            string ameFilter;
            if (model.categoryFilter != null)
            {
                catFilter = model.categoryFilter;
            } else
            {
                catFilter = TempData["categoryFilter"].ToString();
            }

            if (model.amenityFilter != null)
            {
                ameFilter = model.amenityFilter;
            } else
            {
                ameFilter = TempData["amenityFilter"].ToString();
            }

            // Recover the search text.
            model.searchText = TempData["searchfor"].ToString();

            // Initiate a new search.
            await RunQueryAsync(model, 0, 0, catFilter, ameFilter).ConfigureAwait(false);
        }
        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "2" });
        }

        return View("Index", model);
    }
    ```

### <a name="set-up-the-search-filter"></a>Configurar el filtro de búsqueda

Cuando un usuario selecciona una faceta determinada, por ejemplo, hace clic en la categoría **Resort and Spa**, en los resultados solo se deben devolver los hoteles que se especifican en esta categoría. Para limitar una búsqueda de este modo, se debe configurar un _filtro_.

1. Reemplace el método **RunQueryAsync** con el código siguiente. Principalmente, se toma una cadena de filtro de categoría y una cadena de filtro de servicios, y se establece el parámetro **Filter** del objeto **SearchOptions**.

    ```cs
    private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage, string catFilter, string ameFilter)
    {
        InitSearch();

        string facetFilter = "";

        if (catFilter.Length > 0 && ameFilter.Length > 0)
        {
            // Both facets apply.
            facetFilter = $"{catFilter} and {ameFilter}"; 
        } else
        {
            // One, or zero, facets apply.
            facetFilter = $"{catFilter}{ameFilter}";
        }

        var options = new SearchOptions
        {
            Filter = facetFilter,

            SearchMode = SearchMode.All,

            // Skip past results that have already been returned.
            Skip = page * GlobalVariables.ResultsPerPage,

            // Take only the next page worth of results.
            Size = GlobalVariables.ResultsPerPage,

            // Include the total number of results.
            IncludeTotalCount = true,
        };

        // Return information on the text, and number, of facets in the data.
        options.Facets.Add("Category,count:20");
        options.Facets.Add("Tags,count:20");

        // Enter Hotel property names into this list, so only these values will be returned.
        options.Select.Add("HotelName");
        options.Select.Add("Description");
        options.Select.Add("Category");
        options.Select.Add("Tags");

        // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
        model.resultList = await _searchClient.SearchAsync<Hotel>(model.searchText, options).ConfigureAwait(false);

        // This variable communicates the total number of pages to the view.
        model.pageCount = ((int)model.resultList.TotalCount + GlobalVariables.ResultsPerPage - 1) / GlobalVariables.ResultsPerPage;

        // This variable communicates the page number being displayed to the view.
        model.currentPage = page;

        // Calculate the range of page numbers to display.
        if (page == 0)
        {
            leftMostPage = 0;
        }
        else if (page <= leftMostPage)
        {
            // Trigger a switch to a lower page range.
            leftMostPage = Math.Max(page - GlobalVariables.PageRangeDelta, 0);
        }
        else if (page >= leftMostPage + GlobalVariables.MaxPageRange - 1)
        {
            // Trigger a switch to a higher page range.
            leftMostPage = Math.Min(page - GlobalVariables.PageRangeDelta, model.pageCount - GlobalVariables.MaxPageRange);
        }
        model.leftMostPage = leftMostPage;

        // Calculate the number of page numbers to display.
        model.pageRange = Math.Min(model.pageCount - leftMostPage, GlobalVariables.MaxPageRange);

        // Ensure Temp data is stored for the next call.
        TempData["page"] = page;
        TempData["leftMostPage"] = model.leftMostPage;
        TempData["searchfor"] = model.searchText;
        TempData["categoryFilter"] = catFilter;
        TempData["amenityFilter"] = ameFilter;

        // Return the new view.
        return View("Index", model);
    }
    ```

    Observe que las propiedades **Category** y **Tags** se agregan a la lista de elementos **Select** que se devuelven. Esta adición no es un requisito para que la navegación por facetas funcione, pero usamos esta información para comprobar que los filtros funcionan correctamente.

### <a name="add-lists-of-facet-links-to-the-view"></a>Agregar listas de vínculos de faceta a la vista

La vista necesitará algunos cambios importantes. 

1. Comience por abrir el archivo hotels.css (en la carpeta wwwroot/css) y luego agregue las siguientes clases.

    ```html
    .facetlist {
        list-style: none;
    }

    .facetchecks {
        width: 250px;
        display: normal;
        color: #666;
        margin: 10px;
        padding: 5px;
    }

    .facetheader {
        font-size: 10pt;
        font-weight: bold;
        color: darkgreen;
    }
    ```

1. Para la vista, organice la salida en una tabla, con el fin de alinear de manera ordenada las listas de facetas a la izquierda y los resultados a la derecha. Abra el archivo index.cshtml. Reemplace todo el contenido de las etiquetas &lt;body&gt; del código HTML por el código siguiente.

    ```html
    <body>
        @using (Html.BeginForm("Index", "Home", FormMethod.Post))
        {
            <table>
                <tr>
                    <td></td>
                    <td>
                        <h1 class="sampleTitle">
                            <img src="~/images/azure-logo.png" width="80" />
                            Hotels Search - Facet Navigation
                        </h1>
                    </td>
                </tr>

                <tr>
                    <td></td>
                    <td>
                        <!-- Display the search text box, with the search icon to the right of it.-->
                        <div class="searchBoxForm">
                            @Html.TextBoxFor(m => m.searchText, new { @class = "searchBox" }) <input value="" class="searchBoxSubmit" type="submit">
                        </div>
                    </td>
                </tr>

                <tr>
                    <td valign="top">
                        <div id="facetplace" class="facetchecks">

                            @if (Model != null && Model.resultList != null)
                            {
                                List<string> categories = Model.resultList.Facets["Category"].Select(x => x.Value.ToString()).ToList();

                                if (categories.Count > 0)
                                {
                                    <h5 class="facetheader">Category:</h5>
                                    <ul class="facetlist">
                                        @for (var c = 0; c < categories.Count; c++)
                                        {
                                            var facetLink = $"{categories[c]} ({Model.resultList.Facets["Category"][c].Count})";
                                            <li>
                                                @Html.ActionLink(facetLink, "FacetAsync", "Home", new { categoryFilter = $"Category eq '{categories[c]}'" }, null)
                                            </li>
                                        }
                                    </ul>
                                }

                                List<string> tags = Model.resultList.Facets["Tags"].Select(x => x.Value.ToString()).ToList();

                                if (tags.Count > 0)
                                {
                                    <h5 class="facetheader">Amenities:</h5>
                                    <ul class="facetlist">
                                        @for (var c = 0; c < tags.Count; c++)
                                        {
                                            var facetLink = $"{tags[c]} ({Model.resultList.Facets["Tags"][c].Count})";
                                            <li>
                                                @Html.ActionLink(facetLink, "FacetAsync", "Home", new { amenityFilter = $"Tags/any(t: t eq '{tags[c]}')" }, null)
                                            </li>
                                        }
                                    </ul>
                                }
                            }
                        </div>
                    </td>
                    <td valign="top">
                        <div id="resultsplace">
                            @if (Model != null && Model.resultList != null)
                            {
                                // Show the result count.
                                <p class="sampleText">
                                    @Model.resultList.TotalCount Results
                                </p>

                                var results = Model.resultList.GetResults().ToList();

                                @for (var i = 0; i < results.Count; i++)
                                {
                                    string amenities = string.Join(", ", results[i].Document.Tags);

                                    string fullDescription = results[i].Document.Description;
                                    fullDescription += $"\nCategory: {results[i].Document.Category}";
                                    fullDescription += $"\nAmenities: {amenities}";


                                    // Display the hotel name and description.
                                    @Html.TextAreaFor(m => results[i].Document.HotelName, new { @class = "box1" })
                                    @Html.TextArea($"desc{i}", fullDescription, new { @class = "box2" })
                                }
                            }
                        </div>
                    </td>
                </tr>

                <tr>
                    <td></td>
                    <td valign="top">
                        @if (Model != null && Model.pageCount > 1)
                        {
                            // If there is more than one page of results, show the paging buttons.
                            <table>
                                <tr>
                                    <td class="tdPage">
                                        @if (Model.currentPage > 0)
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink("|<", "PageAsync", "Home", new { paging = "0" }, null)
                                            </p>
                                        }
                                        else
                                        {
                                            <p class="pageButtonDisabled">|&lt;</p>
                                        }
                                    </td>

                                    <td class="tdPage">
                                        @if (Model.currentPage > 0)
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink("<", "PageAsync", "Home", new { paging = "prev" }, null)
                                            </p>
                                        }
                                        else
                                        {
                                            <p class="pageButtonDisabled">&lt;</p>
                                        }
                                    </td>

                                    @for (var pn = Model.leftMostPage; pn < Model.leftMostPage + Model.pageRange; pn++)
                                    {
                                        <td class="tdPage">
                                            @if (Model.currentPage == pn)
                                            {
                                                // Convert displayed page numbers to 1-based and not 0-based.
                                                <p class="pageSelected">@(pn + 1)</p>
                                            }
                                            else
                                            {
                                                <p class="pageButton">
                                                    @Html.ActionLink((pn + 1).ToString(), "PageAsync", "Home", new { paging = @pn }, null)
                                                </p>
                                            }
                                        </td>
                                    }

                                    <td class="tdPage">
                                        @if (Model.currentPage < Model.pageCount - 1)
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink(">", "PageAsync", "Home", new { paging = "next" }, null)
                                            </p>
                                        }
                                        else
                                        {
                                            <p class="pageButtonDisabled">&gt;</p>
                                        }
                                    </td>

                                    <td class="tdPage">
                                        @if (Model.currentPage < Model.pageCount - 1)
                                        {
                                            <p class="pageButton">
                                                @Html.ActionLink(">|", "PageAsync", "Home", new { paging = Model.pageCount - 1 }, null)
                                            </p>
                                        }
                                        else
                                        {
                                            <p class="pageButtonDisabled">&gt;|</p>
                                        }
                                    </td>
                                </tr>
                            </table>
                        }
                    </td>
                </tr>
            </table>
        }
    </body>
    ```

    Observe el uso de la llamada a **Html.ActionLink**. Esta llamada comunica las cadenas de filtro válidas al controlador, cuando el usuario hace clic en un vínculo de faceta. 

### <a name="run-and-test-the-app"></a>Ejecutar y probar la aplicación

La ventaja de la navegación por facetas para el usuario es que este puede limitar las búsquedas con un solo clic, lo que podemos mostrar en la siguiente secuencia.

1. Ejecute la aplicación, escriba "airport" como el texto de búsqueda. Compruebe que la lista de facetas aparece de manera ordenada a la izquierda. Estas facetas son las que se aplican a los hoteles que tengan "airport" en sus datos de texto, con un recuento de la frecuencia con la que se producen.

    ![Usar la navegación por facetas para limitar una búsqueda de "airport"](./media/tutorial-csharp-create-first-app/azure-search-facet-airport.png)

1. Haga clic en la categoría **Resort and Spa**. Compruebe que todos los resultados están en esta categoría.

    ![Limitar la búsqueda a "Resort and Spa"](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras.png)

1. Haga clic en el servicio **continental breakfast**. Compruebe que todos los resultados aún se encuentran en la categoría "Resort and Spa", con el servicio seleccionado.

    ![Limitar la búsqueda a "continental breakfast"](./media/tutorial-csharp-create-first-app/azure-search-facet-airport-ras-cb.png)

1. Pruebe a seleccionar otra categoría, luego un servicio, y consulte los resultados de limitación. Luego, pruebe lo inverso, un servicio y después una categoría. Envíe una búsqueda vacía para restablecer la página.

    >[!Note]
    > Cuando se hace una selección en una lista de facetas (por ejemplo, una categoría), se invalidará toda selección anterior dentro de la lista de categorías.

## <a name="takeaways"></a>Puntos clave

Tenga en cuenta las siguientes conclusiones de este proyecto:

* Es imperativo marcar cada campo al que se puedan agregar facetas con la propiedad **IsFacetable** para su inclusión en la navegación por facetas.
* Las facetas se combinan con los filtros para reducir los resultados.
* Las facetas son acumulativas y cada selección se basa en la anterior para restringir aún más los resultados.

## <a name="next-steps"></a>Pasos siguientes

En el siguiente tutorial, nos centramos en cómo ordenar los resultados. Llegado a este punto, los resultados se ordenan simplemente en el orden en que se encuentran en la base de datos.

> [!div class="nextstepaction"]
> [Tutorial de C#: Ordenación de los resultados: Azure Cognitive Search](tutorial-csharp-orders.md)
