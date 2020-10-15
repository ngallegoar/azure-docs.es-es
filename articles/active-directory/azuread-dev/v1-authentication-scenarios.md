---
title: Azure AD para desarrolladores (v1.0) | Azure
description: Aprenda los conceptos básicos sobre la autenticación de Azure AD para desarrolladores (v 1.0), como el modelo de aplicaciones, la API, el aprovisionamiento y los escenarios de autenticación más comunes.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.topic: conceptual
ms.workload: identity
ms.date: 10/14/2019
ms.author: ryanwi
ms.reviewer: saeeda, sureshja, hirsin
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: 36b39f3706db615e40ebfadebf36be4d8b29c33e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80154736"
---
# <a name="what-is-authentication"></a>¿Qué es la autenticación?

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

La *autenticación*es acto de solicitar a un usuario credenciales legítimas, lo que proporciona la base para la creación de una entidad de seguridad que se utilizará para el control de identidades y de acceso. En términos más sencillos, es el proceso de comprobar que es quien dice ser. A veces, la autenticación se abrevia como AuthN.

La *autorización* es el acto de conceder un permiso a la entidad de seguridad autenticada para hacer algo. Especifica a qué datos puede acceder y qué puede hacer con ellos. A veces, la autorización se abrevia como AuthZ.

Azure Active Directory para desarrolladores (v1.0) (Azure AD) simplifica la autenticación para los desarrolladores de aplicaciones, ya que ofrece la identidad como servicio, con compatibilidad con protocolos estándares del sector, como OAuth 0 y OpenID Connect, además de bibliotecas de código abierto para distintas plataformas para poder empezar a programar rápidamente.

Hay dos casos de uso principales en el modelo de programación de Azure AD:

* Durante un flujo de concesión de autorización de OAuth 2.0: cuando el propietario del recurso autoriza a la aplicación cliente, lo que permite al cliente acceder a los recursos del propietario del recurso.
* Durante el acceso a los recursos por el cliente: cuando lo implementa el servidor de recursos, mediante el uso de los valores de notificación presentes en el token de acceso para tomar decisiones de control de acceso basadas en ellos.

## <a name="authentication-basics-in-azure-ad"></a>Conceptos básicos sobre la autenticación de Azure AD

Veamos el escenario más básico en el que es necesario identificarse: un usuario de un explorador web debe autenticarse en una aplicación web. El siguiente diagrama muestra este escenario:

![Información general de inicio de sesión en la aplicación web](./media/v1-authentication-scenarios/auth-basics-microsoft-identity-platform.svg)

Esto es lo que necesita saber sobre los distintos componentes que se muestran en el diagrama:

* Azure AD es el proveedor de identidades. El proveedor de identidades es el responsable de comprobar la identidad de los usuarios y las aplicaciones que existen en el directorio de una organización y de emitir tokens de seguridad tras la autenticación correcta de dichos usuarios y aplicaciones.
* Cualquier aplicación que necesite externalizar la autenticación a Azure AD se debe registrar en Azure Active Directory (Azure AD). Azure AD registra la aplicación y la identifica de forma única en el directorio.
* Los desarrolladores pueden usar las bibliotecas de autenticación de código abierto de Azure AD para facilitar la autenticación, ya que administran los detalles de los protocolos para el usuario. Para más información, consulte las [bibliotecas de autenticación v2.0](../develop/reference-v2-libraries.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) y las [bibliotecas de autenticación v1.0](active-directory-authentication-libraries.md) de la plataforma de identidad de Microsoft.
* Una vez autenticado el usuario, la aplicación debe validar el token de seguridad de este para garantizar que la autenticación se realizó correctamente. Puede encontrar guías de inicio rápido, tutoriales y ejemplos de código en una variedad de lenguajes y marcos que muestran lo que debe hacer la aplicación.
  * Para crear rápidamente una aplicación y agregar funcionalidad similar a obtener tokens, actualizar tokens, iniciar la sesión de un usuario, mostrar información de usuario y mucho más, vea la sección **Inicios rápidos** de la documentación.
  * Para obtener procedimientos detallados basados en un escenario para tareas de autenticación principales para desarrolladores, como obtener tokens de acceso y usarlos en las llamadas a Microsoft Graph API y otras API, implementar el inicio de sesión con Microsoft con una aplicación tradicional basada en explorador web mediante OpenID Connect y más, consulte la sección **Tutoriales** de la documentación.
  * Para descargar ejemplos de código, vaya a [GitHub](https://github.com/Azure-Samples?q=active-directory).
* El flujo de solicitudes y respuestas del proceso de autenticación lo determina el protocolo de autenticación que usó, como OAuth 2.0, OpenID Connect, WS-Federation o SAML 2.0. Para más información sobre los protocolos, consulte la sección **Conceptos > Protocolo de autenticación** de la documentación.

En el escenario de ejemplo anterior, puede clasificar las aplicaciones según estos dos roles:

* Aplicaciones que necesitan acceso seguro a recursos
* Aplicaciones que desempeñan el rol del propio recurso

### <a name="how-each-flow-emits-tokens-and-codes"></a>Cómo emite cada flujo los tokens y los códigos

En función de cómo se compile el cliente, puede usar uno o varios de los flujos de autenticación que admite Azure AD. Estos flujos pueden generar diversos tokens (id_tokens, tokens de actualización, tokens de acceso) además de códigos de autorización, y requieren distintos tokens para que funcionen. En este gráfico se ofrece una información general:

|Flujo | Requiere | ID_token | de la aplicación Twitter | Token de actualización | código de autorización | 
|-----|----------|----------|--------------|---------------|--------------------|
|[Flujo de código de autorización](v1-protocols-oauth-code.md) | | x | x | x | x|  
|[Flujo implícito](v1-oauth2-implicit-grant-flow.md) | | x        | x    |      |                    |
|[Flujo de OIDC híbrido](v1-protocols-openid-connect-code.md#get-access-tokens)| | x  | |          |            x   |
|[Redención de token de actualización](v1-protocols-oauth-code.md#refreshing-the-access-tokens) | Token de actualización | x | x | x| |
|[Flujo en nombre de](v1-oauth2-on-behalf-of-flow.md) | de la aplicación Twitter| x| x| x| |
|[Credenciales de cliente](v1-oauth2-client-creds-grant-flow.md) | | | x (solo aplicación)| | |

Los tokens emitidos a través del modo implícito tienen una limitación de longitud debido a que se pasan al explorador a través de la dirección URL (donde `response_mode` es `query` o `fragment`).  Algunos exploradores tienen un límite en el tamaño de la dirección URL que se puede colocar en la barra del explorador y producen un error cuando es demasiado larga.  Por lo tanto, estos tokens no tienen notificaciones `groups` ni `wids`. 

Ahora que ya tiene información general sobre los conceptos básicos, siga leyendo para comprender la API y el modelo de aplicaciones de identidad, el funcionamiento del aprovisionamiento en Azure AD y enlaces a información detallada sobre los escenarios comunes que admite Azure AD.

## <a name="application-model"></a>Modelo de aplicación

Azure AD representa aplicaciones siguiendo un modelo específico que se ha diseñado para satisfacer dos funciones principales:

* **Identificar la aplicación de acuerdo con los protocolos de autenticación que admite**: esto implica enumerar todos los identificadores, direcciones URL, secretos e información relacionada necesaria en el momento de la autenticación. Aquí, Azure AD:

    * Contiene todos los datos necesarios para admitir la autenticación en tiempo de ejecución.
    * Contiene todos los datos para decidir qué podría necesitar una aplicación para acceder y si una solicitud determinada se debería completar y en qué circunstancias.
    * Proporciona la infraestructura para implementar el aprovisionamiento de la aplicación dentro del inquilino del desarrollador de la aplicación y en cualquier otro inquilino de Azure AD.

* **Controlar el consentimiento del usuario en el momento de solicitud del token y facilitar el aprovisionamiento dinámico de aplicaciones en varios inquilinos**. Este caso, Azure AD:

    * Permite a los usuarios y administradores conceder o denegar el consentimiento dinámicamente para que la aplicación acceda a recursos en su nombre.
    * Permite a los administradores decidir qué pueden hacer las aplicaciones en última instancia, qué usuarios pueden utilizar aplicaciones específicas y cómo se accede a los recursos de directorio.

En Azure AD, un **objeto de aplicación** describe una aplicación como entidad abstracta. Los desarrolladores trabajan con aplicaciones. Durante la implementación, Azure AD usa un objeto de aplicación determinado como plano técnico para crear una **entidad de servicio**, que representa una instancia concreta de una aplicación en un directorio o inquilino. Es la entidad de servicio la que define lo que la aplicación realmente puede hacer en un directorio de destino específico, quién la puede usar, a qué recursos tiene acceso, etc. Azure AD crea una entidad de servicio desde un objeto de aplicación a través del **consentimiento**.

En el siguiente diagrama se muestra un flujo de aprovisionamiento de Azure AD basado en el consentimiento.  En él, existen dos inquilinos (A y B), el inquilino A es propietario de la aplicación y el inquilino B crea una instancia de la aplicación mediante una entidad de servicio.  

![Flujo de aprovisionamiento simplificado basado en el consentimiento](./media/v1-authentication-scenarios/simplified-provisioning-flow-consent-driven.svg)

En este flujo de aprovisionamiento:

1. Un usuario del inquilino B intenta iniciar sesión con la aplicación y el punto de conexión de autorización solicita un token para la aplicación.
1. Se adquieren y verifican las credenciales de usuario para la autenticación.
1. El usuario debe dar su consentimiento para que la aplicación tenga acceso al inquilino B.
1. Azure AD usa el objeto de aplicación del inquilino A como plano técnico para crear una entidad de servicio en el inquilino B.
1. El usuario recibe el token solicitado

Puede repetir este proceso tantas veces como quiera para otros inquilinos (C, D, etc.). El inquilino A conserva el plano técnico de la aplicación (objeto de aplicación). Los usuarios y administradores del resto de inquilinos en que se concede consentimiento a la aplicación conservan el control sobre lo que puede hacer la aplicación a través del objeto de la entidad de servicio correspondiente de cada inquilino. Para más información, consulte [Objetos de aplicación y de entidad de servicio en la plataforma de identidad de Microsoft](../develop/app-objects-and-service-principals.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

## <a name="claims-in-azure-ad-security-tokens"></a>Notificaciones de tokens de seguridad de Azure AD

Los tokens de seguridad (tokens de identificación y acceso) que emite Azure AD contienen notificaciones o aserciones de información sobre el usuario que se ha autenticado. Las aplicaciones pueden usar notificaciones para varias tareas, como:

* Validar el token
* Identificar el inquilino del directorio del firmante
* Mostrar información de usuario
* Determinar la autorización del firmante

Las notificaciones presentes en cualquier token de seguridad dependen del tipo de token, el tipo de credencial que se usa para autenticar al usuario y la configuración de la aplicación.

En la tabla siguiente se proporciona una breve descripción de cada tipo de notificación que Azure AD emite. Para obtener más información, consulte los [tokens de acceso](../develop/access-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) y [tokens de identificador](../develop/id-tokens.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json) que emite Azure AD.

| Notificación | Descripción |
| --- | --- |
| Identificador de aplicación | Identifica la aplicación que está usando el token. |
| Público | Identifica el recurso de destinatario al que está destinado el token. |
| Referencia de clase de contexto de autenticación de aplicación | Indica cómo se autenticó el cliente (cliente público frente a cliente confidencial). |
| Instante de autenticación | Registra la fecha y la hora de la autenticación. |
| Método de autenticación | Indica cómo se autenticó el firmante del token (contraseña, certificado, etc.). |
| Nombre | Proporciona el nombre de usuario, tal como se estableció en Azure AD. |
| Grupos | Contiene identificadores de objeto de los grupos de Azure AD de los que es miembro el usuario. |
| Proveedor de identidades | Registra el proveedor de identidades que autenticó al firmante del token. |
| Emitido a las | Registra la hora a la que se emitió el token; suele utilizarse para la actualización del token. |
| Emisor | Identifica al STS que emitió el token, así como el inquilino de Azure AD. |
| Apellido | Proporciona los apellidos del usuario, tal como se establecieron en Azure AD. |
| Nombre | Proporciona un valor en lenguaje natural que identifica al firmante del token. |
| Id. de objeto | Contiene un identificador único e inmutable del firmante en Azure AD. |
| Roles | Contiene los nombres descriptivos de los roles de aplicación de Azure AD que se han concedido al usuario. |
| Ámbito | Indica los permisos concedidos a la aplicación cliente. |
| Asunto | Indica la entidad de seguridad sobre la que el token valida información. |
| Id. de inquilino | Contiene un identificador único e inmutable del inquilino de directorio que emitió el token. |
| Vigencia del token | Define el intervalo de tiempo dentro del cual es válido un token. |
| Nombre principal del usuario | Contiene el nombre principal de usuario del firmante. |
| Versión | Contiene el número de versión del token. |

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre los [tipos de aplicaciones y escenarios que se admiten en la plataforma de identidad de Microsoft](app-types.md)
