---
title: Elección del protocolo de federación adecuado para una aplicación multiinquilino
description: Guía para proveedores de software independientes sobre la integración con Azure Active Directory
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeeds
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b3edbbe037c3874d639476e516b3732b7573d9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443376"
---
# <a name="choose-the-right-federation-protocol-for-your-multi-tenant-application"></a>Elección del protocolo de federación correcto para una aplicación multiinquilino

Cuando se desarrollan aplicaciones de software como servicio (SaaS), es preciso seleccionar el protocolo de federación que mejor satisfaga las necesidades tanto propias como las de sus clientes. Esta decisión se basa en su plataforma de desarrollo y en su deseo de realizar la integración con los datos disponibles en el ecosistema de Office 365 y Azure AD de sus clientes.

Vea la lista completa de [protocolos disponibles para las integraciones de SSO](what-is-single-sign-on.md) en Azure Active Directory.
La siguiente tabla compara 
* Open Authentication 2.0 (OAuth 2.0)
* Open ID Connect (OIDC)
* Lenguaje de marcado de aserción de seguridad (SAML)
* Web Services Federation (WSFed)

| Capacidad| OAuth/OIDC| SAML/WSFed |
| - |-|-|
| Inicio de sesión único basado en web| √| √ |
| Cierre de sesión único basado en web| √| √ |
| Inicio de sesión único basado en dispositivos móviles| √| √* |
| Cierre de sesión único basado en dispositivos móviles| √| √* |
| Directivas de acceso condicional para aplicaciones para dispositivos móviles| √| X |
| Experiencia MFA completa para aplicaciones para dispositivos móviles| √| X |
| Acceso a Microsoft Graph| √| X |

*Es posible, pero Microsoft no proporciona ejemplos ni guía.

## <a name="oauth-20-and-open-id-connect"></a>OAuth 2.0 y Openid Connect

OAuth 2.0 es un protocolo [estándar de la industria](https://oauth.net/2/) para la autorización. OIDC (OpenID Connect) es un nivel de autenticación de identidad [estándar de la industria](https://openid.net/connect/) creado a partir del protocolo OAuth 2.0.

### <a name="benefits"></a>Ventajas

Microsoft recomienda el uso de OIDC/OAuth 2.0, ya que cuentan con autenticación y autorización integradas en los protocolos. Con SAML, además es preciso implementar la autorización.

La autorización inherente a estos protocolos permite a cualquier aplicación no solo acceder a un gran número de datos de los usuarios y de la organización sino también integrarse en ellos mediante Microsoft Graph API.

El uso de OAuth 2.0 y OIDC simplifica la experiencia de usuario final de sus clientes al adoptar el inicio de sesión único para su aplicación. Puede definir fácilmente los conjuntos de permisos necesarios, que luego se representan automáticamente para que el administrador o el usuario final den su consentimiento.

Además, el uso de estos protocolos permite a los clientes usar directivas de Microsoft Azure Multi-Factor Authentication y de acceso condicional para controlar el acceso a las aplicaciones. Microsoft proporciona bibliotecas y [ejemplos de código en varias plataformas tecnológicas](https://github.com/AzureAD/microsoft-authentication-library-for-js/wiki/Samples) para facilitar el desarrollo.  

### <a name="implementation"></a>Implementación

Registre la aplicación en Microsoft Identity, que es un proveedor de OAuth 2.0. Luego también puede registrar su aplicación basada en OAuth 2.0 en cualquier otro proveedor de identidades en el que desee integrarse. 

Para obtener información acerca de cómo registrar una aplicación e implementar estos protocolos para el inicio de sesión único de las aplicaciones web, consulte [Autorización del acceso a aplicaciones web con OpenID Connect y Azure Active Directory](../develop/sample-v2-code.md).  Para obtener información acerca de cómo implementar estos protocolos para el inicio de sesión único en aplicaciones para dispositivos móviles, consulte lo siguiente: 

* [Android](../develop/quickstart-v2-android.md)

* [iOS](../develop/quickstart-v2-ios.md)

* [Plataforma universal de Windows](../develop/quickstart-v2-uwp.md)

## <a name="saml-20-and-wsfed"></a>SAML 2.0 y WSFed

Lenguaje de marcado de aserción de seguridad (SAML) se utiliza normalmente para las aplicaciones web. Para obtener información general, consulte [Uso del protocolo SAML por parte de Azure AD](../develop/active-directory-saml-protocol-reference.md). 

Web Services Federation (WSFed) es un [estándar de la industria](https://docs.oasis-open.org/wsfed/federation/v1.2/ws-federation.html) que normalmente se usa para las aplicaciones web que se desarrollan mediante la plataforma. NET.

### <a name="benefits"></a>Ventajas

SAML 2.0 es un estándar maduro y la mayoría de las plataformas tecnológicas admiten las bibliotecas de código abierto para SAML 2.0. Puede proporcionar a los clientes una interfaz de administración para configurar el inicio de sesión único de SAML. Puede configurar el inicio de sesión único de SAML para Microsoft Azure AD y cualquier otro proveedor de identidades que sea compatible con SAML 2.0

### <a name="trade-offs"></a>Ventajas y desventajas

Al usar los protocolos SAML 2.0 o WSFed para aplicaciones para dispositivos móviles, determinadas directivas de acceso condicional, incluida la autenticación multifactor (MFA), tendrán una experiencia que se verá limitada. Además, si desea acceder a Microsoft Graph, será preciso que implemente la autorización mediante OAuth 2.0 para generar los tokens necesarios. 

### <a name="implementation"></a>Implementación

Microsoft no proporciona bibliotecas para la implementación de SAML ni bibliotecas específicas recomendadas. Hay muchas bibliotecas de código abierto disponibles.

## <a name="sso-and-using-microsoft-graph-rest-api"></a>Inicio de sesión único y uso de la API REST de Microsoft Graph 

Microsoft Graph es el tejido de datos de todos los elementos de Microsoft 365, entre los que se incluyen Office 365, Windows 10 y Enterprise Mobility y Security, y otros productos como Dynamics 365. Aquí se incluyen los esquemas principales de las entidades, como Usuarios, Grupos, Calendario, Correo electrónico, Archivos, etc, que potencian la productividad del usuario. Microsoft Graph ofrece tres interfaces a los desarrolladores: una API basada en REST, una conexión de datos de Microsoft Graph y Conectores que permiten a los desarrolladores agregar sus propios datos a Microsoft Graph.  

El uso de cualquiera de los protocolos anteriores para el inicio de sesión único permite el acceso de su aplicación a la gran cantidad de datos disponibles a través de la API REST de Microsoft Graph. Esto permite a los clientes obtener más valor de su inversión en Microsoft 365. Por ejemplo, la aplicación puede llamar a Microsoft Graph API para integrarse con la instancia de Office 365 de sus clientes y con los elementos de Microsoft Office y SharePoint de la aplicación de sus usuarios expuestos. 

Si utiliza Open ID Connect para autenticarse, su experiencia de desarrollo será perfecta, ya que usará OAuth2, la base de Open ID Connect, para adquirir tokens que pueden usarse para invocar Microsoft Graph API. Si la aplicación usa SAML o WSFed, debe agregar código adicional dentro ella para obtener OAuth2 para adquirir los tokens necesarios para invocar Microsoft Graph API. 

## <a name="next-steps"></a>Pasos siguientes

[Habilitar el inicio de sesión único para la aplicación multiinquilino](isv-sso-content.md)

[Creación de documentación para una aplicación multiinquilino](isv-create-sso-documentation.md)
