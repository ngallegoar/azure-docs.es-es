---
title: 'Especificar una clave proporcionada por el cliente en una solicitud a Blob Storage con .NET: Azure Storage'
description: Obtenga información sobre cómo especificar una clave proporcionada por el cliente en una solicitud a Blob Storage mediante .NET.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 11/26/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: ea8254f108aed9d40e6970a27409035b1e10ab41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74807007"
---
# <a name="specify-a-customer-provided-key-on-a-request-to-blob-storage-with-net"></a>Especificar una clave proporcionada por el cliente en una solicitud a Blob Storage con .NET

Los clientes que realizan solicitudes en Azure Blob Storage tienen la opción de proporcionar una clave de cifrado en una solicitud individual. La inclusión de la clave de cifrado en la solicitud proporciona un control detallado sobre la configuración de cifrado para las operaciones de almacenamiento de blobs. Las claves proporcionadas por el cliente (versión preliminar) se pueden almacenar en Azure Key Vault o en otro almacén de claves.

En este artículo se muestra cómo especificar una clave proporcionada por el cliente en una solicitud con .NET.

[!INCLUDE [storage-install-packages-blob-and-identity-include](../../../includes/storage-install-packages-blob-and-identity-include.md)]

Para más información sobre cómo autenticarse con la biblioteca de cliente de Azure Identity desde Azure Storage, consulte la sección titulada **Autenticación con la biblioteca de Azure Identity** en [Autorización del acceso a blobs y colas con Azure Active Directory e identidades administradas para los recursos de Azure](../common/storage-auth-aad-msi.md?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticate-with-the-azure-identity-library).

## <a name="example-use-a-customer-provided-key-to-upload-a-blob"></a>Ejemplo: Uso de una clave proporcionada por el cliente para cargar un blob

En el ejemplo siguiente, se crea una clave proporcionada por el cliente y se usa esa clave para cargar un blob. El código carga un bloque y luego confirma la lista de bloques para escribir el blob en Azure Storage.

```csharp
async static Task UploadBlobWithClientKey(string accountName, string containerName,
    string blobName, Stream data, byte[] key)
{
    const string blobServiceEndpointSuffix = ".blob.core.windows.net";
    Uri accountUri = new Uri("https://" + accountName + blobServiceEndpointSuffix);

    // Specify the customer-provided key on the options for the client.
    BlobClientOptions options = new BlobClientOptions()
    {
        CustomerProvidedKey = new CustomerProvidedKey(key)
    };

    // Create a client object for the Blob service, including options.
    BlobServiceClient serviceClient = new BlobServiceClient(accountUri, 
        new DefaultAzureCredential(), options);

    // Create a client object for the container.
    // The container client retains the credential and client options.
    BlobContainerClient containerClient = serviceClient.GetBlobContainerClient(containerName);

    // Create a new block blob client object.
    // The blob client retains the credential and client options.
    BlobClient blobClient = containerClient.GetBlobClient(blobName);

    try
    {
        // Create the container if it does not exist.
        await containerClient.CreateIfNotExistsAsync();

        // Upload the data using the customer-provided key.
        await blobClient.UploadAsync(data);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Cifrado de Azure Storage para datos en reposo](../common/storage-service-encryption.md)
- [Autorización del acceso a blobs y colas con Azure Active Directory e identidades administradas para los recursos de Azure](../common/storage-auth-aad-msi.md)
