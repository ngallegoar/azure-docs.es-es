---
title: 'Problemas comunes con la autenticación en dos fases de una cuenta: Azure AD'
description: Soluciones para algunos de los problemas más comunes de la verificación en dos fases y la cuenta profesional o educativa.
services: active-directory
author: curtand
manager: daveba
ms.assetid: 8f3aef42-7f66-4656-a7cd-d25a971cb9eb
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 04/02/2020
ms.author: curtand
ms.reviewer: kexia
ms.openlocfilehash: 8d2e34bcfd180dfeb814dace2a496f3ac593c5bf
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2020
ms.locfileid: "83738615"
---
# <a name="common-problems-with-two-factor-verification-and-your-work-or-school-account"></a>Problemas comunes con la verificación en dos fases y la cuenta profesional o educativa

Cuando una organización de Azure Active Directory (Azure AD) activa la verificación en dos fases, el inicio de sesión de la cuenta profesional o educativa requiere una combinación del nombre de usuario, la contraseña y un dispositivo móvil o un teléfono. Este método es más seguro que solo una contraseña, porque se basa en dos formas de autenticación: algo que se sabe y algo que se tiene. La verificación en dos fases ayuda a impedir que hackers malintencionados finjan ser usted, ya que, incluso si conocen su contraseña, lo más probable es que no tengan también su dispositivo.

<center>

![Imagen de los métodos de autenticación conceptuales](../authentication/media/concept-mfa-howitworks/methods.png)</center>

Hay algunos problemas comunes en la verificación en dos fases que parecen ocurrir con más frecuencia de la que nos gustaría. Hemos elaborado este artículo con el objeto de solucionar los problemas más comunes y proporcionar algunas posibles correcciones.

>[!Important]
>Si es administrador, puede encontrar más información sobre cómo configurar y administrar su entorno de Azure AD en la [documentación de Azure AD](https://docs.microsoft.com/azure/active-directory).
>
>Este contenido también está diseñado solo para su uso con la cuenta profesional o educativa, que es la cuenta que le proporcionó su organización (por ejemplo, alain@contoso.com). Si tiene problemas con la verificación en dos fases y su cuenta personal de Microsoft, que es una cuenta que configuró por sí mismo (por ejemplo, danielle@outlook.com), consulte [Activar o desactivar la verificación en dos pasos para la cuenta de Microsoft](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off).

## <a name="i-dont-have-my-mobile-device-with-me"></a>No llevo conmigo mi dispositivo móvil

Pasa algunas veces. Ha dejado el dispositivo móvil en casa y ahora no puede usar el teléfono para comprobar que es quien dice ser. Si previamente ha agregado otro método para iniciar sesión en su cuenta, como su teléfono de la oficina, debería poder usar ese método ahora. Si nunca ha agregado un método de verificación adicional, deberá ponerse en contacto con el departamento de soporte técnico de la organización para que le ayude a volver a su cuenta.

### <a name="to-sign-in-to-your-work-or-school-account-using-another-verification-method"></a>Para iniciar sesión en su cuenta profesional o educativa con otro método de verificación

1. Inicie sesión en la cuenta, pero seleccione **Sign in another way** (Iniciar sesión de otro modo) en la página **Two-step verification** (Verificación en dos fases).

    ![Cambio del método de comprobación de inicio de sesión](./media/multi-factor-authentication-end-user-troubleshoot/two-factor-auth-signin-another-way.png)

    Si no ve el vínculo **Sign in another way** (Iniciar sesión de otro modo) significa que no ha configurado ningún otro método de comprobación. Tendrá que ponerse en contacto con el administrador para que lo ayude a iniciar sesión en la cuenta.

2. Elija el método de comprobación alternativo y siga con el proceso de verificación en dos fases.

## <a name="i-lost-my-mobile-device-or-it-was-stolen"></a>He perdido mi dispositivo móvil o me lo han robado

Si ha perdido el dispositivo móvil o se lo han robado, puede iniciar sesión con un método diferente o puede pedir al departamento de soporte técnico de la organización que borre la configuración. Le recomendamos que informe al departamento de soporte técnico de la organización que el teléfono se perdió o que se lo robaron, para que puedan hacer las actualizaciones correspondientes en su cuenta. Una vez borrada su configuración, se le pedirá que se [registre en la verificación en dos fases](multi-factor-authentication-end-user-first-time.md) la próxima vez que inicie sesión.

## <a name="im-not-getting-the-verification-code-sent-to-my-mobile-device"></a>No recibo el código de verificación que se envía al dispositivo móvil

No recibir el código de verificación es un problema habitual que suele estar relacionado con el dispositivo móvil y su configuración. Posibles soluciones:

Probar esto | Directrices
--------- | ------------
Reiniciar el dispositivo móvil | A veces, el dispositivo solo necesita una actualización. Al reiniciar el dispositivo, los procesos o servicios que se estuvieran ejecutando en segundo plano y que pudieran provocar problemas terminan, al tiempo que se actualizan los componentes principales del dispositivo y se reinician en caso de que estuvieran bloqueados en algún punto.
Comprobar que la información de seguridad sea correcta | Asegúrese de que la información de seguridad del método de verificación es correcta, especialmente los números de teléfono. Si pone un número de teléfono equivocado, todas las alertas irán a ese número incorrecto. Afortunadamente, ese usuario no podrá hacer nada con esas alertas, pero esto tampoco le permitirá iniciar sesión en su cuenta. Para asegurarse de que la información es correcta, consulte las instrucciones del artículo [Administración de la configuración del método de verificación en dos fases](multi-factor-authentication-end-user-manage-settings.md).
Comprobar que las notificaciones estén activadas | Asegúrese de que el dispositivo móvil tiene las notificaciones activadas y de que ha seleccionado un método de notificación que permita llamadas telefónicas, la aplicación de autenticación y la aplicación de mensajería (para mensajes de texto) para enviar notificaciones de alerta visibles a su dispositivo móvil.
Asegurarse de que el dispositivo tenga cobertura y conexión a Internet | Asegúrese de que llegan llamadas telefónicas y mensajes de texto al dispositivo móvil. Pida a un amigo que le llame y le envíe un mensaje de texto para asegurarse de que recibe ambos. Si no es así, primero asegúrese de que el dispositivo móvil esté encendido. Si el dispositivo está encendido pero aún así no recibe la llamada o el mensaje de texto, lo más probable es que haya un problema con la red y que tenga que comunicarse con el proveedor. Si suele experimentar problemas relacionados con la cobertura, le recomendamos que instale y use la [aplicación Microsoft Authenticator](user-help-auth-app-download-install.md) en su dispositivo móvil. Esta aplicación puede generar códigos de seguridad aleatorios para iniciar sesión, sin necesidad de una conexión a Internet ni de cobertura del móvil.
Desactivar la opción No molestar | Asegúrese de que no ha activado la característica **No molestar** del dispositivo móvil. Si esta característica está activada, no permite que reciba notificaciones en su dispositivo móvil. Consulte el manual del dispositivo móvil para obtener instrucciones sobre cómo desactivar esta característica.
Desbloquear los números de teléfono | En Estados Unidos, las llamadas de voz de Microsoft proceden de los números siguientes: +1 (866) 539 4191, +1 (855) 330 8653 y +1 (877) 668 6536.
Comprobar la configuración relacionada con la batería | Esto puede parecer extraño de entrada, pero si ha configurado la optimización de la batería para que impida que las aplicaciones menos usadas permanezcan activas en segundo plano, lo más probable es que esto haya afectado al sistema de notificaciones. Para intentar solucionar este problema, desactive la optimización de batería para las aplicaciones de autenticación y mensajería, e intente iniciar sesión de nuevo en su cuenta.
Deshabilitación de aplicaciones de seguridad de terceros | Si tiene una aplicación que protege mensajes de texto o llamadas telefónicas para minimizar la cantidad de llamadas procedentes de orígenes desconocidos, esta puede impedir que se reciba el código de verificación. Pruebe a deshabilitar las aplicaciones de seguridad de terceros en el teléfono y, a continuación, solicite que se envíe otro código de verificación.

## <a name="im-not-getting-prompted-for-my-second-verification-information"></a>No se me pide la segunda información de verificación

Si ha iniciado sesión en su cuenta profesional o educativa con su nombre de usuario y contraseña, pero no se le ha solicitado la información adicional de verificación de seguridad, es posible que no haya configurado el dispositivo todavía. El dispositivo móvil debe estar configurado para que funcione con el método de verificación de seguridad adicional específico. Para asegurarse de que ha encendido el dispositivo móvil y que está disponible para su uso con el método de verificación, consulte el artículo [Administración de la configuración del método de verificación en dos fases](multi-factor-authentication-end-user-manage-settings.md). Si no ha configurado el dispositivo o su cuenta, puede hacerlo ahora siguiendo los pasos del artículo [Configuración de mi cuenta para la verificación en dos pasos](multi-factor-authentication-end-user-first-time.md).

## <a name="i-got-a-new-phone-number-and-i-want-to-add-it"></a>Tengo un nuevo número de teléfono y quiero agregarlo

Si tiene un nuevo número de teléfono, deberá actualizar la información en el método de verificación de seguridad para que los avisos vayan a la ubicación correcta. Para actualizar el método de verificación, siga los pasos de la sección sobre cómo **agregar o cambiar el número de teléfono** en el artículo [Administración de la configuración del método de verificación en dos fases](multi-factor-authentication-end-user-manage-settings.md#add-or-change-your-phone-number).

## <a name="i-got-a-new-mobile-device-and-i-want-to-add-it"></a>Tengo un nuevo dispositivo móvil y quiero agregarlo

Si tiene un nuevo dispositivo móvil, deberá configurarlo para que funcione con la verificación en dos fases. Esta solución consta de varios pasos:

1. Configure el dispositivo para que funcione con la cuenta profesional o educativa siguiendo los pasos del artículo [Configuración de mi cuenta para la verificación en dos pasos](multi-factor-authentication-end-user-first-time.md).

1. Actualice la información de la cuenta y del dispositivo en la página **Comprobación de seguridad adicional**; elimine el dispositivo antiguo y agregue el nuevo. Para más información, consulte el artículo [Administración de la configuración del método de verificación en dos fases](multi-factor-authentication-end-user-manage-settings.md).

Pasos opcionales:

- Descargue, instale y configure la aplicación Microsoft Authenticator en el dispositivo móvil siguiendo los pasos del artículo [Descarga e instalación de la aplicación Microsoft Authenticator](user-help-auth-app-download-install.md).

- Active la verificación en dos fases para los dispositivos de confianza siguiendo los pasos descritos en la sección sobre cómo **activar los avisos de la verificación en dos fases en un dispositivo de confianza**, en el artículo [Administración de la configuración del método de verificación en dos fases](multi-factor-authentication-end-user-manage-settings.md#turn-on-two-factor-verification-prompts-on-a-trusted-device).

## <a name="im-having-problems-signing-in-on-my-mobile-device-while-traveling"></a>Tengo problemas para iniciar sesión en el dispositivo móvil mientras viajo

Es posible que le resulte más difícil usar un método de verificación relacionado con dispositivos móviles, como un mensaje de texto, mientras se encuentra en el extranjero. También es posible que el dispositivo móvil pueda provocar cargos de itinerancia. En estos casos, se recomienda usar la aplicación Microsoft Authenticator y, opcionalmente, conectarse a una zona activa Wi-Fi. Para más información sobre la descarga, instalación y configuración de la aplicación Microsoft Authenticator en el dispositivo móvil, consulte el artículo [Descarga e instalación de la aplicación Microsoft Authenticator](user-help-auth-app-download-install.md).

## <a name="i-cant-get-my-app-passwords-to-work"></a>No consigo que funcionen mis contraseñas de aplicación

Las contraseñas de aplicación reemplazan a las contraseñas normales de las aplicaciones de escritorio más antiguas que no admiten la verificación en dos fases. En primer lugar, asegúrese de escribir correctamente la contraseña. Si eso no soluciona el problema, intente crear una nueva contraseña de aplicación para la aplicación siguiendo los pasos descritos en la sección sobre cómo **crear y eliminar contraseñas de aplicación mediante el portal Aplicaciones** del artículo [Administración de las contraseñas de aplicaciones para la verificación en dos pasos](multi-factor-authentication-end-user-app-passwords.md#create-and-delete-app-passwords-from-the-additional-security-verification-page).

## <a name="i-cant-turn-two-factor-verification-off"></a>¿Por qué no se puede desactivar la verificación en dos fases?

Si usa la verificación en dos pasos con su cuenta profesional o educativa (por ejemplo, alain@contoso.com), lo más probable es que la organización haya decidido que debe usar esta característica de seguridad adicional. Si este es el caso, no hay ninguna manera de desactivarla individualmente. Sin embargo, si usa la verificación en dos pasos con una cuenta personal, como alain@outlook.com, tiene la posibilidad de activar y desactivar la característica. Para instrucciones sobre cómo controlar la verificación en dos fases en sus cuentas personales, consulte [Activar o desactivar la verificación en dos pasos para la cuenta de Microsoft](https://support.microsoft.com/help/4028586/microsoft-account-turning-two-step-verification-on-or-off).

Si no puede desactivar la comprobación en dos fases, también podría deberse a los valores predeterminados de seguridad que se aplicaron en el nivel de organización. Para obtener más información sobre los valores predeterminados de seguridad, consulte [¿Qué son los valores predeterminados de seguridad?](../fundamentals/concept-fundamentals-security-defaults.md)

## <a name="i-didnt-find-an-answer-to-my-problem"></a>No encuentro una respuesta a mi problema.

Si ya intentó estos pasos pero sigue teniendo problemas, póngase en contacto con el departamento de soporte técnico de la organización para que le ayude.

## <a name="related-articles"></a>Artículos relacionados

- [Administración de la configuración del método de verificación en dos fases](multi-factor-authentication-end-user-manage-settings.md)

- [Configuración de mi cuenta para la verificación en dos pasos](multi-factor-authentication-end-user-first-time.md)

- [Preguntas frecuentes de la aplicación Microsoft Authenticator](user-help-auth-app-faq.md)
