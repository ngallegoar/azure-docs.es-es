---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/14/2019
ms.author: glenga
ms.openlocfilehash: f64892193eb6cfcce8f948b54e5557b5fa3d90ab
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878259"
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

El conjunto actual de extensiones que instala la agrupación predeterminada se enumera en este [archivo extensions.json](https://github.com/Azure/azure-functions-extension-bundles/blob/dev/src/Microsoft.Azure.Functions.ExtensionBundle/extensions.json).
