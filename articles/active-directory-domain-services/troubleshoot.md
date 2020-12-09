---
title: Solución de problemas de Azure Active Directory Domain Services | Microsoft Docs
description: Aprenda a solucionar errores comunes al crear o administrar una instancia de Azure Active Directory Domain Services
services: active-directory-ds
author: justinha
manager: daveba
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/06/2020
ms.author: justinha
ms.openlocfilehash: 89b04f86d41f8e4828580f70a9aec8acea3e0053
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96618457"
---
# <a name="common-errors-and-troubleshooting-steps-for-azure-active-directory-domain-services"></a>Errores comunes y pasos de solución de problemas para Azure Active Directory Domain Services

Como parte central de la identificación y la autenticación en las aplicaciones, Azure Active Directory Domain Services (Azure AD DS) a veces presenta problemas. Si es el caso, hay algunos mensajes de error comunes y pasos de solución de problemas asociados que le ayudarán a poner todo de nuevo en funcionamiento. En cualquier momento también puede [abrir una solicitud de soporte técnico de Azure][azure-support] para obtener ayuda adicional de solución de problemas.

En este artículo se proporcionan los pasos para la solución de problemas comunes en Azure AD DS.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>No puede habilitar Azure AD Domain Services para su directorio de Azure AD

Si tiene problemas para habilitar Azure AD DS, revise los siguientes errores comunes y los pasos para resolverlos:

| **Ejemplo de mensaje de error** | **Resolución** |
| --- |:--- |
| *El nombre aaddscontoso.com ya se está usando en esta red. Especifique un nombre que no esté en uso.* |[Conflicto de nombres de dominio en la red virtual](troubleshoot.md#domain-name-conflict) |
| *No se pudo habilitar Domain Services en este inquilino de Azure AD. El servicio no tiene los permisos adecuados para la aplicación llamada 'Azure AD Domain Services Sync'. Elimine la aplicación llamada "Azure AD Domain Services Sync" e intente habilitar Domain Services para el inquilino de Azure AD.)* |[Domain Services carece de permisos suficientes para la aplicación Azure AD Domain Services Sync](troubleshoot.md#inadequate-permissions) |
| *No se pudo habilitar Domain Services en este inquilino de Azure AD. La aplicación de Domain Services en el inquilino de Azure AD carece de los permisos necesarios para habilitar Domain Services. Elimine la aplicación con el identificador d87dcbc6-a371-462e-88e3-28ad15ec4e64 e intente habilitar Domain Services para el inquilino de Azure AD.* |[La aplicación de Domain Services no está configurada correctamente en el inquilino de Azure AD](troubleshoot.md#invalid-configuration) |
| *No se pudo habilitar Domain Services en este inquilino de Azure AD. La aplicación Microsoft Azure AD está deshabilitada en el inquilino de Azure AD. Habilite la aplicación con el identificador 00000002-0000-0000-c000-000000000000 e intente habilitar Domain Services para el inquilino de Azure AD.* |[La aplicación Microsoft Graph está deshabilitada en el inquilino de Azure AD](troubleshoot.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Conflicto de nombres de dominio

**Mensaje de error**

*El nombre aaddscontoso.com ya se está usando en esta red. Especifique un nombre que no esté en uso.*

**Resolución**

Compruebe que no tenga un entorno de AD DS con el mismo nombre de dominio en la misma red virtual o una emparejada. Por ejemplo, puede tener un dominio de AD DS denominado *aaddscontoso.com* que se ejecute en máquinas virtuales de Azure. Al intentar habilitar un dominio administrado de Azure AD DS con el mismo nombre de dominio (*aaddscontoso.com*) en esa red virtual, la operación solicitada genera un error

que se debe a los conflictos de nombre en el nombre de dominio de la red virtual. Una búsqueda DNS comprueba si un entorno de AD DS existente responde en el nombre de dominio solicitado. Para resolver este error, use un nombre diferente para configurar el dominio administrado o anule el aprovisionamiento del dominio de AD DS existente e inténtelo de nuevo para habilitar Azure AD DS.

### <a name="inadequate-permissions"></a>Permisos insuficientes

**Mensaje de error**

*No se pudo habilitar Domain Services en este inquilino de Azure AD. El servicio no tiene los permisos adecuados para la aplicación llamada 'Azure AD Domain Services Sync'. Elimine la aplicación llamada "Azure AD Domain Services Sync" e intente habilitar Domain Services para el inquilino de Azure AD.)*

**Resolución**

Compruebe si hay una aplicación con el nombre *Azure AD Domain Services Sync* en el directorio de Azure AD. Si existe, elimínela y vuelva a habilitar Azure AD DS. Para buscar una aplicación existente y eliminarla si es necesario, complete los pasos siguientes:

1. En Azure Portal, seleccione **Azure Active Directory** en el menú de navegación izquierdo.
1. Seleccione **Aplicaciones empresariales**. En el menú desplegable **Tipo de aplicación**, seleccione *Todas las aplicaciones* y **Aplicar**.
1. En el cuadro de búsqueda, escriba *Azure AD Domain Services Sync*. Si la aplicación existe, selecciónela y elija **Eliminar**.
1. Cuando haya eliminado la aplicación, intente volver a habilitar Azure AD DS.

### <a name="invalid-configuration"></a>Configuración no válida

**Mensaje de error**

*No se pudo habilitar Domain Services en este inquilino de Azure AD. La aplicación de Domain Services en el inquilino de Azure AD carece de los permisos necesarios para habilitar Domain Services. Elimine la aplicación con el identificador d87dcbc6-a371-462e-88e3-28ad15ec4e64 e intente habilitar Domain Services para el inquilino de Azure AD.*

**Resolución**

Compruebe si hay una aplicación con el nombre *AzureActiveDirectoryDomainControllerServices* con el identificador de aplicación *d87dcbc6-a371-462e-88e3-28ad15ec4e64* en el directorio de Azure AD. Si existe, elimínela y vuelva a habilitar Azure AD DS.

Use el siguiente script de PowerShell para buscar una instancia de aplicación existente y eliminarla si es necesario:

```powershell
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```

### <a name="microsoft-graph-disabled"></a>Microsoft Graph deshabilitado

**Mensaje de error**

*No se pudo habilitar Domain Services en este inquilino de Azure AD. La aplicación Microsoft Azure AD está deshabilitada en el inquilino de Azure AD. Habilite la aplicación con el identificador 00000002-0000-0000-c000-000000000000 e intente habilitar Domain Services para el inquilino de Azure AD.*

**Resolución**

Compruebe si ha deshabilitado una aplicación con el identificador *00000002-0000-0000-c000-000000000000*. Se trata de la aplicación de Microsoft Azure AD, que proporciona acceso al inquilino de Azure AD a Graph API. Para sincronizar el inquilino de Azure AD, esta aplicación debe estar habilitada.

Para comprobar el estado de esta aplicación y habilitarla si es necesario, complete los pasos siguientes:

1. En Azure Portal, seleccione **Azure Active Directory** en el menú de navegación izquierdo.
1. Seleccione **Aplicaciones empresariales**. En el menú desplegable **Tipo de aplicación**, seleccione *Todas las aplicaciones* y **Aplicar**.
1. En el cuadro de búsqueda, escriba *00000002-0000-0000-c000-00000000000*. Seleccione la aplicación y elija **Propiedades**.
1. Si la opción **¿Habilitado para que los usuarios inicien sesión?** está establecida en *No*, cambie el valor a *Sí* y seleccione **Guardar**.
1. Cuando haya habilitado la aplicación, intente volver a habilitar Azure AD DS.

## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Los usuarios no pueden iniciar sesión en el dominio administrado de los Servicios de dominio de Azure AD

Si uno o más usuarios de su inquilino de Azure AD no pueden iniciar sesión en el dominio administrado, complete los siguientes pasos de solución de problemas:

* **Formato de las credenciales**: pruebe a usar el formato UPN para especificar las credenciales, por ejemplo, `dee@aaddscontoso.onmicrosoft.com`. El formato UPN es el recomendado para especificar las credenciales en Azure AD DS. Asegúrese de que este UPN está configurado correctamente en Azure AD.

    El atributo *SAMAccountName* de su cuenta, como *AADDSCONTOSO\driley*, se puede generar automáticamente si hay varios usuarios con el mismo prefijo UPN en el inquilino o si el prefijo UPN es demasiado largo. Por lo tanto, el formato del atributo *SAMAccountName* de su cuenta puede que no sea el que espera o el que usa en su dominio local.

* **Sincronización de contraseña**: Asegúrese de que la ha habilitado para los [usuarios exclusivos de la nube][cloud-only-passwords] o para [entornos híbridos mediante Azure AD Connect][hybrid-phs].
    * **Cuentas sincronizadas híbridas:** Si las cuentas de usuario afectadas se sincronizan desde un directorio local, compruebe que ha hecho lo siguiente:
    
      * Implementar la [versión más reciente recomendada de Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) o actualizar a esta versión.
      * Configurar Azure AD Connect para [realizar una sincronización completa][hybrid-phs].
      * En función del tamaño de su directorio, las cuentas de usuario y los hash de credenciales pueden tardar unos minutos en estar disponibles en el dominio administrado. Espere el tiempo necesario antes de intentar autenticarse en el dominio administrado.
      * Si el problema persiste después de comprobar los pasos anteriores, pruebe a reiniciar los *servicios de sincronización de Microsoft Azure Active Directory*. En el servidor de Azure AD Connect, abra un símbolo del sistema y luego, ejecute los siguientes comandos:
    
        ```console
        net stop 'Microsoft Azure AD Sync'
        net start 'Microsoft Azure AD Sync'
        ```

    * **Cuentas de solo nube**: Si la cuenta de usuario afectada es solo para la nube, asegúrese de que el [usuario ha cambiado su contraseña después de habilitar Azure AD DS][cloud-only-passwords]. Este restablecimiento de contraseña hace que se generen los hash de credenciales necesarios para el dominio administrado.

* **Verifique que la cuenta de usuario está activa**: De manera predeterminada, si se escribe una contraseña incorrecta en el dominio administrado cinco veces en un lapso de dos minutos, la cuenta de usuario quedará bloqueada durante 30 minutos. El usuario no podrá iniciar sesión mientras la cuenta esté bloqueada. Pasados los 30 minutos, la cuenta de usuario se desbloqueará automáticamente.
  * Los intentos no válidos de escribir la contraseña del dominio administrado no provocarán el bloqueo de la cuenta de usuario en Azure AD. La cuenta de usuario solo se bloquea en el dominio administrado. Compruebe el estado de la cuenta de usuario en la *consola de administración de Active Directory (ADAC)* desde la [máquina virtual de administración][management-vm], no en Azure AD.
  * También puede [configurar directivas de contraseña específica][password-policy] para cambiar el umbral y la duración de bloqueo predeterminados.

* **Cuentas externas**: asegúrese de que la cuenta de usuario afectada no es una cuenta externa del inquilino de Azure AD. Entre los ejemplos de las cuentas externas se incluyen las cuentas de Microsoft como `dee@live.com` o las cuentas de usuario de un directorio de Azure AD externo. Azure AD DS no almacena las credenciales de las cuentas de usuario externas, por lo que estas no pueden iniciar sesión en el dominio administrado.

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Hay una o varias alertas en el dominio administrado

Si hay alertas activas en el dominio administrado, puede impedir que el proceso de autenticación funcione correctamente.

Para ver si hay alguna alerta activa, [compruebe el estado de mantenimiento de un de dominio administrado][check-health]. Si se muestra alguna alerta, [solucione los problemas][troubleshoot-alerts].

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Los usuarios quitados del inquilino de Azure AD no se quitan de su dominio administrado

Azure AD protege contra la eliminación accidental de objetos de usuario. Al eliminar una cuenta de usuario del inquilino de Azure AD, se mueve el objeto de usuario correspondiente a la papelera de reciclaje. Cuando esta operación de eliminación se sincroniza con el dominio administrado, la cuenta de usuario correspondiente se marca como deshabilitada. Esta característica ayuda a recuperar o restaurar la cuenta de usuario.

La cuenta de usuario permanece en estado deshabilitado en el dominio administrado, incluso si vuelve a crear una cuenta de usuario con el mismo UPN en el directorio de Azure AD. Para quitar la cuenta de usuario del dominio administrado, debe eliminarla forzosamente del inquilino de Azure AD.

Para quitar completamente una cuenta de usuario de un dominio administrado, elimine el usuario de forma permanente del inquilino de Azure AD con el cmdlet [Remove-MsolUser][Remove-MsolUser] de PowerShell con el parámetro `-RemoveFromRecycleBin`.

## <a name="next-steps"></a>Pasos siguientes

Si los problemas persisten, [abra una solicitud de soporte técnico de Azure][azure-support] para obtener ayuda adicional de solución de problemas.

<!-- INTERNAL LINKS -->
[cloud-only-passwords]: tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds
[hybrid-phs]: tutorial-configure-password-hash-sync.md
[management-vm]: tutorial-create-management-vm.md
[password-policy]: password-policy.md
[check-health]: check-health.md
[troubleshoot-alerts]: troubleshoot-alerts.md
[Remove-MsolUser]: /powershell/module/MSOnline/Remove-MsolUser
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
