---
title: Script de PowerShell para crear un bloqueo de recursos para una tabla de Table API en Azure Cosmos
description: Creación de un bloqueo de recurso para una tabla de Table API en Azure Cosmos
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.topic: sample
ms.date: 06/12/2020
ms.openlocfilehash: f8b7cfd79188ca742ee163a02c2b6c3b8f36e6c2
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92481727"
---
# <a name="create-a-resource-lock-for-azure-cosmos-table-api-table-using-azure-powershell"></a>Creación de un bloqueo de recursos para una tabla de Table API en Azure Cosmos con Azure PowerShell

[!INCLUDE [updated-for-az](../../../../../includes/updated-for-az.md)]

[!INCLUDE [sample-PowerShell-install](../../../../../includes/sample-PowerShell-install-no-ssh.md)]

> [!IMPORTANT]
> Los bloqueos de recursos no funcionarán para los cambios que realicen los usuarios que se conecten mediante cualquier SDK de Cosmos DB, todas las herramientas que se conecten mediante claves de cuenta o Azure Portal, a menos que se bloquee en primer lugar la cuenta de Cosmos DB con la propiedad `disableKeyBasedMetadataWriteAccess` habilitada. Para más información sobre cómo habilitar esta propiedad, consulte [Evitar cambios de SDK](../../../role-based-access-control.md#prevent-sdk-changes).

## <a name="sample-script"></a>Script de ejemplo

[!code-PowerShell[main](../../../../../PowerShell_scripts/cosmosdb/table/ps-table-lock.ps1 "Create, list, and remove resource locks")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos y todos los recursos asociados.

```PowerShell
Remove-AzResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
|**Recurso de Azure**| |
| [New-AzResourceLock](/PowerShell/module/az.resources/new-azresourcelock) | Crea un bloqueo de recursos. |
| [Get-AzResourceLock](/PowerShell/module/az.resources/get-azresourcelock) | Obtiene un bloqueo de recursos o enumera los bloqueos de recursos. |
| [Remove-AzResourceLock](/PowerShell/module/az.resources/remove-azresourcelock) | Elimina un bloqueo de recursos. |
|||

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure PowerShell, consulte la [documentación de Azure PowerShell](/PowerShell/).