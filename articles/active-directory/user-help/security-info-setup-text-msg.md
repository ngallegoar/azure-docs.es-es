---
title: 'Configuración de mensajería de texto como su método de comprobación: Azure AD'
description: Cómo configurar su página de información de seguridad (versión preliminar) para comprobar su identidad mediante mensajes de texto como su método de comprobación.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 02/13/2019
ms.author: curtand
ms.openlocfilehash: 0fe191ac827c76afd497896611430f6efc6283aa
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83747238"
---
# <a name="set-up-text-messaging-as-your-verification-method"></a>Configuración de mensajería de texto como su método de comprobación

Puede seguir estos pasos para agregar sus métodos de restablecimiento de contraseña y verificación en dos pasos. Una vez que haya configurado esto por primera vez, puede volver a la página **Información de seguridad** para agregar, actualizar o eliminar su información de seguridad.

Si se le pide que configure esto justo después de haber iniciado sesión en su cuenta profesional o educativa, consulte los pasos detallados en el artículo de [configuración de su información de seguridad desde el símbolo del sistema de página de inicio de sesión](security-info-setup-signin.md).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Si no ve una opción telefónica, es posible que su organización no le permita usar esta opción para la comprobación. En este caso, tendrá que elegir otro método o ponerse en contacto con el departamento de soporte técnico de su organización para que le proporcione más ayuda.

## <a name="set-up-text-messages-from-the-security-info-page"></a>Configuración de mensajes de texto desde la página Información de seguridad

En función de la configuración de su organización, puede usar mensajes de texto como método de información de seguridad. La opción de los mensajes de texto es una parte de la opción telefónica, por lo que deberá realizar exactamente la misma configuración que para usar un número de teléfono, salvo que, en lugar de que Microsoft le llame, elegirá la opción de usar un mensaje de texto.

>[!Note]
>Si desea recibir una llamada de teléfono en lugar de un mensaje de texto, siga los pasos descritos en el artículo [Configuración de la información de seguridad para usar las llamadas de teléfono](security-info-setup-phone-number.md).

### <a name="to-set-up-text-messages"></a>Para configurar mensajes de texto

1. Inicie sesión en su cuenta profesional o educativa y vaya a la página https://myaccount.microsoft.com/.

    ![Página Mi Perfil, que muestra los vínculos de Información de seguridad resaltados](media/security-info/securityinfo-myprofile-page.png)

2. Seleccione **Información de seguridad** en el panel de navegación izquierdo o en el vínculo del bloque **Información de seguridad** y, después, seleccione **Agregar método** en la página **Información de seguridad**.

    ![Página Información de seguridad con la opción Agregar método resaltada](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. En la página **Agregar un método**, seleccione **Teléfono** en la lista desplegable y, después, seleccione **Agregar**.

    ![Cuadro Agregar método, con Teléfono seleccionado](media/security-info/securityinfo-myprofile-addphonetext.png)

4. En la página **Teléfono**, escriba el número de teléfono de su dispositivo móvil, elija **Enviarme un código por mensaje de texto**  y, después, seleccione **Siguiente**.

    ![Adición de un número de teléfono y elección de mensajes de texto](media/security-info/securityinfo-myprofile-phonetext-addnumber.png)

5. Escriba el código que se ha enviado por mensaje de texto al dispositivo móvil y, después, seleccione **Siguiente**.

    ![Adición de un número de teléfono y elección de mensajes de texto](media/security-info/securityinfo-myprofile-phonetext-entercode.png)

    La página cambia para mostrar que la operación se ha realizado correctamente.

    ![Notificación de éxito, que conecta el número de teléfono, la opción para recibir mensajes de texto y su cuenta.](media/security-info/securityinfo-myprofile-phonetext-success.png)

    La información de seguridad se actualiza y puede usar mensajes de texto para comprobar su identidad al usar la verificación en dos pasos o el restablecimiento de contraseña. Si desea que los mensajes de texto sean el método predeterminado, consulte la sección acerca de cómo [cambiar su método de información de seguridad predeterminado](#change-your-default-security-info-method) en este mismo artículo.

## <a name="delete-text-messaging-from-your-security-info-methods"></a>Eliminación de mensajes de texto de los métodos de información de seguridad

Si ya no desea usar los mensajes de texto como método de información de seguridad, puede quitarlo de la página **Información de seguridad**.

>[!Important]
>Si elimina la opción de mensajes de texto por error, no hay forma de deshacerlo. Tendrá que volver a agregar el método, para lo que debe seguir los pasos de la sección sobre cómo [configurar los mensajes de texto](#set-up-text-messages-from-the-security-info-page) de este mismo artículo.

### <a name="to-delete-text-messaging"></a>Para eliminar mensajes de texto

1. En la página **Información de seguridad**, seleccione el vínculo **Eliminar**, que se encuentra al lado de la opción **Teléfono**.

    ![Vínculo para eliminar el método del teléfono y de los mensajes de texto de la información de seguridad](media/security-info/securityinfo-myprofile-phonetext-delete.png)

2. Seleccione **Sí** en el cuadro de confirmación para eliminar el número de **teléfono**. Una vez que su número de teléfono se elimine, se quitará de su información de seguridad y desaparecerá de la página **Información de seguridad**. Si **Teléfono** es su método predeterminado, el valor predeterminado cambiará a otro método disponible.

## <a name="change-your-default-security-info-method"></a>Cambio de su método de información de seguridad predeterminado

Si quiere que los mensajes de texto sean el método predeterminado que se usa al iniciar sesión en su cuenta profesional o educativa mediante la verificación en dos pasos o para las solicitudes de restablecimiento de contraseña, puede establecerlo en la página **Información de seguridad**.

### <a name="to-change-your-default-security-info-method"></a>Para cambiar el método de información de seguridad predeterminado

1. En la página **Información de seguridad**, seleccione el vínculo **Cambiar** situado junto a la información **Método de inicio de sesión predeterminado**.

    ![Cambio de vínculo del método de inicio de sesión predeterminado](media/security-info/securityinfo-myprofile-phonetext-defaultchange.png)

2. Seleccione **Teléfono: texto (*_su_número_de_teléfono_*)** en la lista desplegable de métodos disponibles y, después, seleccione **Confirmar**.

    ![Elección de método para el inicio de sesión predeterminado](media/security-info/securityinfo-myprofile-phonetext-changeddefault.png)

    El método predeterminado que se usa para el inicio de sesión se cambia en **Teléfono: texto (*_su_número_de_teléfono_*)** .

## <a name="additional-security-info-methods"></a>Métodos de información de seguridad adicionales

Tiene opciones adicionales para determinar cómo su organización se pone en contacto con usted para comprobar su identidad, basándose en lo está intentando hacer. Entre estas opciones se incluyen:

- **Aplicación autenticadora.** Descargue y use una aplicación autenticadora para obtener una notificación de aprobación o un código de aprobación generado de forma aleatoria para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo configurar y usar la aplicación Microsoft Authenticator, consulte [Set up security info to use an authenticator app](security-info-setup-auth-app.md) (Configuración de la información de seguridad para usar una aplicación de autenticador).

- **Llamada a dispositivo móvil o al teléfono del trabajo.** Escriba el número del dispositivo móvil y recibirá una llamada telefónica para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo comprobar su identidad con un número de teléfono, consulte [Configuración de la información de seguridad para usar llamadas de teléfono](security-info-setup-phone-number.md).

- **Clave de seguridad.** Registre la clave de seguridad compatible con Microsoft y úsela junto con un PIN para la verificación en dos pasos o el restablecimiento de contraseña. Para instrucciones paso a paso sobre cómo comprobar su identidad con una clave de seguridad, consulte [Configuración de la información de seguridad para usar una clave de seguridad](security-info-setup-security-key.md).

- **Dirección de correo electrónico.** Escriba la dirección de correo electrónico profesional o educativa para recibir un correo electrónico para el restablecimiento de contraseña. Esta opción no está disponible para la verificación en dos pasos. Para obtener instrucciones detalladas sobre cómo configurar el correo electrónico, vea [Configuración de la información de seguridad para usar el correo electrónico](security-info-setup-email.md).

- **Preguntas de seguridad.** Responda a algunas preguntas de seguridad creadas por el administrador para su organización. Esta opción solo está disponible para el restablecimiento de contraseña y no para la verificación en dos pasos. Para obtener instrucciones paso a paso sobre cómo configurar las preguntas de seguridad, consulte el artículo [Configuración de la información de seguridad para usar las preguntas de seguridad](security-info-setup-questions.md).

    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="next-steps"></a>Pasos siguientes

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](active-directory-passwords-update-your-own-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
