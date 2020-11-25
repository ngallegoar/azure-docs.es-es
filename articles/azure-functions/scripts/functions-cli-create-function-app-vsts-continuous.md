---
title: 'Creación de una aplicación de funciones con implementación de DevOps: CLI de Azure'
description: Creación de una instancia de Function App e implementación de código de función desde Azure DevOps
ms.date: 07/03/2018
ms.topic: sample
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 25e0f43a851bbf7658847aa52cc9ee778941f718
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94565218"
---
# <a name="create-a-function-in-azure-that-is-deployed-from-azure-devops"></a>Creación de una función en Azure que se implementa desde Azure DevOps

En este tema se explica cómo usar Azure Functions para crear una aplicación de función [sin servidor](https://azure.microsoft.com/solutions/serverless/) con el [plan de consumo](../functions-scale.md#consumption-plan). La aplicación de función, que es un contenedor para las funciones, se implementa continuamente desde un repositorio de Azure DevOps. 

Para realizar este tema, necesita lo siguiente:

* Un repositorio de Azure DevOps que contenga el proyecto de aplicación de función y para el que disponga de permisos administrativos.
* Un [token de acceso personal (PAT)](/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate) para acceder al repositorio de Azure DevOps.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Este tutorial requiere la versión 2.0 de la CLI de Azure o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente. 

## <a name="sample-script"></a>Script de ejemplo

Este ejemplo crea una instancia de Azure Function App e implementa código de la función desde Azure DevOps.

[!code-azurecli-interactive[main](../../../cli_scripts/azure-functions/deploy-function-app-with-function-vsts/deploy-function-app-with-function-vsts.sh?highlight=3-4 "Azure Service")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una cuenta de almacenamiento, una aplicación de función y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az storage account create](/cli/azure/storage/account#az-storage-account-create) | Crea la cuenta de almacenamiento necesaria para la aplicación de función. |
| [az functionapp create](/cli/azure/functionapp#az-functionapp-create) | Crea una aplicación de función en el [Plan de consumo](../functions-scale.md#consumption-plan) sin servidor. |
| [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#az-functionapp-deployment-source-config) | Asocia una aplicación de función con un repositorio GIT o Mercurial. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Encontrará más ejemplos de scripts de CLI para Azure Functions en la [documentación de Azure Functions](../functions-cli-samples.md).
