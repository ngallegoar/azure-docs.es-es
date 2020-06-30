---
title: 'Inicio rápido: biblioteca cliente de Azure Key Vault para .NET (SDK v3)'
description: Obtenga información sobre cómo crear, recuperar y eliminar secretos desde una instancia de Azure Key Vault mediante la biblioteca cliente de .NET (v3)
author: msmbaldwin
ms.author: mbaldwin
ms.date: 11/05/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: 0dba746379a6b5338c9e67fe312c392f37399012
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2020
ms.locfileid: "85125794"
---
# <a name="quickstart-azure-key-vault-client-library-for-net-sdk-v3"></a>Inicio rápido: Biblioteca cliente de Azure Key Vault para .NET (SDK v3)

Comience con la biblioteca cliente de Azure Key Vault para .NET. Siga estos pasos para instalar el paquete y probar el código de ejemplo para realizar tareas básicas.

> [!NOTE]
> En esta guía de inicio rápido se usa la versión v3.0.4 de la biblioteca cliente Microsoft.Azure.KeyVault. Para usar la versión más actualizada de la biblioteca cliente de Key Vault, consulte [Biblioteca cliente de Azure Key Vault para .NET (SDK v4)](quick-create-net.md). 

Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Use la biblioteca cliente de Key Vault para .NET para:

- Aumentar la seguridad y el control sobre claves y contraseñas
- Crear e importar claves de cifrado en minutos
- Reducir la latencia con escala en la nube y redundancia global
- Simplificar y automatizar tareas para certificados TLS/SSL
- Utilizar módulos HSM con certificación FIPS 140-2 nivel 2

[Documentación de referencia de la API](/dotnet/api/overview/azure/key-vault?view=azure-dotnet) | [Código fuente de la biblioteca](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault) | [Paquete (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/)

> [!NOTE]
> Cada almacén de claves debe tener un nombre único. Reemplace <nombre-almacén de claves-único> por el nombre del almacén de claves en los ejemplos siguientes.


## <a name="prerequisites"></a>Requisitos previos

* Una suscripción a Azure: [cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [SDK de .NET Core 3.1 o posterior](https://dotnet.microsoft.com/download/dotnet-core/3.1).
* [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) o [Azure PowerShell](/powershell/azure/overview)

En este inicio rápido se supone que está ejecutando `dotnet`, la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest) y comandos de Windows en un terminal de Windows (como [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-6), [Windows PowerShell](/powershell/scripting/install/installing-windows-powershell?view=powershell-6) o [Azure Cloud Shell](https://shell.azure.com/)).

## <a name="setting-up"></a>Instalación

### <a name="create-new-net-console-app"></a>Creación de una aplicación de consola de .NET

En una ventana de consola, utilice el comando `dotnet new` para crear una nueva aplicación de consola de .NET con el nombre `akv-dotnet`.


```console
dotnet new console -n akvdotnet
```

Cambie el directorio a la carpeta de aplicaciones recién creada. Para compilar la aplicación:

```console
dotnet build
```

La salida de la compilación no debe contener advertencias ni errores.

```console
Build succeeded.
 0 Warning(s)
 0 Error(s)
```

### <a name="install-the-package"></a>Instalar el paquete

En la ventana de la consola, instale la biblioteca cliente de Azure Key Vault para .NET:

```console
dotnet add package Microsoft.Azure.KeyVault
```

Para este inicio rápido, deberá instalar también los siguientes paquetes:

```console
dotnet add package System.Threading.Tasks
dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory
dotnet add package Microsoft.Azure.Management.ResourceManager.Fluent
```

### <a name="create-a-resource-group-and-key-vault"></a>Creación de un grupo de recursos y de un almacén de claves

En este inicio rápido se usa un almacén de claves de Azure creado previamente. Puede crear un almacén de claves siguiendo los pasos descritos en el [inicio rápido de CLI de Azure](quick-create-cli.md), [inicio rápido de Azure PowerShell](quick-create-powershell.md) o [inicio rápido de Azure Portal](quick-create-portal.md). Como alternativa, puede ejecutar simplemente los siguientes comandos de la CLI de Azure.

> [!Important]
> Cada almacén de claves debe tener un nombre único. Reemplace <nombre-almacén de claves-único> por el nombre del almacén de claves en los ejemplos siguientes.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

### <a name="create-a-service-principal"></a>Creación de una entidad de servicio

La manera más sencilla de autenticar una aplicación .NET basada en la nube es con una identidad administrada. Consulte [Uso de identidades administradas de App Service para acceder a Azure Key Vault](../general/managed-identity.md) para más información. 

Sin embargo, en aras de la simplicidad, en este inicio rápido se crea una aplicación de consola .NET, que requiere el uso de una entidad de servicio y una directiva de control de acceso. La entidad de servicio requiere un nombre único con el formato "http://&lt;my-unique-service-principal-name&gt;".

Cree una entidad de servicio mediante el comando [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) de la CLI de Azure:

```azurecli
az ad sp create-for-rbac -n "http://&lt;my-unique-service-principal-name&gt;" --sdk-auth
```

Esta operación devolverá una serie de pares clave-valor. 

```console
{
  "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
  "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
  "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
  "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Tome nota de los valores de clientId y clientSecret, ya que los usaremos en el paso [Autenticación en el almacén de claves](#authenticate-to-your-key-vault) que se indica a continuación.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Acceso de la entidad de servicio al almacén de claves

Cree una directiva de acceso para el almacén de claves que conceda permiso a la entidad de servicio pasando clientId al comando [az keyvault set-policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy). Proporcione a la entidad de servicio los permisos get, list y set para las claves y los secretos.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

## <a name="object-model"></a>Modelo de objetos

La biblioteca cliente de Azure Key Vault para .NET le permite administrar las claves y los recursos relacionados, como certificados y secretos. Los ejemplos de código siguientes le mostrarán cómo establecer y recuperar un secreto.

Toda la aplicación de consola está disponible en https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/akvdotnet.

## <a name="code-examples"></a>Ejemplos de código

### <a name="add-directives"></a>Adición de directivas

Agregue las siguientes directivas al principio del código:

[!code-csharp[Directives](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=directives)]

### <a name="authenticate-to-your-key-vault"></a>Autenticación en el almacén de claves

Este inicio rápido de .NET se basa en las variables de entorno para almacenar las credenciales que no deberían incluirse en el código. 

Antes de crear y ejecutar la aplicación, use el comando `setx` para establecer las variables de entorno `akvClientId`, `akvClientSecret`, `akvTenantId` y `akvSubscriptionId` en los valores que anotó anteriormente.

**Windows**

```console
setx akvClientId "<your-clientID>"
setx akvClientSecret "<your-clientSecret>"
```

**Linux**

```bash
export akvClientId = "<your-clientID>"
export akvClientSecret = "<your-clientSecret>"
```

**macOS**

```bash
export akvClientId = "<your-clientID>"
export akvClientSecret = "<your-clientSecret>"
```

Asigne estas variables de entorno a las cadenas del código y, a continuación, autentique la aplicación pasándola a la [clase KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient):

[!code-csharp[Authentication](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=authentication)]

### <a name="save-a-secret"></a>Almacenamiento de un secreto

Ahora que la aplicación se ha autenticado, puede colocar un secreto en el almacén de claves mediante el [método SetSecretAsync](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.setsecretasync). Esto requiere la dirección URL del almacén de claves, que tiene el formato `https://<your-unique-keyvault-name>.vault.azure.net/secrets/`. También requiere un nombre para el secreto: usamos "mySecret". 

[!code-csharp[Set secret](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=setsecret)]

Puede comprobar que el secreto se ha establecido con el comando [az keyvault secret show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show):

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Recuperación de un secreto

Ahora puede recuperar el valor previamente establecido con el [método GetSecretAsync](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync).

[!code-csharp[Get secret](~/samples-key-vault-dotnet-quickstart/akvdotnet/Program.cs?name=getsecret)]

El secreto se guarda ahora como `keyvaultSecret.Value;`.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, puede usar la CLI de Azure o Azure PowerShell para quitar el almacén de claves y el grupo de recursos correspondiente.

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado un almacén de claves y ha almacenado un secreto en él. Consulte la [aplicación de consola completa en GitHub](https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/akvdotnet).

Para más información sobre Key Vault y cómo integrarlo con las aplicaciones, continúe con los artículos siguientes.

- Implemente [Autenticación entre servicios en Azure Key Vault mediante .NET](../general/service-to-service-authentication.md).
- Lea una [introducción a Azure Key Vault](../general/overview.md).
- Consulte la [guía del desarrollador de Azure Key Vault](../general/developers-guide.md).
- Consulte los [procedimientos recomendados de Azure Key Vault](../general/best-practices.md).
