---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 0dd6618bdee8e6810d414d4b04b16a1e0a9c90ed
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2020
ms.locfileid: "84905635"
---
Puede acceder a los registros de consola generados desde dentro del contenedor. En primer lugar, active el registro de contenedores mediante la ejecución del siguiente comando en Cloud Shell:

```azurecli-interactive
az webapp log config --name <app-name> --resource-group myResourceGroup --docker-container-logging filesystem
```

Una vez que se active el registro de contenedor, ejecute el siguiente comando para ver el flujo del registro:

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group myResourceGroup
```

Si no ve los registros de la consola de inmediato, vuelve a comprobarlo en 30 segundos.

> [!NOTE]
> También puede inspeccionar los archivos de registro desde el explorador en `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

Para detener el streaming del registro en cualquier momento, escriba `Ctrl`+`C`.
