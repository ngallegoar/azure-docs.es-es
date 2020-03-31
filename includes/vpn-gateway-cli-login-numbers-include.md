---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 21dbec52dded5a195cc764741ab9e8d79737c549
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186167"
---
1. Inicie sesión en la suscripción de Azure con el comando [az login](/cli/azure/) y siga las instrucciones que aparecen en pantalla. Para más información sobre el inicio de sesión, consulte [Introducción a la CLI de Azure](/cli/azure/get-started-with-azure-cli).

   ```azurecli
   az login
   ```
2. Si tiene más de una suscripción de Azure, enumere las suscripciones de la cuenta.

   ```azurecli
   az account list --all
   ```
3. Especifique la suscripción que desea usar.

   ```azurecli
   az account set --subscription <replace_with_your_subscription_id>
   ```
