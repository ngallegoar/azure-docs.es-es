---
title: 'Inicio de sesión sin contraseña con la aplicación Microsoft Authenticator: Azure Active Directory'
description: Habilitar el inicio de sesión sin contraseña en Azure AD con la aplicación Microsoft Authenticator (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 09/29/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 053a489993c31344b96e83253c88eed93b27b145
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91964832"
---
# <a name="enable-passwordless-sign-in-with-the-microsoft-authenticator-app-preview"></a>Habilitar el inicio de sesión sin contraseña en Azure AD con la aplicación Microsoft Authenticator (versión preliminar)

La aplicación Microsoft Authenticator se puede utilizar para iniciar sesión en cualquier cuenta de Azure AD sin utilizar una contraseña. Similar a la tecnología de [Windows Hello para empresas](/windows/security/identity-protection/hello-for-business/hello-identity-verification), Microsoft Authenticator usa la autenticación basada en claves para habilitar una credencial de usuario que está asociada a un dispositivo y usa un sistema biométrico o un PIN. Este método de autenticación se puede usar en cualquier plataforma de dispositivos, incluidos dispositivos móviles, y con cualquier aplicación o sitio web que se integre con las bibliotecas de autenticación de Microsoft.

![Ejemplo de un inicio de sesión de explorador que solicita al usuario que apruebe el inicio de sesión](./media/howto-authentication-passwordless-phone/phone-sign-in-microsoft-authenticator-app.png)

En lugar de ver la petición de una contraseña después de escribir un nombre de usuario, una persona que ha habilitado el inicio de sesión por teléfono desde la aplicación Microsoft Authenticator ve un mensaje que le solicita pulsar un número en la aplicación. Para completar el proceso de inicio de sesión en la aplicación, el usuario debe hacer coincidir el número, elegir **Aprobar** y, después, proporcionar el PIN o información biométrica.

## <a name="prerequisites"></a>Requisitos previos

Para usar el inicio de sesión en el teléfono sin contraseña con la aplicación Microsoft Authenticator, se deben cumplir los siguientes requisitos previos:

- Azure Multi-Factor Authentication, con notificaciones de inserción permitidas como método de comprobación.
- Versión más reciente de Microsoft Authenticator instalada en dispositivos que ejecutan iOS 8.0 o superior o Android 6.0 o superior.

> [!NOTE]
> Si habilitó la versión preliminar de inicio de sesión sin contraseña de la aplicación Microsoft Authenticator mediante Azure AD PowerShell, se habilitó para todo el directorio. Si habilita con este nuevo método, reemplaza a la directiva de PowerShell. Se recomienda habilitarla para todos los usuarios del inquilino mediante el menú de los nuevos *métodos de autenticación*; de lo contrario, los usuarios que no estén en la nueva directiva ya no podrán iniciar sesión sin contraseña.

## <a name="enable-passwordless-authentication-methods"></a>Habilitar métodos de autenticación sin contraseña

Para usar la autenticación sin contraseña en Azure AD, primero debe habilitar la experiencia de registro combinada y, a continuación, habilitar el método sin contraseña para los usuarios.

### <a name="enable-the-combined-registration-experience"></a>Habilitar la experiencia de registro combinado

Las características de registro de los métodos de autenticación sin contraseña se basan en la característica de registro combinado. Para permitir que los usuarios completen por sí mismos el registro combinado, siga los pasos para [habilitar el registro de información de seguridad combinado](howto-registration-mfa-sspr-combined.md).

### <a name="enable-passwordless-phone-sign-in-authentication-methods"></a>Habilitar métodos de autenticación de inicio de sesión en el teléfono sin contraseña

Azure AD le permite elegir los métodos de autenticación que se pueden usar durante el proceso de inicio de sesión. A continuación, los usuarios se registran en los métodos que desean usar.

Siga estos pasos para habilitar el método de autenticación para el inicio de sesión en teléfono sin contraseña:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta de *administrador global*.
1. Busque *Azure Active Directory*, selecciónelo y vaya luego a **Seguridad** > **Métodos de autenticación** > **Directiva de métodos de autenticación (versión preliminar)** .
1. En **Inicio de sesión telefónico sin contraseña**, elija las opciones siguientes:
   1. **Habilitar**: Sí o No
   1. **Destino**: Todos los usuarios o Seleccionar usuarios
1. Para aplicar la nueva directiva, seleccione **Guardar**.

## <a name="user-registration-and-management-of-microsoft-authenticator-app"></a>Registro de usuario y administración de la aplicación Microsoft Authenticator

Con el método de autenticación sin contraseña disponible para su uso en Azure AD, los usuarios deben registrarse ahora para el método de autenticación sin contraseña mediante los pasos siguientes:

1. Vaya a [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) .
1. Inicie sesión y, después, agregue la aplicación Authenticator seleccionando **Agregar método > Aplicación Authenticator** y **Agregar**.
1. Siga las instrucciones para instalar y configurar la aplicación Microsoft Authenticator en el dispositivo.
1. Seleccione **Listo** para completar la configuración de Authenticator.
1. En la aplicación **Microsoft Authenticator**, seleccione **Habilitar inicio de sesión en el teléfono** en el menú desplegable para la cuenta registrada.
1. Siga las instrucciones de la aplicación para terminar de registrar la cuenta para el inicio de sesión en el teléfono sin contraseña.

Las organizaciones pueden remitir a sus usuarios a [Inicio de sesión con el teléfono, no con la contraseña](../user-help/user-help-auth-app-sign-in.md) para obtener más ayuda para la configuración de la aplicación Microsoft Authenticator y habilitar el inicio de sesión en el teléfono.

> [!NOTE]
> Los usuarios que no estén permitidos por la directiva para usar el inicio de sesión con el teléfono ya no pueden habilitarla en la aplicación Microsoft Authenticator.  

## <a name="sign-in-with-passwordless-credential"></a>Iniciar sesión con credenciales sin contraseña

Un usuario puede empezar a utilizar el inicio de sesión sin contraseña una vez que un administrador haya habilitado su inquilino y el usuario haya actualizado la aplicación Microsoft Authenticator para habilitar el inicio de sesión en el teléfono.

Para empezar a usar el inicio de sesión con el teléfono, después de escribir un nombre de usuario en la página de inicio de sesión y seleccionar **Siguiente**, es posible que los usuarios tengan que seleccionar **Otras formas de iniciar sesión** y **Aprobar una solicitud en la aplicación Microsoft Authenticator**. Los usuarios verán entonces un número y se les pedirá en su aplicación Microsoft Authenticator que seleccionen el número correcto para autenticarse, en lugar de usar su contraseña. Una vez que los usuarios hayan usado el inicio de sesión en el teléfono sin contraseña, se les pedirá que lo utilicen de nuevo, hasta que usen elija otro método.

![Ejemplo de inicio de sesión en el explorador con la aplicación Microsoft Authenticator](./media/howto-authentication-passwordless-phone/web-sign-in-microsoft-authenticator-app.png)

## <a name="known-issues"></a>Problemas conocidos

La experiencia en versión preliminar actual presenta los siguientes problemas conocidos.

### <a name="not-seeing-option-for-passwordless-phone-sign-in"></a>No se ve la opción para el inicio de sesión con el teléfono sin contraseña.

Si un usuario tiene pendiente una comprobación de inicio de sesión en el teléfono sin contraseña sin contestar e intenta volver a iniciar sesión, es posible que el usuario solo vea una opción para especificar una contraseña. Abra Microsoft Authenticator y responda a las solicitudes de notificación para seguir usando el inicio de sesión con el teléfono sin contraseña.

### <a name="federated-accounts"></a>Cuentas federadas

Cuando un usuario ha habilitado una credencial que no tiene contraseña, los inicios de sesión de Azure AD dejarán de usar login_hint para llevar más rápido al usuario a una ubicación de inicio de sesión federada. Esta lógica evita que los usuarios de un inquilino híbrido se dirijan a AD FS para la comprobación de inicios de sesión sin que el usuario realice un paso adicional para hacer clic en "Use su contraseña en su lugar".

### <a name="azure-mfa-server"></a>Servidor de Azure MFA

Los usuarios finales que están habilitados para MFA mediante un servidor de Azure MFA local de la organización pueden seguir creando y usando una credencial de inicio de sesión único telefónica sin contraseña. Si el usuario intenta actualizar varias instalaciones (más de 5) de Microsoft Authenticator con la credencial, este cambio puede dar lugar a un error.  

### <a name="device-registration"></a>Registro de dispositivos

Uno de los requisitos previos para crear esta nueva credencial segura es que el dispositivo, donde está instalada la aplicación Microsoft Authenticator, también debe estar registrado en el inquilino de Azure AD para un usuario individual. Debido a las actuales restricciones del registro de dispositivos, solo se puede registrar un dispositivo en un inquilino. Este límite significa que solo se puede habilitar una cuenta profesional o educativa en la aplicación Microsoft Authenticator para el inicio de sesión telefónico.

> [!NOTE]
> El registro de dispositivos no es lo mismo que la administración de dispositivos o "MDM". Solo asocia un identificador de dispositivo y un identificador de usuario en el directorio de Azure AD.  

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre la autenticación de Azure AD y los métodos sin contraseña, consulte los siguientes artículos:

* [Obtener información sobre cómo funciona la autenticación con contraseñas](concept-authentication-passwordless.md)
* [Obtenga información sobre el registro de dispositivos](../devices/overview.md#getting-devices-in-azure-ad)
* [Más información sobre Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)
