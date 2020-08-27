---
title: Registro de las extensiones de enlace de Azure Functions
description: Aprenda a registrar una extensión de enlace de Azure Functions basada en su entorno.
author: craigshoemaker
ms.topic: reference
ms.date: 07/08/2019
ms.author: cshoe
ms.openlocfilehash: 43bc278ea3cbd14690f1a9ac9263872536b5b174
ms.sourcegitcommit: 152c522bb5ad64e5c020b466b239cdac040b9377
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2020
ms.locfileid: "88224788"
---
# <a name="register-azure-functions-binding-extensions"></a>Registro de las extensiones de enlace de Azure Functions

A partir de la versión 2.x de Azure Functions, los [enlaces](./functions-triggers-bindings.md) están disponibles como paquetes independientes del entorno de ejecución de Functions. Aunque las funciones de .NET acceden a los enlaces a través de paquetes NuGet, los grupos de extensiones permiten a otras funciones acceder a todos los enlaces mediante una opción de configuración.

Tenga en cuenta los siguientes elementos relacionados con las extensiones de enlaces:

- Las extensiones de enlaces no están registradas explícitamente en Functions 1.x, excepto cuando [se crea una biblioteca de clases en C# mediante Visual Studio](#local-csharp).

- De forma predeterminada, se admiten los desencadenadores de HTTP y de temporizador y no requieren una extensión.

En la tabla siguiente se indica cuándo y cómo registrar los enlaces.

| Entorno de desarrollo |Registro<br/> en Functions 1.x  |Registro<br/> en Functions 3.x/2.x  |
|-------------------------|------------------------------------|------------------------------------|
|Portal de Azure|Automático|Automático<sup>*</sup>|
|Desarrollo en lenguajes que no son .NET o en Azure Functions Core Tools local|Automático|[Uso de Azure Functions Core Tools y agrupaciones de extensiones](#extension-bundles)|
|Biblioteca de clases de C# con Visual Studio|[Uso de herramientas NuGet](#vs)|[Uso de herramientas NuGet](#vs)|
|Biblioteca de clases de C# con Visual Studio Code|N/D|[Uso de la CLI de .NET Core](#vs-code)|

<sup>*</sup> El portal usa conjuntos de extensiones.

## <a name="extension-bundles"></a><a name="extension-bundles"></a>Conjuntos de extensiones

Los conjuntos de extensiones son una manera de agregar un conjunto compatible de extensiones de enlace de Functions a la aplicación de funciones. Cuando se usan conjuntos, se agrega un conjunto predefinido de extensiones al compilar la aplicación. Los paquetes de extensiones definidos en un conjunto se comprueban para saber si son compatibles entre sí, lo que ayuda a evitar conflictos entre ellos. Los conjuntos de extensiones evitan tener que publicar el código del proyecto .NET con un proyecto de funciones que no sea .NET. Las agrupaciones de extensiones se habilitan en el archivo host.json de la aplicación.  

Puede usar agrupaciones de extensiones con la versión 2.x y versiones posteriores del tiempo de ejecución de Functions. 

Use agrupaciones de extensiones para todo el desarrollo local con Azure Functions Core Tools o Visual Studio Code. Cuando desarrolle localmente, asegúrese de que está utilizando la versión más reciente de [Azure Functions Core Tools](functions-run-local.md#v2). Los conjuntos de extensiones también se usan al desarrollar funciones en Azure Portal. 

Si no usa conjuntos de extensiones, debe instalar el SDK de .NET Core 2.x en el equipo local antes de [instalar explícitamente las extensiones de enlace](#explicitly-install-extensions). Al proyecto se agrega un archivo extensions.csproj, que define explícitamente las extensiones necesarias. Los conjuntos de extensiones permiten eliminar este requisito para el desarrollo local. 

Para usar agrupaciones de extensiones, actualice el archivo *host.json* para que incluya la siguiente entrada para `extensionBundle`:
 
[!INCLUDE [functions-extension-bundles-json](../../includes/functions-extension-bundles-json.md)]

## <a name="explicitly-install-extensions"></a>Instalación explícita de extensiones

[!INCLUDE [functions-extension-register-core-tools](../../includes/functions-extension-register-core-tools.md)]

## <a name="nuget-packages"></a><a name="local-csharp"></a>Paquetes NuGet

En un proyecto de funciones basados en la biblioteca de clases de C#, se deben instalar las extensiones directamente. Los conjuntos de extensiones se han diseñado específicamente para proyectos que no están basados en la biblioteca de clases de C#.

### <a name="c-class-library-with-visual-studio"></a><a name="vs"></a> Biblioteca de clases de C\# con Visual Studio

En **Visual Studio**, puede instalar paquetes desde la Consola del Administrador de paquetes mediante el comando [Install-Package](/nuget/tools/ps-ref-install-package), tal como se muestra en el ejemplo siguiente:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <TARGET_VERSION>
```

El nombre del paquete que se usa para un enlace determinado se proporciona en el artículo de referencia de ese enlace. Para obtener un ejemplo, consulte la [sección de paquetes del artículo de referencia de enlace de Service Bus](functions-bindings-service-bus.md#functions-1x).

Reemplace `<TARGET_VERSION>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

Si usa `Install-Package` para hacer referencia a un enlace, no es necesario usar [agrupaciones de extensiones](#extension-bundles). Este enfoque es específico de las bibliotecas de clases compiladas en Visual Studio.

## <a name="c-class-library-with-visual-studio-code"></a><a name="vs-code"></a> Biblioteca de clases de C# con Visual Studio Code

En **Visual Studio Code**, instale paquetes para un proyecto de biblioteca de clases de C# desde el símbolo del sistema mediante el comando [dotnet add package](/dotnet/core/tools/dotnet-add-package) de la CLI de .NET Core. En el ejemplo siguiente se muestra cómo agregar un enlace:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.<BINDING_TYPE_NAME> --version <TARGET_VERSION>
```

La CLI de .NET Core solo puede utilizarse para el desarrollo de Azure Functions 2.x.

Reemplace `<BINDING_TYPE_NAME>` por el nombre del paquete que contiene el enlace que necesita. Puede encontrar el artículo de referencia de enlace deseado en la [lista de enlaces admitidos](./functions-triggers-bindings.md#supported-bindings).

Reemplace `<TARGET_VERSION>` en el ejemplo con una versión específica del paquete, como `3.0.0-beta5`. Las versiones válidas se enumeran en las páginas individuales del paquete en [NuGet.org](https://nuget.org). Las versiones principales que corresponden al tiempo de ejecución de Functions 1.x o 2.x se especifican en el artículo de referencia del enlace.

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Ejemplo de desencadenador y enlace de Azure Functions](./functions-bindings-example.md)
