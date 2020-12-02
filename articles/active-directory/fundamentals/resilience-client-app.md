---
title: Aumento de la resistencia de la autenticación y la autorización en las aplicaciones cliente que se desarrollan
titleSuffix: Microsoft identity platform
description: Guía para aumentar la resistencia de la autenticación y la autorización en una aplicación cliente que usa la Plataforma de identidad de Microsoft
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: knicholasa
ms.author: nichola
manager: martinco
ms.date: 11/23/2020
ms.openlocfilehash: 9189d4d8cda5f9fcfce7e6ac2097414aa29f0a68
ms.sourcegitcommit: e5f9126c1b04ffe55a2e0eb04b043e2c9e895e48
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2020
ms.locfileid: "96317476"
---
# <a name="increase-the-resilience-of-authentication-and-authorization-in-client-applications-you-develop"></a>Aumento de la resistencia de la autenticación y la autorización en las aplicaciones cliente que se desarrollan

Esta sección constituye una guía sobre la compilación de resistencia en aplicaciones cliente que usan la Plataforma de identidad de Microsoft y Azure Active Directory para el inicio de sesión de los usuarios y la ejecución de acciones en nombre de estos.

## <a name="use-the-microsoft-authentication-library-msal"></a>Uso de la Biblioteca de autenticación de Microsoft (MSAL)

La [Biblioteca de autenticación de Microsoft (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview) es un elemento clave de la [Plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop). Simplifica y administra la adquisición, la administración, el almacenamiento en caché y la actualización de tokens y usa procedimientos recomendados para la resistencia. MSAL está diseñada como solución segura sin que los desarrolladores tengan que preocuparse por los detalles de implementación.

MSAL almacena en caché los tokens y usa un patrón de adquisición de tokens silencioso. También serializa automáticamente la caché de tokens en plataformas que proporcionan almacenamiento seguro de forma nativa, como Windows UWP, iOS y Android. Los desarrolladores pueden personalizar el comportamiento de serialización mediante [Microsoft.Identity.Web](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization), [MSAL.NET](https://docs.microsoft.com/azure/active-directory/develop/msal-net-token-cache-serialization), [MSAL para Java](https://docs.microsoft.com/azure/active-directory/develop/msal-java-token-cache-serialization) y [MSAL para Python](https://docs.microsoft.com/azure/active-directory/develop/msal-python-token-cache-serialization).

![Imagen de dispositivo con aplicación que usa MSAL para llamar a la Plataforma de identidad de Microsoft](media/resilience-client-app/resilience-with-microsoft-authentication-library.png)

Al usar MSAL, se obtiene almacenamiento en caché, actualización y adquisición de tokens silenciosa con el siguiente patrón.

```csharp
try
{
    result = await app.AcquireTokenSilent(scopes, account).ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
    result = await app.AcquireToken(scopes).WithClaims(ex.Claims).ExecuteAsync()
}
```

En algunos casos, MSAL puede actualizar tokens de forma proactiva. Cuando la Plataforma de identidad de Microsoft emite un token de larga duración, puede enviar información al cliente sobre el momento óptimo para actualizar el token ("refresh\_in"). MSAL actualiza de forma proactiva el token en función de esta información. La aplicación sigue ejecutándose mientras el token anterior es válido, pero tiene un período de tiempo más largo durante el cual realizar otra adquisición de token correcta.

### <a name="stay-up-to-date"></a>Manténgase actualizado.

Los desarrolladores deben tener un proceso de actualización a la versión de MSAL más reciente. La autenticación es parte de la seguridad de la aplicación y esta debe mantenerse actualizada con las mejoras de seguridad incluidas en las nuevas versiones de MSAL. Este suele ser un procedimiento que se recomienda para las bibliotecas en desarrollo continuo y que garantiza que se disponga del código más actualizado con respecto a la resistencia de la aplicación. Puesto que la Plataforma de identidad de Microsoft sigue innovando para ofrecer maneras en que las aplicaciones sean más resistentes, aquellas aplicaciones que usan la versión más reciente de MSAL son las más preparadas para compilar conforme a estas innovaciones.

[Vea la versión más reciente de MSAL.js y las notas de la versión](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)

[Vea la versión más reciente de MSAL.NET y las notas de la versión](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/releases)

[Vea la versión más reciente de MSAL Python y las notas de la versión](https://github.com/AzureAD/microsoft-authentication-library-for-python/releases)

[Vea la versión más reciente de MSAL Java y las notas de la versión](https://github.com/AzureAD/microsoft-authentication-library-for-java/releases)

[Vea la versión más reciente de MSAL iOS y macOS y las notas de la versión](https://github.com/AzureAD/microsoft-authentication-library-for-objc/releases)

[Vea la versión más reciente de MSAL Android y las notas de la versión](https://github.com/AzureAD/microsoft-authentication-library-for-android/releases)

[Vea la versión más reciente de MSAL Angular y las notas de la versión](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases)

[Vea la versión más reciente de Microsoft.Identity.Web y las notas de la versión](https://github.com/AzureAD/microsoft-identity-web/releases)

## <a name="if-not-using-msal-use-these-resilient-patterns-for-token-handling"></a>Patrones de resistencia para el control de tokens si no se usa MSAL

En general, una aplicación que usa autenticación moderna llama a un punto de conexión para recuperar los tokens que autentican al usuario o autorizan a la aplicación a llamar a API protegidas. MSAL está diseñada para controlar los detalles de autenticación e implementa varios patrones para mejorar la resistencia de este proceso. Use la guía de esta sección para implementar procedimientos recomendados si decide usar una biblioteca que no sea MSAL. Si usa MSAL, obtiene todos estos procedimientos recomendados de forma gratuita, ya que MSAL los implementa automáticamente.

### <a name="cache-tokens"></a>Almacenamiento de tokens en caché

Las aplicaciones deben almacenar en caché correctamente los tokens recibidos de la Plataforma de identidad de Microsoft. Cuando la aplicación recibe tokens, la respuesta HTTP que contiene los tokens también contiene una propiedad "expires_in" que indica a la aplicación durante cuánto tiempo se debe almacenar en caché, y volver a usar, el token. Es importante que las aplicaciones usen la propiedad "expires_in" para determinar la duración del token. La aplicación nunca debe intentar descodificar un token de acceso de API.

![Aplicación realizando una llamada a la Plataforma de identidad de Microsoft, pero la llamada pasa a través de una caché de tokens en el dispositivo que ejecuta la aplicación](media/resilience-client-app/token-cache.png)

El uso de tokens almacenados en caché evita el tráfico innecesario entre la aplicación y la Plataforma de identidad de Microsoft, y hace que la aplicación sea menos susceptible a errores de adquisición de tokens al reducir el número de llamadas de adquisición de tokens. Los tokens almacenados en caché también mejoran el rendimiento de la aplicación, ya que esta necesita bloquear menos la adquisición de tokens. El usuario puede mantener la sesión iniciada en la aplicación durante la vigencia del token.

### <a name="serialize-and-persist-tokens"></a>Serialización y conservación de tokens

Las aplicaciones deben serializar de forma segura su caché de tokens para conservar los tokens entre instancias de la aplicación. Los tokens se pueden volver a usar siempre que sigan vigentes. Los [tokens de actualización](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow#refresh-the-access-token) y, cada vez más, los [tokens de acceso](https://docs.microsoft.com/azure/active-directory/develop/access-tokens), se emiten para muchas horas. Esta vigencia válida puede abarcar muchos inicios de la aplicación por parte de un usuario. Cuando se inicia la aplicación, debe comprobar si hay un token de acceso o actualización válido que se pueda usar. Esto aumenta la resistencia y el rendimiento de la aplicación, ya que evita las llamadas innecesarias a la Plataforma de identidad de Microsoft.

![Aplicación realizando una llamada a la Plataforma de identidad de Microsoft, pero la llamada pasa a través de una caché de tokens y un almacén de tokens en el dispositivo que ejecuta la aplicación](media/resilience-client-app/token-store.png)

El almacenamiento de tokens persistente debe tener el acceso controlado y estar cifrado para el usuario propietario o la identidad del proceso. En plataformas como aquellas para dispositivos móviles, Windows y Mac, el desarrollador debe aprovechar las capacidades integradas para el almacenamiento de credenciales.

### <a name="acquire-tokens-silently"></a>Adquisición silenciosa de tokens

El proceso de autenticación de un usuario o de recuperación de la autorización para llamar a una API puede requerir varios pasos en la Plataforma de identidad de Microsoft. Por ejemplo, cuando el usuario inicia sesión por primera vez, es posible que necesite escribir credenciales y realizar una autenticación multifactor mediante un mensaje de texto. Cada paso agrega una dependencia sobre el recurso que proporciona ese servicio. La mejor experiencia para el usuario, y la que conlleva menos dependencias, es intentar adquirir un token de manera silenciosa para evitar estos pasos adicionales antes de solicitar la interacción del usuario.

![Diagrama en el que se muestran los diversos servicios de la Plataforma de identidad de Microsoft que pueden necesitar ejecutarse para completar el proceso de autenticación o autorización de un usuario](media/resilience-client-app/external-dependencies.png)

La adquisición de un token de forma silenciosa comienza con el uso de un token válido de la caché de tokens de la aplicación. Si no hay ningún token válido disponible, la aplicación debe intentar adquirir uno mediante un token de actualización, si lo hubiera, y el punto de conexión del token. Si ninguna de estas opciones está disponible, la aplicación debe adquirir un token mediante el parámetro "prompt=none". Así se usa el punto de conexión de autorización, pero no se muestra ninguna interfaz de usuario a este. Si la Plataforma de identidad de Microsoft puede proporcionar un token a la aplicación sin interactuar con el usuario, lo va a hacer. Si ninguno de estos métodos se traduce en un token, el usuario tiene que volver a autenticarse de forma interactiva.

> [!NOTE]
> En general, las aplicaciones deben evitar el uso de solicitudes para el "inicio de sesión" y el "consentimiento", ya que obligan a la interacción del usuario aunque no se requiera ninguna.

## <a name="handle-service-responses-properly"></a>Control adecuado de las respuestas del servicio

Aunque las aplicaciones deben controlar todas las respuestas de error, hay algunas que pueden afectar a la resistencia. Si la aplicación recibe un código de respuesta HTTP 429, Demasiadas solicitudes, la Plataforma de identidad de Microsoft está limitando las solicitudes. Si la aplicación continúa realizando demasiadas solicitudes, se sigue limitando, lo que evita que reciba tokens. La aplicación no debe intentar adquirir un token de nuevo hasta que haya pasado el tiempo, en segundos, del campo de respuesta Retry-After. La recepción de una respuesta 429 suele indicar que la aplicación no está almacenando en caché ni reutilizando los tokens correctamente. Los desarrolladores deben revisar el modo en que los tokens se almacenan en caché y se reutilizan en la aplicación.

Cuando una aplicación recibe un código de respuesta HTTP 5xx, no debe entrar en un bucle de reintentos rápido. Si está presente, la aplicación debe respetar el mismo control Retry-After que para una respuesta 429. Si la respuesta no proporciona ningún encabezado Retry-After, se recomienda implementar un reintento de retroceso exponencial con el primer reintento al menos 5 segundos después de la respuesta.

Cuando una solicitud agota el tiempo de espera, las aplicaciones no deben volver a intentarlo inmediatamente. Implemente un reintento de retroceso exponencial con el primer reintento al menos 5 segundos después de la respuesta.

## <a name="evaluate-options-for-retrieving-authorization-related-information"></a>Evaluación de las opciones para recuperar información relacionada con la autorización

Muchas aplicaciones y API necesitan información específica sobre el usuario para tomar decisiones de autorización. Hay varias maneras de que una aplicación obtenga esta información. Cada método tiene sus ventajas e inconvenientes. Los desarrolladores deben sopesarlas para determinar qué estrategia es mejor para la resistencia de su aplicación.

### <a name="tokens"></a>Tokens

Los tokens de identidad (id.) y los tokens de acceso contienen notificaciones estándar que proporcionan información sobre el sujeto. Están documentados en [Tokens de id. de la Plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/id-tokens) y [Tokens de acceso de la Plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/access-tokens). Si la información que necesita la aplicación ya está en el token, la técnica más eficaz para recuperar esos datos es usar notificaciones de token, ya que esto ahorra la sobrecarga de una llamada de red adicional para recuperar la información por separado. Menos llamadas de red significan una mayor resistencia general de la aplicación.

> [!NOTE]
> Algunas aplicaciones llaman al punto de conexión UserInfo para recuperar notificaciones sobre el usuario autenticado. La información disponible en el token de id. que la aplicación puede recibir es un superconjunto de la información que puede obtener del punto de conexión de UserInfo. La aplicación debe usar el token de id. para obtener información sobre el usuario en lugar de llamar al punto de conexión UserInfo.

Un desarrollador de aplicaciones puede aumentar las notificaciones de token estándar con [notificaciones opcionales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims). Una notificación opcional común es [grupos](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims#configuring-groups-optional-claims). Existen varias formas de agregar notificaciones de grupo. La opción "Grupo de aplicaciones" solo incluye los grupos asignados a la aplicación. Las opciones "Todos" o "Grupos de seguridad" incluyen los grupos de todas las aplicaciones del mismo inquilino, que pueden agregar muchos grupos al token. Es importante evaluar el efecto en su caso, ya que puede anular la eficacia lograda al solicitar grupos en el token al provocar el sobredimensionamiento de los tokens e incluso requerir llamadas adicionales para obtener la lista completa de grupos.

En lugar de usar grupos en el token, puede emplear e incluir roles de aplicación. Los desarrolladores pueden definir [roles de aplicación](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) para sus aplicaciones y API que el cliente pueda administrar desde su directorio mediante el portal o las API. Luego, los profesionales de TI pueden asignar roles a diferentes usuarios y grupos para controlar quién tiene acceso a qué contenido y funcionalidad. Cuando se emite un token para la aplicación o la API, los roles asignados al usuario están disponibles en la notificación de roles del token. La obtención de esta información directamente en un token puede ahorrar llamadas API adicionales.

Por último, los administradores de TI también pueden agregar notificaciones basadas en información específica de un inquilino. Por ejemplo, una empresa puede contar con una extensión para tener un identificador de usuario específico de la empresa.

En todos los casos, la incorporación de información del directorio directamente a un token puede ser eficaz y aumentar la resistencia de las aplicaciones al reducir el número de dependencias que tiene la aplicación. Por otro lado, no soluciona ningún problema de resistencia relacionado con la imposibilidad de adquirir un token. Solo debe agregar notificaciones opcionales en los escenarios principales de la aplicación. Si la aplicación únicamente necesita información para la funcionalidad de administración, es mejor que la obtenga solo cuando sea necesario.

### <a name="microsoft-graph"></a>Microsoft Graph

Microsoft Graph proporciona un punto de conexión de API unificado para acceder a los datos de Microsoft 365 que describen los patrones de productividad, identidad y seguridad de una organización. Las aplicaciones que usan Microsoft Graph pueden emplear cualquier información de Microsoft 365 para tomar decisiones de autorización.

Las aplicaciones solo requieren un token para acceder a todo Microsoft 365. Esto ofrece más resistencia que el uso de las API anteriores que son específicas de componentes de Microsoft 365 como Microsoft Exchange o Microsoft SharePoint, donde se requieren varios tokens.

Al usar API de Microsoft Graph, se sugiere emplear un [SDK de Microsoft Graph](https://docs.microsoft.com/graph/sdks/sdks-overview). Los SDK de Microsoft Graph se han diseñado para simplificar la compilación de aplicaciones de alta calidad, eficaces y resistentes que acceden a Microsoft Graph.

En el caso de las decisiones de autorización, los desarrolladores deben considerar cuándo usar las notificaciones disponibles en un token como alternativa a algunas llamadas de Microsoft Graph. Como se ha mencionado anteriormente, los desarrolladores podrían solicitar grupos, roles de aplicación y notificaciones opcionales en sus tokens. En lo que respecta a la resistencia, el uso de Microsoft Graph para la autorización requiere llamadas de red adicionales que se basan en la Plataforma de identidad de Microsoft (para obtener el token para acceder a Microsoft Graph), así como el propio Microsoft Graph. Pero si la aplicación ya se basa en Microsoft Graph como capa de datos, confiar en Graph para la autorización no supone ningún riesgo adicional.

## <a name="use-broker-authentication-on-mobile-devices"></a>Uso de la autenticación de agente en dispositivos móviles

En los dispositivos móviles, el uso de un agente de autenticación como Microsoft Authenticator mejora la resistencia. El agente aporta más ventajas que otras opciones, como el explorador del sistema o una vista web insertada. El agente de autenticación puede usar un [token de actualización principal](https://docs.microsoft.com/azure/active-directory/devices/concept-primary-refresh-token) (PRT), que contiene notificaciones sobre el usuario y el dispositivo, y se puede usar para obtener tokens de autenticación para acceder a otras aplicaciones desde el dispositivo. Cuando se usa un PRT para solicitar acceso a una aplicación, sus notificaciones de dispositivo y MFA son de confianza para Azure AD. Esto aumenta la resistencia al evitar pasos adicionales para autenticar el dispositivo de nuevo. No se desafía a los usuarios con varias solicitudes de MFA en el mismo dispositivo, con lo que se aumenta la resistencia al reducir las dependencias sobre servicios externos y se mejora la experiencia del usuario.

![Aplicación realizando una llamada a la Plataforma de identidad de Microsoft, pero la llamada pasa a través de una caché de tokens, un almacén de tokens y un agente de autenticación en el dispositivo que ejecuta la aplicación](media/resilience-client-app/authentication-broker.png)

MSAL admite automáticamente la autenticación de agente. Puede encontrar más información sobre el uso de la autenticación asincrónica en las páginas siguientes:

- [Configuración del inicio de sesión único en macOS e iOS](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-macos-ios#sso-through-authentication-broker-on-ios)
- [Procedimiento: Habilitación de SSO entre aplicaciones en Android mediante MSAL](https://docs.microsoft.com/azure/active-directory/develop/msal-android-single-sign-on)

## <a name="adopt-continuous-access-evaluation"></a>Adopción de la Evaluación continua de acceso

La [Evaluación continua de acceso (CAE)](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-continuous-access-evaluation) es un desarrollo reciente que puede aumentar la seguridad y la resistencia de las aplicaciones con tokens de larga duración. CAE es un estándar emergente del sector que se desarrolla en el grupo de trabajo Shared Signals and Events de OpenID Foundation. Con CAE, un token de acceso se puede revocar basándose en [eventos críticos](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-continuous-access-evaluation#critical-event-evaluation) y [evaluación de directivas](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-continuous-access-evaluation#conditional-access-policy-evaluation-preview) en lugar de en una duración de token breve. En el caso de algunas API de recursos, ya que el riesgo y la directiva se evalúan en tiempo real, CAE puede aumentar considerablemente la duración del token hasta 28 horas. A medida que las API de recursos y las aplicaciones adopten CAE, la Plataforma de identidad de Microsoft podrá emitir tokens de acceso que sean revocables y válidos durante períodos de tiempo prolongados. Estos tokens de larga duración se actualizarán de forma proactiva mediante MSAL.

Aunque CAE está en una fase temprana, es posible [en la actualidad desarrollar aplicaciones cliente que se beneficien de CAE](../develop/app-resilience-continuous-access-evaluation.md) si los recursos (API) que usa la aplicación adoptan CAE. A medida que más recursos adopten CAE, la aplicación podrá adquirir tokens habilitados para CAE para esos recursos. La API de Microsoft Graph y los [SDK de Microsoft Graph](https://docs.microsoft.com/graph/sdks/sdks-overview) tendrán una versión preliminar de la capacidad CAE a principios de 2021. Si quiere participar en la versión preliminar pública de Microsoft Graph con CAE, puede comunicarnos que está interesado aquí: [https://aka.ms/GraphCAEPreview](https://aka.ms/GraphCAEPreview).

Si desarrolla API de recursos, se le recomienda participar en el [grupo de trabajo Shared Signals and Events](https://openid.net/wg/sse/). Se está colaborando con este grupo para habilitar el uso compartido de eventos de seguridad entre la Plataforma de identidad de Microsoft y los proveedores de recursos.

## <a name="next-steps"></a>Pasos siguientes

- [Uso de las API habilitadas para la evaluación continua de acceso en las aplicaciones](../develop/app-resilience-continuous-access-evaluation.md)
- [Compilación de resistencia en aplicaciones demonio](resilience-daemon-app.md)
- [Compilación de resistencia en la infraestructura de administración de identidades y acceso](resilience-in-infrastructure.md)
- [Aumento de la resistencia en los sistemas CIAM](resilience-b2c.md)
