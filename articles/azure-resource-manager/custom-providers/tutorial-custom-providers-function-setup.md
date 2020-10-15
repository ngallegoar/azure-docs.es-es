---
title: Configuración de Azure Functions
description: En este tutorial se explica cómo crear una aplicación de funciones de Azure y cómo configurarla para que funcione con proveedores personalizados de Azure.
author: jjbfour
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 6e11e08d861538ddf395f522495798546e04ca7c
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2020
ms.locfileid: "91948959"
---
# <a name="set-up-azure-functions-for-azure-custom-providers"></a>Configuración de Azure Functions para los proveedores personalizados de Azure

Un proveedor personalizado es un contrato entre Azure y un punto de conexión. Con los proveedores personalizados, puede cambiar los flujos de trabajo en Azure. En este tutorial se muestra cómo configurar una aplicación de funciones de Azure para que funcione como un punto de conexión del proveedor personalizado.

## <a name="create-the-azure-function-app"></a>Creación de la aplicación de funciones de Azure

> [!NOTE]
> En este tutorial, va a crear un punto de conexión de servicio sencillo que usa una aplicación de funciones de Azure. Sin embargo, un proveedor personalizado puede usar cualquier punto de conexión accesible públicamente. Entre las alternativas se incluyen Azure Logic Apps, Azure API Management y la característica Web Apps de Azure App Service.

Para iniciar este tutorial, debe seguir el tutorial [Creación de su primera función en Azure Portal](../../azure-functions/functions-create-first-azure-function.md). El tutorial crea una función de webhook de .NET Core que puede modificarse en Azure Portal. También es la base del tutorial actual.

## <a name="install-azure-table-storage-bindings"></a>Instalación de enlaces de Azure Table Storage

Para instalar los enlaces de Azure Table Storage:

1. Vaya a la pestaña **Integrar** para buscar HttpTrigger.
1. Seleccione **+ Nueva entrada**.
1. Seleccione **Azure Table Storage**.
1. Instale la extensión Microsoft.Azure.WebJobs.Extensions.Storage si aún no lo está.
1. En el cuadro **Nombre de parámetro de tabla**, escriba **tableStorage**.
1. En el cuadro **Nombre de tabla**, escriba **myCustomResources**.
1. Seleccione **Guardar** para almacenar el parámetro de entrada actualizado.

![Información general del proveedor personalizado que muestra enlaces de tabla](./media/create-custom-provider/azure-functions-table-bindings.png)

## <a name="update-restful-http-methods"></a>Actualización de los métodos HTTP de RESTful

Para configurar la función de Azure para que incluya los métodos de solicitud de RESTful del proveedor personalizado:

1. Vaya a la pestaña **Integrar** para buscar HttpTrigger.
1. En **Métodos HTTP seleccionados**, seleccione **GET**, **POST**, **DELETE** y **PUT**.

![Información general del proveedor personalizado que muestra métodos HTTP](./media/create-custom-provider/azure-functions-http-methods.png)

## <a name="add-azure-resource-manager-nuget-packages"></a>Adición de paquetes NuGet de Azure Resource Manager

> [!NOTE]
> Si falta el archivo del proyecto de C# en el directorio del proyecto, puede agregarlo manualmente. O aparecerá después de instalar la extensión Microsoft.Azure.WebJobs.Extensions.Storage en la aplicación de funciones.

Después, actualice el archivo del proyecto de C# para incluir útiles bibliotecas NuGet. Estas bibliotecas facilitan el análisis de las solicitudes entrantes de los proveedores personalizados. Siga los pasos para [agregar extensiones desde el portal](../../azure-functions/functions-bindings-register.md) y actualice el archivo del proyecto de C# para incluir las siguientes referencias del paquete:

```xml
<PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
<PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
<PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
```

El siguiente elemento XML es un archivo del proyecto de C# de ejemplo:

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <WarningsAsErrors />
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.Storage" Version="3.0.4" />
    <PackageReference Include="Microsoft.Azure.Management.ResourceManager.Fluent" Version="1.22.2" />
    <PackageReference Include="Microsoft.Azure.WebJobs.Script.ExtensionsMetadataGenerator" Version="1.1.*" />
  </ItemGroup>
</Project>
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, va a configurar una aplicación de funciones de Azure para que funcione como un punto de conexión de proveedor personalizado de Azure.

Para aprender a crear un punto de conexión del proveedor personalizado de RESTful, consulte [Tutorial: Creación de un punto de conexión de proveedor personalizado de RESTful](./tutorial-custom-providers-function-authoring.md).