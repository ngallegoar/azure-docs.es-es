---
title: 'Configuración del número de teléfono como método de verificación: Azure AD'
description: Configuración de su página de información de seguridad (versión preliminar) para comprobar su identidad mediante el número de teléfono y el dispositivo móvil como método de comprobación.
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
ms.openlocfilehash: 7500b6b146a627dcdebacd9d2e2c7c61bc43c105
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744445"
---
# <a name="set-up-a-phone-number-as-your-verification-method"></a>Configuración de un número de teléfono como método de comprobación

Puede seguir estos pasos para agregar sus métodos de restablecimiento de contraseña y verificación en dos pasos. Una vez que haya configurado esto por primera vez, puede volver a la página **Información de seguridad** para agregar, actualizar o eliminar su información de seguridad.

Si se le pide que configure esto justo después de haber iniciado sesión en su cuenta profesional o educativa, consulte los pasos detallados en el artículo de [configuración de su información de seguridad desde el símbolo del sistema de página de inicio de sesión](security-info-setup-signin.md).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

> [!Note]
> Información de seguridad no admite el uso de extensiones telefónicas. Incluso si agrega el formato correcto +1 4255551234X12345, las extensiones se quitan antes de realizarse la llamada.
>
> Si no ve la opción de teléfono, es posible que su organización no le permita usar esta opción para la comprobación. En este caso, tendrá que elegir otro método o ponerse en contacto con el departamento de soporte técnico de su organización para que le proporcione más ayuda.

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

## <a name="set-up-phone-calls-from-the-security-info-page"></a>Configuración de llamadas de teléfono desde la página Información de seguridad

Dependiendo de la configuración de su organización, es posible que pueda usar llamadas de teléfono como uno de sus métodos de información de seguridad.

>[!Note]
>Si desea recibir un mensaje de texto en lugar de una llamada de teléfono, siga los pasos descritos en el artículo de [configuración de la información de seguridad para usar mensajería de texto](security-info-setup-text-msg.md).

### <a name="to-set-up-phone-calls"></a>Para configurar llamadas de teléfono

1. Inicie sesión en su cuenta profesional o educativa y vaya a la página https://myaccount.microsoft.com/.

    ![Página Mi Perfil, que muestra los vínculos de Información de seguridad resaltados](media/security-info/securityinfo-myprofile-page.png)

2. Seleccione **Información de seguridad** en el panel de navegación izquierdo o en el vínculo del bloque **Información de seguridad** y, después, seleccione **Agregar método** en la página **Información de seguridad**.

    ![Página Información de seguridad con la opción Agregar método resaltada](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. En la página **Agregar un método**, seleccione **Teléfono** en la lista desplegable y, después, seleccione **Agregar**.

    ![Cuadro Agregar método, con Teléfono seleccionado](media/security-info/securityinfo-myprofile-addphonetext.png)

4. En la página **Teléfono**, escriba el número de teléfono de su dispositivo móvil, elija **Llamarme** y, a continuación, seleccione **Siguiente**.

    ![Adición de un número de teléfono y elección de llamadas de teléfono](media/security-info/securityinfo-myprofile-phonecall-addnumber.png)

5. Responda a la llamada de teléfono de verificación, enviada al número de teléfono que escribió, y siga las instrucciones.

    La página cambia para mostrar que la operación se ha realizado correctamente.

    ![Notificación de éxito, que conecta el número de teléfono, la opción para recibir llamadas de teléfono y su cuenta](media/security-info/securityinfo-myprofile-phonetext-success.png)

    La información de seguridad se actualiza y puede usar llamadas de teléfono para comprobar su identidad al usar la comprobación en dos pasos o el restablecimiento de contraseña. Si desea hacer que las llamadas de teléfono sean su método predeterminado, consulte la sección sobre cómo [cambiar su método de información de seguridad predeterminado](#change-your-default-security-info-method) de este artículo.

## <a name="delete-phone-calls-from-your-security-info-methods"></a>Eliminación de llamadas de teléfono de sus métodos de información de seguridad

Si ya no desea usar las llamadas de teléfono como método de información de seguridad, puede quitarlo de la página **Información de seguridad**.

>[!Important]
>Si elimina las llamadas de teléfono por error, no existe modo alguno de deshacerlo. Tendrá que volver a agregar el método, siguiendo los pasos de la sección sobre cómo [configurar llamadas de teléfono](#set-up-phone-calls-from-the-security-info-page) de este artículo.

### <a name="to-delete-phone-calls"></a>Para eliminar las llamadas telefónicas

1. En la página **Información de seguridad**, seleccione el vínculo **Eliminar**, que se encuentra al lado de la opción **Teléfono**.

    ![Vínculo para eliminar el método del teléfono de la información de seguridad](media/security-info/securityinfo-myprofile-phonetext-delete.png)

2. Seleccione **Sí** en el cuadro de confirmación para eliminar el número de **teléfono**. Una vez que su número de teléfono se elimine, se quitará de su información de seguridad y desaparecerá de la página **Información de seguridad**. Si **Teléfono** es su método predeterminado, el valor predeterminado cambiará a otro método disponible.

## <a name="change-your-default-security-info-method"></a>Cambio de su método de información de seguridad predeterminado

Si desea que las llamadas de teléfono sean el método predeterminado usado al iniciar sesión en su cuenta profesional o educativa mediante la comprobación en dos pasos o para las solicitudes de restablecimiento de contraseña, puede establecerlo en la página **Información de seguridad**.

### <a name="to-change-your-default-security-info-method"></a>Para cambiar el método de información de seguridad predeterminado

1. En la página **Información de seguridad**, seleccione el vínculo **Cambiar** situado junto a la información **Método de inicio de sesión predeterminado**.

    ![Cambio de vínculo del método de inicio de sesión predeterminado](media/security-info/securityinfo-myprofile-phonetext-defaultchange.png)

2. Seleccione **Teléfono: llamada (*_su_número_de_teléfono_*)** en la lista desplegable de métodos disponibles y, a continuación, seleccione **Confirmar**.

    ![Elección de método para el inicio de sesión predeterminado](media/security-info/securityinfo-myprofile-phonecall-changeddefault.png)

    El método predeterminado usado para el inicio de sesión se cambia en **Teléfono: llamada (*_su_número_de_teléfono_*)** .

## <a name="additional-security-info-methods"></a>Métodos de información de seguridad adicionales

Tiene opciones adicionales para determinar cómo su organización se pone en contacto con usted para comprobar su identidad, basándose en lo está intentando hacer. Entre estas opciones se incluyen:

- **Aplicación autenticadora.** Descargue y use una aplicación autenticadora para obtener una notificación de aprobación o un código de aprobación generado de forma aleatoria para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones paso a paso sobre cómo configurar y usar la aplicación Microsoft Authenticator, consulte [Set up security info to use an authenticator app](security-info-setup-auth-app.md) (Configuración de la información de seguridad para usar una aplicación de autenticador).

- **Texto de dispositivo móvil.** Escriba el número del dispositivo móvil y obtendrá un código que podrá usar para la verificación en dos pasos o el restablecimiento de contraseña. Para obtener instrucciones detalladas sobre cómo verificar su identidad con un mensaje de texto (SMS), vea [Configuración de la información de seguridad para usar la mensajería de texto (SMS)](security-info-setup-text-msg.md).

- **Clave de seguridad.** Registre la clave de seguridad compatible con Microsoft y úsela junto con un PIN para la verificación en dos pasos o el restablecimiento de contraseña. Para instrucciones paso a paso sobre cómo comprobar su identidad con una clave de seguridad, consulte [Configuración de la información de seguridad para usar una clave de seguridad](security-info-setup-security-key.md).

- **Dirección de correo electrónico.** Escriba la dirección de correo electrónico profesional o educativa para recibir un correo electrónico para el restablecimiento de contraseña. Esta opción no está disponible para la verificación en dos pasos. Para obtener instrucciones detalladas sobre cómo configurar el correo electrónico, vea [Configuración de la información de seguridad para usar el correo electrónico](security-info-setup-email.md).

- **Preguntas de seguridad.** Responda a algunas preguntas de seguridad creadas por el administrador para su organización. Esta opción solo está disponible para el restablecimiento de contraseña y no para la verificación en dos pasos. Para obtener instrucciones paso a paso sobre cómo configurar las preguntas de seguridad, consulte el artículo [Configuración de la información de seguridad para usar las preguntas de seguridad](security-info-setup-questions.md).

    >[!Note]
    >Si faltan algunas de estas opciones, lo más probable es que su organización no permita esos métodos. Si este es el caso, tendrá que elegir uno de los métodos que sí están disponibles o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="next-steps"></a>Pasos siguientes

- Restablezca la contraseña si ha perdido u olvidado la que tenía acudiendo al [portal de restablecimiento de contraseña](https://passwordreset.microsoftonline.com/) o siga los pasos descritos en el artículo [Restablecimiento de la contraseña profesional o educativa](active-directory-passwords-update-your-own-password.md).

- Obtenga soluciones de problemas, sugerencias y ayuda para los problemas de inicio de sesión en el artículo [Cuando no puedes iniciar sesión en tu cuenta de Microsoft](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant).
