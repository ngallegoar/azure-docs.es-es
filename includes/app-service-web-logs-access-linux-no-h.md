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
ms.openlocfilehash: e2c5794e5ce6e23b60bff513562f69c9333d6e34
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92743810"
---
Puede acceder a los registros de consola generados desde dentro del contenedor.

En primer lugar, active el registro de contenedores mediante la ejecución del siguiente comando:

```azurecli-interactive
az webapp log config --name <app-name> --resource-group <resource-group-name> --docker-container-logging filesystem
```

Reemplace `<app-name>` y `<resource-group-name>` por los nombres adecuados para su aplicación web.

Una vez que se active el registro de contenedor, ejecute el siguiente comando para ver el flujo del registro:

```azurecli-interactive
az webapp log tail --name <app-name> --resource-group <resource-group-name>
```

Si no ve los registros de la consola de inmediato, vuelve a comprobarlo en 30 segundos.

Para detener el streaming de registro en cualquier momento, escriba **Ctrl**+**C** .

Los archivos de registro también se pueden inspeccionar en un explorador en `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.
