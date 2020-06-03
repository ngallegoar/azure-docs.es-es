---
title: Iniciar sesión con el correo electrónico como id. de inicio de sesión alternativo para Azure Active Directory
description: Aprenda a configurar y permitir que los usuarios inicien sesión en Azure Active Directory mediante su dirección de correo electrónico como id. de inicio de sesión alternativo (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 05/22/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: scottsta
ms.openlocfilehash: ed317039e683ef36054d5ace612e09ca75dfa11e
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83837314"
---
# <a name="sign-in-to-azure-using-email-as-an-alternate-login-id-preview"></a>Iniciar sesión en Azure mediante el correo electrónico como id. de inicio de sesión alternativo (versión preliminar)

Muchas organizaciones quieren permitir que los usuarios inicien sesión en Azure con las mismas credenciales que su entorno de directorio local. Con este enfoque, conocido como autenticación híbrida, los usuarios solo necesitan recordar un conjunto de credenciales.

Algunas organizaciones no han migrado a la autenticación híbrida por las siguientes razones:

* De forma predeterminada, el nombre principal de usuario (UPN) de Azure Active Directory (Azure AD) se establece en el mismo UPN que el directorio local.
* Al cambiar el Azure AD UPN, se crea una coincidencia incorrecta entre entornos locales y de Azure que podrían causar problemas con ciertas aplicaciones y servicios.
* Debido a las razones empresariales o de cumplimiento, la organización no quiere usar el UPN local para iniciar sesión en Azure.

Para ayudar con el traslado a la autenticación híbrida, ahora puede configurar Azure AD para permitir que los usuarios inicien sesión en Azure con un correo electrónico en el dominio comprobado como un id. de inicio de sesión alternativo. Por ejemplo, si *Contoso* cambia de nombre a *Fabrikam*, en lugar de seguir iniciando sesión con el UPN heredado `balas@contoso.com`, ahora se puede usar el correo electrónico como id. de inicio de sesión alternativo. Para acceder a una aplicación o a los servicios, los usuarios inician sesión en Azure con su correo electrónico asignado, como `balas@fabrikam.com`.

|     |
| --- |
| Iniciar sesión en Azure AD con el correo electrónico como id. de inicio de sesión alternativo es una característica en vista previa (GB) pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="overview-of-azure-ad-sign-in-approaches"></a>Información general de los métodos de inicio de sesión Azure AD

Los nombres principales de usuario (UPN) son identificadores únicos para una cuenta de usuario en el directorio local y en Azure AD. Cada cuenta de usuario de un directorio está representada por un UPN, como `balas@contoso.com`. De forma predeterminada, cuando sincroniza un entorno local de Active Directory Domain Services (AD DS) con Azure AD, el UPN de Azure AD se establece para que coincida con el UPN local.

En muchas organizaciones, es preciso establecer el UPN local y Azure AD UPN para que coincidan. Cuando los usuarios inician sesión en aplicaciones y servicios de Azure, usan su Azure AD UPN. Sin embargo, algunas organizaciones no pueden usar UPN similares para el inicio de sesión debido a las directivas empresariales o problemas de experiencia del usuario.

Las organizaciones que no pueden usar UPN coincidentes en Azure AD tienen algunas opciones:

* Un enfoque consiste en establecer el UPN de Azure AD en algo diferente en función de las necesidades empresariales, como `balas@fabrikam.com`.
    * Sin embargo, no todas las aplicaciones y servicios son compatibles con el uso de un valor diferente para el UPN local y el UPN de Azure AD.
* Un mejor enfoque consiste en asegurarse de que el Azure AD y los UPN locales están establecidos en el mismo valor, y configurar Azure AD para permitir que los usuarios inicien sesión en Azure con su correo electrónico como un identificador de inicio de sesión alternativo.

Con el correo electrónico como identificador de inicio de sesión alternativo, los usuarios aún pueden iniciar sesión en Azure mediante su UPN, pero también pueden iniciar sesión con su correo electrónico. Para admitir esto, defina una dirección de correo electrónico en el atributo *ProxyAddresses* del usuario en el directorio local. Este atributo de *ProxyAddress* admite una o varias dirección de correo electrónico.

## <a name="synchronize-sign-in-email-addresses-to-azure-ad"></a>Sincronizar las direcciones de correo electrónico de inicio de sesión con Azure AD

La autenticación tradicional de Active Directory Domain Services (AD DS) o Servicios de federación de Active Directory (AD FS) se produce directamente en la red y se controla mediante la infraestructura de AD DS. Con la autenticación híbrida, los usuarios pueden iniciar sesión directamente en Azure AD.

Para admitir este enfoque de autenticación híbrida, debe sincronizar el entorno de AD DS local con Azure AD mediante [Azure AD Connect][azure-ad-connect] y configurarlo para que use la sincronización de hash de contraseñas (PHS) o la autenticación de paso a través (PTA).

En ambas opciones de configuración, el usuario envía su nombre de usuario y contraseña a Azure AD, que valida las credenciales y emite un vale. Cuando los usuarios inician sesión en Azure AD, se elimina la necesidad de que su organización albergue y administre una infraestructura de AD FS.

![Diagrama de identidad híbrida de Azure AD con sincronización de hash de contraseñas](media/howto-authentication-use-email-signin/hybrid-password-hash-sync.png)

![Diagrama de identidad híbrida de Azure AD con autenticación de paso a través](media/howto-authentication-use-email-signin/hybrid-pass-through-authentication.png)

Uno de los atributos de usuario que se sincroniza automáticamente mediante Azure AD Connect es *ProxyAddresses*. Si los usuarios tienen una dirección de correo electrónico definida en el entorno de AD DS local como parte del atributo *ProxyAddresses*, se sincroniza automáticamente con Azure AD. Esta dirección de correo electrónico se puede usar directamente en el proceso de inicio de sesión de Azure AD como identificador de inicio de sesión alternativo.

> [!IMPORTANT]
> Solo los mensajes de correo electrónico de los dominios comprobados del inquilino se sincronizan con Azure AD. Cada inquilino de Azure AD tiene uno o varios dominios comprobados, para los que se ha comprobado la propiedad, y se enlazan de forma exclusiva con el inquilino.
>
> Para más información, consulte [Agregar y comprobar un nombre de dominio personalizado en Azure AD][verify-domain].

Para más información, consulte [Selección del método de autenticación adecuado para la solución de identidad híbrida de Azure AD][hybrid-auth-methods].

## <a name="enable-user-sign-in-with-an-email-address"></a>Habilitar el inicio de sesión de usuario con una dirección de correo electrónico

Una vez que los usuarios con el atributo *ProxyAddresses* aplicado se sincronizan con Azure AD mediante Azure AD Connect, necesita habilitar la función para que los usuarios se registren con el correo electrónico como un ID de inicio de sesión alternativo para su inquilino. Esta característica indica a los servidores de inicio de sesión de Azure AD que no solo comprueben el nombre de inicio de sesión con los valores UPN, sino también contra los valores *ProxyAddresses* de la dirección de correo electrónico.

Durante la versión preliminar, actualmente solo puede habilitar el inicio de sesión con correo electrónico como una característica de identificador de inicio de sesión alternativo mediante PowerShell. Necesita permiso de *administrador de inquilinos* para completar los siguientes pasos:

1. Abra una sesión de PowerShell como administrador, luego instale el módulo *AzureADPreview* con el cmdlet [Install-Module][Install-Module]:

    ```powershell
    Install-Module AzureADPreview
    ```

    Si se le solicita, seleccione **Y** para instalar NuGet o para instalar desde un repositorio que no es de confianza.

1. Ingrese a su inquilino Azure AD como *administrador de inquilinos* mediante el cmdlet [Conecta-AzureAD][Connect-AzureAD]:

    ```powershell
    Connect-AzureAD
    ```

    El comando devuelve información acerca de su cuenta, entorno e id. de inquilino.

1. Compruebe si la directiva de *HomeRealmDiscoveryPolicy* ya existe en el inquilino con el cmdlet [Get-AzureADPolicy][Get-AzureADPolicy] como se indica a continuación:

    ```powershell
    Get-AzureADPolicy | where-object {$_.Type -eq "HomeRealmDiscoveryPolicy"} | fl *
    ```

1. Si no hay ninguna directiva configurada actualmente, el comando no devuelve nada. Si se devuelve una directiva, omita este paso y vaya al paso siguiente para actualizar una directiva existente.

    Para agregar la directiva de *HomeRealmDiscoveryPolicy* al inquilino, use el cmdlet [New-AzureADPolicy][New-AzureADPolicy] y establezca el atributo *AlternateIdLogin* en *"Enabled": true* como se muestra en el siguiente ejemplo:

    ```powershell
    New-AzureADPolicy -Definition @('{"HomeRealmDiscoveryPolicy" :{"AlternateIdLogin":{"Enabled": true}}}') `
        -DisplayName "BasicAutoAccelerationPolicy" `
        -IsOrganizationDefault $true `
        -Type "HomeRealmDiscoveryPolicy"
    ```

    Cuando la política se haya creado con éxito, el comando devuelve el Id. de la política, como se muestra en la siguiente salida de ejemplo:

    ```powershell
    Id                                   DisplayName                 Type                     IsOrganizationDefault
    --                                   -----------                 ----                     ---------------------
    5de3afbe-4b7a-4b33-86b0-7bbe308db7f7 BasicAutoAccelerationPolicy HomeRealmDiscoveryPolicy True
    ```

1. Si ya hay una política configurada, compruebe si el atributo *AlternateIdLogin*  está habilitado, como se muestra en la siguiente salida directiva de ejemplo:

    ```powershell
    Id : 5de3afbe-4b7a-4b33-86b0-7bbe308db7f7
    OdataType :
    AlternativeIdentifier :
    Definition : {{"HomeRealmDiscoveryPolicy" :{"AlternateIdLogin":{"Enabled": true}}}}
    DisplayName : BasicAutoAccelerationPolicy
    IsOrganizationDefault : True
    KeyCredentials : {}
    Type : HomeRealmDiscoveryPolicy
    ```

    Si la directiva existe pero el atributo *AlternateIdLogin* no está presente o habilitado, o si existen otros atributos en la directiva que desea conservar, actualice la directiva existente con el cmdlet [Set-AzureADPolicy][Set-AzureADPolicy].

    > [!IMPORTANT]
    > Al actualizar la directiva, asegúrese de que incluye cualquier configuración anterior y el nuevo atributo *AlternateIdLogin*.

    En el ejemplo siguiente se agrega el atributo *AlternateIdLogin* y se conserva el atributo  *AllowCloudPasswordValidation* que se puede haber establecido:

    ```powershell
    Set-AzureADPolicy -id b581c39c-8fe3-4bb5-b53d-ea3de05abb4b `
        -Definition @('{"HomeRealmDiscoveryPolicy" :{"AllowCloudPasswordValidation":true,"AlternateIdLogin":{"Enabled": true}}}') `
        -DisplayName "BasicAutoAccelerationPolicy" `
        -IsOrganizationDefault $true `
        -Type "HomeRealmDiscoveryPolicy"
    ```

    Confirme que la directiva actualizada muestra los cambios y que el atributo *AlternateIdLogin* está habilitado ahora:

    ```powershell
    Get-AzureADPolicy | where-object {$_.Type -eq "HomeRealmDiscoveryPolicy"} | fl *
    ```

## <a name="test-user-sign-in-with-email"></a>Prueba del inicio de sesión de usuario con correo electrónico

Para probar que los usuarios pueden iniciar sesión con el correo electrónico, vaya a [https://myprofile.microsoft.com][my-profile] e inicie sesión con una cuenta de usuario basada en su dirección de correo electrónico, como `balas@fabrikam.com`, no su UPN, como `balas@contoso.com`. La experiencia de inicio de sesión debe verse y sentirse igual que con un evento de inicio de sesión basado en UPN.

## <a name="troubleshoot"></a>Solución de problemas

Si los usuarios tienen problemas con los eventos de inicio de sesión que usan su dirección de correo electrónico, revise los siguientes pasos de solución de problemas:

1. Asegúrese de que la cuenta de usuario tiene su dirección de correo electrónico establecida para el atributo *ProxyAddresses* en el entorno de AD DS local.
1. Compruebe que Azure AD Connect está configurado y sincroniza correctamente las cuentas de usuario del entorno de AD DS local en Azure AD.
1. Confirme que la directiva Azure AD *HomeRealmDiscoveryPolicy* tiene el atributo *AlternateIdLogin* establecido en *"Enabled": true*:

    ```powershell
    Get-AzureADPolicy | where-object {$_.Type -eq "HomeRealmDiscoveryPolicy"} | fl *
    ```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la identidad híbrida, como Proxy de aplicación de Azure AD o Azure AD Domain Services, consulte [Identidad híbrida Azure AD para el acceso y la gestión de las cargas de trabajo locales][hybrid-overview].

Para obtener más información sobre las operaciones de identidad híbrida, consulte cómo funciona [Sincronización hash de contraseña][phs-overview] o [Autenticación de paso a través][pta-overview].

<!-- INTERNAL LINKS -->
[verify-domain]: ../fundamentals/add-custom-domain.md
[hybrid-auth-methods]: ../hybrid/choose-ad-authn.md
[azure-ad-connect]: ../hybrid/whatis-azure-ad-connect.md
[hybrid-overview]: ../hybrid/cloud-governed-management-for-on-premises.md
[phs-overview]: ../hybrid/how-to-connect-password-hash-synchronization.md
[pta-overview]: ../hybrid/how-to-connect-pta-how-it-works.md

<!-- EXTERNAL LINKS -->
[Install-Module]: /powershell/module/powershellget/install-module
[Connect-AzureAD]: /powershell/module/azuread/connect-azuread
[Get-AzureADPolicy]: /powershell/module/azuread/get-azureadpolicy
[New-AzureADPolicy]: /powershell/module/azuread/new-azureadpolicy
[Set-AzureADPolicy]: /powershell/module/azuread/set-azureadpolicy
[my-profile]: https://myprofile.microsoft.com
