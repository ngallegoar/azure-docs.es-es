---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/27/2019
ms.author: glenga
ms.openlocfilehash: d697334fe56fb9133a06cee79067c60bc3a37281
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "68639147"
---
La forma más fácil de instalar extensiones de enlace es habilitar [conjuntos de extensiones](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Al habilitar agrupaciones, un conjunto predefinido de paquetes de extensiones se instala automáticamente.

Para habilitar las agrupaciones de extensiones, abra el archivo host.json y actualice su contenido para que coincida con el siguiente código:

```json
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```