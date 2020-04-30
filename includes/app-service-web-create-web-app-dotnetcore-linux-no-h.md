---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 04/22/2020
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: f397a3df7280b9277b2b7205368ef5788ed321aa
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82206685"
---
Cree una [aplicación web](../articles/app-service/containers/app-service-linux-intro.md) en el plan de App Service de `myAppServicePlan`. 

En Cloud Shell, puede usar el comando [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest). En el siguiente ejemplo, reemplace `<app-name>` por un nombre único global de aplicación (los caracteres válidos son `a-z`, `0-9` y `-`). El entorno de ejecución está establecido en `DOTNETCORE|LTS`, que es .NET Core 3.1. Para ver todos los entornos en tiempo de ejecución admitidos, ejecute [`az webapp list-runtimes --linux`](/cli/azure/webapp?view=azure-cli-latest). 

```azurecli-interactive
# Bash
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "DOTNETCORE|LTS" --deployment-local-git
# PowerShell
az --% webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --runtime "DOTNETCORE|LTS" --deployment-local-git
```

Cuando se haya creado la aplicación web, la CLI de Azure mostrará información similar a la del ejemplo siguiente:

<pre>
Local git is configured with url of 'https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
</pre>

Ha creado una aplicación web vacía en un contenedor de Linux, con la implementación de Git habilitada.

> [!NOTE]
> La dirección URL del repositorio de Git remoto se muestra en la propiedad `deploymentLocalGitUrl`, con el formato `https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Guarde esta dirección URL, ya que la necesitará más adelante.
>

Actualmente, debe ejecutar el siguiente comando adicional para configurar correctamente la versión de .NET Core (reemplace `<app-name>` por la del paso anterior):

```azurecli-interactive
# Bash
az webapp config set --resource-group myResourceGroup --name <app-name> --linux-fx-version "DOTNETCORE|3.1"
# PowerShell
az --% webapp config set --resource-group myResourceGroup --name <app-name> --linux-fx-version "DOTNETCORE|3.1"
```
