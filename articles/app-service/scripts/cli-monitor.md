---
title: 'CLI: Supervisión de una aplicación con registros de servidor web'
description: Aprenda a usar la CLI de Azure para automatizar la implementación y administración de la aplicación App Service. En este ejemplo se muestra cómo supervisar una aplicación con registros de servidor web.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 0887656f-611c-4627-8247-b5cded7cef60
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: 995ca98a21e5ec392182bb48932765cca35acf3a
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2020
ms.locfileid: "94562464"
---
# <a name="monitor-an-app-service-app-with-web-server-logs-using-azure-cli"></a>Supervisión de una aplicación de App Service con registros de servidor web mediante la CLI de Azure

Este script de ejemplo crea un grupo de recursos, un plan de App Service y una aplicación, además de configurar la aplicación para habilitar registros de servidor web. A continuación, descarga los archivos de registro para revisarlos.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Este tutorial requiere la versión 2.0 de la CLI de Azure o cualquier versión posterior. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/monitor-with-logs/monitor-with-logs.sh "Monitor Logs")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una aplicación de App Service y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create) | Crea un plan de App Service, |
| [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) | Crea una aplicación de App Service. |
| [`az webapp log config`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-config) | Configura qué registros conserva una aplicación de App Service. |
| [`az webapp log download`](/cli/azure/webapp/log?view=azure-cli-latest#az-webapp-log-download) | Descarga los registros de una aplicación de App Service en la máquina local. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Puede encontrar ejemplos de script adicionales de la CLI de App Service en la [documentación de Azure App Service](../samples-cli.md).