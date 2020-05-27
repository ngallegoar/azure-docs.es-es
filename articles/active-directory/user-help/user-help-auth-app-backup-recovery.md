---
title: 'Copia de seguridad y recuperación de cuentas con la aplicación Microsoft Authenticator: Azure AD'
description: Obtenga información sobre cómo realizar copias de seguridad y recuperar sus credenciales de cuenta de la copia de seguridad con la aplicación Microsoft Authenticator.
services: active-directory
author: curtand
manager: daveba
ms.subservice: user-help
ms.service: active-directory
ms.workload: identity
ms.topic: end-user-help
ms.date: 02/25/2020
ms.author: curtand
ms.reviewer: olhaun
ms.openlocfilehash: f78ef7ddf48612728c0409b0f904c0964e2272b5
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83741592"
---
# <a name="back-up-and-recover-account-credentials-using-the-microsoft-authenticator-app"></a>Copia de seguridad y recuperación de las credenciales de cuenta con la aplicación Microsoft Authenticator

**Se aplica a:**

- Dispositivos iOS que ejecutan la versión 5.7.0 y versiones posteriores

- Dispositivos Android que ejecutan la versión 6.6.0 y versiones posteriores

La aplicación Microsoft Authenticator realiza una copia de seguridad en la nube de las credenciales de la cuenta y la configuración de aplicación relacionada, como el orden de las cuentas. Después de la copia de seguridad, también puede utilizar la aplicación para recuperar la información en un dispositivo nuevo y evitar así un posible bloqueo o tener que volver a crear las cuentas.

Cada ubicación de almacenamiento de copia de seguridad requiere que tenga una cuenta Microsoft personal, mientras que iOS también requiere que tenga una cuenta de iCloud. Puede tener varias cuentas almacenadas en esa única ubicación. Por ejemplo, puede tener una cuenta personal, una cuenta profesional o educativa, y una cuenta personal que no sea de Microsoft, como Facebook, Google, etc.

> [!IMPORTANT]
> Solo se almacenan las credenciales de la cuenta personal y de terceros, lo que incluye el nombre de usuario y el código de verificación de la cuenta necesarios para probar la identidad. No almacenamos ninguna otra información asociada con sus cuentas, como mensajes de correo electrónico o archivos. Tampoco asociamos ni compartimos sus cuentas de ningún modo con otros productos ni servicios. Finalmente, su administrador de TI no obtendrá información de ninguna de estas cuentas.

## <a name="back-up-your-account-credentials"></a>Copia de seguridad de las credenciales de cuenta

Antes de hacer copias de seguridad de las credenciales, debe tener:

- Una [cuenta Microsoft](https://account.microsoft.com/account) personal como cuenta de recuperación.

- **Solo para iOS**, debe tener una [cuenta de iCloud](https://www.icloud.com/) para la ubicación de almacenamiento real.

### <a name="to-turn-on-cloud-backup-for-ios-devices"></a>Para activar la copia de seguridad en la nube para dispositivos iOS

- En el dispositivo iOS, seleccione **Ajustes**, **Copia de seguridad** y active **Copia en iCloud**.

    Las credenciales de cuenta se copian en la cuenta de iCloud.

    ![Pantalla de configuración de iOS que muestra la ubicación de los valores de Copia en iCloud](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on.png)

### <a name="to-turn-on-cloud-backup-for-android-devices"></a>Para activar la copia de seguridad en la nube para dispositivos Android

- En el dispositivo Android, seleccione **Configuración**, **Copia de seguridad** y active **Copia de seguridad en la nube**.

    Las credenciales de la cuenta se copian en la cuenta en la nube.

    ![Pantalla de configuración de Android que muestra la ubicación de la configuración de la copia de seguridad](./media/user-help-auth-app-backup-recovery/backup-and-recovery-turn-on-android.png)

## <a name="recover-your-account-credentials-on-your-new-device"></a>Recuperación de las credenciales de cuenta en el dispositivo nuevo

Puede recuperar las credenciales de la cuenta desde su cuenta en la nube, pero primero debe asegurarse de que la cuenta que está recuperando no existe en la aplicación Microsoft Authenticator. Por ejemplo, si está recuperando su cuenta Microsoft personal, debe asegurarse de que no tiene ya una cuenta Microsoft personal configurada en la aplicación Authenticator. Esta comprobación es importante para asegurarnos de que no estamos sobrescribiendo ni borrando una cuenta existente por equivocación.

### <a name="to-recover-your-information"></a>Recuperación de la información

1. En el dispositivo móvil, abra la aplicación Microsoft Authenticator y seleccione **Iniciar recuperación** en la parte inferior de la pantalla.

    ![Aplicación de Microsoft Authenticator que muestra dónde debe hacer clic en Iniciar recuperación](./media/user-help-auth-app-backup-recovery/backup-and-recovery-begin-recovery.png)

2. Inicie sesión en la cuenta de recuperación con la misma cuenta Microsoft personal que utilizó durante la copia de seguridad.

    Las credenciales de la cuenta se recuperan en el dispositivo nuevo.

Una vez finalizada la recuperación, podrá notar que los códigos de verificación de la cuenta Microsoft personal en la aplicación Microsoft Authenticator difieren en el teléfono nuevo y el antiguo. Esto es así porque cada dispositivo tiene sus propias credenciales únicas, aunque ambas son válidas y funcionan al iniciar sesión con el teléfono asociado.

## <a name="recover-additional-accounts-requiring-more-verification"></a>Recuperación de otras cuentas con más detalles de verificación

Si usa notificaciones push con las cuentas personales, profesionales o educativas, recibirá una alerta en la pantalla para que proporcione más detalles de verificación para recuperar la información. Dado que las notificaciones push requieren credenciales vinculadas con los dispositivos específicos y no se envían a través de la red, debe demostrar su identidad para que se creen en el dispositivo.

Para las cuentas Microsoft personales, puede probar su identidad mediante la especificación de la contraseña junto con un número de teléfono o correo electrónico alternativos. Para las cuentas profesionales o educativas, debe digitalizar un código QR que le proporcione el proveedor de la cuenta.

### <a name="to-provide-additional-verification-for-personal-accounts"></a>Proporcionar verificación adicional para las cuentas personales

1. En la pantalla **Cuentas** de la aplicación Microsoft Authenticator, seleccione la cuenta que desea recuperar. En un dispositivo Android, seleccione la flecha situada junto a la cuenta que desea recuperar.

    ![Aplicación Microsoft Authenticator que muestra las cuentas disponibles con las flechas desplegables asociadas](./media/user-help-auth-app-backup-recovery/backup-and-recovery-arrow.png)

    En un dispositivo iOS, pulse la cuenta que desea recuperar para abrir la vista de pantalla completa de la cuenta.

    ![Aplicación Microsoft Authenticator que muestra las cuentas disponibles con las flechas desplegables asociadas](./media/user-help-auth-app-backup-recovery/ios-select-account-to-recover.png)

2. Inicie sesión para recuperar la cuenta. En un dispositivo Android, seleccione **Iniciar sesión para completar la recuperación**.

    ![Aplicación Microsoft Authenticator para escribir la información de inicio de sesión en Android](./media/user-help-auth-app-backup-recovery/backup-and-recovery-sign-in.png)

    En un dispositivo iOS, pulse el icono de la cuenta que quiere recuperar y, después, pulse la opción para iniciar sesión y completar la recuperación. A continuación, escriba la contraseña y confirme el número de teléfono o la dirección de correo electrónico como verificación adicional.

    ![Aplicación Microsoft Authenticator para escribir la información de inicio de sesión en iOS](./media/user-help-auth-app-backup-recovery/ios-sign-in-to-recover.png)

### <a name="to-provide-additional-verification-for-work-or-school-accounts"></a>Proporcionar verificación adicional para las cuentas profesionales o educativas

1. Inicie sesión para recuperar la cuenta. En un dispositivo Android, seleccione **Iniciar sesión para completar la recuperación**.

    ![Aplicación Microsoft Authenticator recuperando una cuenta profesional o educativa en Android](./media/user-help-auth-app-backup-recovery/backup-and-recovery-additional-accts.png)

    En un dispositivo iOS, pulse la cuenta que desea recuperar para abrir la vista de pantalla completa de la cuenta.

    ![Aplicación Microsoft Authenticator recuperando una cuenta profesional o educativa en iOS](./media/user-help-auth-app-backup-recovery/ios-recover-work-or-school-account.png)

2. Puede usar un código QR para recuperar una cuenta. Seleccione **Digitalizar código QR para completar la recuperación** y lea el código QR.

    En Android:

    ![Aplicación Microsoft Authenticator en Android, que permite digitalizar el código QR](./media/user-help-auth-app-backup-recovery/backup-and-recovery-scan-qr-code.png)

    En iOS:

    ![Aplicación Microsoft Authenticator en iOS, que permite digitalizar el código QR](./media/user-help-auth-app-backup-recovery/ios-scan-qr-code.png)

    >[!NOTE]
    >Para más información acerca de los códigos QR y cómo obtener uno, consulte [Introducción a la aplicación Microsoft Authenticator](https://docs.microsoft.com/azure/active-directory/user-help/user-help-auth-app-download-install) o [Configuración de la información de seguridad para usar una aplicación autenticadora](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-auth-app), en función de si el administrador ha activado la información de seguridad.
    >
    >Si es la primera vez que configura la aplicación Microsoft Authenticator, es posible que reciba un mensaje en el que se le pregunta si quiere permitir que la aplicación acceda a la cámara (iOS) o permitir que la aplicación tome fotografías y grabe vídeo (Android). Debe seleccionar **Permitir** para que la aplicación de autenticación pueda acceder a la cámara y tomar una fotografía del código QR en el paso siguiente. Aunque no otorgue permiso a la cámara, podrá configurar la aplicación de autenticación, pero tendrá que agregar la información de código manualmente. Para más información sobre cómo agregar manualmente código, consulte [Agregar manualmente una cuenta a la aplicación](user-help-auth-app-add-account-manual.md).

## <a name="troubleshoot-backup-and-recovery-problems"></a>Solución de problemas de copia de seguridad y recuperación

Existen diversos motivos para que la copia de seguridad no esté disponible

- **Cambio de sistema operativo**. La copia de seguridad se almacena en iCloud para iOS y en el proveedor de almacenamiento en la nube de Microsoft para Android. Esto significa que la copia de seguridad no está disponible si se cambia entre dispositivos iOS y Android. Si realiza el cambio, debe volver a crear las cuentas manualmente en la aplicación Microsoft Authenticator.

- **Problemas de red**. Si tiene problemas relacionados con la red, asegúrese de que está conectado a la red y de que ha iniciado sesión correctamente en su cuenta.

- **Problemas de cuenta**. Si tiene problemas relacionados con la cuenta, asegúrese de que ha iniciado sesión correctamente en la cuenta. En el caso de iOS, esto implica que debe haber iniciado sesión en iCloud con la misma cuenta de Apple que en el iPhone.

- **Eliminación por error**. Es posible que haya eliminado la cuenta de copia de seguridad del dispositivo anterior o al administrar la cuenta de almacenamiento en la nube. En esta situación, debe volver a crear manualmente la cuenta en la aplicación.

- **Cuentas de Microsoft Authenticator existentes**. Si ya ha configurado cuentas en la aplicación Microsoft Authenticator, la aplicación no podrá recuperar las cuentas de copia de seguridad. Evitar la recuperación garantiza que los datos de la cuenta no se sobreescriben con información obsoleta. En esta situación, debe eliminar cualquier información de cuenta existente de la configuración de las cuentas existentes en la aplicación Authenticator para recuperar la copia de seguridad.

- **La copia de seguridad está obsoleta**. Si la información de copia de seguridad no está actualizada, es posible que se le pida que actualice la información. Para ello, vuelva a iniciar sesión en su cuenta de recuperación de Microsoft. La cuenta de recuperación es la cuenta Microsoft personal que usó inicialmente para almacenar la copia de seguridad. Si se requiere un inicio de sesión, verá un punto rojo en el menú o la barra de acciones, o verá un signo de exclamación que le pedirá que inicie sesión para finalizar la restauración a partir de la copia de seguridad. Después de seleccionar el icono, se le pedirá que vuelva a iniciar sesión para actualizar la información.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha realizado la copia de seguridad de las credenciales de la cuenta y las ha recuperado en un dispositivo nuevo, use la aplicación Microsoft Authenticator para verificar su identidad. Para más información, consulte [Inicio de sesión en sus cuentas mediante la aplicación Microsoft Authenticator](user-help-sign-in.md).

## <a name="related-articles"></a>Artículos relacionados

- [¿Qué es la aplicación Microsoft Authenticator?](user-help-auth-app-overview.md)

- [Preguntas frecuentes de la aplicación Microsoft Authenticator](user-help-auth-app-faq.md)

- [Multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/)
