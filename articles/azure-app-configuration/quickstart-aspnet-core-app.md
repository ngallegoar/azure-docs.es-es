---
title: Inicio rápido de Azure App Configuration con ASP.NET Core | Microsoft Docs
description: Creación de una aplicación de ASP.NET Core con Azure App Configuration para centralizar el almacenamiento y la administración de la configuración de la aplicación de una aplicación de ASP.NET Core.
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.devlang: csharp
ms.custom: devx-track-csharp, contperfq1
ms.topic: quickstart
ms.date: 09/25/2020
ms.author: lcozzens
ms.openlocfilehash: 5fd042b91ede91491590a53abf4dec552fbf6487
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91440423"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Inicio rápido: Creación de una aplicación ASP.NET Core con Azure App Configuration

En este inicio rápido usará Azure App Configuration para centralizar el almacenamiento y la administración de la configuración de la aplicación de una aplicación de ASP.NET Core. ASP.NET Core genera un solo objeto de configuración basado en un par clave-valor mediante el uso de la configuración de uno o varios orígenes de datos especificados por una aplicación. Estos orígenes de datos se conocen como *proveedores de configuración*. Dado que el cliente .NET Core de App Configuration se implementa como un proveedor de configuraciones, el servicio parece como cualquier otro origen de datos.

## <a name="prerequisites"></a>Prerrequisitos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/dotnet)
* [SDK de .NET Core](https://dotnet.microsoft.com/download)

> [!TIP]
> Azure Cloud Shell es un shell interactivo gratuito que se puede usar para ejecutar las instrucciones de línea de comandos en este artículo. Tiene herramientas comunes de Azure preinstaladas, lo que incluye el SDK de .NET Core. Si ha iniciado sesión en su suscripción de Azure, inicie [Azure Cloud Shell](https://shell.azure.com) desde shell.azure.com. Para más información sobre Azure Cloud Shell, [lea la documentación](../cloud-shell/overview.md).

## <a name="create-an-app-configuration-store"></a>Creación de un almacén de App Configuration

[!INCLUDE[Azure App Configuration resource creation steps](../../includes/azure-app-configuration-create.md)]

7. Seleccione **Operaciones** > **Explorador de configuración** > **Crear** > **Clave-valor** para agregar los siguientes pares clave-valor:

    | Clave                                | Value                               |
    |------------------------------------|-------------------------------------|
    | `TestApp:Settings:BackgroundColor` | *#FFF*                              |
    | `TestApp:Settings:FontColor`       | *#000*                              |
    | `TestApp:Settings:FontSize`        | *24*                                |
    | `TestApp:Settings:Message`         | *Datos de Azure App Configuration* |

    Deje **Etiqueta** y **Tipo de contenido** en blanco, por ahora. Seleccione **Aplicar**.

## <a name="create-an-aspnet-core-web-app"></a>Cree una aplicación web ASP.NET Core

Utilice la [interfaz de la línea de comandos (CLI) de .NET Core](/dotnet/core/tools) para crear un proyecto de ASP.NET Core MVC. [Azure Cloud Shell](https://shell.azure.com) proporciona estas herramientas. También están disponibles en las plataformas Windows, macOS y Linux.

Ejecute el siguiente comando para crear un proyecto de ASP.NET Core MVC en una nueva carpeta *TestAppConfig*:

```dotnetcli
dotnet new mvc --no-https --output TestAppConfig
```

[!INCLUDE[Add Secret Manager support to an ASP.NET Core project](../../includes/azure-app-configuration-add-secret-manager.md)]

## <a name="connect-to-the-app-configuration-store"></a>Conexión al almacén de App Configuration

1. Ejecute el siguiente comando para agregar un paquete NuGet [Microsoft.Azure.AppConfiguration.AspNetCore](https://www.nuget.org/packages/Microsoft.Azure.AppConfiguration.AspNetCore) de referencia:

    ```dotnetcli
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore
    ```

1. Ejecute el siguiente comando en el mismo directorio que el archivo *.csproj*. El comando usa el Administrador de secretos para almacenar un secreto denominado `ConnectionStrings:AppConfig`, que almacena la cadena de conexión para el almacén de App Configuration. Reemplace el marcador de posición `<your_connection_string>` por la cadena de conexión del almacén de App Configuration. Puede encontrar la cadena de conexión en **Claves de acceso** en Azure Portal.

    ```dotnetcli
    dotnet user-secrets set ConnectionStrings:AppConfig "<your_connection_string>"
    ```

    > [!IMPORTANT]
    > Algunos shells truncarán la cadena de conexión, salvo que esté entre comillas. Asegúrese de que la salida del comando `dotnet user-secrets` muestra toda la cadena de conexión. Si no lo hace, vuelva a ejecutar el comando y escriba la cadena de conexión entre comillas.

    Secret Manager solo se utiliza para probar la aplicación web localmente. Cuando la aplicación se implementa en [Azure App Service](https://azure.microsoft.com/services/app-service/web), usará el valor de la aplicación configuración de la aplicación **Cadenas de conexión** en App Service, en lugar de Administrador de secretos para almacenar la cadena de conexión.

    Obtenga acceso a este secreto mediante la API de configuración de .NET Core. Un signo de dos puntos (`:`) funciona en el nombre de configuración con la API de configuración en todas las plataformas compatibles. Para más información, vea [Claves y valores de configuración](/aspnet/core/fundamentals/configuration#configuration-keys-and-values).

1. En *Program.cs*, agregue una referencia al espacio de nombres de la API de configuración de .NET Core:

    ```csharp
    using Microsoft.Extensions.Configuration;
    ```

1. Actualice el método `CreateWebHostBuilder` para usar App Configuration; para ello, llame al método `AddAzureAppConfiguration`.

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
                    config.AddAzureAppConfiguration(connection);
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
                config.AddAzureAppConfiguration(connection);
            })
            .UseStartup<Startup>();
    ```

    ---

    Con el cambio anterior, el [proveedor de configuración de App Configuration](https://go.microsoft.com/fwlink/?linkid=2074664) se ha registrado con la API de configuración de .NET Core.

## <a name="read-from-the-app-configuration-store"></a>Lectura desde el almacén de App Configuration

Complete los pasos siguientes para leer y mostrar los valores almacenados en el almacén de App Configuration. La API de configuración de .NET Core se usará para tener acceso al almacén. Sintaxis Razor se usará para mostrar los valores de las claves.

Abra el archivo *\<app root>/Views/Home/Index.cshtml* y reemplace el contenido por el código siguiente:

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<style>
    body {
        background-color: @Configuration["TestApp:Settings:BackgroundColor"]
    }
    h1 {
        color: @Configuration["TestApp:Settings:FontColor"];
        font-size: @Configuration["TestApp:Settings:FontSize"]px;
    }
</style>

<h1>@Configuration["TestApp:Settings:Message"]</h1>
```

En el código anterior, las claves del almacén de App Configuration se usan como sigue:

* El valor de la clave `TestApp:Settings:BackgroundColor` se asigna a la propiedad `background-color` de CSS.
* El valor de la clave `TestApp:Settings:FontColor` se asigna a la propiedad `color` de CSS.
* El valor de la clave `TestApp:Settings:FontSize` se asigna a la propiedad `font-size` de CSS.
* El valor de la clave `TestApp:Settings:Message` se muestra como un título.

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

1. Para compilar la aplicación mediante la CLI de .NET Core, desplácese hasta el directorio raíz del proyecto. Ejecute el comando siguiente en el shell de comandos:

    ```dotnetcli
    dotnet build
    ```

1. Una vez que la compilación se haya realizado correctamente, ejecute el siguiente comando para ejecutar la aplicación web localmente:

    ```dotnetcli
    dotnet run
    ```

1. Si trabaja en un equipo local, use un explorador para ir a `http://localhost:5000`. Esta es la dirección URL predeterminada de la aplicación web hospedada localmente. Si trabajando en Azure Cloud Shell, seleccione el botón **Vista previa web** y después **Configurar**.

    ![Busque el botón Vista previa web](./media/quickstarts/cloud-shell-web-preview.png)

    Cuando se le pida que configure el puerto para la versión preliminar, escriba *5000* y seleccione **Abrir y examinar**. En la página web aparecerá "Data from Azure App Configuration" (Datos de Azure App Configuration).

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE[Azure App Configuration cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido:

* Ha aprovisionado un nuevo almacén de App Configuration.
* Ha registrado el proveedor de configuración de .NET Core del almacén de App Configuration.
* Ha leído las claves del almacén de App Configuration con el proveedor de configuración.
* Ha mostrado los valores de la clave del almacén de App Configuration mediante sintaxis Razor.

Para aprender a configurar una aplicación de ASP.NET Core para actualizar dinámicamente la configuración, vaya al siguiente tutorial.

> [!div class="nextstepaction"]
> [Habilitación de la configuración dinámica](./enable-dynamic-configuration-aspnet-core.md)
