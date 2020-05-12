---
title: 'Tutorial: Concesión de acceso de usuario a recursos de Azure mediante una plantilla de Azure Resource Manager: Azure RBAC'
description: En este tutorial aprenderá a conceder acceso de usuario a recursos de Azure mediante una plantilla de Azure Resource Manager y el control de acceso basado en rol (Azure RBAC).
services: role-based-access-control,azure-resource-manager
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/15/2019
ms.author: rolyon
ms.openlocfilehash: f98b1e9c57cf9f624c5af3e59c8afd56d073dd28
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2020
ms.locfileid: "82735477"
---
# <a name="tutorial-grant-a-user-access-to-azure-resources-using-an-azure-resource-manager-template"></a>Tutorial: Concesión de acceso de usuario a recursos de Azure mediante una plantilla de Azure Resource Manager

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en la que se administra el acceso a los recursos de Azure. En este tutorial, se crea un grupo de recursos y se concede acceso a un usuario para crear y administrar máquinas virtuales en el grupo de recursos. Este tutorial se centra en el proceso de implementación de una plantilla de Resource Manager para conceder el acceso. Para más información sobre el desarrollo de plantillas de Resource Manager, consulte la [documentación de Resource Manager](/azure/azure-resource-manager/) y la [referencia de plantilla](/azure/templates/microsoft.authorization/allversions
).

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conceder acceso a un usuario en el ámbito del grupo de recursos
> * Validación de la implementación
> * Limpieza

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Prerrequisitos

Para agregar y quitar las asignaciones de roles, debe tener:

* Permisos `Microsoft.Authorization/roleAssignments/write` y `Microsoft.Authorization/roleAssignments/delete`, como [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator) o [propietario](built-in-roles.md#owner)

## <a name="grant-access"></a>Conceder acceso

La plantilla usada en esta guía de inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-rbac-builtinrole-resourcegroup/). [Aquí](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization) puede encontrar más plantillas relacionadas con la autorización de Azure.

Para implementar la plantilla, seleccione **Pruébelo** para abrir Azure Cloud Shell y pegue el siguiente script de PowerShell en la ventana del shell. Para pegar el código, haga clic con el botón derecho en la ventana del shell y seleccione **Pegar**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$emailAddress = Read-Host -Prompt "Enter your email address that is associated with your Azure subscription (used to find the principal ID)"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

$resourceGroupName = "${projectName}rg"
$roleAssignmentName = New-Guid
$principalId = (Get-AzAdUser -Mail $emailAddress).id
$roleDefinitionId = (Get-AzRoleDefinition -name "Virtual Machine Contributor").id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-rbac-builtinrole-resourcegroup/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -roleAssignmentName $roleAssignmentName -roleDefinitionID $roleDefinitionId -principalId $principalId
```

## <a name="validate-the-deployment"></a>Validación de la implementación

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Abra el grupo de recursos creado en el último procedimiento. El nombre predeterminado es el nombre del proyecto con **rg** anexado.
1. Seleccione **Control de acceso (IAM)** en el menú izquierdo.
1. Seleccione **Asignaciones de roles**. 
1. En **Nombre**, escriba la dirección de correo electrónico que escribió en el último procedimiento. Verá que el usuario con la dirección de correo electrónico tiene el rol de **colaborador de máquina virtual**.

## <a name="clean-up"></a>Limpieza

Para eliminar el grupo de recursos creado en el último procedimiento, seleccione **Pruébelo** para abrir Azure Cloud Shell y pegue el siguiente script de PowerShell en la ventana del shell.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a same project name you used in the last procedure"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Concesión de acceso de usuario a recursos de Azure mediante Azure PowerShell](tutorial-role-assignments-user-powershell.md)