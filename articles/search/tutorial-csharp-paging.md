---
title: Tutorial de C# sobre la paginación de los resultados de la búsqueda
titleSuffix: Azure Cognitive Search
description: Agregue botones de navegación y paginación a los resultados de la búsqueda, y realice la compilación sobre un proyecto de hoteles existente para agregar los botones primero, siguiente, anterior, último y numerados. Un segundo sistema de paginación usa el desplazamiento infinito, desencadenado al mover una barra de desplazamiento vertical en el límite inferior.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 09/30/2020
ms.custom: devx-track-js, devx-track-csharp
ms.openlocfilehash: a08756a1e3153aa69bd0e79dc23e88d4bf211e5d
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2020
ms.locfileid: "91950693"
---
# <a name="tutorial-add-paging-to-search-results-using-the-net-sdk"></a>Tutorial: Incorporación de la paginación a los resultados de búsqueda mediante el SDK de .NET

Aprenda a implementar dos sistemas de paginación distintos, el primero basado en los números de página y el segundo en el desplazamiento infinito. Ambos sistemas de paginación se utilizan ampliamente y seleccionar el correcto depende de la experiencia de usuario que desee con los resultados. 

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Ampliar la aplicación con la paginación numerada
> * Ampliar la aplicación con el desplazamiento infinito

## <a name="overview"></a>Información general

En este tutorial se superpone un sistema de localización en un proyecto creado previamente que se describe en el tutorial [Creación de la primera aplicación de búsqueda](tutorial-csharp-create-first-app.md).

En los proyectos siguientes se pueden encontrar versiones finalizadas del código que va a desarrollar en este tutorial:

* [2a-add-paging (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/2a-add-paging)

* [2b-add-infinite-scroll (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/2b-add-infinite-scroll)

## <a name="prerequisites"></a>Prerrequisitos

* Proyecto [1-basic-search-page (GitHub)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/1-basic-search-page). Este proyecto puede ser su propia versión, que se creó a partir del tutorial anterior o de una copia de GitHub.

Este tutorial se ha actualizado para usar el paquete [Azure.Search.Documents (versión 11)](https://www.nuget.org/packages/Azure.Search.Documents/). Para obtener una versión anterior del SDK de .NET, consulte el [código de ejemplo de Microsoft.Azure.Search (versión 10)](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v10).

## <a name="extend-your-app-with-numbered-paging"></a>Ampliar la aplicación con la paginación numerada

La paginación numerada es el sistema de localización favorito tanto de los principales motores de búsqueda web comerciales como de muchos otros sitios web de búsqueda. La paginación numerado normalmente incluye una opción "siguiente" y "anterior", además de un intervalo de números de página real. También pueden estar disponibles una opción de "última página" y de "primera página". Estas opciones le dan al usuario el control sobre el desplazamiento en los resultados de la página.

En este tutorial, agregará un sistema que incluya las opciones de primera página, anterior, siguiente y última, junto con los números de página que no se inicien en 1, sino usar la página actual en la que esté el usuario (por lo tanto, por ejemplo, si el usuario está viendo la página 10, podrían mostrarse los números de página 8, 9, 10, 11 y 12).

El sistema será lo suficientemente flexible como para permitir que la cantidad de números de página visibles se establezca en una variable global.

El sistema considerará los botones de número de página más a la izquierda y más a la derecha como especiales, lo que significa que desencadenarán el cambio del intervalo de números de página que se muestra. Por ejemplo, si se muestran los números de página 8, 9, 10, 11 y 12, y el usuario hace clic en 8, el intervalo de números de página muestra los cambios a 6, 7, 8, 9 y 10. Y hay un cambio similar a la derecha si se selecciona 12.

### <a name="add-paging-fields-to-the-model"></a>Incorporación de campos de paginación al modelo

Tiene abierta la solución de la página de búsqueda básica.

1. Abra el archivo de modelo SearchData.cs.

1. Agregue variables globales que admitan la paginación. En MVC, las variables globales se declaran en su propia clase estática. **ResultsPerPage** establece el número de resultados por página. **MaxPageRange** determina la cantidad de números de página visibles en la vista. **PageRangeDelta** determina el número de páginas que se deben desplazar a la izquierda o a la derecha cuando se selecciona el número de la página que está más a la izquierda o a la derecha. Normalmente, este último número es en torno a la mitad de **MaxPageRange**. Agregue el código siguiente en el espacio de nombres.

    ```csharp
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
        public static int MaxPageRange
        {
            get
            {
                return 5;
            }
        }

        public static int PageRangeDelta
        {
            get
            {
                return 2;
            }
        }
    }
    ```

    >[!Tip]
    >Si ejecuta este proyecto en un dispositivo con una pantalla más pequeña, como un equipo portátil, considere la posibilidad de cambiar **ResultsPerPage** a 2.

1. Agregue las propiedades de paginación a la clase **SearchData**, después de la propiedad **searchText**.

    ```csharp
    // The current page being displayed.
    public int currentPage { get; set; }

    // The total number of pages of results.
    public int pageCount { get; set; }

    // The left-most page number to display.
    public int leftMostPage { get; set; }

    // The number of page numbers to display - which can be less than MaxPageRange towards the end of the results.
    public int pageRange { get; set; }

    // Used when page numbers, or next or prev buttons, have been selected.
    public string paging { get; set; }
    ```

### <a name="add-a-table-of-paging-options-to-the-view"></a>Incorporación de una tabla de opciones de paginación a la vista

1. Abra el archivo index.cshtml y agregue el código siguiente justo antes de la etiqueta &lt;/body&gt; de cierre. Este nuevo código presenta una tabla de opciones de paginación: primero, anterior, 1, 2, 3, 4, 5, siguiente, última.

    ```html
    @if (Model != null && Model.pageCount > 1)
    {
    // If there is more than one page of results, show the paging buttons.
    <table>
        <tr>
            <td>
                @if (Model.currentPage > 0)
                {
                    <p class="pageButton">
                        @Html.ActionLink("|<", "Page", "Home", new { paging = "0" }, null)
                    </p>
                }
                else
                {
                    <p class="pageButtonDisabled">|&lt;</p>
                }
            </td>

            <td>
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
                <td>
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

            <td>
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

            <td>
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
    ```

    Se usa una tabla HTML para alinear los elementos ordenadamente. Aunque todas las acciones proceden de las instrucciones @Html.ActionLink, cada una invoca al controlador con un modelo **nuevo** creado con entradas diferentes para la propiedad **paging** agregada anteriormente.

    Las opciones de primera y última página no envían cadenas como "first" y "last", sin que, en su lugar, envían los números de página correctos.

1. Agregue clases de paginación a la lista de estilos HTML del archivo hotels.css. La clase **pageSelected** está ahí para identificar la página actual (mediante la aplicación del formato negrita al número de página) en la lista de números de página.

    ```html
    .pageButton {
        border: none;
        color: darkblue;
        font-weight: normal;
        width: 50px;
    }

    .pageSelected {
        border: none;
        color: black;
        font-weight: bold;
        width: 50px;
    }

    .pageButtonDisabled {
        border: none;
        color: lightgray;
        font-weight: bold;
        width: 50px;
    }
    ```

### <a name="add-a-page-action-to-the-controller"></a>Incorporación de una acción Page al controlador

1. Abra el archivo HomeController.cs y agregue la acción **PageAsync**. Esta acción responde a cualquiera de las opciones de página seleccionadas.

    ```csharp
    public async Task<ActionResult> PageAsync(SearchData model)
    {
        try
        {
            int page;

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

            // Recover the search text and search for the data for the new page.
            model.searchText = TempData["searchfor"].ToString();

            await RunQueryAsync(model, page, leftMostPage);

            // Ensure Temp data is stored for next call, as TempData only stores for one call.
            TempData["page"] = (object)page;
            TempData["searchfor"] = model.searchText;
            TempData["leftMostPage"] = model.leftMostPage;
        }

        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "2" });
        }
        return View("Index", model);
    }
    ```

    El método **RunQueryAsync** mostrará ahora un error de sintaxis, debido al tercer parámetro, sobre el que volveremos en un momento.

    > [!Note]
    > Las llamadas a **TempData** almacenan un valor (un **objeto**) en el almacenamiento temporal, aunque este almacenamiento dura _solo_ una llamada. Si se almacena algo en los datos temporales, estará disponible para la siguiente llamada a una acción de la controladora, pero desaparecerá definitivamente tras la llamada. Debido a esta corta duración, almacenamos el texto de búsqueda y las propiedades de paginación en cada almacenamiento temporal y en cada llamada a **PageAsync**.

1. Actualice la acción **Index(model)** para almacenar variables temporales y para agregar el parámetro de la página que está más a la izquierda a la llamada a **RunQueryAsync**.

    ```csharp
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
            await RunQueryAsync(model, 0, 0);

            // Ensure temporary data is stored for the next call.
            TempData["page"] = 0;
            TempData["leftMostPage"] = 0;
            TempData["searchfor"] = model.searchText;
        }

        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "1" });
        }
        return View(model);
    }
    ```

1. El método **RunQueryAsync**, que se presentó en la lección anterior, necesita una modificación para resolver el error de sintaxis. Se usan los campos **Skip**, **Size** e **IncludeTotalCount** de la clase [**SearchOptions**](/dotnet/api/azure.search.documents.searchoptions) para solicitar solo una página de resultados que merezcan la pena, empezando por el valor **Skip**. También es necesario calcular las variables de paginación para nuestra vista. Reemplace el método entero por el código siguiente.

    ```csharp
    private async Task<ActionResult> RunQueryAsync(SearchData model, int page, int leftMostPage)
    {
        InitSearch();

        var options = new SearchOptions
        {
            // Skip past results that have already been returned.
            Skip = page * GlobalVariables.ResultsPerPage,

            // Take only the next page worth of results.
            Size = GlobalVariables.ResultsPerPage,

            // Include the total number of results.
            IncludeTotalCount = true
        };

        // Add fields to include in the search results.
        options.Select.Add("HotelName");
        options.Select.Add("Description");

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

        return View("Index", model);
    }
    ```

1. Por último, realice un pequeño cambio en la vista. La variable **resultList.Results.TotalCount** ahora contendrá el número de resultados devueltos en una sola página (tres en nuestro ejemplo), no el número total. Dado que establecemos **IncludeTotalResultCount** en true, la variable **resultList.TotalCount** ahora contiene el número total de resultados. Por tanto, busque dónde se muestra el número de resultados en la vista y cámbielo al código siguiente.

    ```csharp
    // Show the result count.
    <p class="sampleText">
        @Model.resultList.TotalCount Results
    </p>

    var results = Model.resultList.GetResults().ToList();

    @for (var i = 0; i < results.Count; i++)
    {
        // Display the hotel name and description.
        @Html.TextAreaFor(m => results[i].Document.HotelName, new { @class = "box1" })
        @Html.TextArea($"desc{1}", results[i].Document.Description, new { @class = "box2" })
    }
    ```

    > [!Note]
    > Hay un pequeño aumento del rendimiento cuando **IncludeTotalCount** se establece en true, ya que es necesario que Azure Cognitive Search calcule este total. Con conjuntos de datos complejos, se advierte que el valor devuelto es una _aproximación_. Como el corpus de búsqueda de hoteles es pequeño, será preciso.

### <a name="compile-and-run-the-app"></a>Compilación y ejecución de la aplicación

Ahora, seleccione **Iniciar sin depurar** (o presione la tecla F5).

1. Busque una cadena que devuelva gran cantidad de resultados (como "wifi"). ¿Puede recorrer las páginas correctamente en los resultados?

    ![Paginación numerada en los resultados del "grupo"](./media/tutorial-csharp-create-first-app/azure-search-numbered-paging.png)

1. Pruebe a hacer clic en los números de página de la izquierda, de la derecha y última. ¿Los números de página se ajustan adecuadamente al centro de la página en la que se encuentra?

1. ¿Son útiles las opciones "primera" y "última"? Algunos motores de búsqueda comerciales usan estas opciones, mientras que otros no lo hacen.

1. Vaya a la última página de resultados. La última página es la única que puede contener menos de **ResultsPerPage** resultados.

    ![Examen de la última página de "wifi"](./media/tutorial-csharp-create-first-app/azure-search-pool-last-page.png)

1. Escriba "town" y haga clic en Buscar. Si los resultados ocupan menos de una página, no se muestran opciones de paginación.

    ![Búsqueda de "town"](./media/tutorial-csharp-create-first-app/azure-search-town.png)

Guarde este proyecto y pase a la sección siguiente para conocer otra forma de paginación.

## <a name="extend-your-app-with-infinite-scrolling"></a>Ampliar la aplicación con el desplazamiento infinito

El desplazamiento infinito se desencadena cuando un usuario se desplaza hasta el último de los resultados que se muestra con una barra de desplazamiento vertical. En este evento, se realiza una llamada al servicio de búsqueda para obtener la siguiente página de resultados. Si no hay ningún resultado más, no se devuelve nada y la barra de desplazamiento vertical no cambia. Si hay más resultados, se anexan a la página actual y la barra de desplazamiento cambia para mostrar que hay más resultados disponibles.

Un punto importante que se debe indicar es que la página actual no se reemplaza, sino que se extiende para mostrar los resultados adicionales. Un usuario siempre puede desplazarse hasta los primeros resultados de la búsqueda.

Para implementar el desplazamiento infinito, comencemos con el proyecto antes de que se agregara cualquiera de los elementos de desplazamiento del número de página. En GitHub, esta es la solución [FirstAzureSearchApp](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/create-first-app/v11/1-basic-search-page).

### <a name="add-paging-fields-to-the-model"></a>Incorporación de campos de paginación al modelo

1. En primer lugar, agregue una propiedad **paging** a la clase **SearchData** (en el archivo de modelo SearchData.cs).

    ```csharp
    // Record if the next page is requested.
    public string paging { get; set; }
    ```

    Esta variable es una cadena que contiene "next" (siguiente) si la página siguiente de resultados se debe enviar o es null para la primera página de una búsqueda.

1. En el mismo archivo y dentro del espacio de nombres, agregue una clase de variable global con una propiedad. En MVC, las variables globales se declaran en su propia clase estática. **ResultsPerPage** establece el número de resultados por página. 

    ```csharp
    public static class GlobalVariables
    {
        public static int ResultsPerPage
        {
            get
            {
                return 3;
            }
        }
    }
    ```

### <a name="add-a-vertical-scroll-bar-to-the-view"></a>Incorporación de una barra de desplazamiento vertical a la vista

1. Busque la sección del archivo index.cshtml que muestra los resultados (comienza con **@if (Model !=null)** ).

1. Reemplace la sección por el código siguiente. La nueva sección **&lt;div&gt;** está alrededor del área que debe ser desplazable y agrega tanto un atributo **overflow-y** como una llamada a una función **onscroll** denominada "scrolled()".

    ```csharp
    @if (Model != null)
    {
        // Show the result count.
        <p class="sampleText">
            @Model.resultList.TotalCount Results
        </p>

        var results = Model.resultList.GetResults().ToList();

        <div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()">

            <!-- Show the hotel data. -->
            @for (var i = 0; i < results.Count; i++)
            {
                // Display the hotel name and description.
                @Html.TextAreaFor(m => results[i].Document.HotelName, new { @class = "box1" })
                @Html.TextArea($"desc{i}", results[i].Document.Description, new { @class = "box2" })
            }
    ```

1. Directamente debajo del bucle, después de la etiqueta &lt;/div&gt;, agregue la función **scrolled**.

    ```javascript
    <script>
        function scrolled() {
            if (myDiv.offsetHeight + myDiv.scrollTop >= myDiv.scrollHeight) {
                $.getJSON("/Home/NextAsync", function (data) {
                    var div = document.getElementById('myDiv');

                    // Append the returned data to the current list of hotels.
                    for (var i = 0; i < data.length; i += 2) {
                        div.innerHTML += '\n<textarea class="box1">' + data[i] + '</textarea>';
                        div.innerHTML += '\n<textarea class="box2">' + data[i + 1] + '</textarea>';
                    }
                });
            }
        }
    </script>
    ```

    La instrucción **if** del script anterior comprueba si el usuario se ha desplazado hasta la parte inferior de la barra de desplazamiento vertical. Si lo ha hecho, se realiza una llamada al controlador **Home** para una acción denominada **NextAsync**. El controlador no necesita ninguna otra información, devolverá la siguiente página de datos. A continuación, se da formato a estos datos con estilos HTML idénticos al de la página original. Si no se devuelve ningún resultado, no se anexa nada y todo permanece como esté.

### <a name="handle-the-next-action"></a>Control de acción Next

Solo hay tres acciones que deban enviarse al controlador: la primera ejecución de la aplicación, que llama a **Index()** , la primera búsqueda del usuario, que llama a **Index(model)** y, a continuación, las siguientes llamadas para obtener más resultados mediante **Next(model)** .

1. Abra el archivo del controlador de home y elimine el método **RunQueryAsync** del tutorial original.

1. Reemplace la acción **Index(model)** con el código siguiente. Ahora controla el campo **paging** cuando es null o se establece en "next" y controla la llamada a Azure Cognitive Search.

    ```csharp
    public async Task<ActionResult> Index(SearchData model)
    {
        try
        {
            InitSearch();

            int page;

            if (model.paging != null && model.paging == "next")
            {
                // Increment the page.
                page = (int)TempData["page"] + 1;

                // Recover the search text.
                model.searchText = TempData["searchfor"].ToString();
            }
            else
            {
                // First call. Check for valid text input.
                if (model.searchText == null)
                {
                    model.searchText = "";
                }
                page = 0;
            }

            // Setup the search parameters.
            var options = new SearchOptions
            {
                SearchMode = SearchMode.All,

                // Skip past results that have already been returned.
                Skip = page * GlobalVariables.ResultsPerPage,

                // Take only the next page worth of results.
                Size = GlobalVariables.ResultsPerPage,

                // Include the total number of results.
                IncludeTotalCount = true
            };

            // Specify which fields to include in results.
            options.Select.Add("HotelName");
            options.Select.Add("Description");

            // For efficiency, the search call should be asynchronous, so use SearchAsync rather than Search.
            model.resultList = await _searchClient.SearchAsync<Hotel>(model.searchText, options).ConfigureAwait(false);               

            // Ensure TempData is stored for the next call.
            TempData["page"] = page;
            TempData["searchfor"] = model.searchText;
        }
        catch
        {
            return View("Error", new ErrorViewModel { RequestId = "1" });
        }

        return View("Index", model);
    }
    ```

    Al igual que el método de paginación numerada, las opciones de búsqueda **Skip** y **Size** se usan para solicitar que se devuelvan solo los datos que necesitamos.

1. Agregue la acción **NextAsync** al controlador de inicio. Observe cómo devuelve una lista y cada hotel agrega dos elementos a la lista: un nombre y una descripción del hotel. Este formato se establece para que coincida con el uso de la función **scrolled** de los datos devueltos en la vista.

    ```csharp
    public async Task<ActionResult> NextAsync(SearchData model)
    {
        // Set the next page setting, and call the Index(model) action.
        model.paging = "next";
        await Index(model).ConfigureAwait(false);

        // Create an empty list.
        var nextHotels = new List<string>();

        // Add a hotel name, then description, to the list.
        await foreach (var searchResult in model.resultList.GetResultsAsync())
        {
            nextHotels.Add(searchResult.Document.HotelName);
            nextHotels.Add(searchResult.Document.Description);
        }

        // Rather than return a view, return the list of data.
        return new JsonResult(nextHotels);
    }
    ```

1. Si recibe un error de sintaxis en la cadena **List&lt;&gt;** , agregue la siguiente directiva **using** al encabezado del archivo del controlador.

    ```csharp
    using System.Collections.Generic;
    ```

### <a name="compile-and-run-your-project"></a>Compilación y ejecución del proyecto

Ahora, seleccione **Iniciar sin depurar** (o presione la tecla F5).

1. Escriba un término que proporcione una gran cantidad de resultados (por ejemplo, "pool") y, a continuación, pruebe la barra de desplazamiento vertical. ¿Desencadena una nueva página de resultados?

    ![El desplazamiento infinito por los resultados de "pool"](./media/tutorial-csharp-create-first-app/azure-search-infinite-scroll.png)

    > [!Tip]
    > Para asegurarse de que aparece una barra de desplazamiento en la primera página, la primera página de resultados debe superar ligeramente el alto del área en que se muestran. En nuestro ejemplo **.box1** tiene un alto de 30 píxeles, **.box2** tiene un alto de 100 píxeles _y_ un margen inferior de 24 píxeles. Por lo tanto, cada entrada usa 154 píxeles. Tres entradas pueden ocupar hasta 3 x 154 = 462 píxeles. Para asegurarse de que aparece una barra de desplazamiento vertical, se debe establecer un alto para el área de presentación menor que 462 píxeles, incluso 461 funciona. Este problema se produce únicamente en la primera página, después de que se tenga la seguridad de que la barra de desplazamiento aparecerá. La línea para actualizar es: **&lt;div id="myDiv" style="width: 800px; height: 450px; overflow-y: scroll;" onscroll="scrolled()"&gt;** .

1. Desplácese hacia abajo hasta la parte inferior de los resultados. Tenga en cuenta cómo toda la información ya está en la página de una vista. Puede desplazarse remontándose a la parte superior sin desencadenar ninguna llamada al servidor.

Los sistemas más sofisticados de desplazamiento infinito podrían usar la rueda del mouse u otro mecanismo similar para desencadenar la carga de una nueva página de resultados. No adoptaremos el desplazamiento infinito más en estos tutoriales, aunque resulta atractivo, ya que evita clics adicionales del mouse y es posible que desee investigar otras opciones.

## <a name="takeaways"></a>Puntos clave

Tenga en cuenta las siguientes conclusiones de este proyecto:

* La paginación numerada es útil para las búsquedas en las que el orden de los resultados es bastante arbitrario, lo que significa que puede haber algo de interés para los usuarios en las páginas posteriores.
* El desplazamiento infinito es útil cuando el orden de los resultados es especialmente importante. Por ejemplo, si se ordenan según la distancia desde el centro de una ciudad de destino.
* La paginación numerada mejora la navegación. Por ejemplo, un usuario puede recordar que un resultado interesante estaba en la página 6, mientras que tal referencia fácil no existe en el desplazamiento infinito.
* El desplazamiento infinito tiene una apariencia sencilla y el desplazamiento se produce arriba y abajo sin que haya que hacer clic en los números de página.
* Una característica clave del desplazamiento infinito es que los resultados se anexan a una página existente, no reemplazan esa página, lo que resulta eficaz.
* El almacenamiento temporal se mantiene solo una llamada y debe reiniciarse para sobrevivir a las siguientes.

## <a name="next-steps"></a>Pasos siguientes

La paginación es fundamental en una experiencia de búsqueda. Si la paginación se ha resuelto adecuadamente, el paso siguiente es mejorar aún más la experiencia del usuario incorporando búsquedas de escritura anticipada.

> [!div class="nextstepaction"]
> [Tutorial de C#: Incorporación de la función Autocompletar y las sugerencias en Azure Cognitive Search](tutorial-csharp-type-ahead-and-suggestions.md)