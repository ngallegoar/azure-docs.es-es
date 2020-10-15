---
title: 'Métodos de autenticación por teléfono: Azure Active Directory'
description: Obtenga información sobre el uso de métodos de autenticación por teléfono en Azure Active Directory para ayudar a mejorar y proteger los eventos de inicio de sesión.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/02/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca6b39852668e1b545ce576ac0d04a208e2058e8
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2020
ms.locfileid: "91965427"
---
# <a name="authentication-methods-in-azure-active-directory---phone-options"></a>Métodos de autenticación en Azure Active Directory: opciones de teléfono

En el caso de la autenticación directa mediante mensajes de texto, puede consultar [Configuración y habilitación de los usuarios para la autenticación basada en SMS mediante Azure Active Directory (versión preliminar)](howto-authentication-sms-signin.md). El inicio de sesión basado en SMS es ideal para los trabajadores de primera línea. Con el inicio de sesión basado en SMS, los usuarios no necesitan conocer un nombre de usuario y una contraseña para acceder a las aplicaciones y servicios. En su lugar, el usuario escribe su número de teléfono móvil registrado, recibe un mensaje de texto con un código de verificación, y escribe el código en la interfaz de inicio de sesión.

Los usuarios también pueden verificarse mediante su teléfono móvil o teléfono de la oficina como forma secundaria de autenticación con Azure Multi-Factor Authentication o el autoservicio de restablecimiento de contraseña (SSPR).

Para funcionar correctamente, los números de teléfono deben tener el formato *+códigoPaís númeroTeléfono*, *por ejemplo: +1 4251234567*.

> [!NOTE]
> Debe haber un espacio entre el código de país/región y el número de teléfono.
>
> El restablecimiento de contraseña no admite extensiones telefónicas. Incluso con el formato *+1 4251234567X12345*, las extensiones se quitan antes de hacer la llamada.

## <a name="mobile-phone-verification"></a>Verificación por teléfono móvil

En el caso de Azure Multi-Factor Authentication o autoservicio de restablecimiento de contraseña, los usuarios pueden elegir recibir un mensaje de texto con un código de verificación para acceder a la interfaz de inicio de sesión, o recibir una llamada de teléfono con una notificación para escribir su código PIN definido.

Si los usuarios no quieren que su número de teléfono móvil sea visible en el directorio, pero quieren usarlo para restablecer la contraseña, los administradores no deben rellenar el número de teléfono en el directorio. En su lugar, los usuarios deben rellenar el atributo **Teléfono de autenticación** mediante el registro de información de seguridad combinado en [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo). Los administradores pueden ver esta información en el perfil del usuario, pero no se publica en ningún otro lugar.

![Captura de pantalla de Azure Portal, que muestra los métodos de autenticación con un número de teléfono rellenado](media/concept-authentication-methods/user-authentication-methods.png)

Microsoft no garantiza la entrega rápida y coherente de Azure Multi-Factor Authentication por SMS o llamada de voz en el mismo número. Por el interés de los usuarios, podemos agregar o quitar códigos cortos cuando lo estimemos oportuno en tanto realicemos ajustes de enrutamiento para mejorar la capacidad de entrega de SMS. Microsoft no admite códigos cortos para países o regiones que no sean Estados Unidos y Canadá.

### <a name="text-message-verification"></a>Verificación por mensaje de texto

Con la verificación por mensajes de texto durante el autoservicio de restablecimiento de contraseña o Azure Multi-Factor Authentication, se envía un SMS con un código de verificación al número de teléfono móvil. Para completar el proceso de inicio de sesión, el código de verificación entregado debe introducirse en la interfaz de inicio de sesión.

### <a name="phone-call-verification"></a>Verificación por llamada telefónica

Con la verificación por llamada telefónica durante el autoservicio de restablecimiento de contraseña o Azure Multi-Factor Authentication, se hace una llamada de voz automatizada al número de teléfono registrado por el usuario. Para completar el proceso de inicio de sesión, se pide al usuario que escriba el número PIN, seguido de # en el teclado.

## <a name="office-phone-verification"></a>Verificación por teléfono de la oficina

El administrador de Azure AD administra el atributo de teléfono de la oficina, que no puede ser registrado por el usuario mismo.

Con la verificación por llamada telefónica durante el autoservicio de restablecimiento de contraseña o Azure Multi-Factor Authentication, se hace una llamada de voz automatizada al número de teléfono registrado por el usuario. Para completar el proceso de inicio de sesión, se pide al usuario que escriba el número PIN, seguido de # en el teclado.

## <a name="troubleshooting-phone-options"></a>Solución de problemas de las opciones de teléfono

Si tiene problemas con la autenticación telefónica en Azure AD, revise los siguientes pasos de solución de problemas:

* Identificador de llamada bloqueado en un solo dispositivo.
   * Revise los números bloqueados configurados en el dispositivo.
* Número de teléfono incorrecto o código de país o región incorrecto, o confusión entre el número telefónico personal y el número telefónico del trabajo.
   * Solución de problemas de objeto de usuario y métodos de autenticación configurados. Asegúrese de haber registrado los números de teléfono correctos.
* El PIN especificado es incorrecto.
   * Confirme que el usuario ha usado el PIN correcto, según está registrado para su cuenta.
* La llamada se desvió al correo de voz.
   * Asegúrese de que el usuario tiene encendido el teléfono y que el servicio está disponible en su área, o use un método alternativo.
* El usuario está bloqueado
   * Solicite al administrador de Azure AD que desbloquee el usuario en Azure Portal.
* No se han suscrito los SMS en el dispositivo.
   * Solicite al usuario que cambie los métodos o active los SMS en el dispositivo.
* Problemas con los proveedores de telecomunicaciones, por ejemplo: no se detecta entrada de teléfono, incidencias sobre la falta de tonos DTMF, identificador de llamada bloqueado en varios dispositivos o SMS bloqueados en varios dispositivos.
   * Microsoft usa varios proveedores de telecomunicaciones con el fin de enrutar las llamadas telefónicas y los mensajes SMS para la autenticación. Si observa alguno de los problemas anteriores, solicite al usuario que intente usar el método al menos cinco veces en 5 minutos, y tenga la información del usuario disponible cuando se ponga en contacto con el soporte técnico de Microsoft.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar, consulte el [tutorial del autoservicio de restablecimiento de contraseña][tutorial-sspr] y [Azure Multi-Factor Authentication][tutorial-azure-mfa].

Para obtener más información sobre los conceptos del SSPR, consulte [Funcionamiento: Autoservicio de restablecimiento de contraseña de Azure AD][concept-sspr].

Para obtener más información sobre los conceptos de MFA, consulte [Funcionamiento: Azure Multi-Factor Authentication][concept-mfa].

Más información sobre la configuración de métodos de autenticación con la [Versión beta de la API REST Microsoft Graph API](/graph/api/resources/authenticationmethods-overview?view=graph-rest-beta).

<!-- INTERNAL LINKS -->
[tutorial-sspr]: tutorial-enable-sspr.md
[tutorial-azure-mfa]: tutorial-enable-azure-mfa.md
[concept-sspr]: concept-sspr-howitworks.md
[concept-mfa]: concept-mfa-howitworks.md
