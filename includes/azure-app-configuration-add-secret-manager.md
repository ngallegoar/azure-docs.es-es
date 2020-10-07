---
ms.openlocfilehash: c3c5b8ef94b507cad433e587c9ebfc2ec16c0ff9
ms.sourcegitcommit: 3792cf7efc12e357f0e3b65638ea7673651db6e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91440425"
---
## <a name="add-secret-manager"></a>Incorporación de Secret Manager

Una herramienta denominada Administrador de secretos almacena información confidencial para el trabajo de desarrollo fuera de su árbol de proyecto. Este enfoque ayuda a evitar el uso compartido accidental de secretos de la aplicación en el código fuente. Complete los pasos siguientes para habilitar el uso del Administrador de secretos en el proyecto de ASP.NET Core:

#### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

Desplácese hasta el directorio raíz del proyecto y ejecute el siguiente comando para habilitar el almacenamiento de secretos en el proyecto:

```dotnetcli
dotnet user-secrets init
```

Se agrega un elemento `UserSecretsId` que contiene un GUID al archivo *.csproj*:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    
    <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

</Project>
```

#### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

1. Abra el archivo *.csproj*.

1. Agregue un elemento `UserSecretsId` al archivo *.csproj* como se muestra aquí. Puede usar el mismo identificador único global, o bien puede reemplazar este valor por el suyo propio.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    
        <PropertyGroup>
            <TargetFramework>netcoreapp2.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>
    
        <ItemGroup>
            <PackageReference Include="Microsoft.AspNetCore.App" />
            <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
        </ItemGroup>
    
    </Project>
    ```
    
1. Guarde el archivo *.csproj*.

---

> [!TIP]
> Para más información sobre el Administrador de secretos, consulte [Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core](/aspnet/core/security/app-secrets).
