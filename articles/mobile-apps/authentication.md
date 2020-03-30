---
title: Incorporación de autenticación a las aplicaciones móviles con Visual Studio App Center y servicios de Azure
description: Conozca los servicios de Visual Studio App Center que ayudan a configurar la autenticación de usuarios y a permitir que las aplicaciones móviles se autentiquen con cuentas de redes sociales, Azure Active Directory y autenticación personalizada.
author: codemillmatt
ms.assetid: 34a8a070-2222-4faf-9090-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 820005bca008ea3bdb59659c815da3ec36beb0d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241052"
---
# <a name="add-authentication-and-manage-user-identities-in-your-mobile-apps"></a>Incorporación de autenticación y administración de identidades de usuario en las aplicaciones móviles

Tener una vista de los usuarios y su comportamiento en la aplicación permite a los desarrolladores ofrecerles experiencias a medida para interaccionar mejor con ellos. Tanto si es un desarrollador de aplicaciones que crea una aplicación de colaboración para los usuarios de la organización como si está creando la siguiente plataforma de red social, necesita una manera de autenticar a los usuarios y administrar sus identidades. Un servicio de administración de identidades es una de las características más importantes de un servicio de back-end móvil.

Use los servicios siguientes para permitir la autenticación de usuarios en las aplicaciones móviles.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Auth](/appcenter/auth/) es un servicio de administración de identidades basado en la nube que los desarrolladores pueden usar para autenticar a los usuarios y administrar sus identidades. App Center auth también se integra con otras partes de Visual Studio App Center. Los desarrolladores pueden usar la identidad del usuario para [ver datos del usuario](/appcenter/data/index) en otros servicios e incluso [enviar notificaciones de inserción a los usuarios, en lugar de a dispositivos individuales](/appcenter/push/push-to-user#setting-user-identity). 

**Características principales**
- Con tecnología de Azure Active Directory B2C (Azure AD B2C). 
    - Nivel empresarial.
    - Se trata de un nivel de alta disponibilidad.
    - Servicio seguro y global.
- Bring Your Own Identity y la opción de usar otros proveedores de administración de identidades y acceso conocidos como Auth0 y Firebase.
- Compatibilidad de Azure Active Directory.
    - Conexión de los inquilinos de Azure AD existentes. 
    - Posibilidad de autenticación en un dominio corporativo.
    - Administración del acceso a datos confidenciales.
- Experiencia de usuario sencilla y experiencia mágica con el SDK gracias al encapsulado de la biblioteca de autenticación de Microsoft con el SDK de Visual Studio App Center.
- Compatibilidad con las plataformas iOS, Android, Xamarin y React Native.

**Referencias**
- [Registro con Visual Studio App Center](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs) 
- [Introducción a App Center Auth](/appcenter/auth/)

## <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C
[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/) es un servicio de administración de identidades de negocio a consumidor (B2C) que los desarrolladores pueden usar para autenticar a sus clientes. Este servicio de marca blanca permite a los desarrolladores personalizar y controlar el modo en que los usuarios interactúan de forma segura con sus aplicaciones web, de escritorio, móviles o de página única. Con Azure AD B2C, los usuarios pueden registrarse, iniciar sesión, restablecer contraseñas y editar perfiles. Azure AD B2C implementa un formulario de los protocolos OpenID Connect y OAuth 2.0. 

**Características principales**
- Autenticación segura de los clientes con su proveedor de identidades preferido.
- Administración de identidades y acceso de clientes.
- Obtenga compatibilidad con redes sociales, como Facebook, GitHub, Google, LinkedIn, Twitter, WeChat y Weibo.
- Conexión a las cuentas de los usuarios con protocolos estándar del sector, como OpenID Connect o SAML, para hacer posible la administración de identidades en una gran variedad de plataformas.
- Experiencias de inicio de sesión y registro con personalización de marca.
- Integración fácil con bases de datos de CRM, herramientas de análisis de marketing y sistemas de verificación de cuentas.
- Captura de datos de inicio de sesión, preferencias y conversión de los clientes.

**Referencias**
- [Azure Portal](https://portal.azure.com/)
- [Documentación de Azure AD B2C](/azure/active-directory-b2c/)
- [Guías de inicio rápido](/azure/active-directory-b2c/active-directory-b2c-quickstarts-web-app)
- [Muestras](/azure/active-directory-b2c/code-samples)

## <a name="azure-active-directory"></a>Azure Active Directory
[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) es un servicio en la nube de administración de identidades y acceso de Microsoft que ayuda a los empleados a iniciar sesión y obtener acceso a:
- recursos externos, como Microsoft Office 365, Azure Portal y miles de otras aplicaciones de software como servicio (SaaS).
- Recursos internos, como las aplicaciones de la red corporativa y la intranet, junto con todas las aplicaciones en la nube desarrolladas por su propia organización.

**Características principales**
- Acceso íntegro y seguro al conectar a los usuarios a las aplicaciones que necesitan.
- Protección completa de identidades y seguridad mejorada de las identidades y el acceso en función del usuario, la ubicación, el dispositivo, los datos y el contexto de la aplicación.
- Miles de aplicaciones integradas previamente, tanto comerciales como personalizadas, por ejemplo, Office 365, Salesforce.com y Box.
- Posibilidad de administración del acceso a escala.

**Referencias**
- [Azure Portal](https://portal.azure.com/)
- [¿Qué es Azure AD?](/azure/active-directory/fundamentals/active-directory-whatis)
- [Introducción a Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis)
- [Guías de inicio rápido](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)