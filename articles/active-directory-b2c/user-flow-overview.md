---
title: Flujos de usuario de Azure Active Directory B2C | Microsoft Docs
description: Obtenga más información sobre el marco de directiva extensible de Azure Active Directory B2C y cómo crear distintos flujos de usuario.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.custom: project-no-code
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1dc0e297ca16bf2605993e36942de9d31c331680
ms.sourcegitcommit: 0820c743038459a218c40ecfb6f60d12cbf538b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "87115859"
---
# <a name="user-flows-in-azure-active-directory-b2c"></a>Flujos de usuario en Azure Active Directory B2C

El marco de directiva extensible de Azure Active Directory B2C (Azure AD B2C) es la fortaleza esencial del servicio. Las directivas describen totalmente las experiencias de identidad tales como el registro, el inicio de sesión y la edición de perfil. Para ayudarle a configurar las tareas más comunes de identidad, el portal de Azure AD B2C incluye directivas predefinidas y configurables denominadas **flujos de usuario**.

## <a name="what-are-user-flows"></a>¿Qué son los flujos de usuario?

Un flujo de usuario le permite controlar los comportamientos en sus aplicaciones al configurar los siguientes valores:

- Tipos de cuenta utilizados para iniciar sesión, como cuentas de redes sociales (como Facebook) o cuentas locales
- Atributos que se recopilarán del consumidor durante el registro, como el nombre, código postal y número de calzado
- Azure Multi-Factor Authentication
- Personalización de la interfaz de usuario
- Información que la aplicación recibe como notificaciones en un token

Puede crear muchos flujos de usuario de diferentes tipos en su inquilino y usarlos en sus aplicaciones según sea necesario. Los flujos de usuario se pueden volver a usar en todas las aplicaciones. Esta flexibilidad le permite definir y modificar las experiencias de identidad con cambios mínimos o ningún cambio en el código. La aplicación desencadena un flujo de usuario mediante una solicitud de autenticación HTTP estándar que incluye un parámetro de flujo de usuario. Se recibe un [token](tokens-overview.md) personalizado como respuesta.

En los ejemplos siguientes se muestra el parámetro de cadena de consulta "p" que especifica el flujo de usuario que se usará:

```
https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up user flow
```

```
https://contosob2c.b2clogin.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in user flow
```

## <a name="user-flow-versions"></a>Versiones del flujo de usuario

En Azure Portal, constantemente se agregan nuevas [versiones de flujos de usuario](user-flow-versions.md). Cuando empiece a trabajar con Azure AD B2C, se recomienda que use los flujos de usuario que ya se han probado. Cuando cree un nuevo flujo de usuario, elija el que necesite de la pestaña **Recomendados**.

Actualmente, se recomiendan los siguientes flujos de usuario:

- **Registro e inicio de sesión**: controla las experiencias de registro y de inicio de sesión con una sola configuración. A los usuarios se les lleva por el proceso correcto según el contexto. Le recomendamos que use este flujo de usuario en vez de un flujo de usuario de **registro** y un flujo de usuario de **inicio de sesión**.
- **Edición de perfiles**: permite a los usuarios editar su información de perfil.
- **Restablecimiento de contraseña**: le permite configurar si es posible o no para los usuarios restablecer su contraseña y de qué forma pueden hacerlo.

## <a name="linking-user-flows"></a>Vinculación de los flujos de usuario

Un flujo de usuario de **inicio de sesión o de registro** con cuentas locales incluye el vínculo **¿Olvidó la contraseña?** en la primera página de la experiencia. Al hacer clic en este vínculo, no se desencadena automáticamente ningún flujo de usuario de restablecimiento de contraseña,

sino que se devuelve a la aplicación el código de error `AADB2C90118`. La aplicación debe controlar este código de error mediante la ejecución de un flujo de usuario específico que restablezca la contraseña. Para ver un ejemplo, eche un vistazo a un [ejemplo sencillo de ASP.NET](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIDConnect-DotNet-SUSI) que muestra la vinculación de flujos de usuario.

## <a name="email-address-storage"></a>Almacenamiento de la dirección de correo electrónico

Es posible que se necesite una dirección de correo electrónico como parte de un flujo de usuario. Si el usuario se autentica con un proveedor de identidades de redes sociales, la dirección de correo electrónico se almacena en la propiedad **otherMails**. Si una cuenta local se basa en un nombre de usuario, la dirección de correo electrónico se almacena en una propiedad de detalles de autenticación sólida. Si una cuenta local se basa en una dirección de correo electrónico, dicha dirección se almacena en la propiedad **signInNames**.

No se garantiza que la dirección de correo electrónico pueda comprobarse en cualquiera de estos casos. Un administrador de inquilinos puede deshabilitar la comprobación de correo electrónico en las directivas básicas para las cuentas locales. Incluso si la comprobación de la dirección de correo electrónico está habilitada, las direcciones no se comprueban si provienen de un proveedor de identidades de redes sociales y aún no se cambian.

Solo las propiedades **otherMails** y **signInNames** se exponen a través de Microsoft Graph API. La dirección de correo electrónico en la propiedad de detalles de autenticación sólida no está disponible.

## <a name="next-steps"></a>Pasos siguientes

Para crear los flujos de usuario recomendados, siga las instrucciones en el [Tutorial: Creación de un flujo de usuario](tutorial-create-user-flows.md).
