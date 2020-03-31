---
title: 'Configuración de una dirección de correo electrónico como su método de comprobación: Azure AD'
description: Cómo configurar su página de información de seguridad (versión preliminar) para comprobar su identidad mediante una dirección de email como método de comprobación.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: curtand
ms.openlocfilehash: 9488b96dda15b0ffb794f83826dc299a5f397b77
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "77064009"
---
# <a name="set-up-an-email-address-as-your-verification-method"></a>Configuración de una dirección de correo electrónico como método de comprobación

Puede seguir estos pasos para agregar su método de restablecimiento de la contraseña. Una vez que haya configurado esto por primera vez, puede volver a la página **Información de seguridad** para agregar, actualizar o eliminar su información de seguridad.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Si no ve la opción de correo electrónico, es posible que su organización no le permita usar esta opción para el método de restablecimiento de contraseña. En este caso, tendrá que elegir otro método o ponerse en contacto con el departamento de soporte técnico de su organización para que le proporcione más ayuda.

## <a name="security-vs-password-reset-verification"></a>Comprobación de seguridad frente a comprobación de restablecimiento de contraseña

Los métodos de la información de seguridad se usan tanto para la comprobación de seguridad de dos factores como para al restablecimiento de la contraseña. Sin embargo, no todos los métodos pueden usarse para ambos.

| Método | Se usa para |
| ------ | -------- |
| Aplicación de autenticación | Autenticación mediante la comprobación de dos factores y restablecimiento de la contraseña. |
| Mensajes de texto | Autenticación mediante la comprobación de dos factores y restablecimiento de la contraseña. |
| Llamadas de teléfono | Autenticación mediante la comprobación de dos factores y restablecimiento de la contraseña. |
| Clave de seguridad | Autenticación mediante la comprobación de dos factores y restablecimiento de la contraseña. |
| Cuenta de correo electrónico | Solo autenticación mediante restablecimiento de contraseña. Deberá elegir otro método para la comprobación mediante dos factores. |
| Preguntas de seguridad | Solo autenticación mediante restablecimiento de contraseña. Deberá elegir otro método para la comprobación mediante dos factores. |

## <a name="set-up-your-email-address-from-the-security-info-page"></a>Configuración de una dirección de correo electrónico desde la página Información de seguridad

Dependiendo de la configuración de su organización, es posible que pueda usar una dirección de correo electrónico como uno de sus métodos de información de seguridad.

>[!Note]
>Se recomienda usar una cuenta de correo electrónico que no requiera la contraseña de red para acceder. Si no ve la opción de correo electrónico, es posible que su organización no le permita usar un correo electrónico para la comprobación. Si este es el caso, tendrá que elegir otro método o ponerse en contacto con su administrador para obtener más ayuda.

### <a name="to-set-up-your-email-address"></a>Para configurar su dirección de correo electrónico

1. Inicie sesión en su cuenta profesional o educativa y vaya a la página https://myprofile.microsoft.com/.

    ![Página Mi Perfil, que muestra los vínculos de Información de seguridad resaltados](media/security-info/securityinfo-myprofile-page.png)

2. Seleccione **Información de seguridad** en el panel de navegación izquierdo o en el vínculo del bloque **Información de seguridad** y, después, seleccione **Agregar método** en la página **Información de seguridad**.

    ![Página Información de seguridad con la opción Agregar método resaltada](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. En la página **Agregar un método**, seleccione **Correo electrónico** en la lista desplegable y, después, seleccione **Agregar**.

    ![Cuadro Agregar método, con Correo electrónico seleccionado](media/security-info/securityinfo-myprofile-addemail.png)

4. En la página **Correo electrónico**, escriba su dirección de correo electrónico (por ejemplo, alain@gmail.com) y seleccione **Siguiente**.

    ![Adición de un número de teléfono y elección de llamadas de teléfono](media/security-info/securityinfo-myprofile-emailaddress.png)

    >[!Important]
    >Esta dirección de correo electrónico no puede ser la dirección de correo electrónico profesional o educativa.

5. Escriba el código que se ha enviado a la dirección de correo electrónico que ha especificado y seleccione **Siguiente**.

    ![Adición de un número de teléfono y elección de mensajes de texto](media/security-info/securityinfo-myprofile-emailcode.png)

    La información de seguridad se actualiza y puede usar la dirección de correo electrónico para comprobar su identidad al usar el restablecimiento de contraseña.

## <a name="delete-your-email-address-from-your-security-info-methods"></a>Eliminación de la dirección de correo electrónico de los métodos de información de seguridad

Si no desea usar la dirección de correo electrónico como método de información de seguridad, puede quitarla de la página **Información de seguridad**.

>[!Important]
>Si elimina la dirección de correo electrónico por error, no hay forma de deshacer la operación. Tendrá que volver a agregar el método, para lo que debe seguir los pasos de la sección sobre cómo [Configuración de una dirección de correo electrónico](#set-up-your-email-address-from-the-security-info-page) de este mismo artículo.

### <a name="to-delete-your-email-address"></a>Para eliminar una dirección de correo electrónico

1. En la página **Información de seguridad**, seleccione el vínculo **Eliminar**, que se encuentra al lado de la opción **Correo electrónico**.

    ![Vínculo para eliminar el método del teléfono de la información de seguridad](media/security-info/securityinfo-myprofile-emaildelete.png)

2. Seleccione **Sí** en el cuadro de confirmación para eliminar la cuenta de **Correo electrónico**. Una vez eliminada la cuenta de correo electrónico, se quitará de su información de seguridad y desaparecerá de la página **Información de seguridad**.

## <a name="additional-security-info-methods"></a>Métodos de información de seguridad adicionales

Tiene opciones adicionales para determinar cómo su organización se pone en contacto con usted para comprobar su identidad, basándose en lo está intentando hacer. Entre estas opciones se incluyen:

- **Aplicación autenticadora.** Descargue y use una aplicación autenticadora para obtener una notificación de aprobación o un código de aprobación generado de forma aleatoria para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo configurar y usar la aplicación Microsoft Authenticator, consulte [Set up security info to use an authenticator app](security-info-setup-auth-app.md) (Configuración de la información de seguridad para usar una aplicación de autenticador).

- **Texto de dispositivo móvil.** Escriba el número del dispositivo móvil y obtendrá un código que podrá usar para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones detalladas sobre cómo verificar su identidad con un mensaje de texto (SMS), vea [Configuración de la información de seguridad para usar la mensajería de texto (SMS)](security-info-setup-text-msg.md).

- **Llamada a dispositivo móvil o al teléfono del trabajo.** Escriba el número del dispositivo móvil y recibirá una llamada telefónica para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo comprobar su identidad con un número de teléfono, consulte [Configuración de la información de seguridad para usar llamadas de teléfono](security-info-setup-phone-number.md).

- **Clave de seguridad.** Registre la clave de seguridad compatible con Microsoft y úsela junto con un PIN para la verificación en dos pasos o el restablecimiento de contraseña. Para instrucciones paso a paso sobre cómo comprobar su identidad con una clave de seguridad, consulte [Configuración de la información de seguridad para usar una clave de seguridad](security-info-setup-security-key.md).

- **Preguntas de seguridad.** Responda a algunas preguntas de seguridad creadas por el administrador para su organización. Esta opción solo está disponible para el restablecimiento de contraseña y no para la verificación en dos pasos. Para obtener instrucciones paso a paso sobre cómo configurar las preguntas de seguridad, consulte el artículo [Configuración de la información de seguridad para usar las preguntas de seguridad](security-info-setup-questions.md).

    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="next-steps"></a>Pasos siguientes

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](active-directory-passwords-update-your-own-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
