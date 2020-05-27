---
title: 'Incorporación de una cuenta personal de Microsoft a la aplicación Microsoft Authenticator: Azure AD'
description: Agregue cuentas personales de Microsoft, como Outlook.com o Xbox LIVE, a la aplicación Microsoft Authenticator para comprobar su identidad mientras usa la verificación en dos fases.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 04/08/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: ff83a9a4e4bfd4c27dd3f8d3f212e489c3772eb4
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83741623"
---
# <a name="add-personal-microsoft-accounts-to-the-microsoft-authenticator-app"></a>Incorporación de cuentas personales Microsoft a la aplicación Microsoft Authenticator

Agregue sus cuentas personales de Microsoft, como las cuentas de Outlook.com y Xbox LIVE, a la aplicación Microsoft Authenticator tanto para el proceso de verificación en dos fases estándar como para el método de inicio de sesión por teléfono sin contraseña.

- **Método de verificación en dos fases estándar.** Escriba el nombre de usuario y la contraseña en el dispositivo en el que va a iniciar sesión y luego elija si la aplicación Microsoft Authenticator envía una notificación, o si prefiere copiar el código de verificación asociado desde la pantalla **Cuentas** de la aplicación Microsoft Authenticator.

- **Método de inicio de sesión sin contraseña.** Escriba el nombre de usuario en el dispositivo en el que va a iniciar sesión con su cuenta personal de Microsoft y, después, use su dispositivo móvil para comprobar que es usted mediante el uso de la huella digital, la cara o un PIN. Para este método, no es preciso que escriba su contraseña.

>[!Important]
>Antes de agregar su cuenta, debe descargar e instalar la aplicación Microsoft Authenticator. Si no lo ha hecho aún, siga los pasos descritos en el artículo para [descargar e instalar la aplicación](user-help-auth-app-download-install.md).

Puede agregar su cuenta personal de Microsoft activando primero la verificación en dos fases y, luego, agregando la cuenta a la aplicación. No es necesario activar la verificación en dos fases para usar solo el inicio de sesión por teléfono sin contraseña para su cuenta, pero se recomienda encarecidamente activar la verificación en dos fases para la seguridad adicional de la cuenta.

## <a name="turn-on-two-factor-verification"></a>Activación de la verificación en dos fases

1. En su equipo, vaya a la página [Security basics](https://account.microsoft.com/security) (Aspectos básicos de seguridad) e inicie sesión con su cuenta personal de Microsoft. Por ejemplo, alain@outlook.com.

2. En la parte inferior de la página **Security basics**, elija el vínculo **more security options** (más opciones de seguridad).

    ![Página de aspectos básicos de seguridad con el vínculo de más opciones de seguridad resaltado](./media/user-help-auth-app-add-personal-ms-account/more-security-options-link.png)

3. Vaya a la sección **Verificación en dos pasos** y elija **activar** la característica. También puede desactivarla aquí si ya no quiere usarla con su cuenta personal.

## <a name="add-your-microsoft-account-to-the-app"></a>Adición de su cuenta de Microsoft a la aplicación

1. Abra la aplicación Microsoft Authenticator en el dispositivo móvil.

1. En Android, seleccione **Agregar cuenta** desde el icono **Personalizar y controlar** en la esquina superior derecha.

    ![Páginas de selección de cuenta de Android](media/user-help-auth-app-add-personal-ms-account/customize-and-control-icon.png)

    En iOS, seleccione el icono de signo más en la esquina superior derecha.

    ![Versión de iOS de la experiencia de selección de cuenta](media/user-help-auth-app-add-personal-ms-account/customize-and-control-icon-ios.png)

1. En la página **Agregar cuenta**, elija **Cuenta personal**.

1. Seleccione **Iniciar sesión con Microsoft** para agregar su cuenta. Se puede usar un código QR cuando esté disponible, pero siempre puede agregar su cuenta mediante el inicio de sesión con su nombre de usuario y contraseña.

    ![Seleccione un cuenta de Microsoft o digitalice un código QR cuando esté disponible.](media/user-help-auth-app-add-personal-ms-account/add-account-android.png)

1. Inicie sesión en su cuenta personal, con la dirección de correo electrónico adecuada (como alain@outlook.com) y, luego, seleccione **Siguiente**.

    >[!Note]
    >Si no tiene una cuenta personal de Microsoft, [puede crear una aquí](https://account.microsoft.com/account/Account?refd=www.bing.com&ru=https%3A%2F%2Faccount.microsoft.com%2F%3Frefd%3Dwww.bing.com&destrt=home-index).

1. Escriba la contraseña y seleccione **Iniciar sesión**. Su cuenta personal se agrega a la aplicación Microsoft Authenticator.

## <a name="next-steps"></a>Pasos siguientes

- Después de agregar las cuentas a la aplicación, puede iniciar sesión con la aplicación Authenticator en su dispositivo. Para más información, consulte cómo [iniciar sesión con la aplicación](user-help-auth-app-sign-in.md).

- Si tiene dificultades para obtener el código de verificación de su cuenta Microsoft personal, consulte la sección **Solución de problemas de código de seguridad** del artículo [Información de seguridad y códigos de seguridad de la cuenta de Microsoft](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes).

- En cuanto a los dispositivos con iOS, también puede hacer una copia de seguridad en la nube de las credenciales de su cuenta y de la configuración relacionada de la aplicación, como el orden de las cuentas. Para obtener más información, consulte [Copia seguridad y recuperación con la aplicación Microsoft Authenticator](user-help-auth-app-backup-recovery.md).
