---
title: Creación y administración de grupos de acciones en Azure Portal
description: Obtenga información acerca de cómo crear y administrar grupos de acciones en Azure Portal.
author: dkamstra
ms.topic: conceptual
ms.date: 4/17/2020
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: b88b6d1ea4b9d9fc2b33849157968ee1c2f8c620
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170467"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Creación y administración de grupos de acciones en Azure Portal
Un grupo de acciones es una colección de las preferencias de notificación que el propietario de una suscripción de Azure define. Las alertas de Azure Monitor y Service Health usan grupos de acciones para notificar a los usuarios que se ha desencadenado una alerta. Varias alertas pueden usar el mismo grupo de acciones o distintos grupos de acciones en función de los requisitos del usuario. Puede configurar un máximo de 2000 grupos de acciones en una suscripción.

Al configurar una acción para notificar a una persona por correo electrónico o SMS, la persona recibe una confirmación que le indica que se le ha agregado al grupo de acciones.

En este artículo se muestra cómo crear y administrar grupos de acciones en el portal de Azure.

Cada acción se compone de las siguientes propiedades:

* **Name**: un identificador único dentro del grupo de acciones.  
* **Tipo de acción**: acción realizada. El envío de llamadas de voz, mensajes de texto o correo electrónico o el desencadenamiento de varios tipos de acciones automatizadas son algunos ejemplos. Consulte los tipos más adelante en este artículo.
* **Detalles**: detalles correspondientes que varían según el *tipo de acción*.

Para más información sobre el uso de plantillas de Azure Resource Manager para configurar grupos de acciones, consulte [Plantillas de Resource Manager para grupos de acciones](../../azure-monitor/platform/action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Creación de un grupo de acciones con Azure Portal

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **Monitor**. El panel **Monitor** consolida todas las opciones de configuración y todos los datos de supervisión en una vista.

1. Seleccione **Alertas** y **Administrar acciones**.

    ![Botón Administrar acciones](./media/action-groups/manage-action-groups.png)
    
1. Seleccione **Agregar grupo de acciones** y rellene los campos.

    ![Comando "Agregar grupo de acciones"](./media/action-groups/add-action-group.png)
    
1. Escriba un nombre en el cuadro de texto **Nombre del grupo de acciones** y especifique un nombre en el cuadro de texto **Nombre corto**. El nombre corto se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.

      ![Cuadro de diálogo "Agregar grupo de acciones"](./media/action-groups/action-group-define.png)

1. El cuadro de texto **Suscripción** se rellena automáticamente con la suscripción actual. La suscripción es aquella en la que se guarda el grupo de acciones.

1. Seleccione el **Grupo de recursos** en el que se guarda el grupo de acciones.

1. Defina una lista de acciones. Proporcione lo siguiente para cada acción:

    1. **Name**: escriba un identificador único para esta acción.

    1. **Tipo de acción**: seleccione runbook de Automation, función de Azure, rol de Azure Resource Manager de correo, acción de correo/SMS/push/voz, ITSM, aplicación lógica, webhook seguro, o webhook.

    1. **Detalles**: según el tipo de acción, proporcione un número de teléfono, una dirección de correo electrónico o un identificador URI de webhook, una aplicación de Azure, una conexión de ITSM o un runbook de Automation. Para la acción de ITSM, especifique además **Elemento de trabajo** y otros campos que requiera la herramienta ITSM.
    
    1. **Esquema de alerta común**: puede optar por habilitar el [esquema de alerta común](https://aka.ms/commonAlertSchemaDocs), que proporciona la ventaja de tener una sola carga de alertas, extensible y unificada, para todos los servicios de alerta de Azure Monitor.

1. Seleccione **Aceptar** para crear el grupo de acciones.

## <a name="manage-your-action-groups"></a>Administración de los grupos de acciones

Después de crear un grupo de acciones, puede ver los **grupos de acciones** seleccionando **Administrar acciones** en la página de aterrizaje **Alertas** del panel **Supervisión**. Seleccione el grupo de acciones que desea administrar para:

* Agregar, editar o quitar acciones.
* Eliminar el grupo de acciones.

## <a name="action-specific-information"></a>Información específica de la acción

> [!NOTE]
> Consulte los [límites de servicio de suscripción para la supervisión](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-monitor-limits) para los límites numéricos de cada uno de los siguientes elementos.  

### <a name="automation-runbook"></a>Runbook de automatización
Consulte los [límites de servicio de suscripción de Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md) para los límites relacionados con las cargas de runbook.

En un grupo de acciones puede tener un número limitado de acciones de runbook. 

### <a name="azure-app-push-notifications"></a>Notificaciones push de aplicaciones de Azure
En un grupo de acciones puede tener un número limitado de acciones de aplicación de Azure.

### <a name="email"></a>Email
Se enviarán mensajes de correo electrónico desde las direcciones de correo electrónico siguientes. Asegúrese de que el filtrado de correo electrónico esté configurado correctamente.
- azure-noreply@microsoft.com
- azureemail-noreply@microsoft.com
- alerts-noreply@mail.windowsazure.com

En un grupo de acciones puede tener un número limitado de acciones de correo electrónico. Consulte el artículo de [información sobre las limitaciones](./../../azure-monitor/platform/alerts-rate-limiting.md).

### <a name="email-azure-resource-manager-role"></a>Rol de Azure Resource Manager de correo electrónico
Envíe un correo electrónico a los miembros del rol de la suscripción. El correo electrónico solo se enviará a los miembros **usuarios de Azure AD** del rol. No se enviará correo electrónico a grupos de Azure AD ni entidades de servicio.

En un grupo de acciones puede tener un número limitado de acciones de correo electrónico. Consulte el artículo de [información sobre las limitaciones](./../../azure-monitor/platform/alerts-rate-limiting.md).

### <a name="function"></a>Función
Llama a un punto de conexión del desencadenador HTTP existente en [Azure Functions](../../azure-functions/functions-create-first-azure-function.md#create-a-function-app).

En un grupo de acciones puede tener un número limitado de acciones de función.

### <a name="itsm"></a>ITSM
La acción de ITSM requiere una conexión de ITSM. Aprenda cómo crear una [conexión de ITSM](../../azure-monitor/platform/itsmc-overview.md).

En un grupo de acciones puede tener un número limitado de acciones de ITSM. 

### <a name="logic-app"></a>Aplicación lógica
En un grupo de acciones puede tener un número limitado de acciones de aplicación lógica.

### <a name="secure-webhook"></a>Webhook seguro
La acción de webhook de Grupos de acciones le permite aprovechar Azure Active Directory para proteger la conexión entre el grupo de acciones y la API web protegida (punto de conexión de webhook). A continuación se describe el flujo de trabajo general para aprovechar esta funcionalidad. Para una introducción a las entidades de servicio y aplicaciones de Azure AD, consulte [Introducción a la Plataforma de identidad de Microsoft (versión 2.0)](https://docs.microsoft.com/azure/active-directory/develop/v2-overview).

1. Cree una aplicación Azure AD para la API web. Vea https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-overview.
    - Configure la API protegida para que la llame una aplicación de demonio.
    
1. Habilite Grupos de acciones para usar la aplicación de Azure AD.

    > [!NOTE]
    > Debe ser miembro del [rol Administrador de aplicaciones de Azure AD](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#available-roles) para ejecutar este script.
    
    - Modifique la llamada Connect-AzureAD del script de PowerShell para usar el identificador de inquilino de Azure AD.
    - Modifique la variable del script de PowerShell $myAzureADApplicationObjectId para usar el identificador de objeto de la aplicación de Azure AD.
    - Ejecute el script modificado.
    
1. Configure la acción de webhook seguro del grupo de acciones.
    - Copie el valor $myApp.ObjectId del script e introdúzcalo en el campo Id. de objeto de aplicación en la definición de acción de webhook.
    
    ![Acción de webhook seguro](./media/action-groups/action-groups-secure-webhook.png)

#### <a name="secure-webhook-powershell-script"></a>Script de PowerShell de webhook seguro

```PowerShell
Connect-AzureAD -TenantId "<provide your Azure AD tenant ID here>"
    
# This is your Azure AD Application's ObjectId. 
$myAzureADApplicationObjectId = "<the Object Id of your Azure AD Application>"
    
# This is the Action Groups Azure AD AppId
$actionGroupsAppId = "461e8683-5575-4561-ac7f-899cc907d62a"
    
# This is the name of the new role we will add to your Azure AD Application
$actionGroupRoleName = "ActionGroupsSecureWebhook"
    
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
$actionGroupsSP = Get-AzureADServicePrincipal -Filter ("appId eq '" + $actionGroupsAppId + "'")

Write-Host "App Roles before addition of new role.."
Write-Host $myAppRoles
    
# Create the role if it doesn't exist
if ($myAppRoles -match "ActionGroupsSecureWebhook")
{
    Write-Host "The Action Groups role is already defined.`n"
}
else
{
    $myServicePrincipal = Get-AzureADServicePrincipal -Filter ("appId eq '" + $myApp.AppId + "'")
    
    # Add our new role to the Azure AD Application
    $newRole = CreateAppRole -Name $actionGroupRoleName -Description "This is a role for Action Groups to join"
    $myAppRoles.Add($newRole)
    Set-AzureADApplication -ObjectId $myApp.ObjectId -AppRoles $myAppRoles
}
    
# Create the service principal if it doesn't exist
if ($actionGroupsSP -match "AzNS AAD Webhook")
{
    Write-Host "The Service principal is already defined.`n"
}
else
{
    # Create a service principal for the Action Groups Azure AD Application and add it to the role
    $actionGroupsSP = New-AzureADServicePrincipal -AppId $actionGroupsAppId
}
    
New-AzureADServiceAppRoleAssignment -Id $myApp.AppRoles[0].Id -ResourceId $myServicePrincipal.ObjectId -ObjectId $actionGroupsSP.ObjectId -PrincipalId $actionGroupsSP.ObjectId
    
Write-Host "My Azure AD Application ($myApp.ObjectId): " + $myApp.ObjectId
Write-Host "My Azure AD Application's Roles"
Write-Host $myApp.AppRoles
```

### <a name="sms"></a>sms
Consulte también la [información sobre las limitaciones](./../../azure-monitor/platform/alerts-rate-limiting.md) y el [comportamiento de las alertas por SMS](../../azure-monitor/platform/alerts-sms-behavior.md) para información adicional. 

En un grupo de acciones puede tener un número limitado de acciones de SMS.

> [!NOTE]
> Si la interfaz de usuario del grupo de acciones de Azure Portal no permite seleccionar el código de país o región, no se admite SMS para el país o región.  Si el código de país o región no está disponible, puede votar en [UserVoice](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice) para que el país o región se agregue. Mientras tanto, como solución alternativa, haga que el grupo de acciones llame a un webhook de un proveedor de SMS de terceros con soporte en el país o región.  

Los precios de los países o regiones admitidos se muestran en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).
  

### <a name="voice"></a>Voz
Consulte el artículo [Información sobre las limitaciones](./../../azure-monitor/platform/alerts-rate-limiting.md) para comportamientos importantes adicionales.

En un grupo de acciones puede tener un número limitado de acciones de voz.

> [!NOTE]
> Si la interfaz de usuario del grupo de acciones de Azure Portal no permite seleccionar el código de país o región, no se admiten llamadas de voz en el país o región. Si el código de país o región no está disponible, puede votar en [UserVoice](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice) para que el país o región se agregue.  Mientras tanto, como solución alternativa, haga que el grupo de acciones llame a un webhook de un proveedor de llamadas de voz de terceros con soporte en el país o región.  

Los precios de los países o regiones admitidos se muestran en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

### <a name="webhook"></a>webhook
Los webhooks se reintentan utilizando las siguientes reglas. La llamada de webhook se vuelve a intentar un máximo de 2 veces cuando se devuelven los siguientes códigos de estado HTTP: 408, 429, 503, 504 o el punto de conexión HTTP no responda. El primer reintento se produce transcurridos 10 segundos. El segundo reintento se produce transcurridos 100 segundos. Después de dos errores, ningún grupo de acciones llamará al punto de conexión durante 30 minutos. 

Rangos de direcciones IP de origen
 - 13.72.19.232
 - 13.106.57.181
 - 13.106.54.3
 - 13.106.54.19
 - 13.106.38.142
 - 13.106.38.148
 - 13.106.57.196
 - 13.106.57.197
 - 52.244.68.117
 - 52.244.65.137
 - 52.183.31.0
 - 52.184.145.166
 - 51.4.138.199
 - 51.5.148.86
 - 51.5.149.19

Para recibir actualizaciones sobre los cambios a estas direcciones IP, se recomienda configurar una alerta de Service Health, que supervisa las notificaciones informativas sobre el servicio de grupos de acciones.

En un grupo de acciones puede tener un número limitado de acciones de webhook.



## <a name="next-steps"></a>Pasos siguientes
* Más información sobre el [comportamiento de las alertas por SMS](../../azure-monitor/platform/alerts-sms-behavior.md).  
* [Comprender el esquema de webhook de alertas del registro de actividad](../../azure-monitor/platform/activity-log-alerts-webhook.md).  
* Obtenga más información sobre el [conector de ITSM](../../azure-monitor/platform/itsmc-overview.md).
* Más información sobre la [limitación de velocidad](../../azure-monitor/platform/alerts-rate-limiting.md) en las alertas.
* Consulte la [introducción a las alertas del registro de actividad](../../azure-monitor/platform/alerts-overview.md) y aprenda cómo puede recibir alertas.  
* Aprenda a [configurar alertas siempre que se publique una notificación de mantenimiento de un servicio](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).
