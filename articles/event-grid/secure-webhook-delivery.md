---
title: Protección de la entrega de webhooks con Azure AD en Azure Event Grid
description: Describe cómo enviar eventos a puntos de conexión HTTPS protegidos por Azure Active Directory mediante Azure Event Grid
ms.topic: how-to
ms.date: 10/05/2020
ms.openlocfilehash: dd898fadf718509504d44df36572ac75050b02d6
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92371671"
---
# <a name="publish-events-to-azure-active-directory-protected-endpoints"></a>Publicación de eventos en puntos de conexión protegidos por Azure Active Directory

En este artículo se describe cómo aprovechar las ventajas de Azure Active Directory para proteger la conexión entre su suscripción de eventos y el punto de conexión de webhook. Para una introducción a las entidades de servicio y aplicaciones de Azure AD, consulte [Introducción a la Plataforma de identidad de Microsoft (versión 2.0)](../active-directory/develop/v2-overview.md).

En este artículo se usa Azure Portal para la demostración; sin embargo, la característica también se puede habilitar mediante la CLI, PowerShell o los SDK.


## <a name="create-an-azure-ad-application"></a>Creación de una aplicación de Azure AD

Empiece por crear una aplicación de Azure AD para el punto de conexión protegido. Vea https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Configure la API protegida para que la llame una aplicación de demonio.
    
## <a name="enable-event-grid-to-use-your-azure-ad-application"></a>Habilitación de Event Grid para que use la aplicación de Azure AD
En esta sección se muestra cómo habilitar Event Grid para usar la aplicación Azure AD. 

> [!NOTE]
> Debe ser miembro del [rol Administrador de aplicaciones de Azure AD](../active-directory/roles/permissions-reference.md#available-roles) para ejecutar este script.

### <a name="connect-to-your-azure-tenant"></a>Conéctese a su inquilino de Azure
En primer lugar, conéctese a su inquilino de Azure mediante el comando `Connect-AzureAD`. 

```PowerShell
# This is your Tenant Id. 
$myTenantId = "<the Tenant Id of your Azure AD Application>"
Connect-AzureAD -TenantId $myTenantId
```

### <a name="create-microsofteventgrid-service-principal"></a>Cree la entidad de servicio Microsoft.EventGrid
Ejecute el siguiente script para crear la entidad de servicio para **Microsoft.EventGrid** si aún no existe. 

```PowerShell
# This is the "Azure Event Grid" Azure Active Directory AppId
$eventGridAppId = "4962773b-9cdb-44cf-a8bf-237846a00ab7"
    
$eventGridSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $eventGridAppId + "'")

# Create the service principal if it doesn't exist
if ($eventGridSP -match "Microsoft.EventGrid")
{
    Write-Host "The Service principal is already defined.`n"
} else
{
    # Create a service principal for the "Azure Event Grid" Azure AD Application and add it to the role
    $eventGridSP = New-AzureADServicePrincipal -AppId $eventGridAppId
}
```

### <a name="create-a-role-for-your-application"></a>Cree un rol para la aplicación   
Ejecute el siguiente script para crear un rol para la aplicación Azure AD. En este ejemplo, el nombre del rol es: **AzureEventGridSecureWebhook** . Modifique el valor `$myTenantId` del script de PowerShell para usar el id. de inquilino de Azure AD y `$myAzureADApplicationObjectId` con el id. de objeto de la aplicación Azure AD.

```PowerShell
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the name of the new role we will add to your Azure AD Application
$eventGridRoleName = "AzureEventGridSecureWebhook"
    
# Create an application role of given name and description
Function CreateAppRole([string] $Name, [string] $Description)
{
    $appRole = New-Object Microsoft.Open.AzureAD.Model.AppRole
    $appRole.AllowedMemberTypes = New-Object System.Collections.Generic.List[string]
    $appRole.AllowedMemberTypes.Add("Application");
    $appRole.DisplayName = $Name
    $appRole.Id = New-Guid
    $appRole.IsEnabled = $true
    $appRole.Description = $Description
    $appRole.Value = $Name;
    return $appRole
}

# Get my Azure AD Application, it's roles and service principal
$myApp = Get-AzureADApplication -ObjectId $myAzureADApplicationObjectId
$myAppRoles = $myApp.AppRoles

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match $eventGridRoleName)
{
    Write-Host "The Azure Event Grid role is already defined.`n"
} else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $eventGridRoleName -Description "Azure Event Grid Role"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}

# print application's roles
Write-Host "My Azure AD Application's Roles: "
Write-Host $myAppRoles
```

### <a name="add-event-grid-service-principal-to-the-role"></a>Agregue la entidad de servicio de Event Grid al rol    
A continuación, ejecute el comando `New-AzureADServiceAppRoleAssignment` para asignar la entidad de servicio de Event Grid al rol que creó en el paso anterior. 

```powershell
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $eventGridSP.ObjectId -PrincipalId $eventGridSP.ObjectId
```

Ejecute los siguientes comandos para obtener información sobre la salida que usará en los pasos siguientes. 

```powershell    
Write-Host "My Azure AD Tenant Id: $myTenantId"
Write-Host "My Azure AD Application Id: $($myApp.AppId)"
Write-Host "My Azure AD Application ObjectId: $($myApp.ObjectId)"
```
    
## <a name="configure-the-event-subscription"></a>Configuración de la suscripción de eventos

En el flujo de creación de la suscripción de eventos, seleccione el tipo de punto de conexión "Webhook". Una vez proporcionado el identificador URI del punto de conexión, haga clic en la pestaña Características adicionales en la parte superior de la hoja Crear suscripciones de eventos.

![Selección del tipo de punto de conexión webhook](./media/secure-webhook-delivery/select-webhook.png)

En la pestaña Características adicionales, active la casilla "Usar autenticación de AAD" y configure el identificador de inquilino y el identificador de aplicación:

* Copie el identificador de inquilino de Azure AD de la salida del script y escríbalo en el campo Identificador de inquilino de AAD.
* Copie el identificador de aplicación de Azure AD de la salida del script y escríbalo en el campo Identificador de aplicación de AAD.

    ![Acción de webhook seguro](./media/secure-webhook-delivery/aad-configuration.png)

## <a name="next-steps"></a>Pasos siguientes

* Para información sobre la supervisión de las entregas de eventos, consulte [Supervisar la entrega de mensajes de Event Grid](monitor-event-delivery.md).
* Para más información sobre la clave de autenticación, vea [Seguridad y autenticación de Event Grid](security-authentication.md).
* Para más información acerca de la creación de una suscripción de Azure Event Grid, consulte [Esquema de suscripción de Event Grid](subscription-creation-schema.md).
