---
title: Inicio rápido de Azure App Configuration con .NET Framework | Microsoft Docs
description: En este artículo se crea una aplicación de .NET Framework con Azure App Configuration para centralizar el almacenamiento y la administración de la configuración de la aplicación de forma independiente del código.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
ms.service: azure-app-configuration
ms.custom: devx-track-csharp
ms.topic: quickstart
ms.date: 09/28/2020
ms.author: lcozzens
ms.openlocfilehash: d338ca5cf2c794f15d3eb535b5280208236d1e34
ms.sourcegitcommit: d9ba60f15aa6eafc3c5ae8d592bacaf21d97a871
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91767750"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Inicio rápido: Creación de una aplicación de .NET Framework con Azure App Configuration

En este inicio rápido incorporará Azure App Configuration a una aplicación de consola basada en .NET Framework para centralizar el almacenamiento y la administración de la configuración de la aplicación de forma independiente del código.

## <a name="prerequisites"></a>Prerrequisitos

- Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/dotnet)
- [Visual Studio 2019](https://visualstudio.microsoft.com/vs)
- [.NET Framework 4.7.2](https://dotnet.microsoft.com/download)

## <a name="create-an-app-configuration-store"></a>Creación de un almacén de App Configuration

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

7. Seleccione **Explorador de configuración** > **Crear** > **Clave-valor** para agregar los siguientes pares clave-valor:

    | Clave | Value |
    |---|---|
    | TestApp:Settings:Message | Datos de Azure App Configuration |

    Deje **Etiqueta** y **Tipo de contenido** en blanco, por ahora.

8. Seleccione **Aplicar**.

## <a name="create-a-net-console-app"></a>Creación de una aplicación de consola de .NET

1. Inicie Visual Studio y seleccione **Archivo** > **Nuevo** > **Proyecto**.

1. En **Crear un proyecto**, filtre por el tipo de proyecto **Consola** y haga clic en **Aplicación de consola (.NET Framework)** . Seleccione **Next** (Siguiente).

1. En **Configurar el nuevo proyecto**, asígnele al proyecto un nombre. En **Marco**, seleccione **.NET Framework 4.7.1** o una versión posterior. Seleccione **Crear**.

## <a name="connect-to-an-app-configuration-store"></a>Conexión a un almacén de App Configuration

1. Haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**. En la pestaña **Examinar**, busque y agregue los siguientes paquetes NuGet al proyecto.

    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 or later
    System.Configuration.ConfigurationManager version 4.6.0 or later
    ```

1. Actualice el archivo *App.config* del proyecto como sigue:

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   La cadena de conexión del almacén de App Configuration se lee en la variable de entorno `ConnectionString`. Agregue el generador de configuración `Environment` antes de `MyConfigStore` en la propiedad `configBuilders` de la sección `appSettings`.

1. Abra *Program.cs* y actualice el método `Main` para usar App Configuration mediante una llamada a `ConfigurationManager`.

    ```csharp
    static void Main(string[] args)
    {
        string message = System.Configuration.ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
        Console.ReadKey();
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Compilación y ejecución de la aplicación en un entorno local

1. Actualice el archivo **App.config** reemplazando `${ConnectionString}` por la cadena de conexión real a la instancia de configuración de la aplicación. Puede encontrarla en la pestaña **Claves de acceso** del recurso de configuración de la aplicación de Azure Portal.

1. Presione Ctrl + F5 para compilar y ejecutar la aplicación de consola.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado un almacén de App Configuration y lo ha usado con una aplicación de consola de .NET Framework. El valor `AppSettings` de `ConfigurationManager` no cambiará una vez iniciada la aplicación. La biblioteca del proveedor de configuraciones de .NET Standard de App Configuration, pero también se puede usar en una aplicación de .NET Framework. Para aprender a habilitar la aplicación de .NET Framework para actualizar dinámicamente la configuración, vaya al siguiente tutorial.

> [!div class="nextstepaction"]
> [Habilitación de la configuración dinámica](./enable-dynamic-configuration-dotnet.md)
