---
title: Registro de las extensiones de enlace de Azure Functions
description: Aprenda a registrar una extensión de enlace de Azure Functions basada en su entorno.
author: craigshoemaker
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 1688fe848beb62731391bf4399a0dabec5265320
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235176"
---
# <a name="register-azure-functions-binding-extensions"></a>Registro de las extensiones de enlace de Azure Functions

En la versión de Azure Functions 2.x, los [enlaces](./functions-triggers-bindings.md) están disponibles como paquetes independientes de Functions Runtime. Aunque las funciones de .NET acceden a los enlaces a través de paquetes NuGet, los grupos de extensiones permiten a otras funciones acceder a todos los enlaces mediante una opción de configuración.

Tenga en cuenta los siguientes elementos relacionados con las extensiones de enlaces:

- Las extensiones de enlaces no están registradas explícitamente en Functions 1.x, excepto cuando [se crea una biblioteca de clases en C# mediante Visual Studio](#local-csharp).

- De forma predeterminada, se admiten los desencadenadores de HTTP y de temporizador y no requieren una extensión.

En la tabla siguiente se indica cuándo y cómo registrar los enlaces.

| Entorno de desarrollo |Registro<br/> en Functions 1.x  |Registro<br/> en Functions 2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Portal de Azure|Automático|Automático|
|Desarrollo en lenguajes que no son .NET o en Azure Functions Core Tools local|Automático|[Uso de Azure Functions Core Tools y agrupaciones de extensiones](#extension-bundles)|
|Biblioteca de clases de C# con Visual Studio|[Uso de herramientas NuGet](#vs)|[Uso de herramientas NuGet](#vs)|
|Biblioteca de clases de C# con Visual Studio Code|N/D|[Uso de la CLI de .NET Core](#vs-code)|

## <a name="extension-bundles-for-local-development"></a><a name="extension-bundles"></a>Agrupaciones de extensiones para el desarrollo local

Las agrupaciones de extensiones son una tecnología de implementación que permite agregar un conjunto compatible de extensiones de enlace de Functions a la aplicación de función. Al compilar la aplicación, se agrega un conjunto predefinido de extensiones. Los paquetes de extensiones definidos en una agrupación son compatibles entre sí, lo cual le ayuda a evitar conflictos entre ellos. Las agrupaciones de extensiones se habilitan en el archivo host.json de la aplicación.  

Puede usar agrupaciones de extensiones con la versión 2.x y versiones posteriores del tiempo de ejecución de Functions. Cuando desarrolle localmente, asegúrese de que está utilizando la versión más reciente de [Azure Functions Core Tools](functions-run-local.md#v2).

Use agrupaciones de extensiones para todo el desarrollo local con Azure Functions Core Tools o Visual Studio Code.

Si no usa agrupaciones de extensiones, deberá instalar el SDK de .NET Core 2.x en el equipo local antes de instalar las extensiones de enlaces. Las agrupaciones de extensiones permiten eliminar este requisito para el desarrollo local. 

Para usar agrupaciones de extensiones, actualice el archivo *host.json* para que incluya la siguiente entrada para `extensionBundle`:
 
[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

<a name="local-csharp"></a>

## <a name="c-class-library-with-visual-studio"></a><a name="vs"></a> Biblioteca de clases de C\# con Visual Studio

En **Visual Studio**, puede instalar paquetes desde la Consola del Administrador de paquetes mediante el comando [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package), tal como se muestra en el ejemplo siguiente:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

El nombre del paquete que se usa para un enlace determinado se proporciona en el artículo de referencia de ese enlace. Para obtener un ejemplo, consulte la [sección de paquetes del artículo de referencia de enlace de Service Bus](functions-bindings-service-bus.md#functions-1x).

Reemplace `<TARGET_VERSION>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

Si usa `Install-Package` para hacer referencia a un enlace, no es necesario usar [agrupaciones de extensiones](#extension-bundles). Este enfoque es específico de las bibliotecas de clases compiladas en Visual Studio.

## <a name="c-class-library-with-visual-studio-code"></a><a name="vs-code"></a> Biblioteca de clases de C# con Visual Studio Code

En **Visual Studio Code**, instale paquetes para un proyecto de biblioteca de clases de C# desde el símbolo del sistema mediante el comando [dotnet add package](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) de la CLI de .NET Core. En el ejemplo siguiente se muestra cómo agregar un enlace:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

La CLI de .NET Core solo puede utilizarse para el desarrollo de Azure Functions 2.x.

Reemplace `<BINDING_TYPE_NAME>` por el nombre del paquete que contiene el enlace que necesita. Puede encontrar el artículo de referencia de enlace deseado en la [lista de enlaces admitidos](./functions-triggers-bindings.md#supported-bindings).

Reemplace `<TARGET_VERSION>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Ejemplo de desencadenador y enlace de Azure Functions](./functions-bindings-example.md)
