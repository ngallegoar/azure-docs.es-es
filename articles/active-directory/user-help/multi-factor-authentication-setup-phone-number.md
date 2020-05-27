---
title: 'Configuración de un dispositivo móvil como método de verificación en dos fases: Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar un dispositivo móvil como método de verificación en dos fases.
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 08/12/2019
ms.author: curtand
ms.openlocfilehash: 76809a41400502ec48c2dd674f8976bb168ec9f9
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83735997"
---
# <a name="set-up-a-mobile-device-as-your-two-factor-verification-method"></a>Configuración de un dispositivo móvil como método de verificación en dos fases

Es posible configurar el dispositivo móvil para que actúe como método de verificación en dos fases. En el teléfono móvil se puede recibir un mensaje de texto con un código de verificación o bien una llamada telefónica.

>[!Note]
> Si la opción de teléfono de autenticación está atenuada, es posible que su organización no le permita usar un número de teléfono o un mensaje de texto para la verificación. En este caso, tendrá que seleccionar otro método o ponerse en contacto con su administrador para obtener más ayuda.

## <a name="set-up-your-mobile-device-to-use-a-text-message-as-your-verification-method"></a>Configuración del dispositivo móvil para usar un mensaje de texto como método de verificación

1. En la página **Comprobación de seguridad adicional**, seleccione **Teléfono de autenticación** en el área **Paso 1: ¿De qué manera deberíamos ponernos en contacto con usted?** , seleccione su país o región en la lista desplegable y, después, escriba el número de teléfono del dispositivo móvil.

2. En el área **Método**, seleccione **Enviarme un código mediante mensaje de texto** y, después, seleccione **Siguiente**.

    ![Página Comprobación de seguridad adicional, con teléfono de autenticación y mensaje de texto](media/multi-factor-authentication-verification-methods/multi-factor-authentication-text-message.png)

3. Escriba el código de verificación del mensaje de texto enviado por Microsoft en el área **Paso 2: Hemos enviado un mensaje de texto a su teléfono** y, después, seleccione **Verificar**.

    ![Página Comprobación de seguridad adicional, con teléfono de autenticación y mensaje de texto](media/multi-factor-authentication-verification-methods/multi-factor-authentication-text-message-test.png)

4. En el área **Paso 3: Siga usando sus aplicaciones existentes** se indicará una contraseña de aplicación; cópiela y péguela en un lugar seguro.

    ![Área de contraseñas de aplicación de la página Comprobación de seguridad adicional](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Para obtener información sobre el uso de la contraseña de aplicación con las aplicaciones antiguas, consulte [Administración de las contraseñas de aplicaciones](multi-factor-authentication-end-user-app-passwords.md). Solo tiene que usar contraseñas de aplicación si va a seguir usando aplicaciones antiguas que no admiten la verificación en dos fases.

5. Seleccione **Listo**.

## <a name="set-up-your-mobile-device-to-receive-a-phone-call"></a>Configuración del dispositivo móvil para recibir una llamada de teléfono

1. En la página **Comprobación de seguridad adicional**, seleccione **Teléfono de autenticación** en el área **Paso 1: ¿De qué manera deberíamos ponernos en contacto con usted?** , seleccione su país o región en la lista desplegable y, después, escriba el número de teléfono del dispositivo móvil.

2. Seleccione **Llámeme** en el área **Método** y, después, **Siguiente**.

    ![Página Comprobación de seguridad adicional, con teléfono de autenticación y llamada de teléfono](media/multi-factor-authentication-verification-methods/multi-factor-authentication-phone-call.png)

3. Recibirá una llamada de teléfono de Microsoft en la que se le pedirá que presione el signo de almohadilla (#) en el dispositivo móvil para verificar su identidad.

    ![Prueba del número de teléfono especificado](media/multi-factor-authentication-verification-methods/multi-factor-authentication-phone-call-test.png)

4. En el área **Paso 3: Siga usando sus aplicaciones existentes** se indicará una contraseña de aplicación; cópiela y péguela en un lugar seguro.

    ![Área de contraseñas de aplicación de la página Comprobación de seguridad adicional](media/multi-factor-authentication-verification-methods/multi-factor-authentication-app-passwords.png)

    >[!Note]
    >Para obtener información sobre el uso de la contraseña de aplicación con las aplicaciones antiguas, consulte [Administración de las contraseñas de aplicaciones](multi-factor-authentication-end-user-app-passwords.md). Solo tiene que usar contraseñas de aplicación si va a seguir usando aplicaciones antiguas que no admiten la verificación en dos fases.

5. Seleccione **Listo**.

## <a name="next-steps"></a>Pasos siguientes

Después de configurar el método de verificación en dos fases, puede agregar otros métodos, administrar la configuración y las contraseñas de aplicación, iniciar sesión u obtener ayuda con algunos problemas comunes relacionados con la verificación en dos fases.

- [Administración de la configuración del método de verificación en dos fases](multi-factor-authentication-end-user-manage-settings.md)

- [Administración de las contraseñas de aplicación](multi-factor-authentication-end-user-app-passwords.md)

- [Inicio de sesión con la verificación en dos pasos](multi-factor-authentication-end-user-signin.md)

- [Obtención de ayuda con la verificación en dos fases](multi-factor-authentication-end-user-troubleshoot.md)
