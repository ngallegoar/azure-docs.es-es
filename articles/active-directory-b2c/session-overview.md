---
title: Sesiones de SSO de Azure Active Directory B2C | Microsoft Docs
description: Configuración del comportamiento de la sesión en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 1f7be4d01dd930e9ff421b2a163f1648f1793da9
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "82230794"
---
# <a name="azure-ad-b2c-session"></a>Sesión de Azure AD B2C

El inicio de sesión único (SSO) agrega seguridad y comodidad cuando los usuarios inician sesión en las aplicaciones de Azure Active Directory B2C (Azure AD B2C). Este artículo describe los métodos de inicio de sesión único que se usan en Azure AD B2C y le ayuda a elegir el más apropiado al configurar la directiva.

Con el inicio de sesión único, los usuarios inician sesión una vez con una sola cuenta y obtienen acceso a varias aplicaciones. La aplicación puede ser una aplicación web, móvil o de una sola página, independientemente de la plataforma o del nombre de dominio.

Cuando el usuario inicia sesión por primera vez en una aplicación, Azure AD B2C conserva una sesión basada en cookies. En las solicitudes de autenticación posteriores, Azure AD B2C lee y valida la sesión basada en cookies y emite un token de acceso sin pedir al usuario que vuelva a iniciar sesión. Si la sesión basada en cookies expira o deja de ser válida, se solicita al usuario que vuelva a iniciar sesión.  

## <a name="sso-session-types"></a>Tipos de sesión de SSO

La integración con Azure AD B2C implica tres tipos de sesiones de SSO:

- **Azure AD B2C**: sesión que administra Azure AD B2C.
- **Proveedor de identidades federado**: sesión que administra el proveedor de identidades; por ejemplo, una cuenta de Microsoft, Facebook o Salesforce.
- **Aplicación**: sesión que administra la aplicación web, móvil o de una sola página.

![Sesión de SSO](media/session-overview/sso-session-types.png)

### <a name="azure-ad-b2c-sso"></a>SSO de Azure AD B2C 

Cuando un usuario se autentica correctamente con una cuenta local o social, Azure AD B2C almacena una sesión basada en cookies en el explorador del usuario. La cookie se almacena en el nombre de dominio del inquilino de Azure AD B2C, como `https://contoso.b2clogin.com`.

Si un usuario inicia sesión por primera vez con una cuenta federada y, durante el período de la sesión (período de vida o TTL), inicia sesión en la misma aplicación o en otra, Azure AD B2C intenta adquirir un nuevo token de acceso del proveedor de identidades federado. Si la sesión del proveedor de identidades federado expiró o no es válida, el proveedor de identidades federado solicita al usuario sus credenciales. Si la sesión sigue activa (o el usuario ha iniciado sesión con una cuenta local en lugar de una cuenta federada), Azure AD B2C autoriza al usuario y elimina los mensajes posteriores.

Puede configurar el comportamiento de la sesión, incluido el TTL de la sesión y cómo Azure AD B2C comparte la sesión entre las directivas y las aplicaciones.

### <a name="federated-identity-provider-sso"></a>SSO del proveedor de identidades federado

Un proveedor de identidades social o empresarial administra su propia sesión. La cookie se almacena en el nombre de dominio del inquilino del proveedor de identidades, como `https://login.salesforce.com`. Azure AD B2C no controla la sesión del proveedor de identidades federado. En su lugar, el comportamiento de la sesión viene determinado por el proveedor de identidades federado. 

Considere el caso siguiente:

1. Un usuario inicia sesión en Facebook para comprobar su sección de noticias.
2. Más adelante, abre su aplicación e inicia el proceso de inicio de sesión. La aplicación redirige al usuario a Azure AD B2C para completar el proceso de inicio de sesión.
3. En la página de registro o inicio de sesión de Azure AD B2C, el usuario elige iniciar sesión con su cuenta de Facebook. Se redirige al usuario a Facebook. Si hay una sesión activa en Facebook, no se le pedirá al usuario que proporcione sus credenciales y se redirigirá inmediatamente a Azure AD B2C con un token de Facebook.

### <a name="application-sso"></a>Inicio de sesión único de aplicaciones

Una aplicación web, móvil o de una sola página se puede proteger mediante el acceso de OAuth, los tokens de identificador o los tokens de SAML. Cuando un usuario intenta acceder a un recurso protegido en la aplicación, esta comprueba si hay una sesión activa en el lado de la aplicación. Si no hay ninguna sesión de aplicación o la sesión expiró, la aplicación llevará al usuario a la página de inicio de sesión de Azure AD B2C.

La sesión de la aplicación puede ser una sesión basada en cookies almacenada en el nombre de dominio de la aplicación, como `https://contoso.com`. Las aplicaciones móviles pueden almacenar la sesión de forma distinta, aunque con una estrategia similar.

## <a name="azure-ad-b2c-session-configuration"></a>Configuración de la sesión de Azure AD B2C

### <a name="session-scope"></a>Ámbito de sesión

La sesión de Azure AD B2C se puede configurar con los siguientes ámbitos:

- **Inquilino**: esta es la configuración predeterminada. Esta configuración permite que varias aplicaciones y flujos de usuario del inquilino B2C compartan la misma sesión de usuario. Por ejemplo, una vez que un usuario inicia sesión en una aplicación, puede también iniciar sesión perfectamente en otra simplemente al acceder a ella.
- **Aplicación**: este valor permite mantener una sesión de usuario exclusivamente para una aplicación, independientemente de otras aplicaciones. Por ejemplo, puede usar esta opción si quiere que el usuario inicie sesión en Contoso Pharmacy, independientemente de si ya inició sesión en Contoso Groceries.
- **Directiva**: este valor permite mantener una sesión de usuario exclusivamente para un flujo de usuario, independientemente de las aplicaciones que lo usen. Por ejemplo, si el usuario ya ha iniciado sesión y ha realizado un paso de autenticación multifactor (MFA), se le puede dar acceso a zonas de mayor seguridad en varias aplicaciones mientras no expire la sesión asociada al flujo de usuario.
- **Deshabilitado**: este valor obliga al usuario a ejecutar todo el flujo de usuario cada vez que se ejecuta la directiva.

### <a name="session-life-time"></a>Duración de la sesión

La **duración de la sesión** es la cantidad de tiempo durante la que la cookie de la sesión de Azure AD B2C está almacenada en el explorador del usuario después de una autenticación correcta. Puede establecer la duración de la sesión en un valor entre 15 y 720 minutos.

### <a name="keep-me-signed-in"></a>Mantener la sesión iniciada

La característica [Mantener mi sesión iniciada](custom-policy-keep-me-signed-in.md) amplía la duración de la sesión mediante el uso de una cookie persistente. La sesión permanece activa cuando el usuario cierra y vuelve a abrir el explorador. La sesión se revoca solo cuando un usuario cierra sesión. La característica Mantener mi sesión iniciada solo se aplica al inicio de sesión con cuentas locales.

Esta característica tiene prioridad sobre la duración de la sesión. Si está habilitada y el usuario la selecciona, dicha característica indicará cuándo expira la sesión. 

### <a name="session-expiry-type"></a>Tipo de expiración de la sesión

El **tipo de expiración de la sesión** indica cómo se extiende una sesión, ya sea mediante el valor de duración de la sesión o mediante la configuración Mantener mi sesión iniciada.

- **Rolling** (acumulado): indica que la sesión se extiende cada vez que el usuario realiza una autenticación basada en cookies (predeterminado).
- **Absolute** (absoluto): indica que el usuario está obligado a autenticarse de nuevo tras el período especificado.

## <a name="sign-out"></a>Cierre de sesión

Si desea cerrar la sesión del usuario de la aplicación, no basta con borrar las cookies de la aplicación o finalizar la sesión con el usuario. Debe redireccionar al usuario a Azure AD B2C para cerrar la sesión. De lo contrario, el usuario podría autenticarse de nuevo en la aplicación sin volver a escribir sus credenciales.

Después de una solicitud de cierre de sesión, Azure AD B2C hará lo siguiente:

1. Invalidar la sesión basada en cookies de Azure AD B2C.
1. Tratar de cerrar la sesión de los proveedores de identidades federados:
   - OpenID Connect: si el punto de conexión de configuración conocido del proveedor de identidades especifica una ubicación `end_session_endpoint`.
   - SAML: si los metadatos del proveedor de identidades contienen la ubicación `SingleLogoutService`.
1. También puede cerrar la sesión de otras aplicaciones. Para obtener más información, consulte la sección [Cierre de sesión único](#single-sign-out).

> [!NOTE]
> El cierre de sesión borrará el estado de inicio de sesión único del usuario con Azure AD B2C, pero podría no cerrar la sesión del proveedor de identidades social del usuario. Si el usuario selecciona el mismo proveedor de identidades durante un inicio de sesión posterior, podría volver a autenticarse sin especificar sus credenciales. Si un usuario quiere cerrar sesión en su aplicación, eso no significa necesariamente que quiera cerrar sesión en su cuenta de Facebook. Sin embargo, si se usan cuentas locales, la sesión del usuario finalizará correctamente.

### <a name="single-sign-out"></a>Cierre de sesión único

Cuando redirige al usuario al punto de conexión de cierre de sesión de Azure AD B2C (para los protocolos OAuth2 y SAML), Azure AD B2C borra la sesión del usuario del explorador. Sin embargo, el usuario podría permanecer conectado a otras aplicaciones que usan Azure AD B2C para la autenticación. Para permitir que esas aplicaciones cierren la sesión del usuario de manera simultánea, Azure AD B2C envía una solicitud HTTP GET al elemento `LogoutUrl` registrado de todas las aplicaciones en las que tiene una sesión iniciada.

Las aplicaciones deben responder a esta solicitud mediante la eliminación de la sesión que identifica al usuario y la devolución de una respuesta `200`. Si quiere admitir el inicio de sesión único en la aplicación, debe implementar un elemento `LogoutUrl` en el código de la aplicación. Puede habilitar `LogoutUrl` desde Azure Portal:

1. Acceda a [Azure Portal](https://portal.azure.com).
1. Para elegir la instancia de Active Directory B2C, haga clic en su cuenta en la esquina superior derecha de la página.
1. En el panel de navegación izquierdo, elija **Azure AD B2C**, seleccione **Registros de aplicaciones** y, después, seleccione la aplicación.
1. Seleccione **Configuración**, **Propiedades** y después busque el cuadro de texto **URL de cierre de sesión**. 


## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [configurar el comportamiento de la sesión en el flujo de usuario](session-behavior.md).
- Obtenga información sobre cómo [configurar el comportamiento de la sesión en la directiva personalizada](custom-policy-manage-sso-and-token-config.md#session-behavior-and-sso).