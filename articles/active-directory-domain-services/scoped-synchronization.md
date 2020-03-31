---
title: Sincronización con ámbito en Azure AD Domain Services | Microsoft Docs
description: Aprenda a configurar la sincronización con ámbito entre Azure AD y un dominio administrado de Azure Active Directory Domain Services.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 9389cf0f-0036-4b17-95da-80838edd2225
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 11/26/2019
ms.author: iainfou
ms.openlocfilehash: cc126af67a0d8627d61e595cee56f3df8973340d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77613047"
---
# <a name="configure-scoped-synchronization-from-azure-ad-to-azure-active-directory-domain-services"></a>Configuración de la sincronización con ámbito entre Azure AD y Azure Active Directory Domain Services

Para proporcionar servicios de autenticación, Azure Active Directory Domain Services (Azure AD DS) sincroniza los usuarios y grupos de Azure AD. En un entorno híbrido, los usuarios y grupos de un entorno local de Active Directory Domain Services (AD DS) se pueden sincronizar primero con Azure AD mediante Azure AD Connect y, luego, con Azure AD DS.

De forma predeterminada, todos los usuarios y grupos de un directorio de Azure AD se sincronizan con un dominio administrado de Azure AD DS. Si tiene necesidades específicas, puede optar por sincronizar solo un conjunto definido de usuarios.

En este artículo se muestra cómo crear un dominio administrado de Azure AD DS que use la sincronización con ámbito y, luego, cambiar o deshabilitar el conjunto de usuarios con ámbito.

## <a name="scoped-synchronization-overview"></a>Introducción a la sincronización con ámbito

De forma predeterminada, todos los usuarios y grupos de un directorio de Azure AD se sincronizan con un dominio administrado de Azure AD DS. Si solo unos cuantos usuarios necesitan acceso al dominio administrado, puede sincronizar únicamente esas cuentas de usuario. Esta sincronización son ámbito se basa en grupos. Al configurar la sincronización con ámbito basada en grupos, solo las cuentas de usuario que pertenecen a los grupos que especifique se sincronizan con el dominio administrado de Azure AD DS.

En la tabla siguiente se describe cómo usar la sincronización con ámbito:

| Estado actual | Estado deseado | Configuración necesaria |
| --- | --- | --- |
| Un dominio administrado existente está configurado para sincronizar todas las cuentas de usuario y grupos. | Quiere sincronizar únicamente las cuentas de usuario que pertenecen a grupos específicos. | No puede pasar de sincronizar todos los usuarios a usar la sincronización con ámbito. [Elimine el dominio administrado existente](delete-aadds.md) y, luego, siga los pasos de este artículo para volver a crear un dominio administrado de Azure AD DS con la sincronización con ámbito configurada. |
| No existen dominios administrados. | Quiere crear un dominio administrado y sincronizar solo las cuentas de usuario que pertenecen a grupos específicos. | Siga los pasos de este artículo para crear un dominio administrado de Azure AD DS con la sincronización con ámbito configurada. |
| Un dominio administrado existente está configurado para sincronizar únicamente las cuentas que pertenecen a grupos específicos. | Quiere modificar la lista de grupos cuyos usuarios se deben sincronizar con el dominio administrado de Azure AD DS. | Siga los pasos de este artículo para modificar la sincronización con ámbito. |

Use Azure Portal o PowerShell para configurar la sincronización con ámbito:

| Acción | | |
|--|--|--|
| Creación de un dominio administrado de Azure AD DS y configuración de la sincronización con ámbito | [Azure Portal](#enable-scoped-synchronization-using-the-azure-portal) | [PowerShell](#enable-scoped-synchronization-using-powershell) |
| Modificación de la sincronización con ámbito | [Azure Portal](#modify-scoped-synchronization-using-the-azure-portal) | [PowerShell](#modify-scoped-synchronization-using-powershell) |
| Deshabilitación de la sincronización con ámbito | [Azure Portal](#disable-scoped-synchronization-using-the-azure-portal) | [PowerShell](#disable-scoped-synchronization-using-powershell) |

> [!WARNING]
> Al cambiar el ámbito de sincronización el dominio administrado de Azure AD DS vuelve a sincronizar todos los datos.
> 
>  * Cuando se cambia el ámbito de sincronización de un dominio administrado de Azure AD DS, se produce una resincronización completa.
>  * Los objetos que ya no son necesarios en el dominio administrado de Azure AD DS se eliminan. Se crean objetos en el dominio administrado.
>  * La resincronización puede tardar mucho tiempo en completarse; depende del número de objetos, como usuarios, grupos y pertenencias a grupos en el dominio administrado de Azure AD DS y el directorio de Azure AD. En directorios grandes con muchos cientos de miles de objetos, la resincronización puede tardar algunos días.

## <a name="enable-scoped-synchronization-using-the-azure-portal"></a>Habilitación de la sincronización con ámbito mediante Azure Portal

1. Siga el [tutorial para crear y configurar una instancia de Azure AD DS](tutorial-create-instance-advanced.md). Aparte del ámbito de sincronización, complete todos los requisitos previos y los pasos de implementación.
1. Elija **Ámbito** en el paso de sincronización y, después, seleccione los grupos de Azure AD que se van a sincronizar con la instancia de Azure AD DS.

El dominio administrado de Azure AD DS puede tardar hasta una hora en finalizar la implementación. En Azure Portal, la página **Información general** del dominio administrado de Azure AD DS muestra el estado actual durante esta fase de implementación.

Cuando en Azure Portal se muestra que el dominio administrado de Azure AD DS ha terminado de aprovisionarse, es necesario realizar las siguientes tareas:

* Actualice la configuración de DNS para la red virtual de manera que las máquinas virtuales puedan encontrar el dominio administrado para la unión o autenticación de dominios.
    * Para configurar DNS, seleccione el dominio administrado de Azure AD DS en el portal. En la ventana **Información general**, se le pedirá que configure automáticamente estos valores de DNS.
* [Habilite la sincronización de contraseñas en Azure AD Domain Services](tutorial-create-instance-advanced.md#enable-user-accounts-for-azure-ad-ds) de manera que los usuarios finales puedan iniciar sesión en el dominio administrado mediante sus credenciales corporativas.

## <a name="modify-scoped-synchronization-using-the-azure-portal"></a>Modificación de la sincronización con ámbito mediante Azure Portal

Para modificar la lista de grupos cuyos usuarios deben sincronizarse con el dominio administrado de Azure AD DS, lleve a cabo los pasos siguientes:

1. En Azure Portal, busque y seleccione **Azure AD Domain Services**. Seleccione la instancia, como *aaddscontoso.com*.
1. Seleccione **Sincronización** en el menú de la izquierda.
1. Para agregar un grupo, elija **+ Seleccionar grupos** en la parte superior y, después, seleccione los grupos que quiere agregar.
1. Para quitar un grupo del ámbito de sincronización, selecciónelo en la lista de grupos actualmente sincronizados y elija **Quitar grupos**.
1. Cuando se realicen todos los cambios, seleccione **Guardar ámbito de sincronización**.

Al cambiar el ámbito de sincronización el dominio administrado de Azure AD DS vuelve a sincronizar todos los datos. Los objetos que ya no son necesarios en el dominio administrado de Azure AD DS se eliminan y la resincronización puede tardar mucho tiempo en completarse.

## <a name="disable-scoped-synchronization-using-the-azure-portal"></a>Deshabilitación de la sincronización con ámbito mediante Azure Portal

Para deshabilitar la sincronización con ámbito basada en grupos para un dominio administrado de Azure AD DS, lleve a cabo los pasos siguientes:

1. En Azure Portal, busque y seleccione **Azure AD Domain Services**. Seleccione la instancia, como *aaddscontoso.com*.
1. Seleccione **Sincronización** en el menú de la izquierda.
1. Establezca el ámbito de sincronización de **Ámbito** a **Todo** y, luego, seleccione **Guardar ámbito de sincronización**.

Al cambiar el ámbito de sincronización el dominio administrado de Azure AD DS vuelve a sincronizar todos los datos. Los objetos que ya no son necesarios en el dominio administrado de Azure AD DS se eliminan y la resincronización puede tardar mucho tiempo en completarse.

## <a name="powershell-script-for-scoped-synchronization"></a>Script de PowerShell para la sincronización con ámbito

Para configurar la sincronización de ámbito mediante PowerShell, primero guarde el siguiente script en un archivo llamado `Select-GroupsToSync.ps1`. Este script configura Azure AD DS para sincronizar los grupos seleccionados de Azure AD. Todas las cuentas de usuario que forman parte de los grupos especificados se sincronizan con el dominio administrado de Azure AD DS.

Este script se usa en los pasos adicionales de este artículo.

```powershell
param (
    [Parameter(Position = 0)]
    [String[]]$groupsToAdd
)

Connect-AzureAD
$sp = Get-AzureADServicePrincipal -Filter "AppId eq '2565bd9d-da50-47d4-8b85-4c97f669dc36'"
$role = $sp.AppRoles | where-object -FilterScript {$_.DisplayName -eq "User"}

Write-Output "`n****************************************************************************"

Write-Output "Total group-assignments need to be added: $($groupsToAdd.Count)"
$newGroupIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($groupName in $groupsToAdd)
{
    try
    {
        $group = Get-AzureADGroup -Filter "DisplayName eq '$groupName'"
        $newGroupIds.Add($group.ObjectId)

        Write-Output "Group-Name: $groupName, Id: $($group.ObjectId)"
    }
    catch
    {
        Write-Error "Failed to find group: $groupName. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

$currentAssignments = Get-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId
Write-Output "Total current group-assignments: $($currentAssignments.Count), SP-ObjectId: $($sp.ObjectId)"

$currAssignedObjectIds = New-Object 'System.Collections.Generic.HashSet[string]'
foreach ($assignment in $currentAssignments)
{
    Write-Output "Assignment-ObjectId: $($assignment.PrincipalId)"

    if ($newGroupIds.Contains($assignment.PrincipalId) -eq $false)
    {
        Write-Output "This assignment is not needed anymore. Removing it! Assignment-ObjectId: $($assignment.PrincipalId)"
        Remove-AzureADServiceAppRoleAssignment -ObjectId $sp.ObjectId -AppRoleAssignmentId $assignment.ObjectId
    }
    else
    {
        $currAssignedObjectIds.Add($assignment.PrincipalId)
    }
}

Write-Output "****************************************************************************`n"
Write-Output "`n****************************************************************************"

foreach ($id in $newGroupIds)
{
    try
    {
        if ($currAssignedObjectIds.Contains($id) -eq $false)
        {
            Write-Output "Adding new group-assignment. Role-Id: $($role.Id), Group-Object-Id: $id, ResourceId: $($sp.ObjectId)"
            New-AzureADGroupAppRoleAssignment -Id $role.Id -ObjectId $id -PrincipalId $id -ResourceId $sp.ObjectId
        }
        else
        {
            Write-Output "Group-ObjectId: $id is already assigned."
        }
    }
    catch
    {
        Write-Error "Exception occurred assigning Object-ID: $id. Exception: $($_.Exception)."
    }
}

Write-Output "****************************************************************************`n"
```

## <a name="enable-scoped-synchronization-using-powershell"></a>Habilitación de la sincronización con ámbito mediante PowerShell

Use PowerShell para completar este conjunto de pasos. Consulte las instrucciones para [habilitar Azure Active Directory Domain Services mediante PowerShell](powershell-create-instance.md). Un par de pasos de este artículo se han modificado ligeramente para configurar la sincronización con ámbito.

1. Lleve a cabo las siguientes tareas del artículo para habilitar Azure AD DS con PowerShell. Deténgase en el paso para crear realmente el dominio administrado. La sincronización con ámbito se configura para crear el dominio administrado de Azure AD DS.

   * [Instale los módulos de PowerShell necesarios](powershell-create-instance.md#prerequisites).
   * [Cree la entidad de servicio y el grupo de Azure AD necesarios para el acceso administrativo](powershell-create-instance.md#create-required-azure-ad-resources).
   * [Cree recursos de Azure complementarios, como una red virtual y subredes](powershell-create-instance.md#create-supporting-azure-resources).

1. Determine los grupos y los usuarios que contienen que quiere sincronizar desde Azure AD. Cree una lista de los nombres para mostrar de los grupos que se van a sincronizar con Azure AD DS.

1. Ejecute el [script de la sección anterior](#powershell-script-for-scoped-synchronization) y use el parámetro *-groupsToAdd* para pasar la lista de grupos que se van a sincronizar.

   > [!WARNING]
   > Debe incluir el grupo *Administradores de controladores de dominio de AAD* en la lista de grupos para la sincronización con ámbito. Si no incluye este grupo, no se puede usar el dominio administrado de Azure AD DS.

   ```powershell
   .\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName2")
   ```

1. Ahora, cree el dominio administrado de Azure AD DS y habilite la sincronización son ámbito basada en grupos. Incluya *"filteredSync" = "Enabled"* en el parámetro *-Properties*.

    Establezca el identificador de la suscripción de Azure y luego proporcione un nombre para el dominio administrado, como *aaddscontoso.com*. Puede obtener el identificador de la suscripción con el cmdlet [Get-AzSubscription][Get-AzSubscription]. Establezca el nombre del grupo de recursos, el nombre de la red virtual y la región en los valores usados en los pasos anteriores para crear los recursos de Azure complementarios:

   ```powershell
   $AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
   $ManagedDomainName = "aaddscontoso.com"
   $ResourceGroupName = "myResourceGroup"
   $VnetName = "myVnet"
   $AzureLocation = "westus"

   # Enable Azure AD Domain Services for the directory.
   New-AzResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
   -Location $AzureLocation `
   -Properties @{"DomainName"=$ManagedDomainName; "filteredSync" = "Enabled"; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
   -Force -Verbose
   ```

Tarda unos minutos hasta que se crea el recurso y se devuelve el control al símbolo del sistema de PowerShell. El dominio administrado de Azure AD DS se sigue aprovisionando en segundo plano y puede tardar hasta una hora en finalizar la implementación. En Azure Portal, la página **Información general** del dominio administrado de Azure AD DS muestra el estado actual durante esta fase de implementación.

Cuando en Azure Portal se muestra que el dominio administrado de Azure AD DS ha terminado de aprovisionarse, es necesario realizar las siguientes tareas:

* Actualice la configuración de DNS para la red virtual de manera que las máquinas virtuales puedan encontrar el dominio administrado para la unión o autenticación de dominios.
    * Para configurar DNS, seleccione el dominio administrado de Azure AD DS en el portal. En la ventana **Información general**, se le pedirá que configure automáticamente estos valores de DNS.
* Si ha creado un dominio administrado de Azure AD DS en una región que admite Availability Zones, cree un grupo de seguridad de red para restringir el tráfico de la red virtual de ese dominio. Se crea un equilibrador de carga estándar de Azure que exige la presencia de estas reglas. Este grupo de seguridad de red protege Azure AD DS y es necesario para que el dominio administrado funcione correctamente.
    * Para crear el grupo de seguridad de red y las reglas necesarias, seleccione el dominio administrado de Azure AD DS en el portal. En la ventana **Información general**, se le pedirá que cree y configure automáticamente el grupo de seguridad de red.
* [Habilite la sincronización de contraseñas en Azure AD Domain Services](tutorial-create-instance-advanced.md#enable-user-accounts-for-azure-ad-ds) de manera que los usuarios finales puedan iniciar sesión en el dominio administrado mediante sus credenciales corporativas.

## <a name="modify-scoped-synchronization-using-powershell"></a>Modificación de la sincronización con ámbito mediante PowerShell

Para modificar la lista de grupos cuyos usuarios se deben sincronizar con el dominio administrado de Azure AD DS, vuelva a ejecutar el [script de PowerShell](#powershell-script-for-scoped-synchronization) y especifique la nueva lista de grupos. En el ejemplo siguiente, los grupos que se van a sincronizar ya no incluyen *GroupName2*, ahora incluyen *GroupName3*.

> [!WARNING]
> Debe incluir el grupo *Administradores de controladores de dominio de AAD* en la lista de grupos para la sincronización con ámbito. Si no incluye este grupo, no se puede usar el dominio administrado de Azure AD DS.

```powershell
.\Select-GroupsToSync.ps1 -groupsToAdd @("AAD DC Administrators", "GroupName1", "GroupName3")
```

Al cambiar el ámbito de sincronización el dominio administrado de Azure AD DS vuelve a sincronizar todos los datos. Los objetos que ya no son necesarios en el dominio administrado de Azure AD DS se eliminan y la resincronización puede tardar mucho tiempo en completarse.

## <a name="disable-scoped-synchronization-using-powershell"></a>Deshabilitación de la sincronización con ámbito mediante PowerShell

Para deshabilitar la sincronización con ámbito basada en grupos para un dominio administrado de Azure AD DS, establezca *"filteredSync" = "Disabled"* en el recurso de Azure AD DS y, luego, actualice el dominio administrado. Cuando finalice, todos los usuarios y grupos están establecidos para sincronizarse desde Azure AD.

```powershell
// Retrieve the Azure AD DS resource.
$DomainServicesResource = Get-AzResource -ResourceType "Microsoft.AAD/DomainServices"

// Disable group-based scoped synchronization.
$disableScopedSync = @{"filteredSync" = "Disabled"}

// Update the Azure AD DS resource
Set-AzResource -Id $DomainServicesResource.ResourceId -Properties $disableScopedSync
```

Al cambiar el ámbito de sincronización el dominio administrado de Azure AD DS vuelve a sincronizar todos los datos. Los objetos que ya no son necesarios en el dominio administrado de Azure AD DS se eliminan y la resincronización puede tardar mucho tiempo en completarse.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el proceso de sincronización , consulte [Funcionamiento de la sincronización en Azure AD Domain Services](synchronization.md).

<!-- EXTERNAL LINKS -->
[Get-AzSubscription]: /powershell/module/Az.Accounts/Get-AzSubscription
