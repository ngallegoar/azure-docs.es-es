---
title: Administración del acceso de lectura público para contenedores y blobs.
titleSuffix: Azure Storage
description: Obtenga información acerca de cómo permitir el acceso anónimo a contenedores y blobs y cómo tener acceso a ellos mediante programación.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 05/05/2020
ms.author: tamram
ms.reviewer: ozgun
ms.openlocfilehash: 61f27d81cda2e57eb5730016dcc22252508fdd01
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84809003"
---
# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Administración del acceso de lectura anónimo a contenedores y blobs

Puede habilitar el acceso de lectura anónimo y público a un contenedor y sus blobs en Azure Blob Storage. Al hacerlo, puede conceder acceso de solo lectura a estos recursos sin compartir la clave de cuenta y sin necesidad de una firma de acceso compartido (SAS).

El acceso de lectura público es mejor para escenarios donde desea que ciertos blobs estén siempre disponibles para el acceso de lectura anónimo. Para un control más minucioso, puede crear una firma de acceso compartido. Las firmas de acceso compartido le permiten proporcionar acceso restringido con distintos permisos para un período específico. Para obtener más información sobre la creación de firmas de acceso compartido, vea [Uso de firmas de acceso compartido (SAS) en Azure Storage](../common/storage-sas-overview.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Concesión de permisos a usuarios anónimos a contenedores y blobs

De forma predeterminada, solo un usuario al que se han concedido los permisos adecuados puede acceder a un contenedor y a los blobs dentro de él. Para conceder a usuarios anónimos accedo de lectura a un contenedor y sus blobs, puede establecer el nivel de acceso público del contenedor. Cuando concede acceso público a un contenedor, los usuarios anónimos pueden leer los blobs dentro de un contenedor con acceso público sin necesidad de tener que autorizar la solicitud.

Puede configurar un contenedor con los permisos siguientes:

- **Sin acceso de lectura público:** Solo puede acceder al contenedor y a sus blobs el propietario de cuenta de almacenamiento. Este es el valor predeterminado para todos los contenedores nuevos.
- **Acceso de lectura público solo para blobs:** Los blobs dentro de este contenedor pueden leerse a través de una solicitud anónima, pero los datos del contenedor no están disponibles. Los clientes anónimos no pueden enumerar los blobs dentro del contenedor.
- **Acceso de lectura público para contenedores y blobs:** Todos los datos del contenedor y de los blobs se pueden leer mediante una solicitud anónima. Los clientes pueden enumerar los blobs del contenedor a través de una solicitud anónima, pero no pueden enumerar los contenedores que están en la cuenta de almacenamiento.

### <a name="set-container-public-access-level-in-the-azure-portal"></a>Establecer el nivel de acceso público de contenedor en Azure Portal

Desde [Azure Portal](https://portal.azure.com), puede actualizar el nivel de acceso público para uno o varios contenedores:

1. Vaya a la información general de su cuenta de almacenamiento en Azure Portal.
1. En **Blob service** en la hoja de menú, seleccione **Blobs**.
1. Seleccione los contenedores para el que quiere establecer el nivel de acceso público.
1. Use el botón **Cambiar nivel de acceso** para mostrar la configuración de acceso público.
1. Seleccione el nivel de acceso público deseado en la lista desplegable **Nivel de acceso público** y haga clic en el botón Aceptar para aplicar el cambio a los contenedores seleccionados.

En la siguiente captura de pantalla se muestra cómo cambiar el nivel de acceso público para los contenedores seleccionados.

![Captura de pantalla que muestra cómo establecer el nivel de acceso público en el portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

> [!NOTE]
> No puede cambiar el nivel de acceso público para un blob individual. El nivel de acceso público se establece en el nivel de contenedor.

### <a name="set-container-public-access-level-with-net"></a>Establecer el nivel de acceso público de contenedor con .NET

# <a name="net-v12-sdk"></a>[\.SDK de .NET, versión 12](#tab/dotnet)

Para establecer los permisos de un contenedor, llame al método [BlobContainerClient.SetAccessPolicy](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.setaccesspolicy?view=azure-dotnet). 

El ejemplo siguiente establece los permisos del contenedor para el acceso de lectura público completo. Para establecer permisos de acceso de lectura público solo para blobs, pase el campo **PublicAccessType.Blob** al método [BlobContainerClient.SetAccessPolicy](https://docs.microsoft.com/dotnet/api/azure.storage.blobs.blobcontainerclient.setaccesspolicy?view=azure-dotnet). Para quitar todos los permisos para los usuarios anónimos, use el campo **BlobContainerPublicAccessType.None**.

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_SetPublicContainerPermissions":::

# <a name="net-v11-sdk"></a>[\.SDK de .NET, versión 11](#tab/dotnet11)

Para establecer los permisos de un contenedor mediante la biblioteca cliente de Azure Storage para .NET, lo primero que debe hacer es recuperar los permisos existentes del contenedor llamando a uno de los siguientes métodos:

- [GetPermissions](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getpermissions)
- [GetPermissionsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.getpermissionsasync)

Luego, establezca la propiedad **PublicAccess** en el objeto [BlobContainerPermissions](/dotnet/api/microsoft.azure.storage.blob.blobcontainerpermissions) devuelto por el método **GetPermissions**.

Por último, llame a uno de los métodos siguientes para actualizar los permisos del contenedor:

- [SetPermissions](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissions)
- [SetPermissionsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.setpermissionsasync)

El ejemplo siguiente establece los permisos del contenedor para el acceso de lectura público completo. Para establecer permisos de acceso de lectura público solo para blobs, establezca la propiedad **PublicAccess** en **BlobContainerPublicAccessType.Blob**. Para quitar todos los permisos para los usuarios anónimos, establezca la propiedad en **BlobContainerPublicAccessType.Off**.

```csharp
private static async Task SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    await container.SetPermissionsAsync(permissions);

    Console.WriteLine("Container {0} - permissions set to {1}", container.Name, permissions.PublicAccess);
}
```

---

## <a name="access-containers-and-blobs-anonymously"></a>Acceso anónimo a contenedores y blobs

Un cliente que tiene acceso a contenedores y blobs de forma anónima puede utilizar constructores que no requieren credenciales. En los ejemplos siguientes se muestran varias maneras diferentes de hacer referencia a contenedores y blobs de forma anónima.

### <a name="create-an-anonymous-client-object"></a>Creación de un objeto de cliente anónimo

Puede crear un nuevo objeto de cliente de servicio para el acceso anónimo proporcionando el punto de conexión de almacenamiento de Blob para la cuenta. Sin embargo, también debe conocer el nombre de un contenedor en esa cuenta que esté disponible para el acceso anónimo.

# <a name="net-v12-sdk"></a>[\.SDK de .NET, versión 12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_CreateAnonymousBlobClient":::

# <a name="net-v11-sdk"></a>[\.SDK de .NET, versión 11](#tab/dotnet11)

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob storage endpoint for your account.
    CloudBlobClient blobClient = new CloudBlobClient(
        new Uri(@"https://storagesamples.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. 
    // Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
``` 

---

### <a name="reference-a-container-anonymously"></a>Referencia a un contenedor de forma anónima

Si tiene la dirección URL de un contenedor que está disponible de forma anónima, puede utilizarla para hacer referencia al contenedor directamente.

# <a name="net-v12-sdk"></a>[\.SDK de .NET, versión 12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_ListBlobsAnonymously":::

# <a name="net-v11-sdk"></a>[\.SDK de .NET, versión 11](#tab/dotnet11)

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(
        new Uri(@"https://storagesamples.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    // Note this is only possible when the container supports full public read access.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
``` 

---

### <a name="reference-a-blob-anonymously"></a>Referencia a un blob de forma anónima

Si tiene la dirección URL a un blob que está disponible para el acceso anónimo, puede hacer referencia al blob directamente con esa dirección URL:

# <a name="net-v12-sdk"></a>[\.SDK de .NET, versión 12](#tab/dotnet)

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Security.cs" id="Snippet_DownloadBlobAnonymously":::

# <a name="net-v11-sdk"></a>[\.SDK de .NET, versión 11](#tab/dotnet11)

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(
        new Uri(@"https://storagesamples.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", FileMode.Create);
}
``` 

---

## <a name="next-steps"></a>Pasos siguientes

- [Autorización del acceso a Azure Storage](../common/storage-auth.md)
- [Grant limited access to Azure Storage resources using shared access signatures (SAS)](../common/storage-sas-overview.md) (Otorgar acceso limitado a recursos de Azure Storage con firmas de acceso compartido [SAS])
- [Blob Service REST API](/rest/api/storageservices/blob-service-rest-api) (API de REST de Blob service)
