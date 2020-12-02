---
title: Registro e inicio de sesión en el teléfono con directivas personalizadas
titleSuffix: Azure AD B2C
description: Envíe contraseñas de un solo uso (OTP) en mensajes de texto a los teléfonos de los usuarios de la aplicación con directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 09/01/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9d4fa80f669493e4dc47a9ad0f9bfe9390d4ab24
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94953583"
---
# <a name="set-up-phone-sign-up-and-sign-in-with-custom-policies-in-azure-ad-b2c"></a>Configuración del registro y el inicio de sesión en el teléfono con directivas personalizadas en Azure AD B2C

El registro y el inicio de sesión en el teléfono en Azure Active Directory B2C (Azure AD B2C) permite a los usuarios registrarse e iniciar sesión en las aplicaciones mediante una contraseña de un solo uso (OTP) enviada en un mensaje de texto a su teléfono. Las contraseñas de un solo uso pueden ayudar a reducir el riesgo de que los usuarios olviden sus contraseñas o estas se vean comprometidas.

Siga los pasos de este artículo para usar las directivas personalizadas y permitir que los clientes se registren e inicien sesión en las aplicaciones mediante una contraseña de un solo uso enviada a su teléfono.

## <a name="pricing"></a>Precios

Las contraseñas de un solo uso se envían a los usuarios mediante mensajes de texto SMS, y es posible que se le cobre por cada mensaje enviado. Puede encontrar información sobre precios en la sección **Cargos aparte** de la página [Precios de Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="user-experience-for-phone-sign-up-and-sign-in"></a>Experiencia del usuario para registro e inicio de sesión por teléfono

Con el registro y el inicio de sesión por teléfono, el usuario puede suscribirse a la aplicación con un número de teléfono como identificador principal. A continuación, se describe la experiencia del usuario final durante el registro y el inicio de sesión.

> [!NOTE]
> Se recomienda encarecidamente incluir información de consentimiento en su experiencia de registro e inicio de sesión similar a la del texto de ejemplo siguiente. Este texto de ejemplo es meramente informativo. Consulte el documento Short Code Monitoring Handbook en el [sitio web de CTIA](https://www.ctia.org/programs) y póngase en contacto con sus propios expertos legales o de cumplimiento para obtener instrucciones sobre el texto final y la configuración de características que satisfacen sus propias necesidades de cumplimiento:
>
> *Al proporcionar su número de teléfono, da su consentimiento a recibir un código de acceso de un solo uso enviado por mensaje de texto para ayudarle a iniciar sesión en *&lt;insert: el nombre de la aplicación&gt;* . Se pueden aplicar tarifas de datos y mensajes.*
>
> *&lt;insert: un vínculo a la declaración de privacidad&gt;*<br/>*&lt;insert: un vínculo a los términos de servicio&gt;*

Para agregar su propia información de consentimiento, personalice el ejemplo siguiente e inclúyalo en el elemento LocalizedResources de ContentDefinition que usa la página autoafirmada con el control de pantalla (el archivo Phone-Email-Base.xml en el paquete de inicio de registro e inicio de sesión):

```xml
<LocalizedResources Id="phoneSignUp.en">        
    <LocalizedStrings>
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_msg_intro">By providing your phone number, you consent to receiving a one-time passcode sent by text message to help you sign into {insert your application name}. Standard messsage and data rates may apply.</LocalizedString>          
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_link_1_text">Privacy Statement</LocalizedString>                
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_link_1_url">{insert your privacy statement URL}</LocalizedString>          
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_link_2_text">Terms and Conditions</LocalizedString>             
    <LocalizedString ElementType="DisplayControl" ElementId="phoneControl" StringId="disclaimer_link_2_url">{insert your terms and conditions URL}</LocalizedString>          
    <LocalizedString ElementType="UxElement" StringId="initial_intro">Please verify your country code and phone number</LocalizedString>        
    </LocalizedStrings>      
</LocalizedResources>
   ```

### <a name="phone-sign-up-experience"></a>Experiencia de registro por teléfono

Si el usuario aún no tiene una cuenta para la aplicación, puede crear una con el vínculo **Registrarse ahora**. Aparece una página de registro, donde el usuario selecciona su **país**, escribe su número de teléfono y selecciona **Enviar código**.

![El usuario inicia el registro por teléfono](media/phone-authentication/phone-signup-start.png)

Se envía un código de verificación de un solo uso al número de teléfono del usuario. El usuario escribe el **código de verificación** en la página de registro y, luego, selecciona **Comprobar el código**. (Si el usuario no puede recuperar el código, puede seleccionar **Enviar nuevo código**).

![El usuario verifica el código durante el registro por teléfono](media/phone-authentication/phone-signup-verify-code.png)

 El usuario escribe cualquier otra información solicitada en la página de registro, por ejemplo, **Nombre para mostrar**, **Nombre dado** y **Apellido** (el país y el número de teléfono permanecen rellenados). Si el usuario quiere usar un número de teléfono diferente, puede elegir **Cambiar número** para reiniciar el registro. Cuando termine, el usuario selecciona **Continuar**.

![El usuario proporciona información adicional](media/phone-authentication/phone-signup-additional-info.png)

A continuación, se pide al usuario que proporcione un correo electrónico de recuperación. El usuario escribe su dirección de correo electrónico y, luego, selecciona **Enviar código de comprobación**. Se envía un código a la bandeja de entrada de correo electrónico del usuario, que puede recuperar y escribir en el cuadro **Código de verificación**. A continuación, el usuario selecciona **Comprobar código**. 

Una vez que se comprueba el código, el usuario selecciona **Crear** para crear su cuenta. O bien, si el usuario quiere usar una dirección de correo electrónico diferente, puede elegir **Cambiar correo electrónico**.

![El usuario crea una cuenta](media/phone-authentication/email-verification.png)

### <a name="phone-sign-in-experience"></a>Experiencia de inicio de sesión por teléfono

Si el usuario tiene una cuenta con el número de teléfono como identificador, escribe su número de teléfono y selecciona **Continuar**. Confirma el país y el número de teléfono seleccionando **Continuar** y se envía un código de verificación de un solo uso a su teléfono. El usuario escribe el código de verificación y selecciona **Continuar** para iniciar sesión.

![Experiencia de usuario de inicio de sesión por teléfono](media/phone-authentication/phone-signin-screens.png)

## <a name="deleting-a-user-account"></a>Eliminación de cuentas de usuario

En algunos casos, es posible que tenga que eliminar un usuario y los datos asociados del directorio de Azure AD B2C. Para más información sobre cómo eliminar una cuenta de usuario mediante Azure Portal, consulte [estas instrucciones](/microsoft-365/compliance/gdpr-dsr-azure#step-5-delete). 

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]



## <a name="prerequisites"></a>Prerrequisitos

Antes de configurar OTP, necesita tener los siguientes recursos:

* [Inquilino de Azure AD B2C](tutorial-create-tenant.md)
* [Aplicación web registrada](tutorial-register-applications.md) en el inquilino
* [Directivas personalizadas](custom-policy-get-started.md) cargadas en el inquilino

## <a name="get-the-phone-sign-up--sign-in-starter-pack"></a>Obtención del paquete de inicio de registro e inicio de sesión en el teléfono

Empiece por actualizar los archivos de directivas personalizadas de registro e inicio de sesión en el teléfono para que funcionen con el inquilino de Azure AD B2C.

En los siguientes pasos se da por supuesto que ha completado los [requisitos previos](#prerequisites) y ya ha clonado el repositorio del [paquete de inicio de directivas personalizadas][starter-pack] en la máquina local.

1. Busque los [archivos de directivas personalizadas de registro e inicio de sesión en el teléfono][starter-pack-phone] en el clon local del repositorio del paquete de inicio o descárguelos directamente. Los archivos XML de directivas están ubicados en el siguiente directorio:

    `active-directory-b2c-custom-policy-starterpack/scenarios/`**`phone-number-passwordless`**

1. En cada archivo, reemplace la cadena `yourtenant` por el nombre del inquilino de Azure AD B2C. Por ejemplo, si el nombre del inquilino de B2C es *contosob2c*, todas las instancias de `yourtenant.onmicrosoft.com` se convierten en `contosob2c.onmicrosoft.com`.

1. Complete los pasos de la sección [Agregar identificadores de aplicación a la directiva personalizada](custom-policy-get-started.md#add-application-ids-to-the-custom-policy) de [Introducción a las directivas personalizadas en Azure Active Directory B2C](custom-policy-get-started.md). En este caso, actualice `/phone-number-passwordless/` **`Phone_Email_Base.xml`** con los valores de **Id. de aplicación (cliente)** de las dos aplicaciones que registró al completar los requisitos previos, *IdentityExperienceFramework* y *ProxyIdentityExperienceFramework*.

## <a name="upload-the-policy-files"></a>Carga de los archivos de directivas

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al inquilino de Azure AD B2C.
1. En **Directivas**, seleccione **Identity Experience Framework**.
1. Seleccione **Cargar directiva personalizada**.
1. Cargue los archivos de directivas en el siguiente orden:
    1. *Phone_Email_Base.xml*
    1. *SignUpOrSignInWithPhone.xml*
    1. *SignUpOrSignInWithPhoneOrEmail.xml*
    1. *ProfileEditPhoneOnly.xml*
    1. *ProfileEditPhoneEmail.xml*
    1. *ChangePhoneNumber.xml*
    1. *PasswordResetEmail.xml*

A medida que carga cada archivo, Azure agrega el prefijo `B2C_1A_`.

## <a name="test-the-custom-policy"></a>Prueba de la directiva personalizada

1. En **Directivas personalizadas**, seleccione **B2C_1A_SignUpOrSignInWithPhone**.
1. En **Seleccionar aplicación**, seleccione la aplicación *webapp1* que registró al completar los requisitos previos.
1. En **Seleccionar dirección URL de respuesta**, elija `https://jwt.ms`.
1. Seleccione **Ejecutar ahora** y regístrese con una dirección de correo electrónico o un número de teléfono.
1. Seleccione **Ejecutar ahora** de nuevo e inicie sesión con la misma cuenta para confirmar que tiene la configuración correcta.

## <a name="get-user-account-by-phone-number"></a>Obtención de una cuenta de usuario por número de teléfono

Los usuarios que se registran con un número de teléfono pero que no proporcionan una dirección de correo electrónico de recuperación usan dicho número de teléfono como nombre de inicio de sesión para registrarse en el directorio de Azure AD B2C. Si luego el usuario quiere cambiar su número de teléfono, el departamento de soporte técnico o el equipo de soporte técnico deben encontrar primero su cuenta y, luego, actualizar su número de teléfono.

Puede buscar un usuario por su número de teléfono (nombre de inicio de sesión) mediante [Microsoft Graph](manage-user-accounts-graph-api.md):

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+{phone number}' and c/issuer eq '{tenant name}.onmicrosoft.com')
```

Por ejemplo:

```http
GET https://graph.microsoft.com/v1.0/users?$filter=identities/any(c:c/issuerAssignedId eq '+450334567890' and c/issuer eq 'contosob2c.onmicrosoft.com')
```

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar el paquete de inicio de las directivas personalizadas de registro e inicio de sesión en el teléfono (y otros paquetes de inicio) en GitHub: [Azure-Samples/active-directory-b2c-custom-policy-starterpack/scenarios/phone-number-passwordless][starter-pack-phone] Los archivos de directiva del paquete de inicio usan perfiles técnicos de autenticación multifactor y transformaciones de notificaciones de números de teléfono:
* [Definición de un perfil técnico de Azure AD Multi-Factor Authentication](multi-factor-auth-technical-profile.md)
* [Definición de transformaciones de notificaciones de número de teléfono en Azure AD B2C](phone-number-claims-transformations.md)

<!-- LINKS - External -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
[starter-pack-phone]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/phone-number-passwordless