---
title: Informes de acceso y uso para Azure MFA con Azure Active Directory
description: Aquí se describe cómo utilizar la característica de informes de Azure Multi-Factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/30/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5d5354f5bca7a4c9ab00066167ad19890536629
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653622"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Informes en Azure Multi-Factor Authentication

Azure Multi-Factor Authentication proporciona varios tipos de informes que usted o su organización pueden usar a través de Azure Portal. En la tabla siguiente se muestran los informes disponibles:

| Informe | Location | Descripción |
|:--- |:--- |:--- |
| Historial de usuarios bloqueados | Azure AD > Seguridad > MFA > Bloquear y desbloquear usuarios | Muestra el historial de solicitudes para bloquear o desbloquear usuarios. |
| Alertas de fraude y de uso | Azure AD > Inicios de sesión | Proporciona información sobre el uso general, el resumen del usuario, detalles del usuario; así como un historial de alertas de fraude enviadas durante el intervalo de fechas especificado. |
| Uso de componentes locales | Azure AD > Seguridad > MFA > Informe de actividad | Proporciona información sobre el uso general de MFA a través de la extensión NPS, AD FS y el servidor MFA. |
| Historial de usuarios omitidos | Azure AD > Seguridad > MFA > Omisión por única vez | Proporciona un historial de solicitudes para omitir Multi-Factor Authentication para un usuario. |
| Estado del servidor | Azure AD > Seguridad > MFA > Estado del servidor | Muestra el estado de los servidores de Multi-Factor Authentication asociados a su cuenta. |

## <a name="view-mfa-reports"></a>Visualización de informes de MFA

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. En la parte izquierda, seleccione **Azure Active Directory** > **Seguridad** > **MFA**.
3. Seleccione el informe que desee ver.

   ![Informe de estado del Servidor MFA en Azure Portal](./media/howto-mfa-reporting/report.png)

## <a name="azure-ad-sign-ins-report"></a>Informe de inicios de sesión de Azure AD

Con el **informe de actividad de inicios de sesión** de [Azure Portal](https://portal.azure.com), se puede obtener toda la información necesaria para determinar cómo marcha el entorno.

El informe de inicios de sesión puede proporcionarle información acerca del uso de las aplicaciones administradas y actividades de inicio de sesión de usuario, lo que incluye información acerca del uso de la autenticación multifactor (MFA). Los datos MFA le proporcionan información acerca del funcionamiento de MFA en su organización, lo que le permite responder preguntas como estas:

- ¿Supuso un problema la MFA para el inicio de sesión?
- ¿Cómo completo el usuario la MFA?
- ¿Por qué no pudo completar el usuario la MFA?
- ¿En cuántos usuarios se usa MFA?
- ¿Cuántos usuarios no pueden completar el desafío de MFA?
- ¿Cuáles son los problemas de MFA más habituales a los que se enfrentan los usuarios finales?

Estos datos están disponibles a través de [Azure Portal](https://portal.azure.com) y la [API de informes](../reports-monitoring/concept-reporting-api.md).

![Informe de inicios de sesión de Azure AD en Azure Portal](./media/howto-mfa-reporting/sign-in-report.png)

### <a name="sign-ins-report-structure"></a>Estructura del informe de inicios de sesión

Los informes de actividades de inicio de sesión referentes a la MFA le proporcionan acceso a la siguiente información:

**MFA necesaria:** si MFA es necesaria para el inicio de sesión, o no. MFA puede ser necesaria debido a la MFA por usuario, al acceso condicional o a otras razones. Los valores posibles son **Yes** o **No**.

**Resultado de MFA:** más información acerca de si la MFA se ha cumplido o denegado:

- Si se ha cumplido, esta columna proporciona más información acerca de cómo se ha hecho.
   - Azure Multi-Factor Authentication
      - completado en la nube
      - ha expirado debido a las directivas configuradas en el inquilino
      - registro solicitado
      - satisfecho por notificación en el token
      - satisfecho por notificación de proveedor externo
      - satisfecho por autenticación segura
      - se omite, ya que el flujo usado fue el flujo de inicio de sesión del agente de Windows
      - se omite debido a la aplicación de la contraseña
      - se omite debido a la ubicación
      - se omite debido al dispositivo registrado
      - se omite debido al dispositivo recordado
      - completado correctamente
   - Redireccionado a un para la autenticación multifactor

- Si se ha denegado, esta columna proporcionaría el motivo de la denegación.
   - Azure Multi-Factor Authentication denegado;
      - autenticación en curso
      - intento de duplicación de autenticación
      - se ha escribir un código incorrecto demasiadas veces
      - autenticación no válida
      - código de verificación de aplicación móvil no válido
      - error de configuración
      - la llamada de teléfono se dirigió al correo de voz
      - el número de teléfono tiene un formato no válido
      - error del servicio
      - no se puede acceder al teléfono del usuario
      - no se puede enviar la notificación de la aplicación móvil al dispositivo
      - no se puede enviar la notificación de la aplicación móvil
      - el usuario rechazó la autenticación
      - el usuario no respondió a la notificación de la aplicación móvil
      - el usuario no tiene ningún método de comprobación registrado
      - el usuario ha escrito un código incorrecto
      - el usuario ha escrito un PIN incorrecto
      - el usuario contestó la llamada de teléfono sin la autenticación
      - el usuario está bloqueado
      - el usuario no ha escrito el código de verificación
      - el usuario no se encuentra
      - el código de verificación ya se ha usado una vez

**Método de autenticación de MFA:** el método de autenticación que el usuario ha utilizado para completar la MFA. Los valores posibles son:

- mensaje de texto
- Notificación en aplicación móvil
- Llamada de teléfono (teléfono de autenticación)
- Código de verificación de la aplicación móvil
- Llamada de teléfono (teléfono profesional)
- Llamada de teléfono (teléfono de autenticación alternativo)

**Detalles de la autenticación de MFA:** versión limpia del número de teléfono, por ejemplo: + X XXXXXXXX64.

**Acceso condicional:** busque información sobre las directivas de acceso condicional que afectan al intento de inicio de sesión, lo que incluye:

- Nombre de la directiva
- Controles de concesión
- Controles de sesión
- Resultado

## <a name="powershell-reporting-on-users-registered-for-mfa"></a>Creación de informes de PowerShell sobre los usuarios registrados para MFA

En primer lugar, asegúrese de que tiene el [módulo de PowerShell MSOnline V1](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0) instalado.

Identifique los usuarios que se han registrado en MFA mediante el PowerShell siguiente. Este conjunto de comandos excluye los usuarios deshabilitados, ya que estas cuentas no se pueden autenticar en Azure AD.

```Get-MsolUser -All | Where-Object {$.StrongAuthenticationMethods -ne $null -and $.BlockCredential -eq $False} | Select-Object -Property UserPrincipalName```

Identifique los usuarios que no se han registrado en MFA mediante el PowerShell siguiente. Este conjunto de comandos excluye los usuarios deshabilitados, ya que estas cuentas no se pueden autenticar en Azure AD.

```Get-MsolUser -All | Where-Object {$.StrongAuthenticationMethods.Count -eq 0 -and $.BlockCredential -eq $False} | Select-Object -Property UserPrincipalName```

Identifique los usuarios y los métodos de salida registrados. 

```PowerShell
Get-MsolUser -All | Select-Object @{N='UserPrincipalName';E={$_.UserPrincipalName}},

@{N='MFA Status';E={if ($_.StrongAuthenticationRequirements.State){$_.StrongAuthenticationRequirements.State} else {"Disabled"}}},

@{N='MFA Methods';E={$_.StrongAuthenticationMethods.methodtype}} | Export-Csv -Path c:\MFA_Report.csv -NoTypeInformation
```

## <a name="possible-results-in-activity-reports"></a>Posibles resultados de los informes de actividad

La siguiente tabla puede utilizarse para solucionar problemas de autenticación multifactor mediante la versión descargada del informe de actividad de autenticación multifactor. No aparecerá directamente en Azure Portal.

| Resultado de la llamada | Descripción | Descripción amplia |
| --- | --- | --- |
| SUCCESS_WITH_PIN | PIN especificado | El usuario ha especificado un PIN.  Si la autenticación se realizó correctamente, significa que ha escrito el PIN correcto.  Si se deniega la autenticación, ha escrito un PIN incorrecto o el usuario está establecido en modo estándar. |
| SUCCESS_NO_PIN | Solo # especificado | Si el usuario está establecido en modo PIN y se deniega la autenticación, esto significa que el usuario no ha escrito el PIN y solo ha escrito el carácter #.  Si el usuario está establecido en modo estándar y la autenticación se ha realizado correctamente, esto significa que el usuario solo ha escrito el carácter #, que es lo que se debe hacer en modo estándar. |
| SUCCESS_WITH_PIN_BUT_TIMEOUT | # no presionado tras la entrada | El usuario no envió los dígitos DTMF porque no se especificó #.  Los otros dígitos especificados no se han enviado a menos que se presione # para indicar la finalización de la entrada. |
|SUCCESS_NO_PIN_BUT_TIMEOUT | Sin entrada de teléfono - Tiempo de espera agotado | Se respondió la llamada, pero no hubo respuesta.  Esto típicamente indica que la llamada la ha contestado el buzón de voz. |
| SUCCESS_PIN_EXPIRED | PIN expirado y no cambiado | El PIN del usuario ha expirado y se le ha pedido que lo cambie, pero el cambio de PIN no se ha completado correctamente. |
| SUCCESS_USED_CACHE | Caché usada | La autenticación se realizó correctamente sin una llamada a Multi-Factor Authentication, ya que se produjo una autenticación previa correcta para el mismo nombre de usuario dentro del período de caché configurado. |
| SUCCESS_BYPASSED_AUTH | Autenticación omitida | La autenticación se realizó correctamente mediante la omisión por única vez iniciada por el usuario.  Vea el informe del historial de usuarios omitidos para obtener más detalles sobre la omisión. |
| SUCCESS_USED_IP_BASED_CACHE | Caché usada basada en IP | La autenticación se realizó correctamente sin una llamada a Multi-Factor Authentication, ya que se produjo una autenticación previa correcta para el mismo nombre de usuario, nombre de la aplicación y dirección IP dentro del período de caché configurado. |
| SUCCESS_USED_APP_BASED_CACHE | Caché usada basada en la aplicación | La autenticación se realizó correctamente sin una llamada a Multi-Factor Authentication, ya que se produjo una autenticación previa correcta para el mismo nombre de usuario y nombre de la aplicación dentro del período de caché configurado. |
| SUCCESS_INVALID_INPUT | Entrada de teléfono no válido | La respuesta enviada desde el teléfono no es válida.  Podría tratarse de una máquina de fax o módem o quizá que el usuario haya escrito * como parte de su PIN. |
| SUCCESS_USER_BLOCKED | Usuario bloqueado | El número de teléfono del usuario está bloqueado.  El usuario puede iniciar un número bloqueado durante una llamada de autenticación o un administrador mediante Azure Portal. <br> NOTA:   El número de bloqueo también es una característica de la alerta de fraude. |
| SUCCESS_SMS_AUTHENTICATED | Mensaje de texto autenticado | Para el mensaje de prueba bidireccional, el usuario ha respondido correctamente con su código de acceso de un solo uso (OTP) o bien OTP más el PIN. |
| SUCCESS_SMS_SENT | Mensaje de texto enviado | En el caso de los mensajes de texto, el mensaje de texto que contiene el código de acceso de un solo uso (OTP) se envió correctamente.  El usuario especificará el OTP o el OTP y el PIN en la aplicación para completar la autenticación. |
| SUCCESS_PHONE_APP_AUTHENTICATED | Aplicación móvil autenticada | El usuario que se autenticó correctamente mediante la aplicación móvil. |
| SUCCESS_OATH_CODE_PENDING | Código OATH pendiente | Al usuario se le pidió el código OATH pero no respondió. |
| SUCCESS_OATH_CODE_VERIFIED | Código OATH verificado | El usuario ha especificado un código OATH válido cuando se le solicita. |
| SUCCESS_FALLBACK_OATH_CODE_VERIFIED | Código OATH de reserva verificado | Al usuario se le denegó la autenticación con su método principal de Multi-Factor Authentication y luego se le proporcionó un código OATH válido para la reserva. |
| SUCCESS_FALLBACK_SECURITY_QUESTIONS_ANSWERED | Preguntas de seguridad de reserva contestadas | Al usuario se le denegó la autenticación al utilizar su método principal de Multi-Factor Authentication y luego respondió sus preguntas de seguridad correctamente para la reserva. |
| FAILED_PHONE_BUSY | Autenticación en curso | Multi-Factor Authentication ya está procesando una autenticación para este usuario.  Esto es causado a menudo por clientes RADIUS que envían múltiples solicitudes de autenticación durante el mismo inicio de sesión. |
| CONFIG_ISSUE | Teléfono inaccesible | Se intentó realizar una llamada, pero no se pudo realizar o no se respondió.  Esto incluye la señal de ocupado, la señal de ocupado rápida (desconectado), tri-tonos (número que ya no está en servicio), agotamiento del tiempo de espera mientras suena, etc. |
| FAILED_INVALID_PHONENUMBER | Formato de número de teléfono no válido | El número de teléfono tiene un formato no válido.  Los números de teléfono deben ser numéricos y deben tener 10 dígitos para el código de país +1 (Estados Unidos y Canadá). |
| FAILED_USER_HUNGUP_ON_US | El usuario colgó el teléfono | El usuario contestó el teléfono, pero luego colgó sin presionar ningún botón. |
| FAILED_INVALID_EXTENSION | Extensión no válida | La extensión contiene caracteres no válidos.  Solo se permiten dígitos, comas, * y #.  También se puede usar el prefijo @. |
| FAILED_FRAUD_CODE_ENTERED | Código de fraude especificado | El usuario optó por notificar el fraude durante la llamada, lo que dio como resultado una autenticación denegada y un número de teléfono bloqueado.| 
| FAILED_SERVER_ERROR | No se puede realizar la llamada | El servicio Multi-Factor Authentication no pudo realizar la llamada. |
| FAILED_SMS_NOT_SENT | No se pudo enviar el mensaje de texto | No se ha podido enviar el mensaje de texto.  Se deniega la autenticación. |
| FAILED_SMS_OTP_INCORRECT | OTP de mensaje de texto incorrecto | El usuario especificó un código de acceso de un solo uso (OTP) incorrecto del mensaje de texto que recibió.  Se deniega la autenticación. |
| FAILED_SMS_OTP_PIN_INCORRECT | OTP + PIN de mensaje de texto incorrectos | El usuario especificó un código de acceso un solo uso (OTP) o un PIN de usuario incorrectos.  Se deniega la autenticación. |
| FAILED_SMS_MAX_OTP_RETRY_REACHED | Superó el número máximo de intentos de OTP de mensajes de texto | El usuario ha superado el número máximo de intentos de código de acceso de un solo uso (OTP). |
| FAILED_PHONE_APP_DENIED | Aplicación móvil denegada | El usuario denegó la autenticación en la aplicación móvil al presionar el botón Denegar. |
| FAILED_PHONE_APP_INVALID_PIN | PIN de aplicación móvil no válido | El usuario especificó un PIN no válido al autenticarse en la aplicación móvil. |
| FAILED_PHONE_APP_PIN_NOT_CHANGED | PIN de aplicación móvil no cambiado | El usuario no completó correctamente un cambio de PIN necesario en la aplicación móvil. |
| FAILED_FRAUD_REPORTED | Fraude notificado | El usuario notificó un fraude en la aplicación móvil. |
| FAILED_PHONE_APP_NO_RESPONSE | Sin respuesta de aplicación móvil | El usuario no respondió a la solicitud de autenticación de la aplicación móvil. |
| FAILED_PHONE_APP_ALL_DEVICES_BLOCKED | Todos los dispositivos de aplicación móvil bloqueados | Los dispositivos de aplicación móvil para este usuario ya no responden a las notificaciones y se han bloqueado. |
| FAILED_PHONE_APP_NOTIFICATION_FAILED | Error de notificación de aplicación móvil | Se produjo un error al intentar enviar una notificación a la aplicación móvil en el dispositivo del usuario. |
| FAILED_PHONE_APP_INVALID_RESULT | Resultado de aplicación móvil no válido | La aplicación móvil devolvió un resultado no válido. |
| FAILED_OATH_CODE_INCORRECT | Código OATH incorrecto | El usuario especificó un código OATH incorrecto.  Se deniega la autenticación. |
| FAILED_OATH_CODE_PIN_INCORRECT | Código OATH y PIN incorrectos | El usuario especificó un código OATH o un PIN de usuario incorrectos.  Se deniega la autenticación. |
| FAILED_OATH_CODE_DUPLICATE | Código OATH duplicado | El usuario especificó un código OATH que se había utilizado previamente.  Se deniega la autenticación. |
| FAILED_OATH_CODE_OLD | Código OATH no actualizado | El usuario especificó un código OATH que precede a un código OATH utilizado previamente.  Se deniega la autenticación. |
| FAILED_OATH_TOKEN_TIMEOUT | Tiempo de espera de resultado de código OATH | El usuario tardó demasiado en especificar el código OATH y el intento de Multi-Factor Authentication ya se había agotado. |
| FAILED_SECURITY_QUESTIONS_TIMEOUT | Tiempo de espera de resultado de preguntas de seguridad | El usuario tardó demasiado en escribir la respuesta a las preguntas de seguridad y el intento de Multi-Factor Authentication ya se ha agotado. |
| FAILED_AUTH_RESULT_TIMEOUT | Tiempo de espera de resultado de autenticación | El usuario tardó demasiado en completar el intento de Multi-Factor Authentication. |
| FAILED_AUTHENTICATION_THROTTLED | Autenticación limitada | El servicio ha limitado el intento de Multi-Factor Authentication. |

## <a name="next-steps"></a>Pasos siguientes

* [Uso de SSPR y MFA, y creación de informes de información](howto-authentication-methods-usage-insights.md)
* [Para los usuarios](../user-help/multi-factor-authentication-end-user.md)
* [Lugar de implementación](concept-mfa-whichversion.md)
