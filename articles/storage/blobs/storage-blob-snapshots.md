---
title: 'Creación y administración de una instantánea de blob en .NET: Azure Storage'
description: Aprenda a crear una instantánea de solo lectura de un blob para hacer una copia de seguridad de los datos de blob en un momento determinado.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/06/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 9bf5eea55002814f461d375b3db43a37fe4f7aa9
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2020
ms.locfileid: "80474084"
---
# <a name="create-and-manage-a-blob-snapshot-in-net"></a>Creación y administración de una instantánea de blob en .NET

Una instantánea es una versión de solo lectura de un blob que se ha realizado en un momento dado. Las instantáneas son útiles para realizar copias de seguridad de blobs. En este artículo se muestra cómo crear y administrar instantáneas de blob mediante la [biblioteca cliente para .NET de Azure Storage](/dotnet/api/overview/azure/storage?view=azure-dotnet).

## <a name="about-blob-snapshots"></a>Sobre las instantáneas de blob

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

Una instantánea de un blob es idéntica a su blob base, salvo que el identificador URI tiene un valor **DateTime** anexado al URI del blob para indicar el momento en que se tomó la instantánea. Por ejemplo, si el identificador URI de blob en páginas es `http://storagesample.core.blob.windows.net/mydrives/myvhd`, el identificador URI de instantánea es similar a `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Todas las instantáneas comparten el identificador URI del blob base. La única distinción entre el blob base y la instantánea es el valor **DateTime** anexado.
>

Un blob puede tener cualquier número de instantáneas. Las instantáneas se conservan hasta que se eliminan explícitamente, lo que significa que una instantánea no puede durar más que su blob base. Puede enumerar las instantáneas asociadas al blob base para llevar un seguimiento de las instantáneas actuales.

Cuando se crea una instantánea de un blob, las propiedades del sistema se copian en la instantánea con los mismos valores. Los metadatos del blob base también se copian en la instantánea, a menos que especifique metadatos independientes para la instantánea al crearla. Después de crear una instantánea, puede leerla, copiarla o eliminarla, pero no puede modificarla.

Las concesiones asociadas con el blob base no afectan la instantánea. No puede adquirir una concesión en una instantánea.

Se usa un archivo VHD para almacenar el estado y la información actual de un disco de máquina virtual. Puede desconectar un disco desde la máquina virtual o apagándola y, después, realizar una instantánea de su archivo VHD. Puede usar ese archivo de instantánea más adelante para recuperar el archivo VHD en ese momento y volver a crear la máquina virtual.

## <a name="create-a-snapshot"></a>Crear una instantánea

Para crear una instantánea de un blob en bloques, use uno de los métodos siguientes:

- [CreateSnapshot](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshot)
- [CreateSnapshotAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshotasync)

En el ejemplo de código siguiente se muestra cómo crear una instantánea. En este ejemplo se especifican metadatos adicionales para la instantánea cuando se crea.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="delete-snapshots"></a>Eliminar instantáneas

Para eliminar un blob, primero debe eliminar todas las instantáneas de ese blob. Puede eliminar una instantánea individualmente o especificar que se eliminen todas las instantáneas cuando se elimina el blob de origen. Si intenta eliminar un blob que todavía tiene instantáneas, se producirá un error.

Para eliminar instantáneas de blob, use uno de los siguientes métodos de eliminación de blobs e incluya la enumeración [DeleteSnapshotsOption](/dotnet/api/microsoft.azure.storage.blob.deletesnapshotsoption).

- [Eliminar](/dotnet/api/microsoft.azure.storage.blob.cloudblob.delete)
- [DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexistsasync)

En el ejemplo de código siguiente se muestra cómo eliminar un blob y sus instantáneas en. NET, donde `blockBlob` es un objeto del tipo [CloudBlockBlob][dotnet_CloudBlockBlob]:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Devolver el URI absoluto para una instantánea

En este ejemplo de código de C# se crea una instantánea y se escribe el identificador URI absoluto de la ubicación principal.

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Descripción de cómo las instantáneas pueden incrementar los costes

Crear una instantánea, que es una copia de solo lectura de un blob, puede conllevar cargos adicionales de almacenamiento de datos en la cuenta. Al diseñar una aplicación, es importante tener en cuenta cómo se pueden acumular estos gastos para que pueda de minimizar los costos.

### <a name="important-billing-considerations"></a>Consideraciones importantes sobre facturación

La lista siguiente incluye los puntos clave que hay que tener en cuenta a la hora de crear una instantánea.

- La cuenta de almacenamiento genera cargos para páginas o bloques únicos, bien se encuentren en el blob o en la instantánea. La cuenta no generará gastos adicionales para instantáneas asociadas a un blob hasta que actualice el blob en el que se basan. Después de actualizar el blob base, discrepa de sus instantáneas. Cuando esto sucede, se le cobra por las páginas o bloques únicos en cada blob o instantánea.
- Al reemplazar un bloque en un blob en bloques, ese bloque será considerado como un bloque único a la hora de aplicar cargos. Esto es así incluso aunque el bloque tenga el mismo identificador de bloque y los mismos datos que tiene en la instantánea. Cuando se vuelva a confirmar el bloque, divergirá de su equivalente en cualquier instantánea y se aplicarán cargos por sus datos. Lo mismo sucede con una página de un blob en páginas que se haya actualizado con datos idénticos.
- Si se reemplaza un blob en bloques llamando a los métodos [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream] o [UploadFromByteArray][dotnet_UploadFromByteArray], se reemplazan todos los bloques del blob. Si una instantánea está asociada a ese blob, todos los bloques del blob y la instantánea de base divergen ahora y se aplicarán cargos por todos los bloques en ambos blobs. Esto es así incluso si los datos del blob e instantánea de base siguen siendo idénticos.
- Azure Blob service no dispone de medios para determinar si dos bloques contienen datos idénticos. Cada bloque que se carga y confirma se trata como único, incluso si tiene los mismos datos y el mismo identificador de bloque. Dado que los cargos se acumulan en bloques únicos, es importante tener en cuenta que la actualización de un blob que tiene una instantánea genera bloques únicos y cargos adicionales.

### <a name="minimize-cost-with-snapshot-management"></a>Minimización del costo con la administración de instantáneas

Se recomienda administrar las instantáneas con cuidado para evitar cargos adicionales. El seguimiento de estas prácticas recomendadas le ayudará a minimizar los costos que acarreará el almacenamiento de las instantáneas:

- Elimine y vuelva a crear las instantáneas asociadas a un blob siempre que lo actualice, incluso si lo hace con datos idénticos, a menos que el diseño de la aplicación requiera que se conserven las instantáneas. Si elimina y vuelve a crear las instantáneas del blob, puede estar seguro de que el blob y las instantáneas no van a divergir.
- Si va a mantener las instantáneas de un blob, evite llamar a [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream] o [UploadFromByteArray][dotnet_UploadFromByteArray] para actualizar el blob. Dichos métodos reemplazan todos los bloques del blob, lo que provoca que el blob base y sus instantáneas diverjan considerablemente. En su lugar, actualice el menor número posible de bloques mediante los métodos [PutBlock][dotnet_PutBlock] y [PutBlockList][dotnet_PutBlockList].

### <a name="snapshot-billing-scenarios"></a>Escenarios de facturación de instantáneas

En las siguientes situaciones, se muestra cómo se ven incrementados los cargos para un blob en bloques y sus instantáneas.

#### <a name="scenario-1"></a>Escenario 1

En el escenario 1, el blob de base no se ha actualizado después de realizarse la instantánea, así que se aplicarán cargos únicamente para los bloques 1, 2 y 3.

![Recursos de Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

#### <a name="scenario-2"></a>Escenario 2

En el escenario 2, el blob de base está actualizado, pero no así la instantánea. Se ha actualizado el bloque 3 y, aunque contiene los mismos datos y el mismo identificador, no es igual que el bloque 3 de la instantánea. Como resultado, se aplicarán cargos a la cuenta por cuatro bloques.

![Recursos de Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

#### <a name="scenario-3"></a>Escenario 3

En el escenario 3, el blob de base está actualizado, pero no así la instantánea. El bloque 3 se ha reemplazado por el bloque 4 en el blob de base, pero la instantánea sigue reflejando el bloque 3. Como resultado, se aplicarán cargos a la cuenta por cuatro bloques.

![Recursos de Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

#### <a name="scenario-4"></a>Escenario 4

En la situación 4, el blob de base se ha actualizado totalmente y no contiene ninguno de los bloques originales. Como resultado, se aplicarán cargos a la cuenta por la totalidad de los ocho bloques únicos. Esta situación se puede dar si usa un método de actualización como [UploadFromFile][dotnet_UploadFromFile], [UploadText][dotnet_UploadText], [UploadFromStream][dotnet_UploadFromStream] o [UploadFromByteArray][dotnet_UploadFromByteArray], ya que estos métodos reemplazan todos los contenidos de un blob.

![Recursos de Azure Storage](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Pasos siguientes

- Puede encontrar más información acerca de cómo trabajar con instantáneas de discos de máquina virtuales (VM) en [Copias de seguridad de discos de máquinas virtuales de Azure no administrados con instantáneas incrementales](../../virtual-machines/windows/incremental-snapshots.md)

- Para obtener más ejemplos de código que usa Blob Storage, consulte [Ejemplos de código de Azure](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Puede descargar una aplicación de ejemplo y ejecutarla, así como ver el código en GitHub.

[dotnet_AccessCondition]: https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.accesscondition
[dotnet_CloudBlockBlob]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblockblob
[dotnet_CreateSnapshotAsync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudpageblob.createsnapshotasync
[dotnet_HTTPStatusCode]: https://docs.microsoft.com/java/api/com.microsoft.store.partnercenter.network.httpstatuscode
[dotnet_PutBlockList]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblocklist
[dotnet_PutBlock]: /dotnet/api/microsoft.azure.storage.blob.cloudblockblob.putblock
[dotnet_UploadFromByteArray]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblob.uploadfrombytearray
[dotnet_UploadFromFile]: https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob.cloudblob.uploadfromfile
[dotnet_UploadFromStream]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadfromstream
[dotnet_UploadText]: /dotnet/api/microsoft.azure.storage.blob.cloudappendblob.uploadtext
