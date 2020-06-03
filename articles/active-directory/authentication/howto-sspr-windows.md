---
title: 'Autoservicio de restablecimiento de contraseña para Windows: Azure Active Directory'
description: Procedimientos para habilitar el autoservicio de restablecimiento de contraseña mediante la página de contraseña olvidada en la pantalla de inicio de sesión de Windows
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 95d1ffec6a849cb97a6151717c3e30dc362b1403
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826611"
---
# <a name="how-to-enable-password-reset-from-the-windows-login-screen"></a>Procedimientos: Habilitar el restablecimiento de contraseña desde la pantalla de inicio de sesión de Windows

En el caso de los equipos que ejecutan Windows 7, 8, 8.1 y 10, puede permitir que los usuarios restablezcan su contraseña en la pantalla de inicio de sesión de Windows. Los usuarios ya no tienen que buscar un dispositivo con un explorador Web para acceder al [portal de SSPR](https://aka.ms/sspr).

![Pantallas de inicio de sesión de Windows 7 y 10 de ejemplo en las que se muestra el vínculo de SSPR](./media/howto-sspr-windows/windows-reset-password.png)

## <a name="general-limitations"></a>Limitaciones generales

- El restablecimiento de contraseña no se admite actualmente desde Escritorio remoto o sesiones mejoradas de Hyper-V.
- Se sabe que algunos proveedores de credenciales de terceros provocan problemas con esta característica.
- Se sabe que la deshabilitación de UAC mediante la modificación de la [clave del registro EnableLUA](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpsb/958053ae-5397-4f96-977f-b7700ee461ec) provoca problemas.
- Esta característica no funciona para las redes con la autenticación de red 802.1x implementada y la opción "Realizar inmediatamente antes de que el usuario inicie sesión". Para las redes con la autenticación de red 802.1X implementada se recomienda usar la autenticación del equipo para habilitar esta característica.
- Los equipos unidos a Azure AD híbrido deben tener una línea de visión de conectividad de red a un controlador de dominio para usar las nuevas credenciales en caché de contraseña y actualización. Esto significa que los dispositivos deben estar en la red interna de la organización o en una VPN con acceso de red a un controlador de dominio local. 
- Si se usa una imagen, antes de ejecutar sysprep, asegúrese de que se borra la caché web para la cuenta predefinida de administrador antes de realizar el paso de CopyProfile. Puede encontrar más información sobre este paso en el artículo de soporte técnico [Rendimiento deficiente cuando se usa el perfil de usuario predeterminado personalizado](https://support.microsoft.com/help/4056823/performance-issue-with-custom-default-user-profile).
- Se sabe que los valores siguientes interfieren con la capacidad de usar y restablecer contraseñas en dispositivos Windows 10
    - Si en versiones de Windows 10 anteriores a la 1809 la directiva requiere Ctrl+Alt+Del, no funcionará el vínculo **Restablecer contraseña**.
    - Si las notificaciones de la pantalla de bloqueo están desactivadas, tampoco funcionará el vínculo **Restablecer contraseña**.
    - HideFastUserSwitching está establecido en habilitado o 1
    - DontDisplayLastUserName está establecido en habilitado o 1
    - NoLockScreen está establecido en habilitado o 1
    - EnableLostMode se establece en el dispositivo
    - Explorer.exe se reemplaza por un shell personalizado
- La combinación de las tres configuraciones específicas siguientes puede hacer que esta característica no funcione.
    - Inicio de sesión interactivo: No requiere CTRL + ALT + SUPR = Deshabilitado
    - DisableLockScreenAppNotifications = 1 o Habilitado
    - La SKU de Windows no es Home o Professional Edition.

## <a name="windows-10-password-reset"></a>Restablecimiento de contraseña de Windows 10

### <a name="windows-10-prerequisites"></a>Requisitos previos de Windows 10

- Un administrador debe habilitar el autoservicio de restablecimiento de contraseña de Azure AD desde Azure Portal.
- **Los usuarios deben registrarse para SSPR antes de usar esta característica**
- Requisitos del proxy de red
   - Dispositivos Windows 10 
       - Puerto 443 para `passwordreset.microsoftonline.com` y `ajax.aspnetcdn.com`
       - Los dispositivos Windows 10 solo admiten la configuración de proxy de nivel de equipo
- Ejecute al menos la actualización de abril de 2018 (v1803) de Windows 10, y los dispositivos deben estar:
    - Unido a Azure AD
    - Híbrido unido a Azure AD

### <a name="enable-for-windows-10-using-intune"></a>Habilitación para Windows 10 con Intune

La implementación de cambios de configuración para habilitar el restablecimiento de contraseña desde la pantalla de inicio de sesión mediante Intune es el método más flexible. Intune le permite implementar el cambio de configuración en un grupo específico de máquinas que defina. Este método requiere la inscripción en Intune del dispositivo.

#### <a name="create-a-device-configuration-policy-in-intune"></a>Creación de una directiva de configuración de dispositivo en Intune

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y haga clic en **Intune**.
1. Cree un nuevo perfil de configuración de dispositivo; para ello, vaya a **Configuración del dispositivo** > **Perfiles** > **Crear perfil**
   - Escriba un nombre descriptivo para el perfil
   - Opcionalmente, proporcione una descripción detallada del perfil
   - Plataforma **Windows 10 y versiones posteriores**
   - Tipo de perfil **Personalizado**
1. Configuración de los **valores**
   - **Agregue** la siguiente configuración de OMA-URI para habilitar el vínculo de restablecimiento de contraseña
      - Proporcione un nombre descriptivo para explicar lo que hace el valor de configuración
      - Opcionalmente, proporcione una descripción detallada del valor
      - **OMA-URI** establecido en `./Vendor/MSFT/Policy/Config/Authentication/AllowAadPasswordReset`
      - **Tipo de datos** establecido en **Entero**
      - **Valor** establecido en **1**
      - Haga clic en **Aceptar**
   - Haga clic en **Aceptar**
1. Haga clic en **Crear**
1. Esta directiva se puede asignar a usuarios, dispositivos o grupos específicos. Se puede encontrar más información en el artículo [Asignación de perfiles de dispositivo en Microsoft Intune](https://docs.microsoft.com/intune/device-profile-assign).

### <a name="enable-for-windows-10-using-the-registry"></a>Habilitación para Windows 10 con el Registro

1. Inicio de sesión en el equipo Windows con credenciales administrativas
1. Ejecute **regedit** como administrador
1. Establezca la siguiente clave del Registro
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\AzureADAccount`
      - `"AllowPasswordReset"=dword:00000001`

#### <a name="troubleshooting-windows-10-password-reset"></a>Solución de problemas de restablecimiento de contraseña de Windows 10

El registro de auditoría de Azure AD incluirá información sobre la dirección IP y el ClientType donde se produjo el restablecimiento de contraseña.

![Ejemplo de restablecimiento de contraseña de Windows 7 en el registro de auditoría de Azure AD](media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

Cuando los usuarios restablecen su contraseña desde la pantalla de inicio de sesión de un dispositivo Windows 10, se crea una cuenta temporal con pocos privilegios denominada `defaultuser1`. Esta cuenta se usa para proteger el proceso de restablecimiento de contraseña. La propia cuenta tiene una contraseña generada aleatoriamente, no se muestra en el inicio de sesión del dispositivo y se elimina automáticamente después de que el usuario restablece su contraseña. Pueden existir varios perfiles `defaultuser`, pero se pueden omitir con seguridad.

## <a name="windows-7-8-and-81-password-reset"></a>Restablecimiento de contraseña de Windows 7, 8 y 8.1

### <a name="windows-7-8-and-81-prerequisites"></a>Requisitos previos de Windows 7, 8 y 8.1

- Un administrador debe habilitar el autoservicio de restablecimiento de contraseña de Azure AD desde Azure Portal.
- **Los usuarios deben registrarse para SSPR antes de usar esta característica**
- Requisitos del proxy de red
   - Dispositivos Windows 7, 8 y 8.1
       - Puerto 443 para `passwordreset.microsoftonline.com`
- Sistema operativo Windows 7 o Windows 8.1 revisado.
- TLS 1.2 habilitada mediante las instrucciones que se encuentran en [Configuración del registro de TLS](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12).
- Si en el equipo hay más de un proveedor de credenciales de terceros habilitado, los usuarios verán más de un perfil de usuario en la pantalla de inicio de sesión.

> [!WARNING]
> Se debe habilitar TLS 1.2, no solo establecerlo en negociación automática.

### <a name="install"></a>Instalar

1. Descargue el instalador adecuado para la versión de Windows que desea habilitar.
   - El software está disponible en el centro de descarga de Microsoft en [https://aka.ms/sspraddin](https://aka.ms/sspraddin)
1. Inicie sesión en la máquina donde desea instalar y ejecutar el instalador.
1. Después de la instalación, se recomienda encarecidamente reiniciar el equipo.
1. Después del reinicio, en la pantalla de inicio de sesión, elija un usuario y haga clic en "¿Olvidó la contraseña?" para iniciar el flujo de trabajo de restablecimiento de contraseña.
1. Complete el flujo de trabajo siguiendo los pasos que aparecen en la pantalla para restablecer su contraseña.

![Ejemplo de Windows 7 después de hacer clic en "¿Olvidó la contraseña?" Flujo de SSPR](media/howto-sspr-windows/windows-7-sspr.png)

#### <a name="silent-installation"></a>Instalación silenciosa

- Para una instalación silenciosa, use el comando "msiexec /i SsprWindowsLogon.PROD.msi /qn".
- Para una desinstalación silenciosa, use el comando "msiexec /x SsprWindowsLogon.PROD.msi /qn".

#### <a name="troubleshooting-windows-7-8-and-81-password-reset"></a>Solución de problemas de restablecimiento de contraseña de Windows 7, 8 y 8.1

Los eventos se registrarán tanto en la máquina como en Azure AD. Los eventos de Azure AD incluirán información sobre la dirección IP y el ClientType donde se produjo el restablecimiento de contraseña.

![Ejemplo de restablecimiento de contraseña de Windows 7 en el registro de auditoría de Azure AD](media/howto-sspr-windows/windows-7-sspr-azure-ad-audit-log.png)

Si se requiere un registro adicional, se puede cambiar una clave del Registro en la máquina para habilitar el registro detallado. Habilite el registro detallado únicamente para solucionar problemas.

`HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{86D2F0AC-2171-46CF-9998-4E33B3D7FD4F}`

- Para habilitar el registro detallado, cree un `REG_DWORD: "EnableLogging"` y establézcalo en 1.
- Para deshabilitar el registro detallado, cambie `REG_DWORD: "EnableLogging"` a 0.

## <a name="what-do-users-see"></a>Lo que ven los usuarios

Ahora que ha configurado el restablecimiento de contraseña para los dispositivos Windows, ¿qué cambia para el usuario? ¿Cómo sabe que puede restablecer su contraseña en la pantalla de inicio de sesión?

![Pantallas de inicio de sesión de Windows 7 y 10 de ejemplo en las que se muestra el vínculo de SSPR](./media/howto-sspr-windows/windows-reset-password.png)

Cuando los usuarios intenten iniciar sesión, ahora verán un vínculo **Restablecer contraseña** o **He olvidado mi contraseña** que abre la experiencia de autoservicio de restablecimiento de contraseña en la pantalla de inicio de sesión. Esta funcionalidad permite a los usuarios restablecer su contraseña sin tener que usar otro dispositivo para acceder a un explorador web.

Los usuarios encontrarán instrucciones para usar esta característica en el artículo sobre el [restablecimiento de la contraseña profesional o educativa](../user-help/active-directory-passwords-update-your-own-password.md)

## <a name="next-steps"></a>Pasos siguientes

[Planificación de métodos de autenticación permitidos](concept-authentication-methods.md)

[Configuración de Windows 10](https://docs.microsoft.com/windows/configuration/)
