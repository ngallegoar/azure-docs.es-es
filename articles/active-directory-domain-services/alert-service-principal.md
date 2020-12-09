---
title: Resolución de alertas de entidad de servicio en Azure AD Domain Services | Microsoft Docs
description: Información sobre cómo resolver problemas de alertas de configuración no válida de entidades de servicio para Azure Active Directory Domain Services
services: active-directory-ds
author: justinha
manager: daveba
ms.assetid: f168870c-b43a-4dd6-a13f-5cfadc5edf2c
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/09/2020
ms.author: justinha
ms.openlocfilehash: 00ab5c85a477c9c4080acf252cbbde9d4ce816a9
ms.sourcegitcommit: 8192034867ee1fd3925c4a48d890f140ca3918ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2020
ms.locfileid: "96620246"
---
# <a name="known-issues-service-principal-alerts-in-azure-active-directory-domain-services"></a>Problemas conocidos: Alertas de entidades de servicio en Azure Active Directory Domain Services

Las [entidades de servicio](../active-directory/develop/app-objects-and-service-principals.md) son aplicaciones que la plataforma Azure usa para administrar, actualizar y mantener un dominio administrado de Azure Active Directory Domain Services (Azure AD DS). Si se elimina una entidad de servicio, se verá afectada la funcionalidad en el dominio administrado.

Este artículo le ayuda a solucionar problemas y a resolver alertas de configuración relacionadas con las entidades de servicio.

## <a name="alert-aadds102-service-principal-not-found"></a>Alerta AADDS102: no se encuentra la entidad de servicio

### <a name="alert-message"></a>Mensaje de alerta

*Una entidad de servicio necesaria para que Azure AD Domain Services funcione correctamente se ha eliminado del directorio de Azure AD. Esta configuración afecta a la capacidad de Microsoft para supervisar, administrar, revisar y sincronizar el dominio administrado.*

Si se elimina una entidad de servicio necesaria, la plataforma Azure no puede realizar tareas de administración automatizadas. Es posible que el dominio administrado no aplique correctamente las actualizaciones o realice copias de seguridad.

### <a name="check-for-missing-service-principals"></a>Comprobación de las entidades de servicio que faltan

Para comprobar qué entidad de servicio falta y debe volver a crearse, haga lo siguiente:

1. En Azure Portal, seleccione **Azure Active Directory** en el menú de navegación izquierdo.
1. Seleccione **Aplicaciones empresariales**. En el menú desplegable **Tipo de aplicación**, seleccione *Todas las aplicaciones* y **Aplicar**.
1. Busque cada uno de los siguientes identificadores de aplicación. Si no se encuentra ninguna aplicación existente, siga los pasos de la *Resolución* para crear la entidad de servicio o volver a registrar el espacio de nombres.

    | Identificador de aplicación | Resolución |
    | :--- | :--- |
    | 2565bd9d-da50-47d4-8b85-4c97f669dc36 | [Volver a crear una entidad de servicio que falta](#recreate-a-missing-service-principal) |
    | 443155a6-77f3-45e3-882b-22b3a8d431fb | [Volver a realizar el registro en el espacio de nombres de Microsoft.AAD](#re-register-the-microsoft-aad-namespace) |
    | abba844e-bc0e-44b0-947a-dc74e5d09022 | [Volver a realizar el registro en el espacio de nombres de Microsoft.AAD](#re-register-the-microsoft-aad-namespace) |
    | d87dcbc6-a371-462e-88e3-28ad15ec4e64 | [Volver a realizar el registro en el espacio de nombres de Microsoft.AAD](#re-register-the-microsoft-aad-namespace) |

### <a name="recreate-a-missing-service-principal"></a>Volver a crear una entidad de servicio que falta

Si falta el identificador de aplicación *2565bd9d-da50-47d4-8b85-4c97f669dc36* en el directorio Azure AD, use Azure AD PowerShell para completar los pasos siguientes. Para más información, consulte [PowerShell de Azure AD](/powershell/azure/active-directory/install-adv2).

1. Si es necesario, instale el módulo de PowerShell de Azure AD e impórtelo de la forma siguiente:

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

1. Ahora, vuelva a crear la entidad de servicio con el cmdlet [New-AzureAdServicePrincipal][New-AzureAdServicePrincipal]:

    ```powershell
    New-AzureAdServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
    ```

El estado del dominio administrado se actualiza automáticamente en dos horas y quita la alerta.

### <a name="re-register-the-microsoft-aad-namespace"></a>Volver a realizar el registro en el espacio de nombres de Microsoft.AAD

Si falta el identificador de aplicación *443155a6-77f3-45e3-882b-22b3a8d431fb*, *abba844e-bc0e-44b0-947a-dc74e5d09022* o *d87dcbc6-a371-462e-88e3-28ad15ec4e64* del directorio de Azure AD, no se encuentra en el directorio de Azure AD, complete los pasos siguientes para volver a registrar el proveedor de recursos de *Microsoft.AAD*:

1. En Azure Portal, seleccione **Suscripciones**.
1. Elija la suscripción asociada con el dominio administrado.
1. En el panel de navegación izquierdo, elija **Proveedores de recursos**.
1. Busque *Microsoft.AAD* y, después, seleccione **Volver a registrar**.

El estado del dominio administrado se actualiza automáticamente en dos horas y quita la alerta.

## <a name="alert-aadds105-password-synchronization-application-is-out-of-date"></a>AADDS105: la aplicación de sincronización de contraseñas no está actualizada

### <a name="alert-message"></a>Mensaje de alerta

*Se eliminó la entidad de servicio con el identificador de aplicación "d87dcbc6-a371-462e-88e3-28ad15ec4e64" y, a continuación, se volvió a crear. La regeneración excluye los permisos incoherentes en los recursos de Azure AD Domain Services necesarios para atender su dominio administrado. La sincronización de contraseñas en el dominio administrado podría verse afectada.*

Azure AD DS sincroniza automáticamente las cuentas de usuario y las credenciales de Azure AD. Si hay un problema con la aplicación de Azure AD utilizada para este proceso, se producirá un error en la sincronización de credenciales entre Azure AD DS y Azure AD.

### <a name="resolution"></a>Resolución

Para volver a crear la aplicación de Azure AD utilizada para la sincronización de credenciales, use Azure AD PowerShell para completar los pasos siguientes. Para más información, consulte [Instalación de Azure AD PowerShell](/powershell/azure/active-directory/install-adv2).

1. Si es necesario, instale el módulo de PowerShell de Azure AD e impórtelo de la forma siguiente:

    ```powershell
    Install-Module AzureAD
    Import-Module AzureAD
    ```

2. Ahora, elimine la aplicación y el objeto antiguos mediante los siguientes cmdlets de PowerShell:

    ```powershell
    $app = Get-AzureADApplication -Filter "IdentifierUris eq 'https://sync.aaddc.activedirectory.windowsazure.com'"
    Remove-AzureADApplication -ObjectId $app.ObjectId
    $spObject = Get-AzureADServicePrincipal -Filter "DisplayName eq 'Azure AD Domain Services Sync'"
    Remove-AzureADServicePrincipal -ObjectId $spObject
    ```

Después de eliminar ambas aplicaciones, la plataforma Azure las vuelve a crear automáticamente e intenta reanudar la sincronización de contraseñas. El estado del dominio administrado se actualiza automáticamente en dos horas y quita la alerta.

## <a name="next-steps"></a>Pasos siguientes

Si los problemas persisten, [abra una solicitud de soporte técnico de Azure][azure-support] para obtener ayuda adicional de solución de problemas.

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md

<!-- EXTERNAL LINKS -->
[New-AzureAdServicePrincipal]: /powershell/module/AzureAD/New-AzureADServicePrincipal
