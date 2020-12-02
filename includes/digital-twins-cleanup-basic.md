---
author: baanders
description: Archivo de inclusión para limpiar una instancia básica de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 8/13/2020
ms.author: baanders
ms.openlocfilehash: 4c03ef942896dda63f678018cdd257024cfbb6d4
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011292"
---
Cuando ya no necesite los recursos creados en este tutorial, siga estos pasos para eliminarlos.

Con [Azure Cloud Shell](https://shell.azure.com), puede eliminar todos los recursos de Azure de un grupo mediante el comando [az group delete](/cli/azure/group?preserve-view=true&view=azure-cli-latest#az-group-delete). Este comando quita el grupo de recursos y la instancia de Azure Digital Twins.

> [!IMPORTANT]
> La eliminación de un grupo de recursos es irreversible. El grupo de recursos y todos los recursos contenidos en él se eliminan permanentemente. Asegúrese de no eliminar por accidente el grupo de recursos o los recursos equivocados.

Abra Azure Cloud Shell y ejecute el siguiente comando para eliminar el grupo de recursos y todo lo que contiene.

```azurecli-interactive
az group delete --name <your-resource-group>
```