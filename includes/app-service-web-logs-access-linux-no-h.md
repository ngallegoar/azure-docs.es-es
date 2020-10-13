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
ms.openlocfilehash: df71f0804b62eb4b17ff8d2f652b076b5c64c959
ms.sourcegitcommit: d2222681e14700bdd65baef97de223fa91c22c55
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91822762"
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

Para detener el streaming de registro en cualquier momento, escriba **Ctrl**+**C**.

Los archivos de registro también se pueden inspeccionar en un explorador en `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.
