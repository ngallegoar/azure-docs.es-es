---
title: 'Métodos de autenticación: Azure Active Directory'
description: Métodos de autenticación disponibles en Azure AD para MFA y SSPR
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry, michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5a82c69575e82a7cf397955f08c3f114e449ba6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78968771"
---
# <a name="what-are-authentication-methods"></a>¿Qué son los métodos de autenticación?

Como administrador, para la elección de los métodos de autenticación para Azure Multi-factor Authentication y autoservicio de restablecimiento de contraseña (SSPR), se recomienda requerir a los usuarios que registren varios métodos de autenticación. Cuando un método de autenticación no está disponible para un usuario, puede autenticarse con otro método.

Los administradores pueden definir en la directiva qué métodos de autenticación están disponibles para los usuarios de SSPR y MFA. Algunos métodos de autenticación pueden no estar disponibles para todas las características. Para obtener más información acerca de cómo configurar las directivas, vea los artículos [Cómo implantar correctamente el autoservicio de restablecimiento de contraseña](howto-sspr-deployment.md) y [Planificar una autenticación multifactor basada en la nube](howto-mfa-getstarted.md).

Microsoft recomienda encarecidamente a los administradores que permitan a los usuarios seleccionar más del número mínimo requerido de métodos de autenticación en caso de que no tengan acceso a uno.

|Método de autenticación|Uso|
| --- | --- |
| Contraseña | MFA y SSPR |
| Preguntas de seguridad | Solo SSPR |
| Dirección de correo electrónico | Solo SSPR |
| Aplicación Microsoft Authenticator | MFA y SSPR |
| Token de hardware OATH | Versión preliminar pública de MFA y SSPR |
| sms | MFA y SSPR |
| Llamada de voz | MFA y SSPR |
| Contraseñas de aplicación | MFA solo en determinados casos |

![Métodos de autenticación en uso en la pantalla de inicio de sesión](media/concept-authentication-methods/overview-login.png)

|     |
| --- |
| Los tokens OATH de hardware para MFA y SSPR son características en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="password"></a>Contraseña

La contraseña de Azure AD se considera un método de autenticación. Se trata de un método que **no se puede deshabilitar**.

## <a name="security-questions"></a>Preguntas de seguridad

Las preguntas de seguridad están disponibles **solo en el autoservicio de restablecimiento de contraseña de Azure AD** para las cuentas que no sean de administrador.

Si usa preguntas de seguridad, le recomendamos que las use junto con otro método. Las preguntas de seguridad pueden ser menos seguras que otros métodos porque es posible que algunas personas sepan las respuestas de las preguntas de otro usuario.

> [!NOTE]
> Las preguntas de seguridad se almacenan de manera privada y segura en un objeto de usuario del directorio y solo las pueden responder los usuarios durante el registro. No hay forma de que un administrador lea o modifique las preguntas o respuestas de un usuario.
>

### <a name="predefined-questions"></a>Preguntas predefinidas

* ¿En qué ciudad conoció a su cónyuge o pareja?
* ¿En qué ciudad se conocieron sus padres?
* ¿En qué ciudad vive su hermano más próximo?
* ¿En qué ciudad nació su padre?
* ¿En qué ciudad tuvo su primer trabajo?
* ¿En qué ciudad nació su madre?
* ¿En qué ciudad estaba en la Nochevieja del año 2000?
* ¿Cuál es el apellido de su profesor favorito del instituto?
* ¿En qué universidad solicitó plaza pero no asistió?
* ¿Cómo se llama el lugar en el que celebró su primer matrimonio?
* ¿Cuál es el segundo apellido de su padre?
* ¿Cuál es su comida favorita?
* ¿Cuál es el nombre y apellido de su abuela materna?
* ¿Cuál es el segundo apellido de su madre?
* ¿Cuáles son el año y mes de nacimiento del mayor de sus hermanos? (por ejemplo, noviembre de 1985)
* ¿Cuál es el segundo apellido del mayor de sus hermanos?
* ¿Cuál es el nombre y apellido de su abuelo paterno?
* ¿Cuántos años se lleva con el menor de sus hermanos?
* ¿En qué escuela cursó el sexto curso?
* ¿Cuál era el nombre y apellido de su mejor amigo de la infancia?
* ¿Cuál era el nombre y apellido de su primera pareja?
* ¿Cuál era el nombre de su maestro de primaria favorito?
* ¿Cuál era la marca y modelo de su primer coche o moto?
* ¿Cómo se llamaba la primera escuela a la que asistió?
* ¿Cómo se llamaba el hospital en el que nació?
* ¿Cuál era la calle de su primera casa de la infancia?
* ¿Cuál era el nombre de su héroe de la infancia?
* ¿Cuál era el nombre de su peluche favorito?
* ¿Cuál era el nombre de su primera mascota?
* ¿Cuál era su apodo en la infancia?
* ¿Cuál era su deporte favorito en el instituto?
* ¿Cuál fue su primer trabajo?
* ¿Cuáles eran los cuatro últimos números de su teléfono de la infancia?
* Cuando era joven, ¿qué quería ser de mayor?
* ¿Cuál es la persona más famosa que ha conocido?

Todas las preguntas de seguridad predefinidas están traducidas y localizadas en el conjunto completo de idiomas de Office 365 en función de la configuración regional del explorador del usuario.

### <a name="custom-security-questions"></a>Preguntas de seguridad personalizadas

Las preguntas de seguridad personalizadas no se localizan. Todas las preguntas personalizadas se muestran en el mismo idioma en el que se escriben en la interfaz de usuario administrativa, aunque la configuración regional del explorador del usuario sea diferente. Si necesita preguntas localizadas, debería usar las preguntas predefinidas.

La longitud máxima de una pregunta de seguridad personalizada es 200 caracteres.

### <a name="security-question-requirements"></a>Requisitos de las preguntas de seguridad

* El límite mínimo de caracteres de las respuestas es de tres.
* El límite máximo de caracteres de las respuestas es de 40.
* Los usuarios no pueden responder a la misma pregunta más de una vez.
* Los usuarios no pueden proporcionar la misma respuesta a más de una pregunta.
* Se puede usar cualquier juego de caracteres para definir las preguntas y respuestas, incluidos los caracteres Unicode.
* El número de preguntas que se definen debe ser mayor o igual que el número de preguntas que fueron necesarias para registrarse.

## <a name="email-address"></a>Dirección de correo electrónico

La dirección de correo electrónico está disponible **solo en el autoservicio de restablecimiento de contraseña de Azure AD**.

Microsoft recomienda usar una cuenta de correo electrónico que no requiera la contraseña de Azure AD del usuario para acceder.

## <a name="microsoft-authenticator-app"></a>Aplicación Microsoft Authenticator

La aplicación Microsoft Authenticator proporciona un nivel de seguridad adicional para su cuenta profesional o educativa de Azure AD o su cuenta Microsoft.

La aplicación Microsoft Authenticator está disponible para [Android](https://go.microsoft.com/fwlink/?linkid=866594), [iOS](https://go.microsoft.com/fwlink/?linkid=866594) y [Windows Phone](https://www.microsoft.com/p/microsoft-authenticator/9nblgggzmcj6).

> [!NOTE]
> Los usuarios no tendrán la opción de registrar su aplicación móvil cuando se registren en el autoservicio de restablecimiento de contraseña. En su lugar, los usuarios pueden registrar su aplicación móvil en [https://aka.ms/mfasetup](https://aka.ms/mfasetup) o en la versión preliminar del registro de información de seguridad en [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo).
>

### <a name="notification-through-mobile-app"></a>Notificación a través de aplicación móvil

La aplicación Microsoft Authenticator puede ayudar a impedir el acceso no autorizado a las cuentas y detener las transacciones fraudulentas mediante el envío de una notificación al smartphone o a la tableta. Los usuarios ven la notificación y, si es legítima, seleccionan Comprobar. De lo contrario, pueden seleccionar Denegar.

> [!WARNING]
> Para el autoservicio de restablecimiento de contraseña cuando se requiere solo un método para el restablecimiento, el código de verificación es la única opción disponible para los usuarios **para garantizar el máximo nivel de seguridad**.
>
> Si se requieren dos métodos, los usuarios podrán realizar el restablecimiento con la notificación **EITHER** **O** con el código de verificación, además de con cualquier otro método habilitado.
>

Si habilita el uso de la notificación a través de aplicación móvil y del código de verificación de aplicación móvil, los usuarios que registren la aplicación Microsoft Authenticator con una notificación podrán usar tanto la notificación como el código para comprobar su identidad.

> [!NOTE]
> Si su organización tiene personal que trabaja en China o que va a viajar allí, el método **notificación a través de aplicación móvil** en **dispositivos Android** no funciona en ese país. Para esos usuarios tiene que haber métodos alternativos disponibles.

### <a name="verification-code-from-mobile-app"></a>Código de verificación desde aplicación móvil

La aplicación Microsoft Authenticator u otras aplicaciones de terceros pueden utilizarse como un token de software para generar un código de verificación de OATH. Después de escribir el nombre de usuario y la contraseña, especifique el código que facilita la aplicación en la pantalla de inicio de sesión. El código de verificación es una forma adicional de autenticación.

> [!WARNING]
> Para el autoservicio de restablecimiento de contraseña cuando se requiere solo un método para el restablecimiento, el código de verificación es la única opción disponible para los usuarios **para garantizar el máximo nivel de seguridad**.
>

Los usuarios pueden tener una combinación de hasta cinco tokens de hardware de OATH o aplicaciones de autenticación, como la aplicación Microsoft Authenticator, configurada para utilizarse en cualquier momento.

## <a name="oath-hardware-tokens-public-preview"></a>Tokens de hardware OATH (versión preliminar pública)

OATH es un estándar abierto que especifica cómo se general los códigos de contraseña de un solo uso (OTP). Azure AD admitirá el uso de tokens OATH-TOTP SHA-1 de la variedad de 30 o 60 segundos. Los clientes pueden adquirir estos tokens a través del proveedor que elijan. Las claves secretas se limitan a 128 caracteres lo que puede no ser compatible con todos los tokens. La clave secreta solo puede contener los caracteres *a-z* o *A-Z* y los dígitos *1-7* y deben estar codificados en base 32.

![Carga de tokens OATH en la hoja de tokens OATH de MFA](media/concept-authentication-methods/mfa-server-oath-tokens-azure-ad.png)

Los tokens de hardware OATH se admiten como parte de una versión preliminar pública. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

Una vez que se adquieren los tokens, se deben cargar en un formato de archivo de valores separados por comas (CSV), incluidos los valores de UPN, número de serie, clave secreta, intervalo de tiempo, fabricante y modelo, como se muestra en el ejemplo siguiente.

```csv
upn,serial number,secret key,time interval,manufacturer,model
Helga@contoso.com,1234567,1234567abcdef1234567abcdef,60,Contoso,HardwareKey
```

> [!NOTE]
> Asegúrese de incluir la fila de encabezado en el archivo CSV.

Una vez formateado correctamente como un archivo CSV, el administrador puede iniciar sesión en Azure Portal y desplazarse a **Azure Active Directory** > **Seguridad** > **MFA** > **Tokens OATH** y cargar el archivo CSV resultante.

En función del tamaño del archivo CSV, puede tardar unos minutos en procesarse. Haga clic en el botón **Actualizar** para ver el estado actual. Si hay algún error en el archivo, tendrá la opción de descargar un archivo CSV con los errores para poder resolverlos. Los nombres de campo del archivo CSV descargado son diferentes de los de la versión cargada.

Una vez solucionados los errores, para activar las claves, el administrador puede hacer clic en la opción **Activar** del token que debe activarse y escribir la OTP que se muestra en el token.

Los usuarios pueden tener una combinación de hasta cinco tokens de hardware de OATH o aplicaciones de autenticación, como la aplicación Microsoft Authenticator, configurada para utilizarse en cualquier momento.

## <a name="phone-options"></a>Opciones de teléfono

### <a name="mobile-phone"></a>Teléfono móvil

Hay dos opciones disponibles para los usuarios con teléfonos móviles.

Si los usuarios no quieren que su número de teléfono móvil sea visible en el directorio, pero quieren seguir usándolo para restablecer la contraseña, los administradores no deben rellenar este campo en el directorio. Los usuarios deberían rellenar el atributo **Teléfono de autenticación** mediante el [portal de registro del restablecimiento de contraseña](https://aka.ms/ssprsetup). Los administradores pueden ver esta información en el perfil del usuario, pero no se publica en ningún otro lugar.

Para funcionar correctamente, los números de teléfono deben tener el formato *+códigoPaís númeroTeléfono* (ejemplo: +1 4255551234).

> [!NOTE]
> Debe haber un espacio entre el código de país y el número de teléfono.
>
> El restablecimiento de contraseña no admite extensiones telefónicas. Incluso con el formato +1 4255551234X12345, las extensiones se quitan antes de realizarse la llamada.

Microsoft no garantiza la entrega rápida y coherente de Multi-Factor Authentication por SMS o llamada de voz en el mismo número. Por el interés de los usuarios, Microsoft puede agregar o quitar códigos cortos cuando lo estime oportuno, ya que se realizan ajustes de enrutamiento para mejorar la capacidad de entrega de SMS. Microsoft no admite códigos cortos para países o regiones que no sean Estados Unidos y Canadá.

#### <a name="text-message"></a>mensaje de texto

Se envía un SMS al número de teléfono móvil con un código de verificación. Escriba el código de verificación proporcionado en la interfaz de inicio de sesión para continuar.

#### <a name="phone-call"></a>llamada de teléfono

Se realiza una llamada de voz automática al número de teléfono que proporcione. El usuario responde a la llamada y pulsa # en el teclado del teléfono para autenticarse.

> [!IMPORTANT]
> A partir de marzo de 2019, las opciones de llamada de teléfono no estarán disponibles para los usuarios de MFA y SSPR en inquilinos de Azure AD gratis o de evaluación. Los mensajes SMS no se ven afectados por este cambio. Las llamadas de teléfono seguirán estando disponibles para los usuarios de inquilinos de Azure AD de pago. Este cambio solo afecta a los inquilinos de Azure AD gratis o de evaluación.

### <a name="office-phone"></a>Teléfono del trabajo

Se realiza una llamada de voz automática al número de teléfono que proporcione. El usuario responde a la llamada y pulsa # en el teclado del teléfono para autenticarse.

Para funcionar correctamente, los números de teléfono deben tener el formato *+códigoPaís númeroTeléfono* (ejemplo: +1 4255551234).

El administrador administra el atributo del teléfono de la oficina.

> [!IMPORTANT]
> A partir de marzo de 2019, las opciones de llamada de teléfono no estarán disponibles para los usuarios de MFA y SSPR en inquilinos de Azure AD gratis o de evaluación. Los mensajes SMS no se ven afectados por este cambio. Las llamadas de teléfono seguirán estando disponibles para los usuarios de inquilinos de Azure AD de pago. Este cambio solo afecta a los inquilinos de Azure AD gratis o de evaluación.

> [!NOTE]
> Debe haber un espacio entre el código de país y el número de teléfono.
>
> El restablecimiento de contraseña no admite extensiones telefónicas. Incluso con el formato +1 4255551234X12345, las extensiones se quitan antes de realizarse la llamada.

### <a name="troubleshooting-phone-options"></a>Solución de problemas de las opciones de teléfono

Problemas comunes relacionados con los métodos de autenticación con un número de teléfono:

* Identificador de llamada bloqueado en un solo dispositivo.
   * Solución de problemas de dispositivo
* Número de teléfono incorrecto, código de país incorrecto, número de teléfono particular frente a número de teléfono del trabajo.
   * Solución de problemas de objeto de usuario y métodos de autenticación configurados. Asegúrese de que se registran números de teléfono correctos.
* El PIN especificado es incorrecto.
   * Confirme que el usuario ha usado el PIN correcto registrado en el servidor de Azure MFA.
* La llamada se ha desviado al correo de voz.
   * Asegúrese de que el usuario tiene activado el teléfono y que el servicio está disponible en su área o use un método alternativo.
* El usuario está bloqueado
   * Solicite al administrador que desbloquee el usuario en Azure Portal.
* No se han suscrito los SMS en el dispositivo.
   * Solicite al usuario que cambie los métodos o active los SMS en el dispositivo.
* Problemas con los proveedores de telecomunicaciones (no se detecta entrada de teléfono, incidencias sobre la falta de tonos DTMF, identificador de llamada bloqueado en varios dispositivos o SMS bloqueados en varios dispositivos).
   * Microsoft usa varios proveedores de telecomunicaciones con el fin de enrutar las llamadas telefónicas y los mensajes SMS para la autenticación. Si observa alguno de los problemas anteriores, solicite al usuario que intente usar el método al menos 5 veces en 5 minutos y tenga la información del usuario disponible cuando se ponga en contacto con el soporte técnico de Microsoft.

## <a name="app-passwords"></a>Contraseñas de aplicación

Determinadas aplicaciones sin explorador no admiten la autenticación multifactor; si un usuario se ha habilitado para la autenticación multifactor e intenta usar aplicaciones sin explorador, no puede autenticarse. Una contraseña de aplicación permite a los usuarios continuar con la autenticación.

Si se exige la aplicación de Multi-Factor Authentication a través de directivas de acceso condicional y no a través de MFA por usuario, no podrá crear contraseñas de aplicación. Las aplicaciones que utilizan directivas de acceso condicional para controlar el acceso no necesitan contraseñas de aplicación.

Si su organización está federada para SSO con Azure AD y que va a usar Azure MFA, tenga en cuenta los siguientes detalles:

* Azure AD comprueba la contraseña de aplicación y, por tanto, omite la federación. La federación solo se usa activamente al configurar contraseñas de aplicación. Para los usuarios federados (SSO), las contraseñas se almacenan en el identificador de organización. Si el usuario abandona la empresa, esta información tiene que fluir al identificador de la organización con DirSync. La deshabilitación o eliminación de la cuenta puede tardar hasta tres horas en sincronizarse, lo que retrasa la deshabilitación o eliminación de las contraseñas de aplicación en Azure AD.
* La configuración del control de acceso de cliente local no admite la contraseña de aplicación
* No hay ningún registro o funcionalidad de auditoría en la autenticación local que esté disponible para contraseñas de aplicación.
* Puede que algunos diseños de arquitectura avanzada requieran el uso de una combinación de nombre de usuario y contraseñas de la organización y de contraseñas de aplicación cuando se usa la verificación de dos pasos con clientes, en función de dónde se autentiquen. Para los clientes que se autentican en una infraestructura local, usaría un nombre de usuario y una contraseña de la organización. Para los clientes que se autentican en Azure AD, usaría la contraseña de aplicación.
* De forma predeterminada, los usuarios no pueden crear contraseñas de aplicación. Si quiere permitir que los usuarios creen contraseñas de aplicación, seleccione la opción **Permitir a los usuarios crear contraseñas de aplicación para iniciar sesión en aplicaciones que no son de explorador** en el valor de configuración del servicio.

## <a name="next-steps"></a>Pasos siguientes

[Habilitar el autoservicio de restablecimiento de contraseña para la organización](quickstart-sspr.md)

[Habilitar Azure Multi-Factor Authentication para la organización](howto-mfa-getstarted.md)

[Habilitación del registro combinado en el inquilino](howto-registration-mfa-sspr-combined.md)

[Documentación de configuración del método de autenticación de usuario final](https://aka.ms/securityinfoguide)
