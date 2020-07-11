---
title: 'Recopilación de datos de usuario de Azure MFA: Azure Active Directory'
description: ¿Qué información se usa para autenticar a los usuarios mediante Azure Multi-Factor Authentication?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f3b5af972ad6dd15b7c992d5e264ede97bd1dde
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "80653631"
---
# <a name="azure-multi-factor-authentication-user-data-collection"></a>Recopilación de datos de usuario de Azure Multi-Factor Authentication

En este documento se explica cómo saber qué información de usuario recopila el Servidor Microsoft Azure Multi-Factor Authentication (servidor MFA) y Azure MFA (basado en la nube) por si quisiera suprimirla.

[!INCLUDE [gdpr-hybrid-note](../../../includes/gdpr-hybrid-note.md)]

## <a name="information-collected"></a>Información recopilada

El servidor MFA, la extensión de NPS y el adaptador Azure MFA AD FS de Windows Server 2016 recopilan y almacenan la información siguiente durante 90 días.

Intentos de autenticación (esta información se utiliza para crear informes y solucionar problemas):

- Timestamp
- Nombre de usuario
- Nombre
- Apellido
- Dirección de correo electrónico
- Grupo de usuarios
- Método de autenticación (llamada de teléfono, mensaje de texto, aplicación móvil o token OATH)
- Modo de llamada de teléfono (estándar, PIN)
- Dirección de los mensajes de texto (unidireccional o bidireccional)
- Modo de mensaje de texto (OTP, OTP + PIN)
- Modo de aplicación móvil (estándar, PIN)
- Modo de token OATH (estándar, PIN)
- Tipo de autenticación
- Nombre de la aplicación
- Código de país principal de la llamada
- Número de teléfono principal de la llamada
- Extensión principal de la llamada
- Autenticación principal de la llamada
- Resultado principal de la llamada
- Código de país de reserva de la llamada
- Número de teléfono de reserva de la llamada
- Extensión de reserva de la llamada
- Autenticación de reserva de la llamada
- Resultado de reserva de la llamada
- Autenticaciones globales
- Resultado global
- Results
- Autenticadas
- Resultado
- Dirección IP de inicio
- Dispositivos
- Token del dispositivo
- Tipo de dispositivo
- Versión de la aplicación móvil
- Versión del SO.
- Resultado
- Comprobación utilizada para la notificación

Activaciones (intentos de activar una cuenta en la aplicación móvil Microsoft Authenticator):
- Nombre de usuario
- Nombre de cuenta
- Timestamp
- Resultado de la obtención del código de activación
- Activación correcta
- Error de activación
- Resultado del estado de activación
- Nombre de dispositivo
- Tipo de dispositivo
- Versión de la aplicación
- Token OATH habilitado

Bloqueos (esta información se utiliza para crear informes y determinar el estado de bloqueo):

- Bloqueo de la marca de tiempo
- Bloqueo por nombre de usuario
- Nombre de usuario
- Código de país
- Número de teléfono
- Número de teléfono con formato
- Extensión
- Extensión limpia
- Bloqueado
- Razón del bloqueo
- Marca de tiempo de finalización
- Resultados de la finalización
- Bloqueo de cuenta
- Alerta de fraude
- Alerta de fraude no bloqueada
- Idioma

Omisiones (usadas para informes):

- Marca de tiempo de la omisión
- Segundos de omisión
- Omisión por nombre de usuario
- Nombre de usuario
- Código de país
- Número de teléfono
- Número de teléfono con formato
- Extensión
- Extensión limpia
- Razón de omisión
- Marca de tiempo de finalización
- Resultados de la finalización
- Omisión utilizada

Cambios (información que se usa para sincronizar los cambios de usuario en el servidor MFA o en Azure AD):

- Marca de tiempo del cambio
- Nombre de usuario
- Nuevo código de país
- Nuevo número de teléfono
- Nueva extensión
- Nuevo código de país de reserva
- Nuevo número de teléfono de reserva
- Nueva extensión de reserva
- Nuevo PIN
- Se requiere cambiar el PIN
- Token antiguo del dispositivo
- Nuevo token del dispositivo

## <a name="gather-data-from-mfa-server"></a>Recopilar datos del servidor MFA

En la versión 8.0 o posteriores del servidor MFA, el siguiente proceso permite a los administradores exportar todos los datos de los usuarios:

- Inicie sesión en el servidor MFA, vaya a la pestaña **Usuarios**, seleccione el usuario en cuestión y haga clic en el botón **Editar**. Realice capturas de pantalla (Alt-Impr Pant) de cada pestaña para proporcionar a los usuarios su configuración actual de MFA.
- Desde la línea de comandos del servidor MFA, ejecute el siguiente comando cambiando la ruta de acceso según su instalación `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe export <username>` para generar un archivo en formato JSON.
- Los administradores también pueden utilizar la operación GetUserGdpr del SDK de servicio web como una opción para exportar toda la información del servicio en la nube MFA recopilada para un usuario determinado o incorporarla en una solución de informes más grande.
- Busque `C:\Program Files\Multi-Factor Authentication Server\Logs\MultiFactorAuthSvc.log` y cualquier copia de seguridad de "\<username>" (incluya las comillas en la búsqueda) para obtener todas las instancias del registro de usuario que se va a agregar o modificar.
   - Estos registros se pueden limitar (pero no eliminar) desactivando **"Registrar cambios de usuarios"** en la UX del servidor MFA, sección Registro, pestaña Archivos de registro.
   - Si se configura Syslog y se marca **"Registrar cambios de usuarios"** en la UX del servidor MFA, sección Registro, pestaña Syslog, las entradas de registro se podrán recopilar desde Syslog.
- Otras apariciones del nombre de usuario en MultiFactorAuthSvc.log y otros archivos de registro del servidor MFA que pertenezcan a intentos de autenticación se considerarán operacionales y que duplican la información proporcionada con la exportación de MultiFactorAuthGdpr.exe o GetUserGdpr del SDK del servicio web.

## <a name="delete-data-from-mfa-server"></a>Eliminar datos del servidor MFA

Desde la línea de comandos del servidor MFA, ejecute el siguiente comando cambiando la ruta de acceso según su instalación `C:\Program Files\Multi-Factor Authentication Server\MultiFactorAuthGdpr.exe delete <username>` para eliminar toda la información del servicio en la nube de MFA recopilada para este usuario.

- Los datos incluidos en la exportación se eliminan en tiempo real, pero los datos duplicados u operativos pueden tardar hasta 30 días en eliminarse completamente.
- Los administradores también pueden utilizar la operación DeleteUserGdpr del SDK del servicio web como una opción para eliminar toda la información del servicio en la nube MFA recopilada para un usuario determinado o incorporarla en una solución de informes más grande.

## <a name="gather-data-from-nps-extension"></a>Recopilar datos de la extensión NPS

Use el [Portal de privacidad de Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para hacer una solicitud de exportación.

- La información de MFA se incluye en la exportación, un proceso que puede tardar horas o días en completarse.
- Las apariciones del nombre de usuario en los registros de eventos AzureMfa/AuthN/AuthNOptCh, AzureMfa/AuthZ/AuthZAdminCh y AzureMfa/AuthZ/AuthZOptCh se consideran información duplicada y operativa a la proporcionada en la exportación.

## <a name="delete-data-from-nps-extension"></a>Eliminar datos de la extensión NPS

Use el [Portal de privacidad de Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para realizar una solicitud de cierre de cuenta y eliminar así toda la información del servicio en la nube MFA recopilada para este usuario.

- Los datos pueden tardar hasta 30 días en eliminarse completamente.

## <a name="gather-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Recopilar datos del adaptador Azure MFA AD FS de Windows Server 2016

Use el [Portal de privacidad de Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para hacer una solicitud de exportación. 

- La información de MFA se incluye en la exportación, un proceso que puede tardar horas o días en completarse.
- Las apariciones del nombre de usuario en los registros de eventos de seguimiento y depuración de AD FS (si la opción está habilitada) se consideran información duplicada y operativa a la proporcionada en la exportación.

## <a name="delete-data-from-windows-server-2016-azure-mfa-ad-fs-adapter"></a>Eliminar datos del adaptador Azure MFA AD FS de Windows Server 2016

Use el [Portal de privacidad de Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para realizar una solicitud de cierre de cuenta y eliminar así toda la información del servicio en la nube MFA recopilada para este usuario.

- Los datos pueden tardar hasta 30 días en eliminarse completamente.

## <a name="gather-data-for-azure-mfa"></a>Recopilar datos para Azure MFA

Use el [Portal de privacidad de Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para hacer una solicitud de exportación.

- La información de MFA se incluye en la exportación, un proceso que puede tardar horas o días en completarse.

## <a name="delete-data-for-azure-mfa"></a>Eliminar datos de Azure MFA

Use el [Portal de privacidad de Microsoft](https://portal.azure.com/#blade/Microsoft_Azure_Policy/UserPrivacyMenuBlade/Overview) para realizar una solicitud de cierre de cuenta y eliminar así toda la información del servicio en la nube MFA recopilada para este usuario.

- Los datos pueden tardar hasta 30 días en eliminarse completamente.

## <a name="next-steps"></a>Pasos siguientes

[Informes del servidor MFA](howto-mfa-reporting.md)
