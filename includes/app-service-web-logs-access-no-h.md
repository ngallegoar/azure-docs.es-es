---
title: archivo de inclusión
description: archivo de inclusión
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 7803ac9009af1657e7f162d656898492a694e28f
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92743528"
---
Para acceder a los registros de la consola generados desde el código de la aplicación en App Service, active el registro de diagnósticos, para lo que debe ejecutar el siguiente comando en [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp log config --resource-group <resource-group-name> --name <app-name> --application-logging true --level Verbose
```

Los valores posibles de `--level` son: `Error`, `Warning`, `Info` y `Verbose`. Todos los niveles incluyen el nivel anterior. Por ejemplo: `Error` incluye solo los mensajes de error, mientras que `Verbose` incluye todos los mensajes.

Una vez que se activa el registro de contenedor, ejecute el siguiente comando para ver la transmisión del registro:

```azurecli-interactive
az webapp log tail --resource-group <resource-group-name> --name <app-name>
```

Si no ve los registros de la consola de inmediato, vuelve a comprobarlo en 30 segundos.

> [!NOTE]
> También puede inspeccionar los archivos de registro desde el explorador en `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

Para detener el streaming del registro en cualquier momento, escriba `Ctrl`+`C`.
