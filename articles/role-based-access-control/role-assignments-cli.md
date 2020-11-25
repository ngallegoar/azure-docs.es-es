---
title: 'Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure: RBAC de Azure'
description: Aprenda a conceder acceso a recursos de Azure para usuarios, grupos, entidades de servicio e identidades administradas mediante la CLI de Azure y el control de acceso basado en roles de Azure (RBAC de Azure).
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.workload: identity
ms.date: 09/28/2020
ms.author: rolyon
ms.custom: contperfq1, devx-track-azurecli
ms.openlocfilehash: e23b6513751764063e3d8c85e063a24165d34648
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/18/2020
ms.locfileid: "94844887"
---
# <a name="add-or-remove-azure-role-assignments-using-azure-cli"></a>Incorporación o eliminación de asignaciones de roles de Azure mediante la CLI de Azure

[!INCLUDE [Azure RBAC definition grant access](../../includes/role-based-access-control-definition-grant.md)] En este artículo se describe cómo asignar roles con la CLI de Azure.

## <a name="prerequisites"></a>Prerrequisitos

Para agregar o quitar asignaciones de roles, debe tener:

- Permisos `Microsoft.Authorization/roleAssignments/write` y `Microsoft.Authorization/roleAssignments/delete`, como [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator) o [propietario](built-in-roles.md#owner)
- [Bash en Azure Cloud Shell](../cloud-shell/overview.md) o [CLI de Azure](/cli/azure)

## <a name="steps-to-add-a-role-assignment"></a>Pasos para agregar una asignación de roles

En Azure RBAC, para conceder acceso es preciso agregar una asignación de roles. Una asignación de roles consta de tres elementos: entidad de seguridad, definición de rol y ámbito. Para agregar una asignación de roles, siga estos pasos.

### <a name="step-1-determine-who-needs-access"></a>Paso 1: determinar quién necesita acceso

Puede asignar un rol a un usuario, grupo, entidad de servicio o identidad administrada. Para agregar una asignación de roles, es posible que tenga que especificar el id. único del objeto. El identificador tiene el formato: `11111111-1111-1111-1111-111111111111`. Puede obtener el identificador mediante Azure Portal o la CLI de Azure.

**User**

Para un usuario Azure AD, obtenga el nombre principal de usuario, como *patlong\@contoso.com* o el id. de objeto del usuario. Para obtener el id. de objeto, puede usar [az ad user show](/cli/azure/ad/user#az_ad_user_show).

```azurecli
az ad user show --id "{principalName}" --query "objectId" --output tsv
```

**Grupo**

Para un grupo de Azure AD, necesita el id. de objeto de grupo. Para obtener el id. de objeto, puede usar [az ad group show](/cli/azure/ad/group#az_ad_group_show) o [az ad group list](/cli/azure/ad/group#az_ad_group_list).

```azurecli
az ad group show --group "{groupName}" --query "objectId" --output tsv
```

**Entidad de servicio**

Para una entidad de servicio de Azure AD (identidad que usa una aplicación), necesita el id. de objeto de la entidad de servicio. Para obtener el id. de objeto, puede usar [az ad sp list](/cli/azure/ad/sp#az_ad_sp_list). Para una entidad de servicio, use el id. de objeto y **no** el de aplicación.

```azurecli
az ad sp list --all --query "[].{displayName:displayName, objectId:objectId}" --output tsv
az ad sp list --display-name "{displayName}"
```

**Identidad administrada**

Para una identidad administrada que haya asignado el sistema o el usuario, es necesario el id. de objeto. Para obtener el id. de objeto, puede usar [az ad sp list](/cli/azure/ad/sp#az_ad_sp_list).

```azurecli
az ad sp list --all --filter "servicePrincipalType eq 'ManagedIdentity'"
```

Para enumerar las identidades administradas asignadas por el usuario, puede usar [az identity list](/cli/azure/identity#az_identity_list).

```azurecli
az identity list
```
    
### <a name="step-2-find-the-appropriate-role"></a>Paso 2: buscar el rol adecuado

Los permisos se agrupan en roles. Puede seleccionarlos de una lista de varios [roles integrados de Azure](built-in-roles.md) o puede utilizar sus propios roles personalizados. Es recomendable conceder acceso con el menor privilegio necesario, así que evite asignar un rol más amplio.

Para enumerar los roles y obtener el id. de rol único, puede usar [az role definition list](/cli/azure/role/definition#az_role_definition_list).

```azurecli
az role definition list --query "[].{name:name, roleType:roleType, roleName:roleName}" --output tsv
```

Aquí verá cómo mostrar los detalles de un rol determinado.

```azurecli
az role definition list --name "{roleName}"
```

Para obtener más información, consulte la [Lista de definiciones de roles de Azure](role-definitions-list.md#azure-cli).
 
### <a name="step-3-identify-the-needed-scope"></a>Paso 3: identificar el ámbito necesario

Azure proporciona cuatro niveles de ámbito: recursos, [grupo de recursos](../azure-resource-manager/management/overview.md#resource-groups), suscripción y [grupo de administración](../governance/management-groups/overview.md). Es recomendable conceder acceso con el menor privilegio necesario, así que evite asignar un rol a un ámbito más amplio. Para obtener más información sobre el ámbito, vea [Comprensión del ámbito](scope-overview.md).

**Ámbito de recursos**

Para el ámbito de recursos, necesitará el id. de recurso. Puede buscar el id. de recurso examinando las propiedades del recurso en Azure Portal. El id. de recurso tiene el formato siguiente.

```
/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/{providerName}/{resourceType}/{resourceSubType}/{resourceName}
```

**Ámbito de grupo de recursos**

Para el ámbito del grupo de recursos, necesita el nombre del grupo de recursos. Puede encontrar el nombre en la página **Grupos de recursos** de Azure Portal o puede usar [az group list](/cli/azure/group#az_group_list).

```azurecli
az group list --query "[].{name:name}" --output tsv
```

**Ámbito de suscripción** 

Para el ámbito de la suscripción, necesitará el id. de la suscripción. Puede encontrar el id. en la página **Suscripciones** de Azure Portal o puede usar [az account list](/cli/azure/account#az_account_list).

```azurecli
az account list --query "[].{name:name, id:id}" --output tsv
```

**Ámbito del grupo de administración** 

Para el ámbito del grupo de administración, necesita el nombre del grupo de administración. Puede encontrar el nombre en la página de **Grupos de administración** de Azure Portal o puede usar [az account management-group list](/cli/azure/account/management-group#az_account_management_group_list).

```azurecli
az account management-group list --query "[].{name:name, id:id}" --output tsv
```
    
### <a name="step-4-add-role-assignment"></a>Paso 4: Agregar asignación de roles

Para agregar una asignación de roles, use el comando [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create). Dependiendo del ámbito, el comando normalmente tiene uno de los siguientes formatos.

**Ámbito de recursos**

```azurecli
az role assignment create --assignee "{assignee}" \
--role "{roleNameOrId}" \
--scope "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/{providerName}/{resourceType}/{resourceSubType}/{resourceName}"
```

**Ámbito de grupo de recursos**

```azurecli
az role assignment create --assignee "{assignee}" \
--role "{roleNameOrId}" \
--resource-group "{resourceGroupName}"
```

**Ámbito de suscripción** 

```azurecli
az role assignment create --assignee "{assignee}" \
--role "{roleNameOrId}" \
--subscription "{subscriptionNameOrId}"
```

**Ámbito del grupo de administración** 

```azurecli
az role assignment create --assignee "{assignee}" \
--role "{roleNameOrId}" \
--scope "/providers/Microsoft.Management/managementGroups/{managementGroupName}"
``` 

A continuación se muestra un ejemplo de la salida cuando se asigna el rol de [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) a un usuario en un ámbito de grupos de recursos.

```azurecli
{
  "canDelegate": null,
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentId}",
  "name": "{roleAssignmentId}",
  "principalId": "{principalId}",
  "principalType": "User",
  "resourceGroup": "{resourceGroupName}",
  "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "scope": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "type": "Microsoft.Authorization/roleAssignments"
}
```
    
## <a name="add-role-assignment-examples"></a>Ejemplos para agregar asignaciones de roles

### <a name="add-role-assignment-for-a-specific-blob-container-resource-scope"></a>Agregar asignación de roles para un ámbito de recursos de un contenedor de blobs específico

Asigna el rol de [Colaborador de datos de Blob Storage](built-in-roles.md#storage-blob-data-contributor) a una entidad de servicio con el id. de objeto *55555555-5555-5555-5555-555555555555* en el ámbito de recursos de un contenedor de blobs denominado *blob-container-01*.

```azurecli
az role assignment create --assignee "55555555-5555-5555-5555-555555555555" \
--role "Storage Blob Data Contributor" \
--scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg/providers/Microsoft.Storage/storageAccounts/storage12345/blobServices/default/containers/blob-container-01"
```

### <a name="add-role-assignment-for-all-blob-containers-in-a-storage-account-resource-scope"></a>Agregar la asignación de roles a todos los contenedores de blobs en un ámbito de recursos de la cuenta de almacenamiento

Asigna el rol de [Colaborador de datos de Blob Storage](built-in-roles.md#storage-blob-data-contributor) a una entidad de servicio con el id. de objeto *55555555-5555-5555-5555-555555555555* en el ámbito de recursos de una cuenta de almacenamiento denominada *storage12345*.

```azurecli
az role assignment create --assignee "55555555-5555-5555-5555-555555555555" \
--role "Storage Blob Data Contributor" \
--scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg/providers/Microsoft.Storage/storageAccounts/storage12345"
```

### <a name="add-role-assignment-for-a-group-in-a-specific-virtual-network-resource-scope"></a>Agregar asignación de roles para un grupo en un ámbito de recursos de red virtual específico

Asigna el rol [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) al grupo *Ann Mack Team* con el id. 22222222-2222-2222-2222-222222222222 en un ámbito de recurso de una red virtual denominada *pharma-sales-project-network*.

```azurecli
az role assignment create --assignee "22222222-2222-2222-2222-222222222222" \
--role "Virtual Machine Contributor" \
--scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/pharma-sales/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network"
```

### <a name="add-role-assignment-for-a-user-at-a-resource-group-scope"></a>Agregar una asignación de roles para un usuario en el ámbito de un grupo de recursos

Asigna el rol [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) al usuario *patlong\@contoso.com* en el ámbito del grupo de recursos *pharma-sales*.

```azurecli
az role assignment create --assignee "patlong@contoso.com" \
--role "Virtual Machine Contributor" \
--resource-group "pharma-sales"
```

### <a name="add-role-assignment-for-a-user-using-the-unique-role-id-at-a-resource-group-scope"></a>Agregar una asignación de roles para un usuario con el id. de rol único en el ámbito de un grupo de recursos

Hay un par de veces en las que el nombre de un rol puede cambiar, por ejemplo:

- Utiliza su propio rol personalizado y decide cambiar el nombre.
- Utiliza una rol de versión preliminar que tiene **(Versión preliminar)** en el nombre. Cuando se libera el rol, se cambia su nombre.

Incluso si se cambia el nombre de un rol, su identificador no cambia. Si utiliza scripts o automatización para crear las asignaciones de roles, se recomienda utilizar el identificador de rol único en lugar del nombre del rol. Por tanto, si se cambia el nombre de un rol, es más probable que los scripts funcionen.

En el ejemplo siguiente, se asigna el rol [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) al usuario *patlong\@contoso.com* en el ámbito del grupo de recursos *pharma-sales*.

```azurecli
az role assignment create --assignee "patlong@contoso.com" \
--role "9980e02c-c2be-4d73-94e8-173b1dc7cf3c" \
--resource-group "pharma-sales"
```

### <a name="add-role-assignment-for-all-blob-containers-at-a-resource-group-scope"></a>Agregar la asignación de roles en todos los contenedores de blobs en un ámbito de grupo de recursos

Asigna el rol de [Colaborador de datos de Blob Storage](built-in-roles.md#storage-blob-data-contributor) a una entidad de servicio con el id. de objeto *55555555-5555-5555-5555-555555555555* en el ámbito del grupo de recursos *Example-Storage-rg*.

```azurecli
az role assignment create --assignee "55555555-5555-5555-5555-555555555555" \
--role "Storage Blob Data Contributor" \
--resource-group "Example-Storage-rg"
```

Como alternativa, puede especificar el grupo de recursos completo con el parámetro `--scope`:

```azurecli
az role assignment create --assignee "55555555-5555-5555-5555-555555555555" \
--role "Storage Blob Data Contributor" \
--scope "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Example-Storage-rg"
```

### <a name="add-role-assignment-for-an-application-at-a-resource-group-scope"></a>Agregar una asignación de roles para una aplicación en un ámbito de grupo de recursos

Asigna el rol [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) a una aplicación con el id. de objeto de entidad de servicio 44444444-4444-4444-4444-444444444444 en el ámbito del grupo de recursos *pharma-sales*.

```azurecli
az role assignment create --assignee "44444444-4444-4444-4444-444444444444" \
--role "Virtual Machine Contributor" \
--resource-group "pharma-sales"
```

### <a name="add-role-assignment-for-a-new-service-principal-at-a-resource-group-scope"></a>Agregar la asignación de roles de una nueva entidad de servicio en un ámbito de grupo de recursos

Si crea una entidad de servicio e inmediatamente intenta asignarle un rol, esa asignación de roles puede producir un error en algunos casos. Por ejemplo, si usa un script para crear una identidad administrada y luego intenta asignarle un rol a esa entidad de servicio, la asignación de roles podría producir un error. Es probable que el motivo de este error sea un retraso en la replicación. La entidad de servicio se crea en una región; sin embargo, la asignación de roles puede tener lugar en una región distinta que todavía no haya replicado la entidad de servicio. Para abordar este escenario, debe especificar el tipo de entidad de seguridad al crear la asignación de roles.

Para agregar una asignación de roles, use [az role assignment create](/cli/azure/role/assignment#az_role_assignment_create), especifique un valor para `--assignee-object-id` y establezca `--assignee-principal-type` en `ServicePrincipal`.

```azurecli
az role assignment create --assignee-object-id "{assigneeObjectId}" \
--assignee-principal-type "{assigneePrincipalType}" \
--role "{roleNameOrId}" \
--resource-group "{resourceGroupName}" \
--scope "/subscriptions/{subscriptionId}"
```

En el ejemplo siguiente, se asigna el rol [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) a la identidad administrada *msi-test* en el ámbito del grupo de recursos *pharma-sales*:

```azurecli
az role assignment create --assignee-object-id "33333333-3333-3333-3333-333333333333" \
--assignee-principal-type "ServicePrincipal" \
--role "Virtual Machine Contributor" \
--resource-group "pharma-sales"
```

### <a name="add-role-assignment-for-a-user-at-a-subscription-scope"></a>Agregar una asignación de roles para un usuario en un ámbito de suscripción

Asigna el rol de [Lector](built-in-roles.md#reader) al usuario *annm\@example.com* en el ámbito de una suscripción.

```azurecli
az role assignment create --assignee "annm@example.com" \
--role "Reader" \
--subscription "00000000-0000-0000-0000-000000000000"
```

### <a name="add-role-assignment-for-a-group-at-a-subscription-scope"></a>Agregar una asignación de roles para un grupo en el ámbito de una suscripción

Asigna el rol [Lector](built-in-roles.md#reader) al grupo *Ann Mack Team* con el id. 22222222-2222-2222-2222-222222222222 en el ámbito de suscripción.

```azurecli
az role assignment create --assignee "22222222-2222-2222-2222-222222222222" \
--role "Reader" \
--subscription "00000000-0000-0000-0000-000000000000"
```

### <a name="add-role-assignment-for-all-blob-containers-at-a-subscription-scope"></a>Agregar asignación de roles para todos los contenedores de blobs en un ámbito de suscripción

Asigna el rol de [Lector de datos de Blob Storage](built-in-roles.md#storage-blob-data-reader) al usuario *alain\@example.com* en el ámbito de una suscripción.

```azurecli
az role assignment create --assignee "alain@example.com" \
--role "Storage Blob Data Reader" \
--scope "/subscriptions/00000000-0000-0000-0000-000000000000"
```

### <a name="add-role-assignment-for-a-user-at-a-management-group-scope"></a>Agregar una asignación de roles para un usuario en el ámbito de un grupo de administración

Asigna el rol [Lector de facturación](built-in-roles.md#billing-reader) al usuario *alain\@example.com* en un ámbito de grupo de administración.

```azurecli
az role assignment create --assignee "alain@example.com" \
--role "Billing Reader" \
--scope "/providers/Microsoft.Management/managementGroups/marketing-group"
```

## <a name="remove-role-assignment"></a>Eliminar asignación de roles

En Azure RBAC, para quitar el acceso es preciso quitar una asignación de roles mediante [az role assignment delete](/cli/azure/role/assignment#az_role_assignment_delete).

En el ejemplo siguiente, se quita la asignación de roles [Colaborador de la máquina virtual](built-in-roles.md#virtual-machine-contributor) del usuario *patlong\@contoso.com* en el grupo de recursos *pharma-sales*:

```azurecli
az role assignment delete --assignee "patlong@contoso.com" \
--role "Virtual Machine Contributor" \
--resource-group "pharma-sales"
```

Quita el rol [Lector](built-in-roles.md#reader) del grupo *Ann Mack Team* con el id. 22222222-2222-2222-2222-222222222222 en el ámbito de suscripción.

```azurecli
az role assignment delete --assignee "22222222-2222-2222-2222-222222222222" \
--role "Reader" \
--subscription "00000000-0000-0000-0000-000000000000"
```

Quita el rol [Lector de facturación](built-in-roles.md#billing-reader) al usuario *alain\@example.com* en el ámbito de grupo de administración.

```azurecli
az role assignment delete --assignee "alain@example.com" \
--role "Billing Reader" \
--scope "/providers/Microsoft.Management/managementGroups/marketing-group"
```

## <a name="next-steps"></a>Pasos siguientes

- [Enumeración de asignaciones de roles de Azure mediante la CLI de Azure](role-assignments-list-cli.md)
- [Uso de la CLI de Azure para administrar los recursos y grupos de recursos de Azure](../azure-resource-manager/management/manage-resources-cli.md)
