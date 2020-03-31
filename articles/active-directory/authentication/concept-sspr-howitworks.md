---
title: 'Profundización en el autoservicio de restablecimiento de contraseña: Azure Active Directory'
description: ¿Cómo funciona el autoservicio de restablecimiento de contraseña?
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 08/16/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b19c80378aa40a7f791a3eb61130b013217ddee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848585"
---
# <a name="how-it-works-azure-ad-self-service-password-reset"></a>Funcionamiento: Autoservicio de restablecimiento de contraseña de Azure AD

¿Cómo funciona el autoservicio de restablecimiento de contraseña (SSPR)? ¿Qué significa la opción en la interfaz? Siga leyendo para obtener más información acerca de SSPR de Azure Active Directory (Azure AD).

## <a name="how-does-the-password-reset-portal-work"></a>¿Cómo funciona el portal de restablecimiento de contraseñas?

Cuando un usuario visita el portal de restablecimiento de contraseña, se inicia un flujo de trabajo para determinar:

   * ¿Cómo se debe localizar la página?
   * ¿Es válida la cuenta de usuario?
   * ¿A qué organización pertenece el usuario?
   * ¿Dónde se administra la contraseña del usuario?
   * ¿Tiene el usuario licencia para usar la característica?

Lea los pasos siguientes para obtener información sobre la lógica de la página de restablecimiento de contraseña:

1. El usuario selecciona el vínculo **No se puede tener acceso a la cuenta** o va directamente a [https://aka.ms/sspr](https://passwordreset.microsoftonline.com).
   * Según la configuración regional del explorador, la experiencia se representa en el idioma correspondiente. La experiencia de restablecimiento de contraseña está localizada en los mismos idiomas que admite Office 365.
   * Para ver el portal de restablecimiento de contraseñas en un idioma diferente, anexe "?mkt=" al final de la dirección URL de restablecimiento de contraseñas con el ejemplo que sigue para localizar en español [https://passwordreset.microsoftonline.com/?mkt=es-us](https://passwordreset.microsoftonline.com/?mkt=es-us).
2. El usuario escribe un identificador de usuario y pasa un CAPTCHA.
3. Azure AD comprueba que el usuario pueda utilizar esta característica y, para ello, hace las siguientes comprobaciones:
   * Comprueba que el usuario tenga esta característica habilitada y una licencia de Azure AD asignada.
     * Si el usuario no tiene esta característica habilitada o una licencia asignada, se le solicita que se ponga en contacto con el administrador para restablecer la contraseña.
   * Comprueba que el usuario tiene los métodos de comprobación correctos definidos en la cuenta según la directiva del administrador.
     * Si la directiva solo requiere un método, significa que garantiza que el usuario tiene los datos correspondientes definidos para al menos uno de los métodos de autenticación habilitados por la directiva del administrador.
       * Si los métodos de autenticación no están configurados, se recomienda al usuario que se ponga en contacto con el administrador para restablecer la contraseña.
     * Si la directiva solo requiere dos métodos, significa que garantiza que el usuario tiene los datos correspondientes definidos para al menos dos de los métodos de autenticación habilitados por la directiva del administrador.
       * Si los métodos de autenticación no están configurados, se recomienda al usuario que se ponga en contacto con el administrador para restablecer la contraseña.
     * Si se asigna un rol de administrador de Azure al usuario, se aplica la directiva de contraseñas seguras de dos puertas. Puede encontrar más información sobre esta directiva en la sección [Diferencias entre directivas de restablecimiento de administrador](concept-sspr-policy.md#administrator-reset-policy-differences).
   * Comprueba si la contraseña del usuario se administra en un entorno local (federado, con autenticación de paso a través o con sincronización de hash de contraseñas).
     * Si la escritura diferida está implementada y la contraseña del usuario se administra en un entorno local, el usuario puede continuar con la autenticación y restablecer la contraseña.
     * Si la escritura diferida no está implementada y la contraseña del usuario se administra en un entorno local, se pide al usuario que se ponga en contacto con el administrador para restablecer la contraseña.
4. Si se determina que el usuario puede restablecer correctamente la contraseña, se le guiará a través del proceso de restablecimiento.

## <a name="authentication-methods"></a>Métodos de autenticación

Si SSPR está habilitado, tiene que seleccionar al menos una de las opciones siguientes para los métodos de autenticación. En ocasiones, se hace referencia a estas opciones como "puertas". Se recomienda encarecidamente **elegir dos o más métodos de autenticación** para que los usuarios tengan más flexibilidad en caso de que no puedan acceder a uno cuando lo necesiten. Se puede encontrar información adicional sobre los siguientes métodos en el artículo [¿Qué son los métodos de autenticación?](concept-authentication-methods.md).

* Notificación en aplicación móvil
* Código de aplicación móvil
* Email
* Teléfono móvil
* Teléfono del trabajo
* Preguntas de seguridad

Los usuarios solo pueden restablecer su contraseña si tienen datos en los métodos de autenticación que el administrador haya habilitado.

> [!IMPORTANT]
> A partir de marzo de 2019, las opciones de llamada de teléfono no estarán disponibles para los usuarios de MFA y SSPR en inquilinos de Azure AD gratis o de evaluación. Los mensajes SMS no se ven afectados por este cambio. Las llamadas de teléfono seguirán estando disponibles para los usuarios de inquilinos de Azure AD de pago. Este cambio solo afecta a los inquilinos de Azure AD gratis o de evaluación.

> [!WARNING]
> Las cuentas asignadas a los roles de administrador de Azure tendrán que usar los métodos definidos en la sección [Diferencias entre directivas de restablecimiento de administrador](concept-sspr-policy.md#administrator-reset-policy-differences).

![Selección de métodos de autenticación en Azure Portal][Authentication]

### <a name="number-of-authentication-methods-required"></a>Número de métodos de autenticación requeridos

Esta opción determina el número mínimo de métodos de autenticación disponibles o puertas que un usuario debe pasar para restablecer o desbloquear su contraseña. Se puede establecer en uno o dos.

Los usuarios pueden elegir proporcionar más métodos de autenticación si el administrador los habilita.

Si un usuario no tiene los métodos necesarios mínimos registrados, verá una página de error que le redirigirá a solicitar que un administrador restablezca su contraseña.

#### <a name="mobile-app-and-sspr"></a>Aplicación móvil y SSPR

Cuando se usa una aplicación móvil, por ejemplo, la aplicación Microsoft Authenticator, como un método para restablecer la contraseña, debe ser consciente de las siguientes advertencias:

* Cuando los administradores requieren que se utilice un método para restablecer una contraseña, el código de verificación es la única opción disponible.
* Cuando los administradores requieren que se usen dos métodos para restablecer una contraseña, los usuarios podrán utilizar **bien** la notificación **O** el código de verificación, además de con cualquier otro método habilitado.

| Número de métodos requeridos para el restablecimiento | Uno | Dos |
| :---: | :---: | :---: |
| Características de las aplicaciones móviles disponibles | Código | Código o notificación |

Los usuarios no tendrán la opción de registrar su aplicación móvil cuando se registren en el autoservicio de restablecimiento de contraseña[https://aka.ms/ssprsetup](https://aka.ms/ssprsetup). Los usuarios pueden registrar su aplicación móvil en [https://aka.ms/mfasetup](https://aka.ms/mfasetup) o en la nueva versión preliminar del registro de información de seguridad en [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo).

> [!WARNING]
> Debe habilitar [Registro convergente para autoservicio de restablecimiento de contraseña y Azure Multi-Factor Authentication (versión preliminar pública)](concept-registration-mfa-sspr-converged.md) para que los usuarios puedan acceder a la nueva experiencia en [https://aka.ms/setupsecurityinfo](https://aka.ms/setupsecurityinfo).

> [!IMPORTANT]
> Cuando se configura una directiva de una puerta, no se puede seleccionar la aplicación de autenticación como único método de autenticación. Del mismo modo, tampoco se pueden seleccionar la aplicación de autenticación y un único método adicional al configurar una directiva de dos puertas.
> Por tanto, al configurar las directivas de SSPR que incluyen la aplicación de autenticación como método, se debe seleccionar al menos un método adicional si se configura una directiva de una puerta y al menos dos métodos adicionales si se configura una directiva de dos puertas.
> Este requisito se debe a que la experiencia de registro de SSPR actual no incluye la opción para registrar la aplicación de autenticación. La opción para registrar está aplicación se incluye con el nuevo [registro convergente para autoservicio de restablecimiento de contraseña y Azure Multi-Factor Authentication (versión preliminar pública)](concept-registration-mfa-sspr-converged.md).
> Si se permiten directivas que solo usan la aplicación de autenticación (en las directivas de una puerta) o la aplicación de autenticación y un único método adicional (en las directivas de dos puertas), los usuarios podrían ver bloqueado su registro en SSPR hasta que se modificase su configuración para usar la nueva experiencia de registro.

### <a name="change-authentication-methods"></a>Cambio de métodos de autenticación

¿Qué sucede si empieza con una directiva que solo tiene registrado un método de autenticación requerido para el restablecimiento o el desbloqueo y cambia a dos métodos?

| Número de métodos registrados | Número de métodos requeridos | Resultado |
| :---: | :---: | :---: |
| 1 o más | 1 | **Permite** restablecer o desbloquear |
| 1 | 2 | **No permite** restablecer o desbloquear |
| 2 o más | 2 | **Permite** restablecer o desbloquear |

Si cambia los tipos de métodos de autenticación que puede usar un usuario, es posible que impida sin darse cuenta que los usuarios puedan usar SSPR si no tienen la cantidad mínima de datos disponibles.

Ejemplo:
1. La directiva original se configura con dos métodos de autenticación necesarios. Solo usa el número de teléfono de la oficina y las preguntas de seguridad. 
2. El administrador cambia la directiva para dejar de usar las preguntas de seguridad, pero permite el uso de un teléfono móvil y de un correo electrónico alternativo.
3. Los usuarios que no rellenen los campos de teléfono móvil o correo electrónico alternativo no podrán restablecer sus contraseñas.

## <a name="registration"></a>Registro

### <a name="require-users-to-register-when-they-sign-in"></a>Exigir a los usuarios que se registren al iniciar sesión

Al habilitar esta opción, un usuario debe completar el registro de restablecimiento de contraseña si inicia sesión en cualquier aplicación con Azure AD. Este flujo de trabajo incluye las siguientes aplicaciones:

* Office 365
* Portal de Azure
* Panel de acceso
* Aplicaciones federadas
* Aplicaciones personalizadas mediante Azure AD

Cuando se deshabilita la opción para requerir el registro, los usuarios pueden registrarse manualmente. Pueden visitar [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup) o seleccionar el vínculo **Registrarme para restablecer la contraseña** en la pestaña **Perfil** del Panel de acceso.

> [!NOTE]
> Para descartar el portal de registro de restablecimiento de contraseña, los usuarios pueden seleccionar **Cancelar** o cerrar la ventana. Sin embargo, se les pide que se registren cada vez que inician sesión hasta que completan el registro.
>
> Esta interrupción no altera la conexión del usuario si ya ha iniciado sesión.

### <a name="set-the-number-of-days-before-users-are-asked-to-reconfirm-their-authentication-information"></a>Establecer el número de días que pasan hasta que se pide a los usuarios que vuelvan a confirmar su información de autenticación

Esta opción determina el período que transcurre entre el establecimiento y la reconfirmación de la información de autenticación, y solo está disponible si se habilita la opción **¿Desea exigir a los usuarios que se registren al iniciar sesión?**

Los valores válidos son desde 0 hasta 730 días. 0 significa que no se pide a los usuarios que vuelvan a confirmar su información de autenticación.

## <a name="notifications"></a>Notificaciones

### <a name="notify-users-on-password-resets"></a>¿Quiere notificar a los usuarios los restablecimientos de contraseña?

Si esta opción está establecida en **Sí**, los usuarios que restablecen la contraseña reciben un correo electrónico que le informa de que la contraseña se ha cambiado. El correo electrónico se envía a través del portal de SSPR a las direcciones de correo electrónico principal y alternativa registradas en Azure AD. A ningún otro usuario se le informa del evento de restablecimiento.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Notificación a todos los administradores cuando otros administradores restablecen sus contraseñas

Si esta opción está establecida en **Sí**, *todos los administradores* reciben un correo electrónico en la dirección de correo electrónico principal que tienen registrada en Azure AD. En el correo electrónico se les notifica que otro administrador ha cambiado su contraseña mediante SSPR.

Ejemplo: hay cuatro administradores en un entorno. El administrador A restablece su contraseña mediante SSPR. Los administradores B, C y D reciben un correo electrónico que les alerta del restablecimiento de contraseña.

## <a name="on-premises-integration"></a>Integración local

Si instala, configura y habilita Azure AD Connect, tiene las opciones adicionales siguientes para integraciones locales. Si estas opciones están atenuadas, significa que la escritura diferida no se ha configurado correctamente. Para obtener más información, consulte [Configuración de la escritura diferida de contraseñas](howto-sspr-writeback.md).

![Validación de escritura diferida de contraseñas habilitada en funcionamiento][Writeback]

Esta página proporciona un estado rápido del cliente de escritura diferida con los siguientes mensajes que se muestran según la configuración actual:

* El cliente de escritura diferida local está en funcionamiento.
* Azure AD está en línea y conectado al cliente de escritura diferida local. Sin embargo, parece que la versión instalada de Azure AD Connect no está actualizada. Considere la posibilidad de [actualizar Azure AD Connect](../hybrid/how-to-upgrade-previous-version.md) para asegurarse de que tiene las características de conectividad más recientes y las correcciones de errores importantes.
* Desafortunadamente, de momento no podemos comprobar el estado del cliente de escritura diferida local porque la versión instalada de Azure AD Connect no está actualizada. [Actualice Azure AD Connect](../hybrid/how-to-upgrade-previous-version.md) para poder comprobar su estado de conexión.
* Desafortunadamente, parece que no podemos conectarnos a su cliente de escritura diferida local ahora mismo. [Solucione problemas de Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) para restaurar la conexión.
* Desafortunadamente, no podemos conectarnos a su cliente de escritura diferida local porque la escritura diferida de contraseñas no se ha configurado correctamente. [Configure la escritura diferida de contraseñas](howto-sspr-writeback.md) para restaurar la conexión.
* Desafortunadamente, parece que no podemos conectarnos a su cliente de escritura diferida local ahora mismo. Esto puede deberse a problemas temporales por nuestra parte. Si el problema persiste, [solucione los problemas de Azure AD Connect](active-directory-passwords-troubleshoot.md#troubleshoot-password-writeback-connectivity) para restaurar la conexión.

### <a name="write-back-passwords-to-your-on-premises-directory"></a>Escritura diferida de contraseñas en un directorio local

Este control determina si la escritura diferida de contraseñas está habilitada para este directorio. Si lo está, indica el estado del servicio de escritura diferida local. Este control es útil si quiere deshabilitar temporalmente la escritura diferida de contraseñas sin tener que volver a configurar Azure AD Connect.

* Si el modificador se establece en **Sí**, se habilita la escritura diferida y los usuarios federados, con autenticación de paso a través o con sincronización de hash de contraseñas pueden restablecer sus contraseñas.
* Si el modificador se establece en **No**, se deshabilita la escritura diferida y los usuarios federados, con autenticación de paso a través o con sincronización de hash de contraseñas no pueden restablecer sus contraseñas.

### <a name="allow-users-to-unlock-accounts-without-resetting-their-password"></a>Permitir a los usuarios desbloquear las cuentas sin restablecer la contraseña

Este control designa si los usuarios que visitan el portal de restablecimiento de contraseña tendrán la opción de desbloquear sus cuentas de Active Directory locales sin tener que restablecer su contraseña. De forma predeterminada, Azure AD desbloquea las cuentas cuando se realiza un restablecimiento de contraseña. Use esta opción para separar esas dos operaciones.

* Si se establece en **Sí**, los usuarios tienen la opción de restablecer su contraseña y desbloquear la cuenta, o de desbloquear la cuenta sin tener que restablecer la contraseña.
* Si se establece en **No**, los usuarios solo pueden realizar una operación combinada de restablecimiento de contraseña y desbloqueo de cuenta.

### <a name="on-premises-active-directory-password-filters"></a>Filtros de la contraseña de Active Directory local

El autoservicio de restablecimiento de contraseña de Azure AD realiza lo mismo que un restablecimiento de contraseña iniciado por el administrador de Active Directory. Si está usando un filtro de contraseñas de terceros para exigir reglas de contraseñas personalizadas y requiere que se compruebe este filtro de contraseña durante el autoservicio de restablecimiento de contraseña de Azure AD, asegúrese de que la solución de filtro de contraseña de terceros está configurada para aplicarse en el escenario de restablecimiento de contraseña del administrador. De forma predeterminada, [Protección con contraseña de Azure AD para Windows Server Active Directory](concept-password-ban-bad-on-premises.md) es compatible.

## <a name="password-reset-for-b2b-users"></a>Restablecimiento de contraseña para usuarios B2B

El restablecimiento y el cambio de contraseña son totalmente compatibles con todas las configuraciones negocio a negocio (B2B). Se admiten los tres casos siguientes para el restablecimiento de contraseña de usuario B2B:

* **Usuarios de una organización asociada con un inquilino de Azure AD existente**: si la organización con la que se asocia tiene un inquilino de Azure AD existente, *respetamos todas las directivas de restablecimiento de contraseña habilitadas en dicho inquilino*. Para que el restablecimiento de contraseña funcione, la organización asociada solo tiene que asegurarse de que SSPR de Azure AD está habilitado. No hay ningún cargo adicional para los clientes de Office 365 y puede habilitarse siguiendo los pasos descritos en nuestra guía [Introducción a la administración de contraseñas](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords).
* **Usuarios que se registran mediante el** registro de autoservicio: si la organización con la que se asocia usó la característica de [registro de autoservicio](../users-groups-roles/directory-self-service-signup.md) para acceder a un inquilino, le permitimos el restablecimiento de contraseña con el correo electrónico que registró.
* **Usuarios B2B**: los nuevos usuarios B2B creados mediante las nuevas [funcionalidades B2B de Azure AD](../active-directory-b2b-what-is-azure-ad-b2b.md) también podrán restablecer sus contraseñas con el correo electrónico que registraron durante el proceso de invitación.

Para probar este escenario, vaya a https://passwordreset.microsoftonline.com con uno de estos usuarios asociados. Si tienen un correo electrónico alternativo o de autenticación definido, el restablecimiento de contraseña funcionará según lo esperado.

> [!NOTE]
> Las cuentas de Microsoft a las que se ha concedido acceso de invitado para su inquilino de Azure AD, como las de Hotmail.com, Outlook.com u otras direcciones de correo electrónico personales, no pueden usar SSPR de Azure AD. Para restablecer su contraseña, se debe usar la información que se encuentra en el artículo [Cuando no puedes iniciar sesión en tu cuenta Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).

## <a name="next-steps"></a>Pasos siguientes

En los siguientes artículos se proporciona información adicional sobre el restablecimiento de contraseña con Azure AD:

* [¿Cómo se realiza un lanzamiento correcto de SSPR?](howto-sspr-deployment.md)
* [Restablecimiento o modificación de la contraseña](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registro para el autoservicio de restablecimiento de contraseñas](../user-help/active-directory-passwords-reset-register.md)
* [¿Tiene alguna pregunta acerca de las licencias?](concept-sspr-licensing.md)
* [¿Qué datos usa SSPR y cuáles se deben rellenar en lugar de los usuarios?](howto-sspr-authenticationdata.md)
* [¿Qué métodos de autenticación están disponibles para los usuarios?](concept-sspr-howitworks.md#authentication-methods)
* [¿Cuáles son las opciones de directiva con SSPR?](concept-sspr-policy.md)
* [¿Qué es la escritura diferida de contraseñas y por qué nos interesa?](howto-sspr-writeback.md)
* [¿Cómo se informa sobre la actividad de SSPR?](howto-sspr-reporting.md)
* [¿Cuáles son todas las opciones en SSPR y qué significan?](concept-sspr-howitworks.md)
* [Creo que algo se ha roto. ¿Cómo se solucionan problemas en SSPR?](active-directory-passwords-troubleshoot.md)
* [Tengo una pregunta que no se ha comentado en ningún otro sitio](active-directory-passwords-faq.md)

[Authentication]: ./media/concept-sspr-howitworks/manage-authentication-methods-for-password-reset.png "Métodos de autenticación de Azure AD disponibles y cantidad requerida"
[Writeback]: ./media/concept-sspr-howitworks/troubleshoot-on-premises-integration-writeback.png "Información de la configuración de la escritura diferida de contraseñas con integración local y solución de problemas"
