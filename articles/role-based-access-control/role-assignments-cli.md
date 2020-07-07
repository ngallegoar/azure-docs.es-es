---
title: 'Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure: RBAC de Azure'
description: Aprenda a conceder acceso a recursos de Azure para usuarios, grupos, entidades de servicio e identidades administradas mediante la CLI de Azure y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/25/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 95ec9a25f97154d8e2d0e2e5b5f9cd29cf7a9c31
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84983332"
---
# <a name="add-or-remove-azure-role-assignments-using-azure-cli"></a>Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] En este artículo se describe cómo asignar roles con la CLI de Azure.

## <a name="prerequisites"></a>Prerrequisitos

Para agregar o quitar asignaciones de roles, debe tener:

- Permisos `Microsoft.Authorization/roleAssignments/write` y `Microsoft.Authorization/roleAssignments/delete`, como [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator) o [propietario](built-in-roles.md#owner)
- [Bash en Azure Cloud Shell](/azure/cloud-shell/overview) o [CLI de Azure](/cli/azure)

## <a name="get-object-ids"></a>Obtener los identificadores de objeto

Para agregar o quitar asignaciones de roles, es posible que tenga que especificar el identificador único de un objeto. El identificador tiene el formato: `11111111-1111-1111-1111-111111111111`. Puede obtener el identificador mediante Azure Portal o la CLI de Azure.

### <a name="user"></a>Usuario

Para obtener el identificador del objeto de un usuario de Azure AD, use [az ad user show](/cli/azure/ad/user#az-ad-user-show).

```azurecli
az ad user show --id "{email}" --query objectId --output tsv
```

### <a name="group"></a>Grupo

Para obtener el identificador de objeto de un grupo de Azure AD, puede usar [az ad group show](/cli/azure/ad/group#az-ad-group-show) o [az ad group list](/cli/azure/ad/group#az-ad-group-list).

```azurecli
az ad group show --group "{name}" --query objectId --output tsv
```

### <a name="application"></a>Application

Para obtener el identificador de objeto de una entidad de servicio de Azure AD (identidad usada por una aplicación), puede usar [az ad sp list](/cli/azure/ad/sp#az-ad-sp-list). Para una entidad de servicio, use el id. de objeto y **no** el de aplicación.

```azurecli
az ad sp list --display-name "{name}" --query [].objectId --output tsv
```

## <a name="add-a-role-assignment"></a>Adición de una asignación de roles

En Azure RBAC, para conceder acceso es preciso agregar una asignación de roles.

### <a name="user-at-a-resource-group-scope"></a>Usuario en un ámbito del grupo de recursos

Para agregar una asignación de roles para un usuario en un ámbito del grupo de recursos, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create).

```azurecli
az role assignment create --role {roleNameOrId} --assignee {assignee} --resource-group {resourceGroup}
```

En el ejemplo siguiente, se asigna el rol *Colaborador de la máquina virtual* al usuario *patlong\@contoso.com* en el ámbito del grupo de recursos *pharma-sales*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="using-the-unique-role-id"></a>Uso del identificador de rol único

Hay un par de veces en las que el nombre de un rol puede cambiar, por ejemplo:

- Utiliza su propio rol personalizado y decide cambiar el nombre.
- Utiliza una rol de versión preliminar que tiene **(Versión preliminar)** en el nombre. Cuando se libera el rol, se cambia su nombre.

> [!IMPORTANT]
> Se ofrece una versión preliminar sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Incluso si se cambia el nombre de un rol, su identificador no cambia. Si utiliza scripts o automatización para crear las asignaciones de roles, se recomienda utilizar el identificador de rol único en lugar del nombre del rol. Por tanto, si se cambia el nombre de un rol, es más probable que los scripts funcionen.

Para agregar una asignación de roles con el identificador de rol único en lugar del nombre de rol, utilice [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create).

```azurecli
az role assignment create --role {roleId} --assignee {assignee} --resource-group {resourceGroup}
```

En el ejemplo siguiente, se asigna el rol [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) al usuario *patlong\@contoso.com* en el ámbito del grupo de recursos *pharma-sales*. Para obtener el identificador de rol único, puede usar [az role definition list](/cli/azure/role/definition#az-role-definition-list) o consultar [Roles integrados de Azure](built-in-roles.md).

```azurecli
az role assignment create --role 9980e02c-c2be-4d73-94e8-173b1dc7cf3c --assignee patlong@contoso.com --resource-group pharma-sales
```

### <a name="group-at-a-subscription-scope"></a>Grupo en el ámbito de una suscripción

Para agregar una asignación de roles para un grupo, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). Para obtener información sobre cómo obtener el identificador de objeto del grupo, consulte [Obtener identificadores de objeto](#get-object-ids).

```azurecli
az role assignment create --role {roleNameOrId} --assignee-object-id {assigneeObjectId} --resource-group {resourceGroup} --scope /subscriptions/{subscriptionId}
```

En el ejemplo siguiente se asigna el rol *Lector* al grupo *Ann Mack Team* con el identificador 22222222-2222-2222-2222-222222222222 en el ámbito de suscripción.

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000
```

### <a name="group-at-a-resource-scope"></a>Grupo en el ámbito de un recurso

Para agregar una asignación de roles para un grupo, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). Para obtener información sobre cómo obtener el identificador de objeto del grupo, consulte [Obtener identificadores de objeto](#get-object-ids).

En el ejemplo siguiente se asigna el rol *Colaborador de la máquina virtual* al grupo *Ann Mack Team* con el identificador 22222222-2222-2222-2222-222222222222 en un ámbito de recurso de una red virtual denominada *pharma-sales-project-network*.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/pharma-sales/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="application-at-a-resource-group-scope"></a>Aplicación en el ámbito de un grupo de recursos

Para agregar una asignación de roles para una aplicación, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). Para obtener información sobre cómo obtener el identificador de objeto de la aplicación, consulte [Obtener identificadores de objeto](#get-object-ids).

```azurecli
az role assignment create --role {roleNameOrId} --assignee-object-id {assigneeObjectId} --resource-group {resourceGroup}
```

En el ejemplo siguiente, se asigna el rol *Colaborador de la máquina virtual* a una aplicación con el identificador de objeto 44444444-4444-4444-4444-444444444444 en el ámbito del grupo de recursos *pharma-sales*.

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales
```

### <a name="user-at-a-subscription-scope"></a>Usuario en el ámbito de una suscripción

Para agregar una asignación de roles para un usuario en el ámbito de una suscripción, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). El identificador de suscripción lo puede encontrar en la hoja **Suscripciones** de Azure Portal, o bien, puede usar [az account list](/cli/azure/account#az-account-list).

```azurecli
az role assignment create --role {roleNameOrId} --assignee {assignee} --subscription {subscriptionNameOrId}
```

En el ejemplo siguiente se asigna el rol *Lector* al usuario *annm\@example.com* en un ámbito de suscripción.

```azurecli
az role assignment create --role "Reader" --assignee annm@example.com --subscription 00000000-0000-0000-0000-000000000000
```

### <a name="user-at-a-management-group-scope"></a>Usuario en el ámbito de un grupo de administración

Para agregar una asignación de roles para un usuario en el ámbito de un grupo de administración, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create). El identificador del grupo de administración lo puede encontrar en la hoja **Grupos de administración** de Azure Portal, o bien, puede usar [az account management-group list](/cli/azure/account/management-group#az-account-management-group-list).

```azurecli
az role assignment create --role {roleNameOrId} --assignee {assignee} --scope /providers/Microsoft.Management/managementGroups/{groupId}
```

En el ejemplo siguiente se asigna el rol *Lector de facturación* al usuario *alain\@example.com* en un ámbito de grupo de administración.

```azurecli
az role assignment create --role "Billing Reader" --assignee alain@example.com --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

### <a name="new-service-principal"></a>Nueva entidad de servicio

Si crea una entidad de servicio e inmediatamente intenta asignarle un rol, esa asignación de roles puede producir un error en algunos casos. Por ejemplo, si usa un script para crear una identidad administrada y luego intenta asignarle un rol a esa entidad de servicio, la asignación de roles podría producir un error. Es probable que el motivo de este error sea un retraso en la replicación. La entidad de servicio se crea en una región; sin embargo, la asignación de roles puede tener lugar en una región distinta que todavía no haya replicado la entidad de servicio. Para abordar este escenario, debe especificar el tipo de entidad de seguridad al crear la asignación de roles.

Para agregar una asignación de roles, use [az role assignment create](/cli/azure/role/assignment#az-role-assignment-create), especifique un valor para `--assignee-object-id` y establezca `--assignee-principal-type` en `ServicePrincipal`.

```azurecli
az role assignment create --role {roleNameOrId} --assignee-object-id {assigneeObjectId} --assignee-principal-type {assigneePrincipalType} --resource-group {resourceGroup} --scope /subscriptions/{subscriptionId}
```

En el ejemplo siguiente, se asigna el rol *Colaborador de la máquina virtual* a la identidad administrada *msi-test* en el ámbito del grupo de recursos *pharma-sales*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 33333333-3333-3333-3333-333333333333 --assignee-principal-type ServicePrincipal --resource-group pharma-sales
```

## <a name="remove-a-role-assignment"></a>Eliminación de una asignación de rol

En RBAC de Azure, para quitar el acceso es preciso quitar una asignación de roles mediante [az role assignment delete](/cli/azure/role/assignment#az-role-assignment-delete):

```azurecli
az role assignment delete --assignee {assignee} --role {roleNameOrId} --resource-group {resourceGroup}
```

En el ejemplo siguiente, se quita la asignación de roles *Colaborador de la máquina virtual* del usuario *patlong\@contoso.com* en el grupo de recursos *pharma-sales*:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales
```

En el ejemplo siguiente se quita el rol *Lector* al grupo *Ann Mack Team* con el identificador 22222222-2222-2222-2222-222222222222 en un ámbito de suscripción. Para obtener información sobre cómo obtener el identificador de objeto del grupo, consulte [Obtener identificadores de objeto](#get-object-ids).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --subscription 00000000-0000-0000-0000-000000000000
```

En el ejemplo siguiente se quita el rol *Lector de facturación* al usuario *alain\@example.com* en el ámbito de grupo de administración. Para obtener el identificador del grupo de administración, puede usar [az account management-group list](/cli/azure/account/management-group#az-account-management-group-list).

```azurecli
az role assignment delete --assignee alain@example.com --role "Billing Reader" --scope /providers/Microsoft.Management/managementGroups/marketing-group
```

## <a name="next-steps"></a>Pasos siguientes

- [Enumeración de asignaciones de roles de Azure mediante la CLI de Azure](role-assignments-list-cli.md)
- [Uso de la CLI de Azure para administrar los recursos y grupos de recursos de Azure](../azure-resource-manager/cli-azure-resource-manager.md)
