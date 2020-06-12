---
title: 'Creación de grupos de administración para organizar los recursos: gobernanza en Azure'
description: Aprenda a crear grupos de administración de Azure para administrar varios recursos mediante el portal, Azure PowerShell y la CLI de Azure.
ms.date: 04/15/2020
ms.topic: conceptual
ms.openlocfilehash: 0abecf02062dd4461274a9bc3722d46e6e7cc3d1
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220755"
---
# <a name="create-management-groups-for-resource-organization-and-management"></a>Creación de grupos de administración para la organización y la administración de recursos

Los grupos de administración son contenedores que ayudan a administran el acceso, las directivas y el cumplimiento de varias suscripciones. Cree estos contenedores para construir una jerarquía eficaz y eficiente que pueda usarse con [Azure Policy](../policy/overview.md) y los [controles de acceso basados en roles de Azure](../../role-based-access-control/overview.md). Para obtener más información sobre los grupos de administración, consulte [Organización de los recursos con grupos de administración de Azure](overview.md).

El primer grupo de administración creado en el directorio puede tardar hasta 15 minutos en completarse. Hay procesos que se ejecutan la primera vez para configurar el servicio de grupos de administración dentro de Azure para su directorio. Recibirá una notificación cuando finalice el proceso. Para obtener más información, vea [Instalación inicial de los grupos de administración](./overview.md#initial-setup-of-management-groups).

## <a name="create-a-management-group"></a>Creación de un grupo de administración

Cualquier usuario de Azure AD del inquilino puede crear un grupo de administración sin el permiso de escritura del grupo de administración asignado a ese usuario. Este nuevo grupo de administración será un elemento secundario del grupo de administración raíz y se le asignará una asignación de rol de "Propietario" al creador. El servicio del grupo de administración ofrece esta funcionalidad para que las asignaciones de roles no sean necesarias en el nivel raíz. Ningún usuario tiene acceso al grupo de administración raíz cuando se crea. Para sortear el obstáculo de buscar administradores globales de Azure AD para empezar a usar grupos de administración, se permite la creación de grupos de administración iniciales en el nivel de  
raíz.

Puede crear el grupo de administración mediante el portal, una [plantilla de Resource Manager](../../azure-resource-manager/templates/deploy-to-tenant.md#create-management-group), PowerShell o la CLI de Azure.

### <a name="create-in-portal"></a>Crear en el portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. Seleccione **Todos los servicios** > **Administración y gobernanza**.

1. Seleccione **Grupos de administración**.

1. Seleccione **+ Agregar grupo de administración**.

   :::image type="content" source="./media/main.png" alt-text="Página para trabajar con grupos de administración" border="false":::

1. Rellene el campo de identificador de grupo de administración.

   - El **identificador de grupo de administración** es el identificador único de directorio que se usa para enviar comandos en este grupo de administración. Este identificador no es editable una vez creado, dado que se usa en todo el sistema de Azure para identificar este grupo. El [grupo de administración raíz](overview.md#root-management-group-for-each-directory) se crea automáticamente con un ID que es el identificador de Azure Active Directory. Para el resto de grupos de administración, asigne un identificador único.
   - El campo de nombre para mostrar es el nombre que se muestra en Azure Portal. Un nombre para mostrar independiente es un campo opcional al crear el grupo de administración y se puede cambiar en cualquier  
     momento.

   :::image type="content" source="./media/create_context_menu.png" alt-text="Panel de opciones para crear un nuevo grupo de administración" border="false":::

1. Seleccione **Guardar**.

### <a name="create-in-powershell"></a>Crear en PowerShell

Para PowerShell, use el cmdlet [New AzManagementGroup](/powershell/module/az.resources/new-azmanagementgroup) para crear un nuevo grupo de administración.

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso'
```

**GroupName** es un identificador único que se crea. Este identificador lo usan otros comandos para hacer referencia a este grupo y no se puede cambiar más adelante.

Si quiere que el grupo de administración muestre un nombre diferente en Azure Portal, agregue el parámetro **DisplayName**. Por ejemplo, para crear un grupo de administración con el nombre de grupo de Contoso y el nombre para mostrar de "Grupo de Contoso", use el siguiente cmdlet:

```azurepowershell-interactive
New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'
```

En los ejemplos anteriores, se crea el nuevo grupo de administración en el grupo de administración raíz. Para especificar otro grupo de administración como el elemento primario, use el parámetro **ParentId**.

```azurepowershell-interactive
$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id
```

### <a name="create-in-azure-cli"></a>Crear en la CLI de Azure

Para la CLI de Azure, use el comando [az account management-group create](/cli/azure/account/management-group?view=azure-cli-latest#az-account-management-group-create) para crear un nuevo grupo de administración.

```azurecli-interactive
az account management-group create --name Contoso
```

El **nombre** es un identificador único que se crea. Este identificador lo usan otros comandos para hacer referencia a este grupo y no se puede cambiar más adelante.

Si quiere que el grupo de administración muestre un nombre diferente en Azure Portal, agregue el parámetro **display-name**. Por ejemplo, para crear un grupo de administración con el nombre de grupo de Contoso y el nombre para mostrar de "Grupo de Contoso", use el siguiente comando:

```azurecli-interactive
az account management-group create --name Contoso --display-name 'Contoso Group'
```

En los ejemplos anteriores, se crea el nuevo grupo de administración en el grupo de administración raíz. Para especificar otro grupo de administración como el elemento primario, use el parámetro **parent** y proporcione el nombre del grupo principal.

```azurecli-interactive
az account management-group create --name ContosoSubGroup --parent Contoso
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los grupos de administración, consulte:

- [Creación de grupos de administración para organizar los recursos de Azure](./create.md)
- [Cambio, eliminación y administración de los grupos de administración](./manage.md)
- [Revisión de grupos de administración en el módulo de recursos de Azure PowerShell](/powershell/module/az.resources#resources)
- [Revisión de grupos de administración en la API REST](/rest/api/resources/managementgroups)
- [Revisión de grupos de administración en la CLI de Azure](/cli/azure/account/management-group)
