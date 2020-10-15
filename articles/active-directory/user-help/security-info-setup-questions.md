---
title: 'Configuración de preguntas de seguridad como método de comprobación: Azure AD'
description: Cómo configurar su página de información de seguridad (versión preliminar) para comprobar su identidad mediante preguntas de seguridad predefinidas como método de comprobación.
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
ms.openlocfilehash: 0c55f15d12ddc51573f59f56c742a8cac525af0b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83744430"
---
# <a name="set-up-security-questions-as-your-verification-method"></a>Configuración de preguntas de seguridad como método de comprobación

Puede seguir estos pasos para agregar su método de restablecimiento de la contraseña. Una vez que haya configurado esto por primera vez, puede volver a la página **Información de seguridad** para agregar, actualizar o eliminar su información de seguridad.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

>[!Note]
>Si no ve la opción de preguntas de seguridad, es posible que su organización no le permita usar esta opción para el método de restablecimiento de contraseña. En este caso, tendrá que elegir otro método o ponerse en contacto con el departamento de soporte técnico de su organización para que le proporcione más ayuda.

## <a name="security-verification-versus-password-reset-authentication"></a>Autenticación mediante comprobación de seguridad frente a autenticación mediante restablecimiento de contraseña

Los métodos de la información de seguridad se usan tanto para la comprobación de seguridad de dos factores como para al restablecimiento de la contraseña. Sin embargo, no todos los métodos pueden usarse para ambos.

| Método | Se usa para |
| ------ | -------- |
| Aplicación de autenticación | Autenticación mediante la comprobación de dos factores y restablecimiento de la contraseña. |
| Mensajes de texto | Autenticación mediante la comprobación de dos factores y restablecimiento de la contraseña. |
| Llamadas de teléfono | Autenticación mediante la comprobación de dos factores y restablecimiento de la contraseña. |
| Clave de seguridad | Autenticación mediante la comprobación de dos factores y restablecimiento de la contraseña. |
| Cuenta de correo electrónico | Solo autenticación mediante restablecimiento de contraseña. Deberá elegir otro método para la comprobación mediante dos factores. |
| Preguntas de seguridad | Solo autenticación mediante restablecimiento de contraseña. Deberá elegir otro método para la comprobación mediante dos factores. |

## <a name="set-up-your-security-questions-from-the-security-info-page"></a>Configuración de las preguntas de seguridad desde la página Información de seguridad

En función de la configuración de su organización, puede elegir responder algunas preguntas de seguridad como método de información de seguridad. El administrador configura el número de preguntas de seguridad que se deben elegir responder.

Si usa preguntas de seguridad, le recomendamos que las use junto con otro método. Las preguntas de seguridad pueden ser menos seguras que otros métodos porque es posible que algunas personas conozcan las respuestas.

> [!Note]
> Las preguntas de seguridad se almacenan de manera privada y segura en un objeto de usuario del directorio y solo las puede responder el usuario durante el registro. Un administrador no puede leer o modificar de ninguna forma sus preguntas o respuestas.
>
> Si no ve la opción de preguntas de seguridad, es posible que su organización no le permite usar preguntas de seguridad para la comprobación. Si este es el caso, tendrá que elegir otro método o ponerse en contacto con su administrador para obtener más ayuda.
>
> Las cuentas de administrador no pueden usar preguntas de seguridad como método de restablecimiento de contraseña. Si ha iniciado sesión con una cuenta de nivel administrador, no verá estas opciones.

### <a name="to-set-up-your-security-questions"></a>Para configurar las preguntas de seguridad

1. Inicie sesión en su cuenta profesional o educativa y vaya a la página https://myaccount.microsoft.com/.

    ![Página Mi Perfil, que muestra los vínculos de Información de seguridad resaltados](media/security-info/securityinfo-myprofile-page.png)

2. Seleccione **Información de seguridad** en el panel de navegación izquierdo o en el vínculo del bloque **Información de seguridad** y, después, seleccione **Agregar método** en la página **Información de seguridad**.

    ![Página Información de seguridad con la opción Agregar método resaltada](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. En la página **Agregar un método**, seleccione **Preguntas de seguridad** en la lista desplegable y, después, seleccione **Agregar**.

    ![Cuadro Agregar método, con preguntas de seguridad seleccionadas](media/security-info/securityinfo-myprofile-addquestions.png)

4. En la página **Preguntas de seguridad**, elija las preguntas de seguridad, respóndalas y seleccione **Guardar**.

    ![Adición de un número de teléfono y elección de llamadas de teléfono](media/security-info/securityinfo-myprofile-securityquestions.png)

    La información de seguridad se actualiza y puede usar las preguntas de seguridad para comprobar su identidad al usar el restablecimiento de contraseña.

## <a name="delete-security-questions-from-your-security-info-methods"></a>Eliminación de las preguntas de seguridad de los métodos de información de seguridad

Si no desea usar las preguntas de seguridad como método de información de seguridad, puede quitarla de la página **Información de seguridad**.

>[!Important]
>Si elimina las preguntas de seguridad por error, no hay forma de deshacer la operación. Tendrá que volver a agregar el método, para lo que debe seguir los pasos de la sección sobre cómo [configurar las preguntas de seguridad](#set-up-your-security-questions-from-the-security-info-page) de este mismo artículo.

### <a name="to-delete-your-security-questions"></a>Para eliminar las preguntas de seguridad

1. En la página **Información de seguridad**, seleccione el vínculo **Eliminar**, que se encuentra al lado de la opción **Preguntas de seguridad**.

    ![Vínculo para eliminar el método del teléfono de la información de seguridad](media/security-info/securityinfo-myprofile-questionsdelete.png)

2. Seleccione **Sí** en el cuadro de confirmación para eliminar las **preguntas de seguridad**. Una vez se eliminen las preguntas de seguridad, el método se quitará de su información de seguridad y desaparecerá de la página **Información de seguridad**.

## <a name="additional-security-info-methods"></a>Métodos de información de seguridad adicionales

Tiene opciones adicionales para determinar cómo su organización se pone en contacto con usted para comprobar su identidad, basándose en lo está intentando hacer. Entre estas opciones se incluyen:

- **Aplicación autenticadora.** Descargue y use una aplicación autenticadora para obtener una notificación de aprobación o un código de aprobación generado de forma aleatoria para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo configurar y usar la aplicación Microsoft Authenticator, consulte [Set up security info to use an authenticator app](security-info-setup-auth-app.md) (Configuración de la información de seguridad para usar una aplicación de autenticador).

- **Texto de dispositivo móvil.** Escriba el número del dispositivo móvil y obtendrá un código que podrá usar para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones detalladas sobre cómo verificar su identidad con un mensaje de texto (SMS), vea [Configuración de la información de seguridad para usar la mensajería de texto (SMS)](security-info-setup-text-msg.md).

- **Llamada a dispositivo móvil o al teléfono del trabajo.** Escriba el número del dispositivo móvil y recibirá una llamada telefónica para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo comprobar su identidad con un número de teléfono, consulte [Configuración de la información de seguridad para usar llamadas de teléfono](security-info-setup-phone-number.md).

- **Clave de seguridad.** Registre la clave de seguridad compatible con Microsoft y úsela junto con un PIN para la verificación en dos pasos o el restablecimiento de contraseña. Para instrucciones paso a paso sobre cómo comprobar su identidad con una clave de seguridad, consulte [Configuración de la información de seguridad para usar una clave de seguridad](security-info-setup-security-key.md).

- **Dirección de correo electrónico.** Escriba la dirección de correo electrónico profesional o educativa para recibir un correo electrónico para el restablecimiento de contraseña. Esta opción no está disponible para la verificación en dos pasos. Para obtener instrucciones detalladas sobre cómo configurar el correo electrónico, vea [Configuración de la información de seguridad para usar el correo electrónico](security-info-setup-email.md).

    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="next-steps"></a>Pasos siguientes

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](active-directory-passwords-update-your-own-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
