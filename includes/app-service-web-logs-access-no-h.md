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
ms.openlocfilehash: e6c4b07d01a4992e22107cb7d524646f439c37c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905874"
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
