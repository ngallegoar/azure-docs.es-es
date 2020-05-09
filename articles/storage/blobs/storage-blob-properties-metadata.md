---
title: Administración de propiedades y metadatos de un blob con .NET en Azure Storage
description: Aprenda a establecer y recuperar propiedades del sistema y a almacenar metadatos personalizados de blobs en su cuenta de Azure Storage mediante la biblioteca cliente de .NET.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/09/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: 74c023c06e7b28183a53772be6798419c91dd37a
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692463"
---
# <a name="manage-blob-properties-and-metadata-with-net"></a>Administración de propiedades y metadatos de blobs con .NET

Además de los datos que contienen, los blobs admiten propiedades del sistema y metadatos definidos por el usuario. En este artículo se muestra cómo administrar propiedades del sistema y metadatos definidos por el usuario con la [biblioteca cliente de Azure Storage para .NET](/dotnet/api/overview/azure/storage?view=azure-dotnet).

## <a name="about-properties-and-metadata"></a>Acerca de las propiedades y los metadatos

- **Propiedades del sistema**: en cada recurso de almacenamiento de blobs existen propiedades del sistema. Algunas se pueden leer o establecer, mientras que otras son de solo lectura. En segundo plano, algunas propiedades del sistema corresponden a ciertos encabezados HTTP estándar. La biblioteca cliente de Azure Storage para .NET mantiene estas propiedades automáticamente.

- **Metadatos definidos por el usuario**: los metadatos definidos por el usuario se componen de uno o más pares nombre-valor que especifica para un recurso de almacenamiento de blobs. Puede usar metadatos para almacenar valores adicionales con el recurso. Los valores de metadatos se proporcionan para uso personal y no afectan a cómo se comporta el recurso.

> [!NOTE]
> Las etiquetas del Índice de blobs ofrecen la capacidad de almacenar atributos arbitrarios de clave-valor definidas por el usuario junto con un recurso de almacenamiento de blobs. De manera similar a los metadatos, solo las etiquetas del Índice de blobs se indexan automáticamente y se pueden consultar mediante el Blob service nativo. Los metadatos no se pueden indexar ni consultar de forma nativa, a menos que se use un servicio independiente, como Azure Search.
>
> Para obtener más información sobre esta característica, vea [Administración y búsqueda de datos en Azure Blob Storage con el Índice de blobs (versión preliminar)](storage-manage-find-blobs.md).

El proceso de recuperación de los valores de propiedad y metadatos de un recurso de almacenamiento de blobs consta de dos pasos. Para poder leer estos valores, tiene que capturarlos explícitamente llamando al método `FetchAttributes` o `FetchAttributesAsync`. La excepción a esta regla es que los métodos `Exists` y `ExistsAsync` llaman al método `FetchAttributes` adecuado en segundo plano. Cuando se llama a uno de estos métodos, no es necesario llamar también a `FetchAttributes`.

> [!IMPORTANT]
> Si descubre que no se han rellenado los valores de propiedad ni de metadatos para un recurso de almacenamiento, compruebe que su código llama al método `FetchAttributes` o `FetchAttributesAsync`.

## <a name="set-and-retrieve-properties"></a>Establecimiento y recuperación de propiedades

En el ejemplo de código siguiente se establecen las propiedades del sistema `ContentType` y `ContentLanguage` en un blob.

```csharp
public static async Task SetBlobPropertiesAsync(CloudBlob blob)
{
    try
    {
        Console.WriteLine("Setting blob properties.");

        // You must explicitly set the MIME ContentType every time
        // the properties are updated or the field will be cleared.
        blob.Properties.ContentType = "text/plain";
        blob.Properties.ContentLanguage = "en-us";

        // Set the blob's properties.
        await blob.SetPropertiesAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Para recuperar las propiedades del blob, llame al método `FetchAttributes` o `FetchAttributesAsync` en el blob para que rellene la propiedad `Properties`. En el ejemplo de código siguiente se obtienen las propiedades del sistema de un blob y se muestran algunos de los valores:

```csharp
private static async Task GetBlobPropertiesAsync(CloudBlob blob)
{
    try
    {
        // Fetch the blob properties.
        await blob.FetchAttributesAsync();

        // Display some of the blob's property values.
        Console.WriteLine(" ContentLanguage: {0}", blob.Properties.ContentLanguage);
        Console.WriteLine(" ContentType: {0}", blob.Properties.ContentType);
        Console.WriteLine(" Created: {0}", blob.Properties.Created);
        Console.WriteLine(" LastModified: {0}", blob.Properties.LastModified);
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

## <a name="set-and-retrieve-metadata"></a>Establecimiento y recuperación de metadatos

Puede especificar metadatos como uno o más pares nombre-valor en un recurso de blob o contenedor. Para establecer los metadatos, agregue pares nombre-valor a la colección `Metadata` del recurso. A continuación, llame a uno de los métodos siguientes para escribir los valores:

- [SetMetadata](/dotnet/api/microsoft.azure.storage.blob.cloudblob.setmetadata)
- [SetMetadataAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.setmetadataasync)

Los pares de nombre/valor de metadatos son encabezados HTTP válidos y deben cumplir todas las restricciones que gobiernan los encabezados HTTP. Los nombres de los metadatos deben ser nombres válidos de encabezado HTTP e identificadores de C# válidos, y solo pueden contener caracteres ASCII y no distinguir entre mayúsculas y minúsculas. Los valores de los metadatos que contengan caracteres distintos de ASCII deben estar [codificados en Base64](https://docs.microsoft.com/dotnet/api/system.convert.tobase64string) o con [direcciones URL](https://docs.microsoft.com/dotnet/api/system.web.httputility.urlencode).

El nombre de los metadatos debe cumplir las convenciones de nomenclatura para los identificadores de C#. Los nombres de los metadatos conservan las mayúsculas y minúsculas con las que se crearon, pero no las distinguen cuando se establecen o se leen. Si se envían dos o más encabezados de metadatos con el mismo nombre para un recurso, Azure Blob Storage devuelve el código de error HTTP 400 (solicitud incorrecta).

El ejemplo de código siguiente establece los metadatos en un blob. Un valor se establece mediante el método `Add` de la colección. El otro valor se establece mediante la sintaxis implícita de clave/valor.

```csharp
public static async Task AddBlobMetadataAsync(CloudBlob blob)
{
    try
    {
        // Add metadata to the blob by calling the Add method.
        blob.Metadata.Add("docType", "textDocuments");

        // Add metadata to the blob by using key/value syntax.
        blob.Metadata["category"] = "guidance";

        // Set the blob's metadata.
        await blob.SetMetadataAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Para recuperar metadatos, llame al método `FetchAttributes` o `FetchAttributesAsync` en el blob o contenedor para rellenar la colección `Metadata` y, a continuación, lea los valores tal como se muestra en el ejemplo siguiente.

```csharp
public static async Task ReadBlobMetadataAsync(CloudBlob blob)
{
    try
    {
        // Fetch blob attributes in order to populate 
        // the blob's properties and metadata.
        await blob.FetchAttributesAsync();

        Console.WriteLine("Blob metadata:");

        // Enumerate the blob's metadata.
        foreach (var metadataItem in blob.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Consulte también

- [Operación Set Blob Properties](/rest/api/storageservices/set-blob-properties)
- [Operación Get Blob Properties](/rest/api/storageservices/get-blob-properties)
- [Operación Set Blob Metadata](/rest/api/storageservices/set-blob-metadata)
- [Operación Get Blob Metadata](/rest/api/storageservices/get-blob-metadata)
