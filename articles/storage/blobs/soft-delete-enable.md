---
title: Habilitación y administración de la eliminación temporal para blobs
titleSuffix: Azure Storage
description: Habilitación de la eliminación temporal para objetos de blob a fin de recuperar sus datos con mayor facilidad al modificarse o eliminarse estos de forma errónea.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/15/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 970a6dfc167a6bef7984598c60e7ce89c6e4b34c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84463729"
---
# <a name="enable-and-manage-soft-delete-for-blobs"></a>Habilitación y administración de la eliminación temporal para blobs

La eliminación temporal evita que los datos de blob se modifiquen o eliminen de forma accidental o errónea. Cuando la eliminación temporal está habilitada para una cuenta de almacenamiento, los blobs, las versiones del blob (versión preliminar) y las instantáneas de esa cuenta de almacenamiento pueden recuperarse una vez eliminados, dentro de un período de retención que especifique.

Si existe la posibilidad de que una aplicación u otro usuario de la cuenta de almacenamiento modifiquen o borren sus datos accidentalmente, Microsoft aconseja activar la eliminación temporal.

En este artículo se muestra cómo empezar a trabajar con la eliminación temporal.

## <a name="enable-soft-delete"></a>Habilitación de la eliminación temporal

# <a name="portal"></a>[Portal](#tab/azure-portal)

Habilite la eliminación temporal de blobs en la cuenta de almacenamiento mediante Azure Portal:

1. En [Azure Portal](https://portal.azure.com/), seleccione la cuenta de almacenamiento. 

2. Vaya a la opción **Protección de datos** en **Blob service**.

3. Haga clic en **Habilitado** en **Eliminación temporal del blob**

4. Escriba el número de días en *Retener durante* en **Directivas de retención**

5. Elija el botón **Guardar** para confirmar la configuración de protección de datos.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-configuration.png)

Para ver los blobs eliminados temporalmente, active la casilla **Mostrar blobs eliminados**.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted.png)

Para ver instantáneas eliminadas temporalmente de un blob dado, seleccione el blob y haga clic en **Ver instantáneas**.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted-snapshots.png)

Asegúrese de que la casilla **Mostrar instantáneas eliminadas** está activada.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-view-soft-deleted-snapshots-check.png)

Al hacer clic en un blob o en una instantánea eliminados temporalmente, observe las nuevas propiedades del blob. Indican cuándo se eliminó el objeto y el número de días que quedan hasta que el blob o la instantánea de blob expire de forma permanente. Si el objeto que se ha eliminado temporalmente no es una instantánea, también tendrá la opción de recuperarlo.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-properties.png)

Recuerde que al recuperar un blob también se recuperan todas las instantáneas asociadas. Para recuperar las instantáneas eliminadas temporalmente de un blob activo, haga clic en el blob y seleccione **Undelete all snapshots** (Recuperar todas las instantáneas).

![](media/soft-delete-enable/storage-blob-soft-delete-portal-undelete-all-snapshots.png)

Después de recuperar las instantáneas de un blob, puede hacer clic en **Promover** para copiar una instantánea en el blob raíz, con lo que se restaura el blob en la instantánea.

![](media/soft-delete-enable/storage-blob-soft-delete-portal-promote-snapshot.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Para habilitar la eliminación temporal, actualice las propiedades del servicio del cliente del blob. En el ejemplo siguiente se habilita la eliminación temporal en un subconjunto de cuentas de una suscripción:

```powershell
Set-AzContext -Subscription "<subscription-name>"
$MatchingAccounts = Get-AzStorageAccount | where-object{$_.StorageAccountName -match "<matching-regex>"}
$MatchingAccounts | Enable-AzStorageDeleteRetentionPolicy -RetentionDays 7
```

Puede comprobar que la eliminación temporal se ha activado mediante el siguiente comando:

```powershell
$MatchingAccounts | $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context | Select-Object -ExpandProperty DeleteRetentionPolicy
```

Para recuperar los blobs que se eliminaron accidentalmente, llame a Undelete en ellos. Recuerde que al llamar a **Undelete Blob**, tanto en los blobs activos como en los eliminados temporalmente, restaurará todas las instantáneas asociadas eliminadas temporalmente como activas. En el ejemplo siguiente se llama a Undelete en todos los blobs activos y eliminados temporalmente de un contenedor:

```powershell
# Create a context by specifying storage account name and key
$ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Get the blobs in a given container and show their properties
$Blobs = Get-AzStorageBlob -Container $StorageContainerName -Context $ctx -IncludeDeleted
$Blobs.ICloudBlob.Properties

# Undelete the blobs
$Blobs.ICloudBlob.Undelete()
```
Para buscar la directiva de retención de eliminación temporal actual, use el siguiente comando:

```azurepowershell-interactive
   $account = Get-AzStorageAccount -ResourceGroupName myresourcegroup -Name storageaccount
   Get-AzStorageServiceProperty -ServiceType Blob -Context $account.Context
```

# <a name="cli"></a>[CLI](#tab/azure-CLI)

Para habilitar la eliminación temporal, actualice las propiedades del servicio del cliente del blob:

```azurecli-interactive
az storage blob service-properties delete-policy update --days-retained 7  --account-name mystorageaccount --enable true
```

Para comprobar si la opción eliminación temporal está activada, use el siguiente comando: 

```azurecli-interactive
az storage blob service-properties delete-policy show --account-name mystorageaccount 
```

# <a name="python"></a>[Python](#tab/python)

Para habilitar la eliminación temporal, actualice las propiedades del servicio del cliente del blob:

```python
# Make the requisite imports
from azure.storage.blob import BlockBlobService
from azure.storage.common.models import DeleteRetentionPolicy

# Initialize a block blob service
block_blob_service = BlockBlobService(
    account_name='<enter your storage account name>', account_key='<enter your storage account key>')

# Set the blob client's service property settings to enable soft delete
block_blob_service.set_blob_service_properties(
    delete_retention_policy=DeleteRetentionPolicy(enabled=True, days=7))
```

# <a name="net-v12-sdk"></a>[SDK de .NET, versión 12](#tab/dotnet)

Para habilitar la eliminación temporal, actualice las propiedades del servicio del cliente del blob:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_EnableSoftDelete":::

Para recuperar los blobs que se eliminaron accidentalmente, llame a Undelete en ellos. Recuerde que al llamar a **Recuperar**, tanto en los blobs activos como en los eliminados temporalmente, restaurará todas las instantáneas asociadas eliminadas temporalmente como activas. En el ejemplo siguiente se llama a Undelete en todos los blobs activos y eliminados temporalmente de un contenedor:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RecoverDeletedBlobs":::

Para recuperar hasta una versión específica del blob, primero llame a Undelete en un blob y, después, copie la instantánea deseada sobre el blob. En el ejemplo siguiente se recupera un blob en bloques a su última instantánea generada:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/DataProtection.cs" id="Snippet_RecoverSpecificBlobVersion":::

# <a name="net-v11-sdk"></a>[SDK de .NET, versión 11](#tab/dotnet11)

Para habilitar la eliminación temporal, actualice las propiedades del servicio del cliente del blob:

```csharp
// Get the blob client's service property settings
ServiceProperties serviceProperties = blobClient.GetServiceProperties();

// Configure soft delete
serviceProperties.DeleteRetentionPolicy.Enabled = true;
serviceProperties.DeleteRetentionPolicy.RetentionDays = RetentionDays;

// Set the blob client's service property settings
blobClient.SetServiceProperties(serviceProperties);
```

Para recuperar los blobs que se eliminaron accidentalmente, llame a Undelete en ellos. Recuerde que al llamar a **Recuperar**, tanto en los blobs activos como en los eliminados temporalmente, restaurará todas las instantáneas asociadas eliminadas temporalmente como activas. En el ejemplo siguiente se llama a Undelete en todos los blobs activos y eliminados temporalmente de un contenedor:

```csharp
// Recover all blobs in a container
foreach (CloudBlob blob in container.ListBlobs(useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Deleted))
{
       await blob.UndeleteAsync();
}
```

Para recuperar hasta una versión específica del blob, primero llame a Undelete en un blob y, después, copie la instantánea deseada sobre el blob. En el ejemplo siguiente se recupera un blob en bloques a su última instantánea generada:

```csharp
// Undelete
await blockBlob.UndeleteAsync();

// List all blobs and snapshots in the container prefixed by the blob name
IEnumerable<IListBlobItem> allBlobVersions = container.ListBlobs(
    prefix: blockBlob.Name, useFlatBlobListing: true, blobListingDetails: BlobListingDetails.Snapshots);

// Restore the most recently generated snapshot to the active blob    
CloudBlockBlob copySource = allBlobVersions.First(version => ((CloudBlockBlob)version).IsSnapshot &&
    ((CloudBlockBlob)version).Name == blockBlob.Name) as CloudBlockBlob;
blockBlob.StartCopy(copySource);
```  

---

## <a name="next-steps"></a>Pasos siguientes

- [Eliminación temporal para Blob Storage](soft-delete-overview.md)
- [Versiones de blobs (versión preliminar)](versioning-overview.md)