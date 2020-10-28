---
author: baanders
description: Archivo de inclusión para limpiar una instancia básica de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: 29a07ffa917153c0cb062d34e2807d43d039a373
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494670"
---
Cuando ya no necesite los recursos creados en este tutorial, siga estos pasos para eliminarlos.

Con [Azure Cloud Shell](https://shell.azure.com), puede eliminar todos los recursos de Azure de un grupo mediante el comando [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest&preserve-view=true#az-group-delete). Esta acción quita el grupo de recursos y la instancia de Azure Digital Twins.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados. 

Abra una instancia de Azure Cloud Shell y ejecute el siguiente comando para eliminar el grupo de recursos y todo lo que contiene.

```azurecli-interactive
az group delete --name <your-resource-group>
```