---
title: Inicio rápido para agregar marcas de características a ASP.NET Core
description: Adición de marcas de características a aplicaciones de ASP.NET Core y su administración en Azure App Configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.custom: devx-track-csharp
ms.topic: quickstart
ms.date: 09/28/2020
ms.author: lcozzens
ms.openlocfilehash: 7d146004fb4fa120e08b4d67c1d68d51cc6f3c8f
ms.sourcegitcommit: d9ba60f15aa6eafc3c5ae8d592bacaf21d97a871
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91767727"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Inicio rápido: Adición de marcas de características a una aplicación web de ASP.NET Core

En este inicio rápido, va a crear una implementación de un extremo a otro de administración de características en una aplicación de ASP.NET Core mediante Azure App Configuration. Podrá usar el servicio App Configuration para almacenar en una ubicación central todas las marcas de características y controlar sus estados. 

Las bibliotecas de administración de características de .NET Core amplían la plataforma con una compatibilidad completa con las marcas de características. Estas bibliotecas se compilan sobre el sistema de configuración de .NET Core. Se integran sin problemas con App Configuration a través de su proveedor de configuración de .NET Core.

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/dotnet)
* [SDK de .NET Core](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Creación de un almacén de App Configuration

[!INCLUDE[Azure App Configuration resource creation steps](../../includes/azure-app-configuration-create.md)]

8. Seleccione **Operaciones** > **Administrador de características** > **Agregar** para agregar una marca de características denominada *Beta*.

    > [!div class="mx-imgBorder"]
    > ![Habilitación de la marca de característica denominada Beta](media/add-beta-feature-flag.png)

    Deje **Etiqueta** en blanco, por ahora. Seleccione **Aplicar** para guardar la nueva marca de características.

## <a name="create-an-aspnet-core-web-app"></a>Cree una aplicación web ASP.NET Core

Utilice la [interfaz de la línea de comandos (CLI) de .NET Core](/dotnet/core/tools) para crear un proyecto de ASP.NET Core MVC. La ventaja de usar la CLI de .NET Core frente a Visual Studio es que está disponible en las plataformas Windows, macOS y Linux.

Ejecute el siguiente comando para crear un proyecto de ASP.NET Core MVC en una nueva carpeta *TestFeatureFlags*:

```dotnetcli
dotnet new mvc --no-https --output TestFeatureFlags
```

[!INCLUDE[Add Secret Manager support to an ASP.NET Core project](../../includes/azure-app-configuration-add-secret-manager.md)]

## <a name="connect-to-an-app-configuration-store"></a>Conexión a un almacén de App Configuration

1. Instale los paquetes NuGet [Microsoft.Azure.AppConfiguration.AspNetCore](https://www.nuget.org/packages/Microsoft.Azure.AppConfiguration.AspNetCore) y [Microsoft.FeatureManagement.AspNetCore](https://www.nuget.org/packages/Microsoft.FeatureManagement.AspNetCore) mediante la ejecución de los siguientes comandos:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    ```

    ```dotnetcli
    dotnet add package Microsoft.FeatureManagement.AspNetCore
    ```

1. Ejecute el siguiente comando en el mismo directorio que el archivo *.csproj*. El comando usa el Administrador de secretos para almacenar un secreto denominado `ConnectionStrings:AppConfig`, que almacena la cadena de conexión para el almacén de App Configuration. Reemplace el marcador de posición `<your_connection_string>` por la cadena de conexión del almacén de App Configuration. Puede encontrar la cadena de conexión en **Claves de acceso** en Azure Portal.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig "<your_connection_string>"
    ```

    Secret Manager solo se utiliza para probar la aplicación web localmente. Cuando la aplicación se implementa en [Azure App Service](https://azure.microsoft.com/services/app-service/web), usará el valor de la aplicación configuración de la aplicación **Cadenas de conexión** en App Service, en lugar de Administrador de secretos para almacenar la cadena de conexión.

    Obtenga acceso a este secreto mediante la API de configuración de .NET Core. Un signo de dos puntos (`:`) funciona en el nombre de configuración con la API de configuración en todas las plataformas compatibles. Para más información, vea [Claves y valores de configuración](/aspnet/core/fundamentals/configuration#configuration-keys-and-values).

1. En *Program.cs*, actualice el método `CreateWebHostBuilder` para usar App Configuration mediante la llamada al método `AddAzureAppConfiguration`.

    > [!IMPORTANT]
    > `CreateHostBuilder` reemplaza a `CreateWebHostBuilder` en .NET Core 3.x. Seleccione la sintaxis correcta en función de su entorno.

    #### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
                webBuilder.ConfigureAppConfiguration(config =>
                {
                    var settings = config.Build();
                    var connection = settings.GetConnectionString("AppConfig");
                    config.AddAzureAppConfiguration(options =>
                        options.Connect(connection).UseFeatureFlags());
                }).UseStartup<Startup>());
    ```

    #### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
               .ConfigureAppConfiguration(config =>
               {
                   var settings = config.Build();
                   var connection = settings.GetConnectionString("AppConfig");
                   config.AddAzureAppConfiguration(options =>
                       options.Connect(connection).UseFeatureFlags());
               }).UseStartup<Startup>();
    ```

    ---

    Con el cambio anterior, el [proveedor de configuración de App Configuration](https://go.microsoft.com/fwlink/?linkid=2074664) se ha registrado con la API de configuración de .NET Core.

1. En *Startup.cs*, agregue una referencia al administrador de características de .NET Core:

    ```csharp
    using Microsoft.FeatureManagement;
    ```

1. Actualice el método `Startup.ConfigureServices` para agregar compatibilidad con la marca de características mediante una llamada al método `AddFeatureManagement`. También tiene la opción de incluir cualquier filtro para usar con las marcas de características mediante la llamada a `AddFeatureFilter<FilterType>()`:

    #### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

    ```csharp    
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllersWithViews();
        services.AddFeatureManagement();
    }
    ```

    #### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc()
            .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
        services.AddFeatureManagement();
    }
    ```

    ---

1. Agregue un archivo *MyFeatureFlags.cs* al directorio raíz del proyecto con el código siguiente:

    ```csharp
    namespace TestFeatureFlags
    {
        public enum MyFeatureFlags
        {
            Beta
        }
    }
    ```

1. Agregue un archivo *BetaController.cs* al directorio de *Controladores* con el código siguiente:

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.FeatureManagement;
    using Microsoft.FeatureManagement.Mvc;

    namespace TestFeatureFlags.Controllers
    {
        public class BetaController: Controller
        {
            private readonly IFeatureManager _featureManager;

            public BetaController(IFeatureManagerSnapshot featureManager) =>
                _featureManager = featureManager;

            [FeatureGate(MyFeatureFlags.Beta)]
            public IActionResult Index() => View();
        }
    }
    ```

1. En *Views/_ViewImports.cshtml*, registre el asistente de etiquetas del administrador de características mediante una directiva de `@addTagHelper`:

    ```cshtml
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

    El código anterior permite el uso del asistente de etiquetas de `<feature>` en los archivos *.cshtml* del proyecto.

1. En *Views/Shared/_Layout.cshtml*, reemplace el código de barras de `<nav>` que se muestra en `<body>` > `<header>` por el marcado siguiente:

    ```cshtml
    <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
        <div class="container">
            <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">TestFeatureFlags</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse" aria-controls="navbarSupportedContent"
            aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
                <ul class="navbar-nav flex-grow-1">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
                    </li>
                    <feature name="Beta">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Beta" asp-action="Index">Beta</a>
                    </li>
                    </feature>
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    ```

    En el marcado anterior, tenga en cuenta el asistente de etiquetas de `<feature>` que rodea al elemento de lista *Beta*.

1. Cree un directorio *Views/Beta* y un archivo *Index.cshtml* que contenga el marcado siguiente:

    ```cshtml
    @{
        ViewData["Title"] = "Beta Home Page";
    }

    <h1>This is the beta website.</h1>
    ```

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

1. Para compilar la aplicación mediante la CLI de .NET Core, ejecute el siguiente comando en el shell de comandos:

    ```dotnetcli
    dotnet build
    ```

1. Una vez que la compilación se haya realizado correctamente, ejecute el siguiente comando para ejecutar la aplicación web localmente:

    ```dotnetcli
    dotnet run
    ```

1. Inicie una ventana del explorador y vaya a `http://localhost:5000`, que es la dirección URL predeterminada de la aplicación web hospedada localmente. Si trabajando en Azure Cloud Shell, seleccione el botón **Vista previa web** y después **Configurar**. Cuando se le pida, seleccione el puerto 5000.

    ![Busque el botón Vista previa web](./media/quickstarts/cloud-shell-web-preview.png)

    El explorador debe mostrar una página similar a la imagen siguiente.

    :::image type="content" source="media/quickstarts/aspnet-core-feature-flag-local-before.png" alt-text="Aplicación de inicio rápido local antes del cambio" border="true":::

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Todos los recursos** y seleccione la instancia de almacén de App Configuration que creó en el inicio rápido.

1. Seleccione **Administrador de características** y cambie el estado de la clave *Beta* a **Activado**.

1. Vuelva al shell de comandos. Para cancelar el proceso de `dotnet` en ejecución, presione <kbd>CTRL + C</kbd>. Reinicie la aplicación mediante `dotnet run`.

1. Actualice la página del explorador para ver los nuevos valores de configuración.

    :::image type="content" source="media/quickstarts/aspnet-core-feature-flag-local-after.png" alt-text="Aplicación de inicio rápido local antes del cambio" border="true":::

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE[Azure App Configuration cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un almacén de App Configuration y lo ha usado para administrar características en una aplicación web de ASP.NET Core mediante las [bibliotecas de administración de características](https://go.microsoft.com/fwlink/?linkid=2074664).

* Más información sobre la [administración de características](./concept-feature-management.md).
* [Administración de marcas de características](./manage-feature-flags.md).
* [Uso de marcas de características en una aplicación de ASP.NET Core](./use-feature-flags-dotnet-core.md).
* [Uso de la configuración dinámica en una aplicación de ASP.NET Core](./enable-dynamic-configuration-aspnet-core.md)
