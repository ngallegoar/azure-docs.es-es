---
title: Configuración del consentimiento de los usuarios finales a las aplicaciones con Azure AD
description: Aprenda a administrar cómo y cuándo los usuarios pueden dar consentimiento a las aplicaciones que tendrán acceso a los datos de su organización.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mimart
ms.reviewer: arvindh, luleon, phsignor
ms.openlocfilehash: 0a508e52189938447ea6fc1928d441d81deab392
ms.sourcegitcommit: 958f086136f10903c44c92463845b9f3a6a5275f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83714033"
---
# <a name="configure-how-end-users-consent-to-applications"></a>Configuración del consentimiento de los usuarios finales a las aplicaciones

Puede integrar las aplicaciones con la Plataforma de identidad de Microsoft para permitir que los usuarios inicien sesión con su cuenta profesional o educativa y accedan a los datos de su organización con el fin de ofrecer experiencias enriquecidas controladas por datos.

Para que una aplicación pueda acceder a los datos de su organización, primero un usuario debe conceder los permisos de aplicación. Existen diferentes permisos que permiten distintos niveles de acceso. De forma predeterminada, todos los usuarios pueden dar su consentimiento a las aplicaciones para los permisos que no requieren el consentimiento del administrador. Por ejemplo, de forma predeterminada, un usuario puede dar su consentimiento para permitir que una aplicación acceda a su buzón, pero no puede dar su consentimiento para que una aplicación pueda acceder sin restricciones para leer y escribir en todos los archivos de la organización.

Al permitir que los usuarios concedan permiso a las aplicaciones para acceder a los datos, los usuarios pueden adquirir fácilmente aplicaciones útiles y ser productivos. Sin embargo, en algunas situaciones, esta configuración puede representar un riesgo si no se supervisa y controla con cuidado.

## <a name="user-consent-settings"></a>Configuración del consentimiento del usuario

Para controlar en qué casos los usuarios pueden dar su consentimiento a las aplicaciones, elija la directiva de consentimiento que se aplicará a todos los usuarios. Estas son las tres opciones para la directiva de consentimiento:

* **Deshabilitar el consentimiento del usuario**: los usuarios no pueden conceder permisos a las aplicaciones. Los usuarios pueden seguir iniciando sesión en las aplicaciones a las que dieron su consentimiento previamente o que los administradores autorizaron en su nombre, pero estos no podrán dar su consentimiento por sí solos a nuevos permisos ni a nuevas aplicaciones. Solo los usuarios a los que se haya concedido un rol de directorio que incluye el permiso para conceder consentimiento podrán autorizar nuevos permisos o nuevas aplicaciones.

* **Los usuarios pueden dar su consentimiento a las aplicaciones de publicadores comprobados, pero solo para los permisos que seleccione (versión preliminar)** : los usuarios solo pueden dar su consentimiento a las aplicaciones publicadas por un [publicador comprobado](../develop/publisher-verification-overview.md) y a las aplicaciones que están registradas en el inquilino. Los usuarios solo pueden dar su consentimiento a los permisos clasificados como "bajo impacto".

  Asegúrese de [clasificar los permisos](#configure-permission-classifications-preview) para seleccionar aquellos a los que los usuarios pueden dar su consentimiento.

* **Los usuarios pueden dar su consentimiento a todas las aplicaciones**: esta opción permite a todos los usuarios dar su consentimiento a cualquier permiso para todas las aplicaciones, siempre que este no requiera el consentimiento del administrador. 

   Para reducir el riesgo de que las aplicaciones malintencionadas intenten engañar a los usuarios para que les concedan acceso a los datos de su organización, se recomienda permitir el consentimiento del usuario solo para las aplicaciones publicadas por un [publicador comprobado](../develop/publisher-verification-overview.md).

### <a name="configure-user-consent-settings-from-the-azure-portal"></a>Configuración del consentimiento del usuario en Azure Portal

Para configurar el consentimiento del usuario en Azure Portal:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como [Administrador global](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator).
1. Seleccione **Azure Active Directory** > **Aplicaciones empresariales** > **Consentimiento y permisos** > **Configuración del consentimiento de los usuarios**.
1. En **Consentimiento del usuario para las aplicaciones**, seleccione la configuración de consentimiento que desee establecer para todos los usuarios.
1. Haga clic en **Guardar** para guardar la configuración.

![Configuración del consentimiento del usuario](./media/configure-user-consent/setting-for-all-users.png)

> [!TIP]
> Plantéese la posibilidad de [habilitar el flujo de trabajo de consentimiento del administrador](configure-admin-consent-workflow.md) para permitir que los usuarios soliciten la revisión y aprobación por parte de un administrador de una aplicación a la que el usuario no está autorizado a dar su consentimiento, por ejemplo, cuando el consentimiento del usuario se ha deshabilitado o si la aplicación está solicitando permisos que el usuario no puede conceder.

### <a name="configure-user-consent-settings-using-powershell"></a>Configuración del consentimiento del usuario con PowerShell

Puede usar el módulo de versión preliminar de Azure AD PowerShell, [AzureADPreview](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?view=azureadps-2.0-preview), para elegir qué directiva de consentimiento rige el consentimiento del usuario para las aplicaciones.

* **Deshabilitar el consentimiento del usuario**: para deshabilitar el consentimiento del usuario, establezca las directivas de consentimiento que rigen el consentimiento del usuario para que estén vacías.

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @()
  ```

* **Permitir el consentimiento del usuario para las aplicaciones de los publicadores comprobados, para los permisos seleccionados (versión preliminar)** : para permitir el consentimiento limitado del usuario solo para las aplicaciones de los publicadores comprobados y las aplicaciones registradas en el inquilino, y solo para los permisos clasificados como "bajo impacto", configure la directiva de consentimiento integrada denominada `microsoft-user-default-low`:

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @("microsoft-user-default-low")
  ```

   No olvide [clasificar los permisos](#configure-permission-classifications-preview) para seleccionar aquellos a los que los usuarios pueden dar su consentimiento.

* **Permitir el consentimiento del usuario para todas las aplicaciones**: permitir el consentimiento del usuario para todas las aplicaciones:

  ```powershell
  Set-AzureADMSAuthorizationPolicy `
     -Id "authorizationPolicy" `
     -PermissionGrantPolicyIdsAssignedToDefaultUserRole @("microsoft-user-default-legacy")
  ```

   Esta opción permite a todos los usuarios dar su consentimiento a cualquier permiso para todas las aplicaciones, siempre que este no requiera el consentimiento del administrador. Se recomienda permitir el consentimiento del usuario solo para las aplicaciones de los publicadores comprobados.

## <a name="configure-permission-classifications-preview"></a>Configuración de las clasificaciones de permisos (versión preliminar)

Las clasificaciones de permisos permiten identificar el impacto que tienen los distintos permisos según las evaluaciones de riesgo y las directivas de la organización. Por ejemplo, puede usar las clasificaciones de permisos en las directivas de consentimiento para identificar el conjunto de permisos a los que los usuarios pueden dar su consentimiento.

> [!NOTE]
> Actualmente, solo se admite la clasificación de permisos de "bajo impacto". Solo los permisos delegados que no requieren el consentimiento del administrador se pueden clasificar como "bajo impacto".

### <a name="classify-permissions-using-the-azure-portal"></a>Clasificación de permisos en Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como [Administrador global](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator).
1. Seleccione **Azure Active Directory** > **Aplicaciones empresariales** > **Consentimiento y permisos** > **Clasificaciones de permisos**.
1. Elija **Agregar permisos** para clasificar otro permiso como "bajo impacto". 
1. Seleccione la API y, después, seleccione los permisos delegados.

En este ejemplo, hemos clasificado el conjunto mínimo de permisos necesarios para el inicio de sesión único:

![Clasificaciones de permisos](./media/configure-user-consent/permission-classifications.png)

> [!TIP]
> En el caso de Microsoft Graph API, los permisos mínimos necesarios para realizar el inicio de sesión único básico son `openid`, `profile`, `User.Read` y `offline_access`. Con estos permisos, una aplicación puede leer los detalles del perfil del usuario que ha iniciado sesión y puede mantener este acceso incluso cuando el usuario ya no usa la aplicación.

### <a name="classify-permissions-using-powershell"></a>Clasificación de permisos con PowerShell

Puede usar el módulo de versión preliminar de Azure AD PowerShell más reciente, [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview), para clasificar los permisos. Las clasificaciones de permisos se configuran en el objeto **ServicePrincipal** de la API que publica los permisos.

#### <a name="to-read-the-current-permission-classifications-for-an-api"></a>Para leer las clasificaciones de permisos actuales para una API:

1. Recupere el objeto **ServicePrincipal** para la API. Aquí se recupera el objeto ServicePrincipal para Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Lea las clasificaciones de permisos delegados para la API:

   ```powershell
   Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId | Format-Table Id, PermissionName, Classification
   ```

#### <a name="to-classify-a-permission-as-low-impact"></a>Para clasificar un permiso como "bajo impacto":

1. Recupere el objeto **ServicePrincipal** para la API. Aquí se recupera el objeto ServicePrincipal para Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Busque el permiso delegado que desea clasificar:

   ```powershell
   $delegatedPermission = $api.OAuth2Permissions | Where-Object { $_.Value -eq "User.ReadBasic.All" }
   ```

1. Establezca la clasificación de permisos mediante el nombre y el identificador del permiso:

   ```powershell
   Add-AzureADMSServicePrincipalDelegatedPermissionClassification `
      -ServicePrincipalId $api.ObjectId `
      -PermissionId $delegatedPermission.Id `
      -PermissionName $delegatedPermission.Value `
      -Classification "low"
   ```

#### <a name="to-remove-a-delegated-permission-classification"></a>Para quitar una clasificación de permisos delegados:

1. Recupere el objeto **ServicePrincipal** para la API. Aquí se recupera el objeto ServicePrincipal para Microsoft Graph API:

   ```powershell
   $api = Get-AzureADServicePrincipal `
       -Filter "servicePrincipalNames/any(n:n eq 'https://graph.microsoft.com')"
   ```

1. Busque la clasificación de permisos delegados que desea quitar:

   ```powershell
   $classifications = Get-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId
   $classificationToRemove = $classifications | Where-Object {$_.PermissionName -eq "User.ReadBasic.All"}
   ```

1. Elimine la clasificación de permisos:

   ```powershell
   Remove-AzureADMSServicePrincipalDelegatedPermissionClassification `
       -ServicePrincipalId $api.ObjectId `
       -Id $classificationToRemove.Id
   ```

## <a name="configure-group-owner-consent-to-apps-accessing-group-data"></a>Configuración del consentimiento del propietario del grupo para las aplicaciones que acceden a los datos del grupo

Los propietarios de grupo pueden autorizar a las aplicaciones, como las aplicaciones publicadas por otros proveedores, el acceso a los datos de la organización asociados al grupo. Por ejemplo, el propietario de un equipo de Microsoft Teams puede permitir que una aplicación lea todos los mensajes de Teams del equipo o bien mostrar el perfil básico de los miembros de un grupo.

Puede configurar qué usuarios pueden dar su consentimiento a las aplicaciones que tienen acceso a los datos de sus grupos, o puede deshabilitar esta característica.

### <a name="configure-group-owner-consent-using-the-azure-portal"></a>Configuración del consentimiento del propietario del grupo en Azure Portal

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como [Administrador global](../users-groups-roles/directory-assign-admin-roles.md#global-administrator--company-administrator).
2. Seleccione **Azure Active Directory** > **Aplicaciones empresariales** > **Consentimiento y permisos** > **Configuración del consentimiento de los usuarios**.
3. En **Consentimiento del propietario del grupo para las aplicaciones que acceden a los datos**, seleccione la opción que desea habilitar.
4. Haga clic en **Guardar** para guardar la configuración.

En este ejemplo, todos los propietarios de grupos pueden dar su consentimiento a las aplicaciones que acceden a los datos de sus grupos:

![Clasificaciones de permisos](./media/configure-user-consent/group-owner-consent.png)

### <a name="configure-group-owner-consent-using-powershell"></a>Configuración del consentimiento del propietario del grupo con PowerShell

Puede usar el módulo en versión preliminar de Azure AD PowerShell, [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview), para deshabilitar o habilitar la capacidad de los usuarios de dar su consentimiento a las aplicaciones que acceden a los datos de la organización.

1. Asegúrese de que usa el módulo [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview). Este paso es importante si ha instalado el módulo [AzureAD](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0) y el módulo [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview).

    ```powershell
    Remove-Module AzureAD
    Import-Module AzureADPreview
    ```

1. Conéctese a Azure AD PowerShell.

   ```powershell
   Connect-AzureAD
   ```

1. Recupere el valor actual para la configuración del directorio **Consent Policy Settings** del inquilino. Esto requiere comprobar si se ha creado la configuración del directorio para esta característica y, si no es así, usar los valores de la correspondiente plantilla de configuración de directorio.

    ```powershell
    $consentSettingsTemplateId = "dffd5d46-495d-40a9-8e21-954ff55e198a" # Consent Policy Settings
    $settings = Get-AzureADDirectorySetting -All $true | Where-Object { $_.TemplateId -eq $consentSettingsTemplateId }

    if (-not $settings) {
        $template = Get-AzureADDirectorySettingTemplate -Id $consentSettingsTemplateId
        $settings = $template.CreateDirectorySetting()
    }

    $enabledValue = $settings.Values | ? { $_.Name -eq "EnableGroupSpecificConsent" }
    $limitedToValue = $settings.Values | ? { $_.Name -eq "ConstrainGroupSpecificConsentToMembersOfGroupId" }
    ```

1. Comprenda los valores de configuración. Hay dos valores de configuración que definen qué usuarios podrán permitir que una aplicación acceda a los datos de su grupo:

    | Configuración       | Tipo         | Descripción  |
    | ------------- | ------------ | ------------ |
    | _EnableGroupSpecificConsent_   | Boolean | Marca que indica si los propietarios de los grupos pueden conceder permisos específicos del grupo. |
    | _ConstrainGroupSpecificConsentToMembersOfGroupId_ | Guid | Si _EnableGroupSpecificConsent_ se establece en "True" y este valor se establece en el identificador de objeto de un grupo, los miembros del grupo identificado estarán autorizados a conceder permisos específicos de grupo a los grupos que poseen. |

1. Actualice los valores de configuración para la configuración deseada:

    ```powershell
    # Disable group-specific consent entirely
    $enabledValue.Value = "False"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for all users
    $enabledValue.Value = "True"
    $limitedToValue.Value = ""
    ```

    ```powershell
    # Enable group-specific consent for users in a given group
    $enabledValue.Value = "True"
    $limitedToValue.Value = "{group-object-id}"
    ```

1. Guarde la configuración.

    ```powershell
    if ($settings.Id) {
        # Update an existing directory settings
        Set-AzureADDirectorySetting -Id $settings.Id -DirectorySetting $settings
    } else {
        # Create a new directory settings to override the default setting 
        New-AzureADDirectorySetting -DirectorySetting $settings
    }
    ```

## <a name="configure-risk-based-step-up-consent"></a>Configuración del consentimiento activo en función del riesgo

El consentimiento activo en función del riesgo ayuda a reducir la exposición de los usuarios a aplicaciones malintencionadas que realizan [solicitudes de consentimiento ilícitas](https://docs.microsoft.com/microsoft-365/security/office-365-security/detect-and-remediate-illicit-consent-grants). Si Microsoft detecta una solicitud de consentimiento de usuario final de riesgo, la solicitud requerirá un "paso activo" para el consentimiento de administrador. Esta funcionalidad está habilitada de forma predeterminada, pero solo producirá un cambio de comportamiento cuando esté habilitado el consentimiento del usuario final.

Cuando se detecta una solicitud de consentimiento peligrosa, la petición de consentimiento mostrará un mensaje que indica que se necesita la aprobación del administrador. Si el [flujo de trabajo de solicitud de consentimiento de administrador](configure-admin-consent-workflow.md) está habilitado, el usuario puede enviar la solicitud a un administrador para que la revise de nuevo desde la propia petición de consentimiento. Si no está habilitado, se mostrará el mensaje siguiente:

* **AADSTS90094:** &lt;clientAppDisplayName&gt; necesita permiso para acceder a recursos de su organización que solo un administrador puede conceder. Pida a un administrador que conceda permiso a esta aplicación antes de poder usarla.

En este caso, también se registrará un evento de auditoría con la categoría "ApplicationManagement", el tipo de actividad "Consentimiento a la aplicación" y el motivo de estado "Aplicación arriesgada detectada".

> [!IMPORTANT]
> Los administradores deben [evaluar todas las solicitudes de consentimiento](manage-consent-requests.md#evaluating-a-request-for-tenant-wide-admin-consent) detenidamente antes de aprobarlas, especialmente cuando Microsoft haya detectado algún riesgo.

### <a name="disable-or-re-enable-risk-based-step-up-consent-using-powershell"></a>Deshabilitación o nueva habilitación del consentimiento activo en función del riesgo con PowerShell

Puede usar el módulo en versión preliminar de PowerShell de Azure AD, [AzureADPreview](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview), para deshabilitar el paso activo del consentimiento de administrador requerido en los casos en los que Microsoft detecta el riesgo o para volver a habilitarlo si estaba deshabilitado anteriormente.

Para ello, siga los mismos pasos descritos anteriormente para [configurar el consentimiento del propietario del grupo mediante PowerShell](#configure-group-owner-consent-using-powershell), pero sustituyendo un valor de configuración diferente. Hay tres diferencias en los pasos: 

1. Comprenda los valores de configuración para el consentimiento de paso activo en función del riesgo:

    | Configuración       | Tipo         | Descripción  |
    | ------------- | ------------ | ------------ |
    | _BlockUserConsentForRiskyApps_   | Boolean |  Marca que indica si se bloqueará el consentimiento del usuario cuando se detecte una solicitud de riesgo. |

1. Sustituya el siguiente valor en el paso 3:

    ```powershell
    $riskBasedConsentEnabledValue = $settings.Values | ? { $_.Name -eq "BlockUserConsentForRiskyApps" }
    ```
    
1. Sustituya uno de los siguientes en el paso 5:

    ```powershell
    # Disable risk-based step-up consent entirely
    $riskBasedConsentEnabledValue.Value = "False"
    ```

    ```powershell
    # Re-enable risk-based step-up consent, if disabled previously
    $riskBasedConsentEnabledValue.Value = "True"
    ```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información:

* [Configuración del flujo de trabajo de consentimiento del administrador](configure-admin-consent-workflow.md)
* [Aprenda a administrar el consentimiento de aplicaciones y a evaluar las solicitudes de consentimiento](manage-consent-requests.md)
* [Concesión del consentimiento del administrador para todo el inquilino a una aplicación](grant-admin-consent.md)
* [Permisos y consentimiento en la plataforma de identidad de Microsoft](../develop/active-directory-v2-scopes.md)

Para obtener ayuda o encontrar respuestas a sus preguntas:
* [Azure AD en StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
