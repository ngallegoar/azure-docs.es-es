---
title: Recuperación de pares clave-valor desde un momento dado
titleSuffix: Azure App Configuration
description: Recuperar pares clave-valor antiguos mediante instantáneas de un momento dado en Azure App Configuration
services: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: 1e2a4f7a7bc5db1b6a49f085821f7fa2bde54229
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523669"
---
# <a name="point-in-time-snapshot"></a>Instantánea en un momento dado

Azure App Configuration mantiene un registro de los cambios realizados en los pares clave-valor. Este registro proporciona una escala de tiempo de los cambios de clave-valor. Puede reconstruir el historial de cualquier par clave-valor y proporcionar su valor anterior en cualquier momento dentro de los siete días anteriores. Con el uso de esta característica puede "viajar en el tiempo" al pasado y recuperar un antiguo par clave-valor. Por ejemplo, puede recuperar los valores de configuración utilizados antes de la implementación más reciente, para revertir la aplicación a la configuración anterior.

## <a name="key-value-retrieval"></a>Recuperación de pares clave-valor

Puede usar Azure PowerShell para recuperar los pares de clave-valor anteriores.  Use `az appconfig revision list`, agregando los parámetros adecuados para recuperar los valores necesarios.  Especifique la instancia de Azure App Configuration proporcionando el nombre del almacén (`--name {app-config-store-name}`) o mediante una cadena de conexión (`--connection-string {your-connection-string}`). Restrinja la salida especificando un punto concreto en el tiempo (`--datetime`) y el número máximo de elementos que se van a devolver (`--top`).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Recupere todos los cambios registrados en los pares clave-valor.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name}.
```

Recupere todos los cambios registrados para la clave `environment` y las etiquetas `test` y `prod`.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment --label test,prod
```

Recupere todos los cambios registrados en el espacio de clave jerárquico `environment:prod`.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --key environment:prod:* 
```

Recupere todos los cambios registrados para la clave `color` en un momento dado específico.

```azurepowershell
az appconfig revision list --connection-string {your-app-config-connection-string} --key color --datetime "2019-05-01T11:24:12Z" 
```

Recupere los últimos 10 cambios registrados en los pares clave-valor y devuelva solo los valores de `key`, `label`y la marca de tiempo `last-modified`.

```azurepowershell
az appconfig revision list --name {your-app-config-store-name} --top 10 --fields key,label,last-modified
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Cree una aplicación web ASP.NET Core](./quickstart-aspnet-core-app.md)  
