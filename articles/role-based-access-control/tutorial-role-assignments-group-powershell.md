---
title: 'Tutorial: Concesión de acceso de grupo a recursos de Azure mediante Azure PowerShell: Azure RBAC'
description: En este tutorial aprenderá a conceder acceso de grupo a recursos de Azure mediante Azure PowerShell y el control de acceso basado en rol (RBAC).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.openlocfilehash: 0d72ea23b74137e7e57f892b831b0be1b4a89de5
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735528"
---
# <a name="tutorial-grant-a-group-access-to-azure-resources-using-azure-powershell"></a>Tutorial: Concesión de acceso de grupo a recursos de Azure mediante Azure PowerShell

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en la que se administra el acceso a los recursos de Azure. En este tutorial se concede a un grupo acceso para ver todos los elementos de una suscripción y administrar todo el contenido de un grupo de recursos mediante Azure PowerShell.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conceder acceso a un grupo a distintos ámbitos
> * Lista de acceso
> * Quitar acceso

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Prerrequisitos

Para realizar este tutorial, necesita:

- Permisos para crear grupos en Azure Active Directory (o tener un grupo existente)
- [Azure Cloud Shell](/azure/cloud-shell/quickstart-powershell)

## <a name="role-assignments"></a>Asignaciones de roles

En Azure RBAC, para conceder acceso es preciso crear una asignación de roles. Una asignación de roles consta de tres elementos: entidad de seguridad, definición de rol y ámbito. Estas son las dos asignaciones de roles que llevará a cabo en este tutorial:

| Entidad de seguridad | Definición de roles | Ámbito |
| --- | --- | --- |
| Grupo<br>(Grupo de tutorial de RBAC) | [Lector](built-in-roles.md#reader) | Subscription |
| Grupo<br>(Grupo de tutorial de RBAC)| [Colaborador](built-in-roles.md#contributor) | Resource group<br>(rbac-tutorial-resource-group) |

   ![Asignaciones de roles de un grupo](./media/tutorial-role-assignments-group-powershell/rbac-role-assignments.png)

## <a name="create-a-group"></a>Creación de un grupo

Para asignar un rol, se necesita un usuario, un grupo o una entidad de servicio. Si aún no tiene un grupo, puede crearlo.

- En Azure Cloud Shell, cree un nuevo grupo con el comando [New-AzureADGroup](/powershell/module/azuread/new-azureadgroup).

   ```azurepowershell
   New-AzureADGroup -DisplayName "RBAC Tutorial Group" `
     -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
   ```

   ```Example
   ObjectId                             DisplayName         Description
   --------                             -----------         -----------
   11111111-1111-1111-1111-111111111111 RBAC Tutorial Group
   ```

Si no tiene permisos para crear grupos, puede probar el [Tutorial: Concesión de acceso de usuario a recursos de Azure PowerShell](tutorial-role-assignments-user-powershell.md) en su lugar.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Use un grupo de recursos para mostrar cómo asignar un rol a un ámbito de un grupo de recursos.

1. Obtenga una lista de regiones con el comando [Get AzLocation](/powershell/module/az.resources/get-azlocation).

   ```azurepowershell
   Get-AzLocation | select Location
   ```

1. Seleccione una ubicación próxima y asígnela a una variable.

   ```azurepowershell
   $location = "westus"
   ```

1. Cree un grupo de recursos con el comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup).

   ```azurepowershell
   New-AzResourceGroup -Name "rbac-tutorial-resource-group" -Location $location
   ```

   ```Example
   ResourceGroupName : rbac-tutorial-resource-group
   Location          : westus
   ProvisioningState : Succeeded
   Tags              :
   ResourceId        : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
   ```

## <a name="grant-access"></a>Conceder acceso

Para conceder acceso al grupo, use el comando [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) para asignar un rol. Debe especificar la entidad de seguridad, la definición del rol y el ámbito.

1. Obtenga el identificador de objeto del grupo mediante el comando [Get AzureADGroup](/powershell/module/azuread/new-azureadgroup).

    ```azurepowershell
    Get-AzureADGroup -SearchString "RBAC Tutorial Group"
    ```

    ```Example
    ObjectId                             DisplayName         Description
    --------                             -----------         -----------
    11111111-1111-1111-1111-111111111111 RBAC Tutorial Group
    ```

1. Guarde el identificador de objeto del grupo en una variable.

    ```azurepowershell
    $groupId = "11111111-1111-1111-1111-111111111111"
    ```

1. Obtenga el identificador de la suscripción mediante el comando [Get AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription).

    ```azurepowershell
    Get-AzSubscription
    ```

    ```Example
    Name     : Pay-As-You-Go
    Id       : 00000000-0000-0000-0000-000000000000
    TenantId : 22222222-2222-2222-2222-222222222222
    State    : Enabled
    ```

1. Guarde el ámbito de la suscripción en una variable.

    ```azurepowershell
    $subScope = "/subscriptions/00000000-0000-0000-0000-000000000000"
    ```

1. Asigne el rol de [lector](built-in-roles.md#reader) al grupo en el ámbito de la suscripción.

    ```azurepowershell
    New-AzRoleAssignment -ObjectId $groupId `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/44444444-4444-4444-4444-444444444444
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    ```

1. Asigne el rol de [colaborador](built-in-roles.md#contributor) al grupo en el ámbito del grupo de recursos.

    ```azurepowershell
    New-AzRoleAssignment -ObjectId $groupId `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    ```

## <a name="list-access"></a>Lista de acceso

1. Para comprobar el acceso de la suscripción, use el comando [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) para enumerar las asignaciones de roles.

    ```azurepowershell
    Get-AzRoleAssignment -ObjectId $groupId -Scope $subScope
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    ```

    En la salida, puede ver que se ha asignado que el rol de lector se ha asignado al grupo del tutorial de RBAC en el ámbito de la suscripción.

1. Para comprobar el acceso del grupo de recursos, use el comando [Get-AzRoleAssignment](/powershell/module/az.resources/get-azroleassignment) para enumerar las asignaciones de roles.

    ```azurepowershell
    Get-AzRoleAssignment -ObjectId $groupId -ResourceGroupName "rbac-tutorial-resource-group"
    ```

    ```Example
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group/providers/Microsoft.Authorization/roleAssignments/33333333-3333-3333-3333-333333333333
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/rbac-tutorial-resource-group
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Contributor
    RoleDefinitionId   : b24988ac-6180-42a0-ab88-20f7382dd24c
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    
    RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Authorization/roleAssignments/22222222-2222-2222-2222-222222222222
    Scope              : /subscriptions/00000000-0000-0000-0000-000000000000
    DisplayName        : RBAC Tutorial Group
    SignInName         :
    RoleDefinitionName : Reader
    RoleDefinitionId   : acdd72a7-3385-48ef-bd42-f606fba81ae7
    ObjectId           : 11111111-1111-1111-1111-111111111111
    ObjectType         : Group
    CanDelegate        : False
    ```

    En la salida, puede ver que ambos roles, colaborador y lector, se han asignado al grupo del tutorial de RBAC. El rol de colaborador está en el ámbito de rbac-tutorial-resource-group, mientras que el rol de lector se hereda en el ámbito de la suscripción.

## <a name="optional-list-access-using-the-azure-portal"></a>(Opcional) Acceso a la lista mediante Azure Portal

1. Para ver el aspecto de las asignaciones de roles en Azure Portal, vea la hoja **Control de acceso (IAM)** de la suscripción.

    ![Asignaciones de roles de un grupo en un ámbito de la suscripción](./media/tutorial-role-assignments-group-powershell/role-assignments-subscription.png)

1. Vea la hoja **Control de acceso (IAM)** de un grupo de recursos.

    ![Asignaciones de roles de un grupo en un ámbito del grupo de recursos](./media/tutorial-role-assignments-group-powershell/role-assignments-resource-group.png)

## <a name="remove-access"></a>Quitar acceso

Para quitar el acceso de usuarios, grupos y aplicaciones, use [Remove-AzRoleAssignment](/powershell/module/az.resources/remove-azroleassignment) para quitar una asignación de roles.

1. Utilice el siguiente comando para quitar la asignación del rol de colaborador al grupo del ámbito del grupo de recursos.

    ```azurepowershell
    Remove-AzRoleAssignment -ObjectId $groupId `
      -RoleDefinitionName "Contributor" `
      -ResourceGroupName "rbac-tutorial-resource-group"
    ```

1. Utilice el siguiente comando para quitar la asignación del rol de colaborador al grupo del ámbito de la suscripción.

    ```azurepowershell
    Remove-AzRoleAssignment -ObjectId $groupId `
      -RoleDefinitionName "Reader" `
      -Scope $subScope
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Para limpiar los recursos creados por este tutorial puede eliminar tanto el grupo como el grupo de recursos.

1. Elimine el grupo de recursos con el comando [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup).

    ```azurepowershell
    Remove-AzResourceGroup -Name "rbac-tutorial-resource-group"
    ```

    ```Example
    Confirm
    Are you sure you want to remove resource group 'rbac-tutorial-resource-group'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    ```
    
1. Cuando se le pida confirmación, escriba **Y**. Tardará unos segundos en eliminarse.

1. Elimine el grupo mediante el comando [Remove-AzureADGroup](/powershell/module/azuread/remove-azureadgroup).

    ```azurepowershell
    Remove-AzureADGroup -ObjectId $groupId
    ```
    
    Si recibe un error al intentar eliminar el grupo, también puede eliminarlo en el portal.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Incorporación o eliminación de asignaciones de roles de Azure con Azure PowerShell](role-assignments-powershell.md)
