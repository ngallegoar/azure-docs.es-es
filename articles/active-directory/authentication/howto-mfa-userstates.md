---
title: 'Multi-Factor Authentication por usuario: Azure Active Directory'
description: Habilite MFA mediante el cambio de estados de usuarios en Azure Multi-Factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 61d7227c57422cfe2228002750ec29bffa385d44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76756780"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>Exigencia de verificación en dos pasos para un usuario

Puede utilizar uno de estos dos métodos para requerir la verificación en dos pasos, los cuales requieren el uso de una cuenta de administrador global. La primera opción consiste en habilitar a cada usuario para Azure Multi-factor Authentication (MFA). Cuando los usuarios se habilitan de forma individual, realizan la verificación en dos pasos cada vez que inician sesión (con algunas excepciones, como cuando inician sesión desde direcciones IP de confianza o si se activa la característica _recordar dispositivos_). La segunda opción consiste en configurar una directiva de acceso condicional que requiere la verificación en dos pasos en determinadas condiciones.

> [!TIP]
> La habilitación de Azure Multi-Factor Authentication mediante directivas de acceso condicional es el enfoque recomendado. El cambio de estado del usuario ya no se recomienda, a menos que las licencias no incluyan el acceso condicional, ya que esto requerirá que los usuarios realicen MFA cada vez que inicien sesión.

## <a name="choose-how-to-enable"></a>Elección del modo de habilitación

**Enabled by changing user state** (Habilitada mediante el cambio de estado de usuario): se trata del método tradicional para exigir la verificación en dos pasos y se describe en este artículo. Funciona tanto en Azure MFA en la nube como en el servidor de Azure MFA. El uso de este método requiere que los usuarios realicen la verificación en dos pasos **cada vez** que inicien sesión e invalida las directivas de acceso condicional.

**Enabled by Conditional Access policy** (Habilitada mediante la directiva de acceso condicional): se trata del medio más flexible para habilitar la verificación en dos pasos para los usuarios. La habilitación de la directiva de acceso condicional solo funciona con Azure MFA en la nube y es una característica premium de Azure AD. Puede encontrar más información sobre este método en [Implementación de Azure Multi-factor Authentication en la nube](howto-mfa-getstarted.md).

**Enabled by Azure AD Identity Protection** (Habilitada por Azure AD Identity Protection): este método utiliza la directiva de riesgos de Azure AD Identity Protection para exigir la verificación en dos pasos solo según el riesgo de inicio de sesión para todas las aplicaciones en la nube. Este método requiere una licencia de Azure Active Directory P2. Para más información sobre este método, vea [Azure Active Directory Identity Protection](../identity-protection/howto-sign-in-risk-policy.md).

> [!Note]
> Puede encontrar más información sobre licencias y precios en las páginas de precios de [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) y [Multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="enable-azure-mfa-by-changing-user-state"></a>Habilitar Azure MFA mediante el cambio de estado del usuario

Las cuentas de usuario de Azure Multi-Factor Authentication tienen los siguientes tres estados:

> [!IMPORTANT]
> Al habilitar Azure MFA mediante una directiva de acceso condicional, no se cambiará el estado del usuario. Los usuarios a los que no se deben realizar notificaciones de alarma aparecen deshabilitados. El acceso condicional no cambia el estado. **Las organizaciones no deben habilitar o imponer a los usuarios si usan directivas de acceso condicional.**

| Status | Descripción | Aplicaciones que no son de explorador afectadas | Aplicaciones que son de explorador afectadas | Autenticación moderna afectada |
|:---:| --- |:---:|:--:|:--:|
| Disabled | Estado predeterminado para un usuario nuevo no inscrito en Azure MFA. | No | No | No |
| habilitado | El usuario se ha inscrito en Azure MFA, pero no se ha registrado. La próxima vez que inicie sesión, se le pedirá registrarse. | No.  Continúa funcionando hasta que se complete el proceso de registro. | Sí. Una vez que expire la sesión, se requerirá el registro de Azure MFA.| Sí. Una vez que expire el token de acceso, se requerirá el registro de Azure MFA. |
| Aplicado | El usuario se ha inscrito y ha completado el proceso de registro de Azure MFA. | Sí. Las aplicaciones requieren contraseñas de aplicación. | Sí. Se requiere Azure MFA en el inicio de sesión. | Sí. Se requiere Azure MFA en el inicio de sesión. |

El estado de un usuario refleja si un administrador lo ha inscrito en Azure MFA, y si ha completado el proceso de registro.

Todos los usuarios comienzan con el estado *Deshabilitado*. Cuando inscribe usuarios en Azure MFA, cambia a *Habilitado*. Cuando los usuarios habilitados inician sesión y completan el proceso de registro, el estado cambia a *Aplicado*.

> [!NOTE]
> Si se vuelve a habilitar MFA en un objeto de usuario que ya tiene detalles de registro, como el teléfono o el correo electrónico, los administradores deben hacer que el usuario se vuelva a registrar en MFA a través de Azure Portal o PowerShell. Si el usuario no se vuelve a registrar, su estado de MFA no pasa de *Habilitado* a *Aplicado* en la interfaz de usuario de administración de MFA.

### <a name="view-the-status-for-a-user"></a>Ver el estado de un usuario

Para acceder a la página donde puede ver y administrar los estados de usuario, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador.
2. Busque y seleccione *Azure Active Directory*. Seleccione **Usuarios** > **Todos los usuarios**.
3. Seleccione **Multi-Factor Authentication**. Es posible que tenga que desplazarse hacia la derecha para ver esta opción de menú. Seleccione la siguiente captura de pantalla de ejemplo para ver la ventana de Azure Portal completa y la ubicación del menú: [![](media/howto-mfa-userstates/selectmfa-cropped.png "Selección de Multi-Factor Authentication desde la ventana Usuarios en Azure AD")](media/howto-mfa-userstates/selectmfa.png#lightbox)
4. Se abre una nueva página que muestra los estados de usuario.
   ![Estado de usuario de Multi-Factor Authentication (captura de pantalla)](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Cambiar el estado de un usuario

1. Use los pasos anteriores para acceder a la página de **usuarios** de Azure Multi-Factor Authentication.
2. Busque el usuario que desea habilitar para Azure MFA. Puede que necesite cambiar la vista en la parte superior.
   ![Seleccione el usuario para cambiar su estado desde la pestaña de usuarios](./media/howto-mfa-userstates/enable1.png)
3. Active la casilla situada junto a su nombre.
4. En el lado derecho, bajo los **pasos rápidos**, elija **Habilitar** o **Deshabilitar**.
   ![Habilite el usuario seleccionado haciendo clic en Habilitar en el menú de pasos rápidos](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Habilitado* significa que los usuarios cambian automáticamente a *Aplicado* cuando se registren en Azure MFA. No cambie manualmente el estado de usuario a *Aplicado*.

5. Confirme la selección en la ventana emergente que se abre.

Después de habilitar los usuarios, notifíquelos por correo electrónico. Debe indicarles que se les pedirá que se registren la próxima vez que inicien sesión. Además, si su organización utiliza aplicaciones sin explorador que no son compatibles con la autenticación moderna, deben crear contraseñas de aplicación. También puede incluir un vínculo a la [Guía del usuario final de Azure MFA](../user-help/multi-factor-authentication-end-user.md), que les ayudará a empezar a trabajar.

### <a name="use-powershell"></a>Uso de PowerShell

Para cambiar el estado del usuario mediante [Azure AD PowerShell](/powershell/azure/overview), debe cambiar `$st.State`. Hay tres estados posibles:

* habilitado
* Aplicado
* Disabled  

No mueva a los usuarios directamente a estado *Aplicado*. Si lo hace, las aplicaciones que no son de explorador dejan de funcionar porque el usuario no ha pasado a través del proceso de registro de Azure MFA y obtenido una [contraseña de aplicación](howto-mfa-mfasettings.md#app-passwords).

Instale el módulo en primer lugar, mediante:

   ```PowerShell
   Install-Module MSOnline
   ```

> [!TIP]
> No se olvide de conectarse primero con **Connect-MsolService**

   ```PowerShell
   Connect-MsolService
   ```

Este ejemplo de script de PowerShell habilita MFA para un usuario individual:

   ```PowerShell
   Import-Module MSOnline
   Connect-MsolService
   $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
   $st.RelyingParty = "*"
   $st.State = "Enabled"
   $sta = @($st)
   Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
   ```

El uso de PowerShell es una buena opción cuando necesite habilitar usuarios de forma masiva. Por ejemplo, el script siguiente recorre en bucle una lista de usuarios y habilita MFA en sus cuentas:

   ```PowerShell
   $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
   foreach ($user in $users)
   {
       $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
       $st.RelyingParty = "*"
       $st.State = "Enabled"
       $sta = @($st)
       Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
   }
   ```

Para deshabilitar MFA, use este script:

   ```PowerShell
   Get-MsolUser -UserPrincipalName user@domain.com | Set-MsolUser -StrongAuthenticationRequirements @()
   ```

que también se puede abreviar como:

   ```PowerShell
   Set-MsolUser -UserPrincipalName user@domain.com -StrongAuthenticationRequirements @()
   ```

### <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Convertir a los usuarios de MFA por usuario a MFA basado en el acceso condicional

El siguiente comando de PowerShell puede ayudarle a realizar la conversión a Azure Multi-Factor Authentication basado en el acceso condicional.

Ejecútelo en una ventana de ISE o guárdelo como un archivo .PS1 para ejecutarlo localmente.

```PowerShell
# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

# Disable MFA for all users
Get-MsolUser -All | Set-MfaState -State Disabled
```

> [!NOTE]
> Recientemente hemos cambiado el comportamiento y el script de PowerShell anterior en consecuencia. Anteriormente, el script se guardaba fuera de los métodos de MFA, se deshabilitó MFA y restauraba los métodos. Ahora, ya no es necesario que el comportamiento predeterminado para deshabilitar no borre los métodos.
>
> Si se vuelve a habilitar MFA en un objeto de usuario que ya tiene detalles de registro, como el teléfono o el correo electrónico, los administradores deben hacer que el usuario se vuelva a registrar en MFA a través de Azure Portal o PowerShell. Si el usuario no se vuelve a registrar, su estado de MFA no pasa de *Habilitado* a *Aplicado* en la interfaz de usuario de administración de MFA.

## <a name="next-steps"></a>Pasos siguientes

* ¿Por qué se solicitó o no a un usuario que realizará MFA? Consulte la sección [Informe de inicios de sesión de Azure AD en los informes del documento Azure Multi-factor Authentication](howto-mfa-reporting.md#azure-ad-sign-ins-report).
* Para configurar opciones adicionales tales como direcciones IP de confianza, mensajes de voz personalizados y alertas de fraude, vea el artículo [Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)
* Puede encontrar información sobre cómo administrar la configuración de usuario de Azure Multi-factor Authentication en el artículo [Manage user settings with Azure Multi-Factor Authentication in the cloud](howto-mfa-userdevicesettings.md) (Administración de la configuración de usuario con Azure Multi-Factor Authentication en la nube).
