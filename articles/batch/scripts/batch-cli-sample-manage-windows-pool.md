---
title: 'Ejemplo de script de la CLI de Azure: grupo de Windows en Batch'
description: Este script muestra algunos de los comandos disponibles en la CLI de Azure para crear y administrar un grupo de nodos de proceso de Windows en Azure Batch.
ms.topic: sample
ms.date: 12/12/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: fb18f9d8777c17d31a3ab246603df0d9fa162467
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93100946"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>Ejemplo de la CLI: creación y administración de un grupo de Windows en Azure Batch

Este script muestra algunos de los comandos disponibles en la CLI de Azure para crear y administrar un grupo de nodos de proceso de Windows en Azure Batch. Un grupo de Windows se puede configurar de dos formas, ya sea con una configuración de Cloud Services o una configuración de Virtual Machine. En este ejemplo se muestra cómo crear un grupo de Windows con la configuración de Cloud Services.

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- Este tutorial requiere la versión 2.0.20 o posterior de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente. 

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Ejecute el siguiente comando para quitar el grupo de recursos y todos los recursos asociados a él.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az batch account create](/cli/azure/batch/account#az-batch-account-create) | Crea la cuenta de Batch. |
| [az batch account login](/cli/azure/batch/account#az-batch-account-login) | Realiza la autenticación respecto de la cuenta de Batch especificada para una mayor interacción de la CLI. |
| [az batch pool create](/cli/azure/batch/pool#az-batch-pool-create) | Crea un grupo de nodos de proceso.  |
| [az batch pool set](/cli/azure/batch/pool#az-batch-pool-set) | Actualiza las propiedades de un grupo.  |
| [az batch pool autoscale enable](/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-enable) | Habilita el escalado automático de un grupo y aplica una fórmula.  |
| [az batch pool show](/cli/azure/batch/pool#az-batch-pool-show) | Muestra las propiedades de un grupo.  |
| [az batch pool autoscale disable](/cli/azure/batch/pool/autoscale#az-batch-pool-autoscale-disable) | Deshabilita el escalado automático de un grupo. |
| [az group delete](/cli/azure/group#az-group-delete) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).
