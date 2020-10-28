---
author: baanders
description: Archivo de inclusión para limpiar una instancia básica de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: 2e095a7b286a8860535d6b58fa93098735b30c87
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92372331"
---
Cuando ya no necesite los recursos creados en este tutorial, siga estos pasos para eliminarlos.

Con [Azure Cloud Shell](https://shell.azure.com), puede eliminar todos los recursos de Azure de un grupo mediante el comando [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete). Esta acción quita el grupo de recursos y la instancia de Azure Digital Twins.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. 

Abra una instancia de Azure Cloud Shell y ejecute el siguiente comando para eliminar el grupo de recursos y todo lo que contiene.

```azurecli
az group delete --name <your-resource-group>
```