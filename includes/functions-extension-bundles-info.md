---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 1fc37c6f93fba34944caa7a91c2a89ce5dcdc398
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "78201026"
---
::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
> [!TIP]
> Durante el inicio, el host descarga e instala la [extensión de enlace de Storage](../articles/azure-functions/functions-bindings-storage-queue.md#functions-2x-and-higher) y otras extensiones de enlace de Microsoft. Esta instalación se produce porque las extensiones de enlace se habilitan de forma predeterminada en el archivo *host.json* con las siguientes propiedades:
>
> ```json
> {
>     "version": "2.0",
>     "extensionBundle": {
>         "id": "Microsoft.Azure.Functions.ExtensionBundle",
>         "version": "[1.*, 2.0.0)"
>     }
> }
> ```
>
> Si encuentra algún error relacionado con las extensiones de enlace, compruebe que las propiedades anteriores están presentes en *host.json*.
::: zone-end  