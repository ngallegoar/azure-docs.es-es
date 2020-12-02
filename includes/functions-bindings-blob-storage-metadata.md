---
title: archivo de inclusión
description: archivo de inclusión
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: f4e6d5fb41769544b7be0f689447364988d0380d
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/24/2020
ms.locfileid: "95998856"
---
El desencadenador de blobs proporciona varias propiedades de metadatos. Estas propiedades pueden usarse como parte de expresiones de enlace en otros enlaces o como parámetros del código. Estos valores tienen la misma semántica que el tipo [CloudBlob](/dotnet/api/microsoft.azure.storage.blob.cloudblob?view=azure-dotnet).

|Propiedad  |Tipo  |Descripción  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Ruta de acceso del blob de desencadenamiento.|
|`Uri`|`System.Uri`|Es el identificador URI del blob correspondiente a la ubicación principal.|
|`Properties` |[BlobProperties](/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Son las propiedades del sistema del blob. |
|`Metadata` |`IDictionary<string,string>`|Son los metadatos definidos por el usuario para el blob.|

Por ejemplo, los ejemplos de JavaScript y el script de C# siguientes registran la ruta de acceso al blob de desencadenamiento, incluido el contenedor:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```