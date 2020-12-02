---
title: Creación y administración de grupos de acciones en Azure Portal
description: Obtenga información acerca de cómo crear y administrar grupos de acciones en Azure Portal.
author: dkamstra
ms.topic: conceptual
ms.date: 07/28/2020
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: a2144827cd27c0af6610dabdf5ad205359f550cc
ms.sourcegitcommit: 5e5a0abe60803704cf8afd407784a1c9469e545f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2020
ms.locfileid: "96435491"
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a>Creación y administración de grupos de acciones en Azure Portal
Un grupo de acciones es una colección de las preferencias de notificación que el propietario de una suscripción de Azure define. Las alertas de Azure Monitor y Service Health usan grupos de acciones para notificar a los usuarios que se ha desencadenado una alerta. Varias alertas pueden usar el mismo grupo de acciones o distintos grupos de acciones en función de los requisitos del usuario. 

En este artículo se muestra cómo crear y administrar grupos de acciones en el portal de Azure.

Cada acción se compone de las siguientes propiedades:

* **Tipo**: La notificación o acción realizada. El envío de llamadas de voz, mensajes de texto o correo electrónico o el desencadenamiento de varios tipos de acciones automatizadas son algunos ejemplos. Consulte los tipos más adelante en este artículo.
* **Name**: un identificador único dentro del grupo de acciones.
* **Detalles**: detalles correspondientes que varían según el *tipo*.

Para más información sobre el uso de plantillas de Azure Resource Manager para configurar grupos de acciones, consulte [Plantillas de Resource Manager para grupos de acciones](./action-groups-create-resource-manager-template.md).

## <a name="create-an-action-group-by-using-the-azure-portal"></a>Creación de un grupo de acciones con Azure Portal

1. En [Azure Portal](https://portal.azure.com), busque y seleccione **Monitor**. El panel **Monitor** consolida todas las opciones de configuración y todos los datos de supervisión en una vista.

1. Seleccione **Alertas** y **Administrar acciones**.

    ![Botón Administrar acciones](./media/action-groups/manage-action-groups.png)
    
1. Seleccione **Agregar grupo de acciones** y rellene los campos correspondientes en la experiencia del asistente.

    ![Comando "Agregar grupo de acciones"](./media/action-groups/add-action-group.PNG)

### <a name="configure-basic-action-group-settings"></a>Configuración básica del grupo de acciones

En **Detalles del proyecto**:

Seleccione la **Suscripción** y el **Grupo de recursos** donde está guardado el grupo de acciones.

En **Detalles de instancia**:

1. Escriba el **Nombre del grupo de acciones**.

1. Escriba un **Nombre para mostrar**. El nombre para mostrar se utiliza en lugar del nombre completo del grupo de acciones cuando se envían notificaciones mediante este grupo.

      ![Cuadro de diálogo "Agregar grupo de acciones"](./media/action-groups/action-group-1-basics.png)


### <a name="configure-notifications"></a>Configuración de notificaciones

1. Haga clic en el botón **Siguiente: Notificaciones >** para ir a la pestaña **Notificaciones**, o bien seleccione la pestaña **Notificaciones** en la parte superior de la pantalla.

1. Defina una lista de notificaciones que se enviarán cuando se desencadene una alerta. Proporcione lo siguiente para cada notificación:

    a. **Tipo de notificación**: seleccione el tipo de notificación que desea enviar. Las opciones disponibles son:
      * Rol de Azure Resource Manager de correo electrónico: envíe un correo electrónico a los usuarios asignados a determinados roles de ARM en el nivel de suscripción.
      * Correo electrónico/SMS/Inserción/Voz: envíe estos tipos de notificación a destinatarios específicos.
    
    b. **Name**: especifique un nombre único para la notificación.

    c. **Detalles**: en función del tipo de notificación seleccionado, escriba una dirección de correo electrónico, un número de teléfono, etc.
    
    d. **Esquema de alerta común**: puede optar por habilitar el [esquema de alerta común](./alerts-common-schema.md), que proporciona la ventaja de tener una sola carga de alertas, extensible y unificada, para todos los servicios de alerta de Azure Monitor.

    ![Pestaña Notificaciones](./media/action-groups/action-group-2-notifications.png)
    
### <a name="configure-actions"></a>Configuración de acciones

1. Haga clic en el botón **Siguiente: Acciones >** para ir a la pestaña **Acciones**, o bien seleccione la pestaña **Acciones** en la parte superior de la pantalla.

1. Defina una lista de acciones que se ejecutarán cuando se desencadene una alerta. Proporcione lo siguiente para cada acción:

    a. **Tipo de acción**: seleccione Runbook de Automation, Función de Azure, ITSM, Aplicación lógica, Webhook seguro y webhook.
    
    b. **Name**: escriba un nombre único para la acción.

    c. **Detalles**: según el tipo de acción, proporcione un identificador URI de webhook, una aplicación de Azure, una conexión de ITSM o un runbook de Automation. Para la acción de ITSM, especifique además **Elemento de trabajo** y otros campos que requiera la herramienta ITSM.
    
    d. **Esquema de alerta común**: puede optar por habilitar el [esquema de alerta común](./alerts-common-schema.md), que proporciona la ventaja de tener una sola carga de alertas, extensible y unificada, para todos los servicios de alerta de Azure Monitor.
    
    ![Pestaña Acciones](./media/action-groups/action-group-3-actions.png)

### <a name="create-the-action-group"></a>Creación del grupo de acciones

1. Puede explorar la configuración de **Etiquetas** si lo desea. Esto le permite asociar pares clave-valor al grupo de acciones para su categorización, y es una característica disponible para cualquier recurso de Azure.

    ![Pestaña Etiquetas](./media/action-groups/action-group-4-tags.png)
    
1. Haga clic en **Revisar y crear** para revisar la configuración. Se realizará una validación rápida de las entradas para asegurarse de que se seleccionan todos los campos obligatorios. Si hay algún problema, se le indicará aquí. Una vez revisada la configuración, haga clic en **Crear** para aprovisionar el grupo de acciones.
    
    ![Pestaña Revisar y crear](./media/action-groups/action-group-5-review.png)

> [!NOTE]
> Al configurar una acción para notificar a una persona por correo electrónico o SMS, la persona recibe una confirmación que le indica que se le ha agregado al grupo de acciones.

## <a name="manage-your-action-groups"></a>Administración de los grupos de acciones

Después de crear un grupo de acciones, puede ver los **grupos de acciones** seleccionando **Administrar acciones** en la página de aterrizaje **Alertas** del panel **Supervisión**. Seleccione el grupo de acciones que desea administrar para:

* Agregar, editar o quitar acciones.
* Eliminar el grupo de acciones.

## <a name="action-specific-information"></a>Información específica de la acción

> [!NOTE]
> Consulte los [límites de servicio de suscripción para la supervisión](../../azure-resource-manager/management/azure-subscription-service-limits.md#azure-monitor-limits) para los límites numéricos de cada uno de los siguientes elementos.  

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

En un grupo de acciones puede tener un número limitado de acciones de correo electrónico. Consulte el artículo de [información sobre las limitaciones](./alerts-rate-limiting.md).

### <a name="email-azure-resource-manager-role"></a>Rol de Azure Resource Manager de correo electrónico
Envíe un correo electrónico a los miembros del rol de la suscripción. El correo electrónico solo se enviará a los miembros **usuarios de Azure AD** del rol. No se enviará correo electrónico a grupos de Azure AD ni entidades de servicio.

Solo se envía un correo electrónico de notificación a la dirección de *correo electrónico principal*.

En un grupo de acciones puede tener un número limitado de acciones de correo electrónico. Consulte el artículo de [información sobre las limitaciones](./alerts-rate-limiting.md).

### <a name="function"></a>Función
Llama a un punto de conexión del desencadenador HTTP existente en [Azure Functions](../../azure-functions/functions-create-first-azure-function.md#create-a-function-app).

En un grupo de acciones puede tener un número limitado de acciones de función.

### <a name="itsm"></a>ITSM
La acción de ITSM requiere una conexión de ITSM. Aprenda cómo crear una [conexión de ITSM](./itsmc-overview.md).

En un grupo de acciones puede tener un número limitado de acciones de ITSM. 

### <a name="logic-app"></a>Aplicación lógica
En un grupo de acciones puede tener un número limitado de acciones de aplicación lógica.

### <a name="secure-webhook"></a>Webhook seguro

> [!NOTE]
> El uso de la acción de webhook requiere que el punto de conexión del webhook de destino no necesite detalles de la alerta para funcionar correctamente, o que sea capaz de analizar la información de contexto de la alerta que se proporciona como parte de la operación POST. Si el punto de conexión de webhook no puede controlar la información de contexto de la alerta por sí mismo, puede usar una solución como una [acción de aplicación lógica](./action-groups-logic-app.md) para manipular de manera personalizada la información de contexto de la alerta con el fin de que coincida con el formato de datos esperado del webhook.

La acción de webhook de Grupos de acciones le permite aprovechar Azure Active Directory para proteger la conexión entre el grupo de acciones y la API web protegida (punto de conexión de webhook). A continuación se describe el flujo de trabajo general para aprovechar esta funcionalidad. Para una introducción a las entidades de servicio y aplicaciones de Azure AD, consulte [Introducción a la Plataforma de identidad de Microsoft (versión 2.0)](../../active-directory/develop/v2-overview.md).

1. Cree una aplicación Azure AD para la API web. Consulte [API web protegida: registro de aplicación](../../active-directory/develop/scenario-protected-web-api-app-registration.md).
    - Configure la API protegida para que [la llame una aplicación de demonio](../../active-directory/develop/scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app).
    
2. Habilite Grupos de acciones para usar la aplicación de Azure AD.

    > [!NOTE]
    > Debe ser miembro del [rol Administrador de aplicaciones de Azure AD](../../active-directory/roles/permissions-reference.md#available-roles) para ejecutar este script.
    
    - Modifique la llamada Connect-AzureAD del script de PowerShell para usar el identificador de inquilino de Azure AD.
    - Modifique la variable del script de PowerShell $myAzureADApplicationObjectId para usar el identificador de objeto de la aplicación de Azure AD.
    - Ejecute el script modificado.
    
3. Configure la acción de webhook seguro del grupo de acciones.
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
Consulte también la [información sobre las limitaciones](./alerts-rate-limiting.md) y el [comportamiento de las alertas por SMS](./alerts-sms-behavior.md) para información adicional. 

En un grupo de acciones puede tener un número limitado de acciones de SMS.

> [!NOTE]
> Si la interfaz de usuario del grupo de acciones de Azure Portal no permite seleccionar el código de país o región, no se admite SMS para el país o región.  Si el código de país o región no está disponible, puede votar en [UserVoice](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice) para que el país o región se agregue. Mientras tanto, como solución alternativa, haga que el grupo de acciones llame a un webhook de un proveedor de SMS de terceros con soporte en el país o región.  

Los precios de los países o regiones admitidos se muestran en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).
  

### <a name="voice"></a>Voz
Consulte el artículo [Información sobre las limitaciones](./alerts-rate-limiting.md) para comportamientos importantes adicionales.

En un grupo de acciones puede tener un número limitado de acciones de voz.

> [!NOTE]
> Si la interfaz de usuario del grupo de acciones de Azure Portal no permite seleccionar el código de país o región, no se admiten llamadas de voz en el país o región. Si el código de país o región no está disponible, puede votar en [UserVoice](https://feedback.azure.com/forums/913690-azure-monitor/suggestions/36663181-add-more-country-codes-for-sms-alerting-and-voice) para que el país o región se agregue.  Mientras tanto, como solución alternativa, haga que el grupo de acciones llame a un webhook de un proveedor de llamadas de voz de terceros con soporte en el país o región.  

Los precios de los países o regiones admitidos se muestran en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

### <a name="webhook"></a>webhook

> [!NOTE]
> El uso de la acción de webhook requiere que el punto de conexión del webhook de destino no necesite detalles de la alerta para funcionar correctamente, o que sea capaz de analizar la información de contexto de la alerta que se proporciona como parte de la operación POST. Si el punto de conexión de webhook no puede controlar la información de contexto de la alerta por sí mismo, puede usar una solución como una [acción de aplicación lógica](./action-groups-logic-app.md) para manipular de manera personalizada la información de contexto de la alerta con el fin de que coincida con el formato de datos esperado del webhook.

Los webhooks se procesan utilizando las siguientes reglas:
- Una llamada de webhook se intenta un máximo de 3 veces.
- Se volverá a intentar la llamada si no se recibe una respuesta dentro del período de tiempo de espera o se devuelve uno de los siguientes códigos de estado HTTP: 408, 429, 503 o 504.
- La primera llamada esperará diez segundos para obtener una respuesta.
- El segundo y el tercer intento esperarán treinta segundos para obtener una respuesta.
- Después de que los tres intentos de llamada al webhook no se hayan realizado, ningún grupo de acciones llamará al punto de conexión durante quince minutos.

Rangos de direcciones IP de origen:

 - 13.66.60.119/32
 - 13.66.143.220/30
 - 13.66.202.14/32
 - 13.66.248.225/32
 - 13.66.249.211/32
 - 13.67.10.124/30
 - 13.69.109.132/30
 - 13.71.199.112/30
 - 13.77.53.216/30
 - 13.77.172.102/32
 - 13.77.183.209/32
 - 13.78.109.156/30
 - 13.84.49.247/32
 - 13.84.51.172/32
 - 13.84.52.58/32
 - 13.86.221.220/30
 - 13.106.38.142/32
 - 13.106.38.148/32
 - 13.106.54.3/32
 - 13.106.54.19/32
 - 13.106.57.181/32
 - 13.106.57.196/31
 - 20.38.149.132/30
 - 20.42.64.36/30
 - 20.43.121.124/30
 - 20.44.17.220/30
 - 20.45.123.236/30
 - 20.72.27.152/30
 - 20.150.172.228/30
 - 20.192.238.124/30
 - 20.193.202.4/30
 - 40.68.195.137/32
 - 40.68.201.58/32
 - 40.68.201.65/32
 - 40.68.201.206/32
 - 40.68.201.211/32
 - 40.68.204.18/32
 - 40.115.37.106/32
 - 40.121.219.215/32
 - 40.121.221.62/32
 - 40.121.222.201/32
 - 40.121.223.186/32
 - 51.104.9.100/30
 - 52.183.20.244/32
 - 52.183.31.0/32
 - 52.183.94.59/32
 - 52.184.145.166/32
 - 191.233.50.4/30
 - 191.233.207.64/26
 - 2603:1000:4:402::178/125
 - 2603:1000:104:402::178/125
 - 2603:1010:6:402::178/125
 - 2603:1010:101:402::178/125
 - 2603:1010:304:402::178/125
 - 2603:1010:404:402::178/125
 - 2603:1020:5:402::178/125
 - 2603:1020:206:402::178/125
 - 2603:1020:305:402::178/125
 - 2603:1020:405:402::178/125
 - 2603:1020:605:402::178/125
 - 2603:1020:705:402::178/125
 - 2603:1020:805:402::178/125
 - 2603:1020:905:402::178/125
 - 2603:1020:a04:402::178/125
 - 2603:1020:b04:402::178/125
 - 2603:1020:c04:402::178/125
 - 2603:1020:d04:402::178/125
 - 2603:1020:e04:402::178/125
 - 2603:1020:f04:402::178/125
 - 2603:1020:1004:800::f8/125
 - 2603:1020:1104:400::178/125
 - 2603:1030:f:400::978/125
 - 2603:1030:10:402::178/125
 - 2603:1030:104:402::178/125
 - 2603:1030:107:400::f0/125
 - 2603:1030:210:402::178/125
 - 2603:1030:40b:400::978/125
 - 2603:1030:40c:402::178/125
 - 2603:1030:504:802::f8/125
 - 2603:1030:608:402::178/125
 - 2603:1030:807:402::178/125
 - 2603:1030:a07:402::8f8/125
 - 2603:1030:b04:402::178/125
 - 2603:1030:c06:400::978/125
 - 2603:1030:f05:402::178/125
 - 2603:1030:1005:402::178/125
 - 2603:1040:5:402::178/125
 - 2603:1040:207:402::178/125
 - 2603:1040:407:402::178/125
 - 2603:1040:606:402::178/125
 - 2603:1040:806:402::178/125
 - 2603:1040:904:402::178/125
 - 2603:1040:a06:402::178/125
 - 2603:1040:b04:402::178/125
 - 2603:1040:c06:402::178/125
 - 2603:1040:d04:800::f8/125
 - 2603:1040:f05:402::178/125
 - 2603:1040:1104:400::178/125
 - 2603:1050:6:402::178/125
 - 2603:1050:403:400::1f8/125

Para recibir actualizaciones sobre los cambios a estas direcciones IP, se recomienda configurar una alerta de Service Health, que supervisa las notificaciones informativas sobre el servicio de grupos de acciones.

En un grupo de acciones puede tener un número limitado de acciones de webhook.

Las actualizaciones frecuentes de las direcciones IP de origen pueden llevar bastante tiempo en un webhook. El uso de **Etiqueta de servicio** para *ActionGroup* ayuda a minimizar la complejidad de las actualizaciones frecuentes de las direcciones IP manualmente. Microsoft administra automáticamente los prefijos de intervalo de direcciones IP de origen compartidos anteriormente englobados por **Etiqueta de servicio**.

#### <a name="service-tag"></a>Etiqueta de servicio
Una etiqueta de servicio representa un grupo de prefijos de direcciones IP de un servicio de Azure determinado. Microsoft administra los prefijos de direcciones que la etiqueta de servicio incluye y actualiza automáticamente dicha etiqueta a medida que las direcciones cambian, lo que minimiza la complejidad de las actualizaciones frecuentes en las reglas de seguridad de red para un ActionGroup.

1. En Azure Portal, en servicios de Azure, busque *Grupo de seguridad de red*.
2. Haga clic en **Agregar** y cree un grupo de seguridad de red.

   1. Agregue el nombre del grupo de recursos y escriba los *detalles de la instancia*.
   1. Haga clic en **Revisar y crear** y, a continuación, en *Crear*.
   
   :::image type="content" source="media/action-groups/action-group-create-security-group.png" alt-text="Ejemplo de creación de un grupo de seguridad de red"border="true":::.

3. Vaya a Grupo de recursos y haga clic en el *grupo de seguridad de red* que ha creado.

    1. Seleccione *Reglas de seguridad de entrada*.
    1. Haga clic en **Agregar**.
    
    :::image type="content" source="media/action-groups/action-group-add-service-tag.png" alt-text="Ejemplo de incorporación de una etiqueta de servicio."border="true":::

4. Se abrirá una nueva ventana en el panel derecho.
    1.  Seleccione Origen: **Etiqueta de servicio**
    1.  Etiqueta de servicio de origen: **ActionGroup**
    1.  Haga clic en **Agregar**.
    
    :::image type="content" source="media/action-groups/action-group-service-tag.png" alt-text="Ejemplo de incorporación de una etiqueta de servicio."border="true":::

## <a name="next-steps"></a>Pasos siguientes
* Más información sobre el [comportamiento de las alertas por SMS](./alerts-sms-behavior.md).  
* [Comprender el esquema de webhook de alertas del registro de actividad](./activity-log-alerts-webhook.md).  
* Obtenga más información sobre el [conector de ITSM](./itsmc-overview.md).
* Más información sobre la [limitación de velocidad](./alerts-rate-limiting.md) en las alertas.
* Consulte la [introducción a las alertas del registro de actividad](./alerts-overview.md) y aprenda cómo puede recibir alertas.  
* Aprenda a [configurar alertas siempre que se publique una notificación de mantenimiento de un servicio](../../service-health/alerts-activity-log-service-notifications-portal.md).
