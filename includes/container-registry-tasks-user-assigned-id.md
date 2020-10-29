---
title: archivo de inclusión
description: archivo de inclusión
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/12/2019
ms.author: danlep
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: d81b6f5367efa92c9249956faa058441edf98561
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755791"
---
### <a name="create-a-user-assigned-identity"></a>Creación de una identidad asignada por el usuario

Cree una identidad denominada *myACRTasksId* en la suscripción con el comando [az identity create][az-identity-create]. Puede usar el mismo grupo de recursos que usó anteriormente para crear un registro de contenedor o uno diferente.

```azurecli
az identity create \
  --resource-group myResourceGroup \
  --name myACRTasksId
```

Para configurar la identidad asignada por el usuario en los pasos siguientes, use el comando [az identity show][az-identity-show] para almacenar en variables los identificadores de recurso, de entidad de seguridad y de cliente de la identidad.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query id --output tsv)

# Get principal ID of the task's user-assigned identity
principalID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query principalId --output tsv)

# Get client ID of the user-assigned identity
clientID=$(az identity show \
  --resource-group myResourceGroup \
  --name myACRTasksId \
  --query clientId --output tsv)
```

<!-- LINKS - Internal -->
[az-identity-create]: /cli/azure/identity#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
