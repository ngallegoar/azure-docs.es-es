---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4e958026b1167d65f47bddbe5e89ec4d6fed7ee3
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92217989"
---
Puede comprobar que la conexión se realizó correctamente mediante el comando [az network vpn-connection show](/cli/azure/network/vpn-connection). En el ejemplo, " --name" hace referencia al nombre de la conexión que desea probar. Mientras la conexión aún se está estableciendo, su estado de conexión muestra "Conectando". Una vez establecida la conexión, el estado cambia a "Conectado".

```azurecli-interactive
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```
