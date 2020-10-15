---
title: 'Creación de un rol personalizado de Azure mediante plantillas de Azure Resource Manager: Azure RBAC'
description: Aprenda a crear roles personalizados de Azure mediante plantillas de Azure Resource Manager y el control de acceso basado en roles de Azure (RBAC de Azure).
services: role-based-access-control,azure-resource-manager
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.topic: how-to
ms.custom: subject-armqs
ms.workload: identity
ms.date: 06/25/2020
ms.author: rolyon
ms.openlocfilehash: bcf1966ffc326291448cb611d99390fe0d652151
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85398044"
---
# <a name="create-an-azure-custom-role-using-an-azure-resource-manager-template"></a>Creación de un rol personalizado de Azure mediante plantillas de Azure Resource Manager

Si los [roles integrados de Azure](built-in-roles.md) no cumplen las necesidades específicas de su organización, puede crear los [suyos propios](custom-roles.md). En este artículo se describe cómo crear un rol personalizado mediante una plantilla de Resource Manager.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Requisitos previos

Para crear un rol personalizado, debe tener:

- Permisos para crear roles personalizados, como [Propietario](built-in-roles.md#owner) o [Administrador de acceso de usuarios](built-in-roles.md#user-access-administrator)

## <a name="create-a-custom-role"></a>Crear un rol personalizado

Para crear un rol personalizado, especifique un nombre de rol, los permisos y dónde se puede usar el rol. En este artículo, se creará un rol denominado "Rol personalizado: lector RG" con permisos de recursos que se pueden asignar en un ámbito de suscripción o inferior.

### <a name="review-the-template"></a>Revisión de la plantilla

La plantilla usada en este artículo forma parte de las [plantillas de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-deployments/create-role-def). La plantilla tiene cuatro parámetros y una sección de recursos. Los cuatro parámetros son:

- Matriz de acciones con un valor predeterminado de ["Microsoft.Resources/subscriptions/resourceGroups/read"]
- Matriz de notActions con un valor predeterminado vacío
- Nombre de rol con un valor predeterminado de "Rol personalizado: lector RG"
- Descripción del rol con un valor predeterminado de "Implementación del nivel de suscripción de una definición de rol"

El recurso definido en la plantilla es el siguiente:

- [Microsoft.Authorization/roleDefinitions](/azure/templates/Microsoft.Authorization/roleDefinitions)

El ámbito en el que se puede asignar este rol personalizado se establece en la suscripción actual.

:::code language="json" source="~/quickstart-templates/subscription-deployments/create-role-def/azuredeploy.json":::

### <a name="deploy-the-template"></a>Implementación de la plantilla

Siga estos pasos para implementar la plantilla anterior.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Abra Azure Cloud Shell para PowerShell.

1. Copie y pegue el siguiente script en Cloud Shell.

    ```azurepowershell
    $location = Read-Host -Prompt "Enter a location (i.e. centralus)"
    [string[]]$actions = Read-Host -Prompt "Enter actions as a comma-separated list (i.e. action1,action2)"
    $actions = $actions.Split(',')

    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/create-role-def/azuredeploy.json"

    New-AzDeployment -Location $location -TemplateUri $templateUri -actions $actions
    ```

1. Escriba una ubicación para la implementación, como *centralus*.

1. Escriba una lista de acciones para el rol personalizado como una lista separada por comas, como *Microsoft.Resources/resources/read,Microsoft.Resources/subscriptions/resourceGroups/read*.

1. Si fuera necesario, presione Entrar para ejecutar el comando New-AzDeployment.

    El comando [New-AzDeployment](/powershell/module/az.resources/new-azdeployment) implementa la plantilla para crear el rol personalizado.

    Debería ver un resultado similar al siguiente:

    ```azurepowershell
    PS> New-AzDeployment -Location $location -TemplateUri $templateUri -actions $actions
    
    Id                      : /subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/azuredeploy
    DeploymentName          : azuredeploy
    Location                : centralus
    ProvisioningState       : Succeeded
    Timestamp               : 6/25/2020 8:08:32 PM
    Mode                    : Incremental
    TemplateLink            :
                              Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-deployments/create-role-def/azuredeploy.json
                              ContentVersion : 1.0.0.0
    
    Parameters              :
                              Name               Type                       Value
                              =================  =========================  ==========
                              actions            Array                      [
                                "Microsoft.Resources/resources/read",
                                "Microsoft.Resources/subscriptions/resourceGroups/read"
                              ]
                              notActions         Array                      []
                              roleName           String                     Custom Role - RG Reader
                              roleDescription    String                     Subscription Level Deployment of a Role Definition
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="review-deployed-resources"></a>Revisión de los recursos implementados

Siga estos pasos para comprobar que se ha creado el rol personalizado.

1. Ejecute el comando [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) para mostrar el rol personalizado.

    ```azurepowershell
    Get-AzRoleDefinition "Custom Role - RG Reader" | ConvertTo-Json
    ```

    Debería ver un resultado similar al siguiente:

    ```azurepowershell
    {
      "Name": "Custom Role - RG Reader",
      "Id": "11111111-1111-1111-1111-111111111111",
      "IsCustom": true,
      "Description": "Subscription Level Deployment of a Role Definition",
      "Actions": [
        "Microsoft.Resources/resources/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/{subscriptionId}"
      ]
    }
    ```

1. Abra la suscripción en Azure Portal.

1. En el menú de la izquierda, haga clic en **Control de acceso (IAM)** .

1. Haga clic en la pestaña **Roles**.

1. Establezca la lista **Tipo** en **CustomRole**.

1. Compruebe que se muestre el rol **Rol personalizado: lector RG**.

   ![Nuevo rol personalizado en Azure Portal](./media/custom-roles-template/custom-role-template-portal.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar el rol personalizado, siga estos pasos.

1. Ejecute el siguiente comando para quitar el rol personalizado.

    ```azurepowershell
    Get-AzRoleDefinition -Name "Custom Role - RG Reader" | Remove-AzRoleDefinition
    ```

1. Escriba **Y** para confirmar que desea quitar el rol personalizado.

## <a name="next-steps"></a>Pasos siguientes

- [Descripción de las definiciones de roles de Azure](role-definitions.md)
- [Inicio rápido: Adición de una asignación de roles de Azure mediante una plantilla de Azure Resource Manager](quickstart-role-assignments-template.md)
- [Documentación de las plantillas de Resource Manager](../azure-resource-manager/templates/index.yml)
