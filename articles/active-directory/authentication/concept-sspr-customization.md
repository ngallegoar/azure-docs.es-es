---
title: 'Personalización del autoservicio de restablecimiento de contraseña: Azure Active Directory'
description: Opciones de personalización para el autoservicio de restablecimiento de contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6f7f59f7bcc93edafa3cbb47bd432b52bde985c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75979454"
---
# <a name="customize-the-azure-ad-functionality-for-self-service-password-reset"></a>Personalizar la funcionalidad del autoservicio de restablecimiento de contraseña de Azure AD

Los profesionales de TI que quieran implementar el autoservicio de restablecimiento de contraseña (SSPR) en Azure Active Directory (Azure AD) pueden personalizar la experiencia para adaptarla a las necesidades de sus usuarios.

## <a name="customize-the-contact-your-administrator-link"></a>Personalizar el vínculo para ponerse en contacto con el administrador

Los usuarios de autoservicio de restablecimiento de contraseña tienen disponible un vínculo para ponerse en contacto con el administrador en el portal de restablecimiento de contraseña. Si un usuario selecciona este vínculo, hará una de estas dos cosas:

* Si se deja en el estado predeterminado:
   * Se envía un correo electrónico a los administradores en el que se solicita asistencia para cambiar la contraseña del usuario. Consulte el [correo de ejemplo](#sample-email) a continuación.
* Si se personaliza:
   * Se envía al usuario a una página web o dirección de correo electrónico especificada por el administrador para obtener asistencia.

> [!TIP]
> Si se personaliza, se recomienda establecerlo en algo con lo que los usuarios ya estén familiarizados para soporte técnico.

> [!WARNING]
> Si personaliza esta configuración con una dirección de correo electrónico y una cuenta que necesita un restablecimiento de contraseña, es posible que el usuario no pueda pedir asistencia.

### <a name="sample-email"></a>Correo electrónico de ejemplo

![Ejemplo de solicitud para restablecer el correo electrónico que se envía al administrador][Contact]

Este correo electrónico se envía a los siguientes destinatarios en el orden siguiente:

1. Si el rol **Administrador del departamento de soporte técnico** o **Administrador de contraseñas** está asignado, se notificará a todos administradores que los tengan.
1. Si no hay asignado ningún administrador del departamento de soporte técnico o de contraseñas, entonces se notificará a los que tengan el rol de **administrador de usuarios**.
1. Si no se ha asignado ninguno de los roles anteriores, se notificará a los **administradores globales**.

En todos los casos, se notificará a un máximo de 100 destinatarios en total.

Para obtener más información sobre los diferentes roles de administrador y cómo asignarlos, consulte [Asignación de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md).

### <a name="disable-contact-your-administrator-emails"></a>Deshabilitar los correos electrónicos para contactar con los administradores

Si la organización no quiere que se notifique a los administradores de las solicitudes de restablecimiento de contraseñas, se puede habilitar la configuración siguiente:

* Habilite el autoservicio de restablecimiento de contraseñas para todos los usuarios finales. Esta opción está disponible en **Restablecimiento de contraseña** > **Propiedades**. Si no quiere que los usuarios restablezcan sus propias contraseñas, puede aumentar el ámbito del acceso a un grupo vacío. *(Esta opción no se recomienda).*
* Personalice el vínculo del departamento de soporte técnico para proporcionar una dirección URL web o una dirección mailto: que los usuarios puedan usar para obtener asistencia. Esta opción se encuentra en **Restablecimiento de contraseña** > **Personalización** > **Dirección URL o correo electrónico personalizados del departamento de soporte técnico**.

## <a name="customize-the-ad-fs-sign-in-page-for-sspr"></a>Personalizar la página de inicio de sesión de AD FS para SSPR

Los administradores de Servicios de federación de Active Directory (AD FS) pueden agregar un vínculo a su página de inicio de sesión mediante las directrices recogidas en el artículo [Add sign-in page description](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/add-sign-in-page-description) (Agregar la descripción de la página de inicio de sesión).

Para agregar un vínculo a la página de inicio de sesión de AD FS, use el siguiente comando en el servidor de AD FS. Los usuarios pueden usar esta página para especificar el flujo de trabajo de SSPR.

``` powershell
Set-ADFSGlobalWebContent -SigninPageDescriptionText "<p><A href='https://passwordreset.microsoftonline.com' target='_blank'>Can’t access your account?</A></p>"
```

## <a name="customize-the-sign-in-page-and-access-panel-look-and-feel"></a>Personalizar la apariencia del panel de acceso y la página de inicio de sesión

Puede personalizar la página de inicio de sesión. Puede agregar un logotipo que aparezca junto a la imagen que se adapte a la marca de la empresa.

Los gráficos que elija se muestran en los siguientes casos:

* Después de que un usuario escriba su nombre de usuario.
* Si el usuario accede a la dirección URL personalizada:
   * Pasando el parámetro `whr` a la página de restablecimiento de contraseña, como `https://login.microsoftonline.com/?whr=contoso.com`.
   * Pasando el parámetro `username` a la página de restablecimiento de contraseña, como `https://login.microsoftonline.com/?username=admin@contoso.com`.

Obtenga información detallada sobre cómo configurar la personalización de marca de empresa en el artículo [Incorporación de personalización de marca de empresa a su página de inicio de sesión de Azure AD](../fundamentals/customize-branding.md).

### <a name="directory-name"></a>Nombre de directorio

Puede cambiar el atributo del nombre de directorio en **Azure Active Directory** > **Propiedades**. Asimismo, puede mostrar un nombre descriptivo de la organización para que se vea en el portal y en las comunicaciones automatizadas. Esta opción es la más visible en correos electrónicos automatizados, ya que usa los elementos que se indican a continuación:

* Nombre descriptivo del correo electrónico "Demostración de Microsoft en nombre de CONTOSO".
* Línea de asunto del correo electrónico "Código de verificación del correo electrónico de la cuenta de demostración CONTOSO".

## <a name="next-steps"></a>Pasos siguientes

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

[Contact]: ./media/concept-sspr-customization/sspr-contact-admin.png "Póngase en contacto con el administrador para obtener ayuda al restablecer el ejemplo de correo electrónico de contraseña"
