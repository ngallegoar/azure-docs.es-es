---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: f47a543143c949715fe2a49adccf074759a346fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79382112"
---
```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

Las siguientes propiedades están disponibles en `extensionBundle`:

| Propiedad | Descripción |
| -------- | ----------- |
| id | Espacio de nombres de las agrupaciones de extensiones de Microsoft Azure Functions. |
| version | Versión de la agrupación que va a instalar. Functions Runtime siempre elige la máxima versión permitida definida por el rango o intervalo de versiones. El valor de versión anterior permite todas las versiones de agrupaciones desde la 1.0.0 en adelante pero sin incluir la 2.0.0. Para más información, consulte la [notación de intervalo para especificar intervalos de versiones](/nuget/reference/package-versioning#version-ranges). |

Las versiones de las agrupaciones aumentan a medida que cambian los paquetes de la agrupación. Los cambios de versión principal se producen cuando los paquetes de la agrupación se incrementan en una versión principal. Los cambios de versión principales en la agrupación normalmente coinciden con el cambio en la versión principal del runtime de Functions.  

El conjunto actual de extensiones que instala la agrupación predeterminada se enumera en este [archivo extensions.json](https://github.com/Azure/azure-functions-extension-bundles/blob/master/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json).
