---
title: 'Definición de una directiva de acceso almacenada con .NET: Azure Storage'
description: Obtenga información sobre cómo definir una directiva de acceso almacenada mediante la biblioteca cliente de .NET.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/06/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 272d676d0a5a55262b1c68d0bae9a9ab229df72c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68990608"
---
# <a name="define-a-stored-access-policy-with-net"></a>Definición de una directiva de acceso almacenada con .NET

Una directiva de acceso almacenada proporciona un nivel de control adicional sobre las firmas de acceso compartido (SAS) de nivel de servicio en el lado del servidor. La definición de una directiva de acceso almacenada sirve para agrupar firmas de acceso compartido y proporcionar restricciones adicionales para las firmas de acceso compartido que están enlazadas por la directiva. Puede usar una directiva de acceso almacenada para cambiar la hora de inicio, la hora de expiración, los permisos de una SAS ni para revocarla una vez se ha emitido.
  
 Los siguientes recursos de almacenamiento admiten directivas de acceso almacenadas:  
  
- Contenedores de blobs  
- Recursos compartidos de archivos  
- Colas  
- Tablas  
  
> [!NOTE]
> Una directiva de acceso almacenada de un contenedor se puede asociar a una firma de acceso compartido que conceda permisos al propio contenedor o a los blobs que contiene. De la misma manera, una directiva de acceso almacenada de un recurso compartido de archivos se puede asociar a una firma de acceso compartido que conceda permisos al propio recurso compartido o a los archivos que contiene.  
>
> Las directivas de acceso almacenadas son compatibles para una SAS de servicio únicamente. Las directivas de acceso almacenadas no son compatibles para SAS de cuenta ni de delegación de usuarios.  

## <a name="create-a-stored-access-policy"></a>Crear una directiva de acceso almacenada

El código siguiente crea una directiva de acceso almacenada en un contenedor. Puede usar la directiva de acceso para especificar restricciones para una SAS de servicio en el contenedor o sus blobs.

```csharp
private static async Task CreateStoredAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new stored access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when Azure Storage receives the request.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

## <a name="see-also"></a>Consulte también

- [Grant limited access to Azure Storage resources using shared access signatures (SAS)](storage-sas-overview.md) (Otorgar acceso limitado a recursos de Azure Storage con firmas de acceso compartido [SAS])
- [Definición de una directiva de acceso almacenada](/rest/api/storageservices/define-stored-access-policy)

