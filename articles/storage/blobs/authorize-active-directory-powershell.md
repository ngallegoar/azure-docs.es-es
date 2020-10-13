---
title: Ejecución de comandos de PowerShell con credenciales de Azure AD para acceder a los datos de blob
titleSuffix: Azure Storage
description: PowerShell admite el inicio de sesión con credenciales de Azure AD para ejecutar comandos en los datos de blob de Azure Storage. Se proporciona un token de acceso para la sesión y se usa para autorizar operaciones de llamada. Los permisos dependen del rol de Azure asignado a la entidad de seguridad de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 09/14/2020
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: blobs
ms.openlocfilehash: cca0b197bdef04ffca9b71a7f394d3359023e2b7
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91715897"
---
# <a name="run-powershell-commands-with-azure-ad-credentials-to-access-blob-data"></a>Ejecución de comandos de PowerShell con credenciales de Azure AD para acceder a los datos de blob

Azure Storage proporciona extensiones para PowerShell que le permiten iniciar sesión y ejecutar comandos de scripting con credenciales de Azure Active Directory (Azure AD). Al iniciar sesión en PowerShell con credenciales de Azure AD, se devuelve un token de acceso OAuth 2.0. PowerShell usa automáticamente ese token para autorizar las operaciones de datos posteriores en Blob Storage. Para las operaciones admitidas, ya no necesita pasar una clave de cuenta o token SAS con el comando.

Puede asignar permisos en los datos de blob a una entidad de seguridad de Azure AD mediante el control de acceso basado en roles de Azure (Azure RBAC). Para más información sobre los roles de Azure en Azure Storage, consulte [cómo administrar los derechos de acceso a los datos de Azure Storage mediante Azure RBAC](../common/storage-auth-aad-rbac.md).

## <a name="supported-operations"></a>Operaciones compatibles

Las extensiones de Azure Storage se admiten para las operaciones en datos de blob. Las operaciones a las que podrá llamar dependerán de los permisos que se concedan a la entidad de seguridad de Azure AD con la que inicie sesión en PowerShell. Los permisos para contenedores de Azure Storage se asignan mediante Azure RBAC. Por ejemplo, si se le asigna el rol de **lector de datos de blob**, puede ejecutar comandos de scripting que lean datos de un contenedor. Si se le asigna el rol de **colaborador de datos de blob**, podrá ejecutar comandos de scripting que lean, escriban o eliminen un contenedor, o los datos que estos contienen.

Para más información sobre los permisos requeridos para cada operación de Azure Storage en un contenedor, consulte la sección [Llamadas a operaciones de almacenamiento con tokens de OAuth](/rest/api/storageservices/authorize-with-azure-active-directory#call-storage-operations-with-oauth-tokens).  

## <a name="call-powershell-commands-using-azure-ad-credentials"></a>Llamada a comandos de PowerShell mediante credenciales de Azure AD

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Si quiere usar Azure PowerShell para iniciar sesión y ejecutar operaciones posteriores en Azure Storage mediante credenciales de Azure AD, cree un contexto de almacenamiento para hacer referencia a la cuenta de almacenamiento e incluya el parámetro `-UseConnectedAccount`.

En el ejemplo siguiente se muestra cómo crear un contenedor en una nueva cuenta de almacenamiento desde Azure PowerShell mediante las credenciales de Azure AD. No olvide reemplazar los valores del marcador de posición entre corchetes angulares por sus propios valores:

1. Inicie sesión en su cuenta de Azure con el comando [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount):

    ```powershell
    Connect-AzAccount
    ```

    Para obtener más información sobre cómo iniciar sesión en Azure con PowerShell, consulte [Inicio de sesión con Azure PowerShell](/powershell/azure/authenticate-azureps).

1. Cree un grupo de recursos de Azure mediante una llamada a [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). 

    ```powershell
    $resourceGroup = "sample-resource-group-ps"
    $location = "eastus"
    New-AzResourceGroup -Name $resourceGroup -Location $location
    ```

1. Cree una cuenta de almacenamiento mediante una llamada a [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount).

    ```powershell
    $storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
      -Name "<storage-account>" `
      -SkuName Standard_LRS `
      -Location $location `
    ```

1. Obtenga el contexto de la cuenta de almacenamiento que especifica la nueva cuenta de almacenamiento mediante una llamada a [New-AzStorageContext](/powershell/module/az.storage/new-azstoragecontext). Cuando actúe en una cuenta de almacenamiento, puede hacer referencia al contexto en lugar de proporcionar varias veces las credenciales. Incluya el parámetro `-UseConnectedAccount` para llamar a las operaciones de datos posteriores con sus credenciales de Azure AD:

    ```powershell
    $ctx = New-AzStorageContext -StorageAccountName "<storage-account>" -UseConnectedAccount
    ```

1. Antes de crear el contenedor, asígnese a sí mismo el rol [Colaborador de datos de Storage Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor). Aunque sea el propietario de la cuenta, necesita permisos explícitos para realizar operaciones de datos en la cuenta de almacenamiento. Para más información sobre la asignación de roles de Azure, consulte [Uso de Azure Portal para asignar un rol de Azure para el acceso a datos de blobs y colas](../common/storage-auth-aad-rbac.md).

    > [!IMPORTANT]
    > La propagación de las asignaciones de roles de Azure puede tardar unos minutos.

1. Cree un contenedor mediante una llamada a [New-AzStorageContainer](/powershell/module/az.storage/new-azstoragecontainer). Dado que esta llamada usa el contexto creado en los pasos anteriores, el contenedor se crea con sus credenciales de Azure AD.

    ```powershell
    $containerName = "sample-container"
    New-AzStorageContainer -Name $containerName -Context $ctx
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Uso de PowerShell para asignar un rol de Azure para el acceso a datos de blobs y colas](../common/storage-auth-aad-rbac-powershell.md)
- [Autorización del acceso a datos de blobs y colas con identidades administradas para los recursos de Azure](../common/storage-auth-aad-msi.md)
