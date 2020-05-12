---
title: 'Enumeración de asignaciones de roles de Azure mediante la CLI de Azure: RBAC de Azure'
description: Aprenda a determinar a qué recursos tienen acceso los usuarios, grupos, entidades de servicio e identidades administradas mediante la CLI de Azure y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/10/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: f4b635d6867c36b8b0f385320e3720bea41b54d1
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735749"
---
# <a name="list-azure-role-assignments-using-azure-cli"></a>Enumeración de asignaciones de roles de Azure mediante la CLI de Azure

[!INCLUDE [Azure RBAC definition list access](../../includes/role-based-access-control-definition-list.md)] En este artículo se describe cómo enumerar las asignaciones de roles mediante la CLI de Azure.

> [!NOTE]
> Si su organización ha externalizado funciones de administración a un proveedor de servicios que usa la [administración de recursos delegados de Azure](../lighthouse/concepts/azure-delegated-resource-management.md), las asignaciones de roles autorizadas por ese proveedor de servicios no se mostrarán aquí.

## <a name="prerequisites"></a>Prerrequisitos

- [Bash en Azure Cloud Shell](/azure/cloud-shell/overview) o [CLI de Azure](/cli/azure)

## <a name="list-role-assignments-for-a-user"></a>Lista de las asignaciones de rol de un usuario

Para mostrar las asignaciones de roles de un usuario específico use [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli-interactive
az role assignment list --assignee <assignee>
```

De forma predeterminada, se mostrarán únicamente las asignaciones de roles para la suscripción actual. Para ver las asignaciones de roles de la suscripción actual y anteriores, agregue el parámetro `--all`. Para ver las asignaciones de roles heredadas, agregue el parámetro `--include-inherited`.

En el ejemplo siguiente, se muestran las asignaciones de roles asignadas directamente al usuario *patlong\@contoso.com*:

```azurecli-interactive
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
```

## <a name="list-role-assignments-for-a-resource-group"></a>Lista de las asignaciones de roles de un grupo de recursos

Para mostrar las asignaciones de roles de un ámbito de grupo de recursos, utilice [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli-interactive
az role assignment list --resource-group <resource_group>
```

En el ejemplo siguiente se muestran las asignaciones de roles del grupo de recursos *pharma-sales*:

```azurecli-interactive
az role assignment list --resource-group pharma-sales --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"
}

...
```

## <a name="list-role-assignments-for-a-subscription"></a>Enumeración de asignaciones de roles de una subscripción

Para enumerar todas las asignaciones de roles en un ámbito de suscripción, use [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list). El identificador de suscripción lo puede encontrar en la hoja **Suscripciones** de Azure Portal, o bien, puede usar [az account list](/cli/azure/account#az-account-list).

```azurecli-interactive
az role assignment list --subscription <subscription_name_or_id>
```

Ejemplo:

```azurecli-interactive
az role assignment list --subscription 00000000-0000-0000-0000-000000000000 --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-management-group"></a>Lista de asignaciones de rol para un grupo de administración

Para enumerar todas las asignaciones de roles de un ámbito de grupo de administración, use [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list). El identificador del grupo de administración lo puede encontrar en la hoja **Grupos de administración** de Azure Portal, o bien, puede usar [az account management-group list](/cli/azure/account/management-group#az-account-management-group-list).

```azurecli-interactive
az role assignment list --scope /providers/Microsoft.Management/managementGroups/<group_id>
```

Ejemplo:

```azurecli-interactive
az role assignment list --scope /providers/Microsoft.Management/managementGroups/marketing-group --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

## <a name="list-role-assignments-for-a-managed-identity"></a>Lista de asignaciones de roles para una identidad administrada

1. Obtenga el identificador de objeto de la identidad administrada asignada por el usuario o el sistema.

    Para obtener el identificador de objeto de una identidad administrada asignada por el usuario, puede usar [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list) o [az identity list](/cli/azure/identity#az-identity-list).

    ```azurecli-interactive
    az ad sp list --display-name "<name>" --query [].objectId --output tsv
    ```

    Para obtener el identificador de objeto de una identidad administrada asignada por el sistema, puede usar [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list).

    ```azurecli-interactive
    az ad sp list --display-name "<vmname>" --query [].objectId --output tsv
    ```

1. Para mostrar las asignaciones de roles use [az role assignment list](/cli/azure/role/assignment#az-role-assignment-list).

    De forma predeterminada, se mostrarán únicamente las asignaciones de roles para la suscripción actual. Para ver las asignaciones de roles de la suscripción actual y anteriores, agregue el parámetro `--all`. Para ver las asignaciones de roles heredadas, agregue el parámetro `--include-inherited`.

    ```azurecli-interactive
    az role assignment list --assignee <objectid>
    ```

## <a name="next-steps"></a>Pasos siguientes

- [Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure](role-assignments-cli.md)
