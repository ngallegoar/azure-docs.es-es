---
title: Uso de AppAuth en una aplicación de iOS
titleSuffix: Azure AD B2C
description: Cómo crear una aplicación iOS que usa AppAuth con Azure Active Directory B2C para administrar las identidades de usuario y autenticar usuarios.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c659280ebc8c91b53cbc3a176c84397edd942c23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78186835"
---
# <a name="azure-ad-b2c-sign-in-using-an-ios-application"></a>Azure AD B2C: Inicio de sesión con una aplicación iOS

La plataforma Microsoft Identity utiliza estándares abiertos como OAuth2 y OpenID Connect. Usar un protocolo estándar abierto ofrece más opciones a los desarrolladores cuando se selecciona una biblioteca para integrarla con nuestros servicios. Proporcionamos este tutorial y otros similares para ayudar a los desarrolladores a escribir aplicaciones que se conecten con la plataforma de Microsoft Identity. La mayoría de las bibliotecas que implementan [la especificación OAuth2 RFC6749](https://tools.ietf.org/html/rfc6749) pueden conectarse a la plataforma de Microsoft Identity.

> [!WARNING]
> Microsoft no proporciona correcciones para bibliotecas de terceros y no se ha realizado ninguna revisión de esas bibliotecas. Este ejemplo usa una biblioteca de terceros llamada AppAuth cuya compatibilidad se ha probado en escenarios básicos con Azure AD B2C. Los problemas y las solicitudes de características deben dirigirse al proyecto de código abierto de la biblioteca. Para obtener más información, consulte [este artículo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

Si no está familiarizado con OAuth2 o con OpenID Connect, es posible que gran parte de esta configuración de ejemplo no tenga mucho sentido para usted. Se recomienda que consulte una breve [información general sobre el protocolo que hemos documentado aquí](protocols-overview.md).

## <a name="get-an-azure-ad-b2c-directory"></a>Obtener un directorio de Azure AD B2C
Para poder usar Azure AD B2C, debe crear un directorio o inquilino. Un directorio es un contenedor de todos los usuarios, aplicaciones, grupos, etc. Antes de continuar, si no tiene un directorio B2C, [créelo](tutorial-create-tenant.md) .

## <a name="create-an-application"></a>Crear una aplicación

A continuación, registre una aplicación en el inquilino de Azure AD B2C. De esta forma, se proporciona a Azure AD la información que necesita para comunicarse de forma segura con la aplicación.

[!INCLUDE [active-directory-b2c-appreg-native](../../includes/active-directory-b2c-appreg-native.md)]

Anote el **Id. de aplicación (cliente)** para usarlo en un paso posterior.

Registre también el URI de redireccionamiento personalizado para usarlo en un paso posterior. Por ejemplo, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`.

## <a name="create-your-user-flows"></a>Creación de flujos de usuario
En Azure AD B2C, cada experiencia del usuario se define mediante un [flujo de usuario](user-flow-overview.md). Esta aplicación contiene una experiencia de identidad: una combinación de inicio de sesión y registro. Cuando cree el flujo de usuario, asegúrese de hacer lo siguiente:

* En **Atributos de registro**, seleccione **Nombre para mostrar**.  Puede seleccionar también otros atributos.
* En **Notificaciones de aplicación**, seleccione las notificaciones **Nombre para mostrar** e **Identificador de objeto del usuario**. Puede elegir también otras notificaciones.
* Copie el **nombre** de cada flujo de usuario después de crearlo. Cuando se guarda el flujo de usuario, el nombre tiene como prefijo `b2c_1_`.  Esta información la necesitará más adelante.

Después de haber creado los flujos de usuario, está listo para compilar la aplicación.

## <a name="download-the-sample-code"></a>Descarga del código de ejemplo
Hemos proporcionado un ejemplo de trabajo que usa AppAuth con Azure AD B2C [en GitHub](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Puede descargar el código y ejecutarlo. Para usar su propio inquilino de Azure AD B2C, siga las instrucciones que aparecen en el archivo [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md).

Este ejemplo se creó con las instrucciones del archivo README del [proyecto AppAuth de iOS en GitHub](https://github.com/openid/AppAuth-iOS). Para más detalles sobre cómo funcionan el ejemplo y la biblioteca, haga referencia al archivo README de AppAuth en GitHub.

## <a name="modifying-your-app-to-use-azure-ad-b2c-with-appauth"></a>Modificación de la aplicación para usar Azure AD B2C con AppAuth

> [!NOTE]
> AppAuth es compatible con iOS7 y versiones superiores.  Sin embargo, para admitir inicios de sesión sociales en Google, se necesita SFSafariViewController, que requiere iOS 9 o superior.
>

### <a name="configuration"></a>Configuración

Puede configurar la comunicación con Azure AD B2C si especifica tanto los URI del punto de conexión de autorización como del punto de conexión del token.  Para generar estos URI, necesita la información siguiente:
* Identificador de inquilino (por ejemplo, contoso.onmicrosoft.com)
* Nombre del flujo de usuario (por ejemplo, B2C\_1\_SignUpIn)

El URI de punto de conexión de token se puede generar si se reemplaza Tenant\_ID y Policy\_Name en la siguiente dirección URL:

```objc
static NSString *const tokenEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/token";
```

El URI de punto de conexión de autorización se puede generar si se reemplaza Tenant\_ID y Policy\_Name en la siguiente dirección URL:

```objc
static NSString *const authorizationEndpoint = @"https://<Tenant_name>.b2clogin.com/te/<Tenant_ID>/<Policy_Name>/oauth2/v2.0/authorize";
```

Ejecute el código siguiente para crear el objeto AuthorizationServiceConfiguration:

```objc
OIDServiceConfiguration *configuration =
    [[OIDServiceConfiguration alloc] initWithAuthorizationEndpoint:authorizationEndpoint tokenEndpoint:tokenEndpoint];
// now we are ready to perform the auth request...
```

### <a name="authorizing"></a>Autorización

Después de configurar o recuperar una configuración de servicio de autorización, es posible construir una autorización. Para crear la solicitud, necesita la información siguiente:

* El identificador de cliente (identificador de aplicación) que registró anteriormente. Por ejemplo, `00000000-0000-0000-0000-000000000000`.
* El URI de redireccionamiento personalizado que registró anteriormente. Por ejemplo, `com.onmicrosoft.contosob2c.exampleapp://oauth/redirect`.

Ambos elementos se deberían haber guardado cuando [registró la aplicación](#create-an-application).

```objc
OIDAuthorizationRequest *request =
    [[OIDAuthorizationRequest alloc] initWithConfiguration:configuration
                                                  clientId:kClientId
                                                    scopes:@[OIDScopeOpenID, OIDScopeProfile]
                                               redirectURL:[NSURL URLWithString:kRedirectUri]
                                              responseType:OIDResponseTypeCode
                                      additionalParameters:nil];

AppDelegate *appDelegate = (AppDelegate *)[UIApplication sharedApplication].delegate;
appDelegate.currentAuthorizationFlow =
    [OIDAuthState authStateByPresentingAuthorizationRequest:request
                                   presentingViewController:self
                                                   callback:^(OIDAuthState *_Nullable authState, NSError *_Nullable error) {
        if (authState) {
            NSLog(@"Got authorization tokens. Access token: %@", authState.lastTokenResponse.accessToken);
            [self setAuthState:authState];
        } else {
            NSLog(@"Authorization error: %@", [error localizedDescription]);
            [self setAuthState:nil];
        }
    }];
```

Para configurar la aplicación y controlar el redireccionamiento al URI con el esquema personalizado, deberá actualizar la lista de "Esquemas de dirección URL" en Info.pList:
* Abra Info.pList.
* Mantenga el puntero sobre una fila como "Código de tipo de SO del lote" y haga clic en el símbolo \+.
* Cambie el nombre de la fila nueva "Tipos de dirección URL".
* Haga clic en la flecha a la izquierda de "Tipos de dirección URL" para abrir el árbol.
* Haga clic en la flecha situada a la izquierda de "Elemento 0" para abrir el árbol.
* Cambie el nombre del primer elemento de Elemento 0 a "Combinaciones de URL".
* Haga clic en la flecha a la izquierda de "Combinaciones de URL" para abrir el árbol.
* En la columna "Valor", hay un campo en blanco a la izquierda de "Elemento 0" debajo "Combinaciones de URL".  Establezca el valor al esquema único de la aplicación.  Debe coincidir con el esquema de redirectURL cuando se crea el objeto OIDAuthorizationRequest.  En el ejemplo, se usa el esquema "com.onmicrosoft.fabrikamb2c.exampleapp".

Consulte la [guía de AppAuth](https://openid.github.io/AppAuth-iOS/) sobre cómo completar el resto del proceso. Si necesita comenzar rápidamente con una aplicación de trabajo, extraiga [el ejemplo](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c). Siga los pasos que aparecen en el archivo [README.md](https://github.com/Azure-Samples/active-directory-ios-native-appauth-b2c/blob/master/README.md) para escribir su propia configuración de Azure AD B2C.

Siempre estamos abiertos a todo tipo de comentarios y sugerencias. Si tiene dificultades con este artículo o tiene recomendaciones para mejorar este contenido, le agradeceríamos que escriba sus comentarios en la parte inferior de la página. Si tiene solicitudes de características, agréguelas a [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).
