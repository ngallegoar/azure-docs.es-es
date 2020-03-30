---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 5102866cdda51ef545fd95b32946cb17c6e40a3c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "72038179"
---
La [biblioteca del Administrador de configuración de Microsoft Azure para .NET](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/) proporciona una clase para analizar una cadena de conexión desde un archivo de configuración. La clase [CloudConfigurationManager](https://msdn.microsoft.com/library/azure/mt634650.aspx) analiza los valores de configuración. Analiza la configuración de las aplicaciones cliente que se ejecutan en el escritorio, en un dispositivo móvil, en una máquina virtual de Azure o en un servicio en la nube de Azure.

Para hacer referencia al paquete `CloudConfigurationManager`, agregue las siguientes directivas `using`:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage;
```

Este es un ejemplo que muestra cómo recuperar una cadena de conexión de un archivo de configuración:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

El uso del Administrador de configuración de Azure es opcional. También se puede utilizar una API, como la [clase ConfigurationManager](/dotnet/api/system.configuration.configurationmanager)de .NET Framework.
