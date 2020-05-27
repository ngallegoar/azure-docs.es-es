---
title: Incorporación de autenticación a las aplicaciones móviles con Visual Studio App Center y servicios de Azure
description: Conozca los servicios de Visual Studio App Center que ayudan a configurar la autenticación de usuarios y a permitir que las aplicaciones móviles se autentiquen con cuentas de redes sociales, Azure Active Directory y autenticación personalizada.
author: codemillmatt
ms.assetid: 34a8a070-2222-4faf-9090-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: a20ec1039dc8fc477410891495762395b408f2d6
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2020
ms.locfileid: "83870570"
---
# <a name="add-authentication-and-manage-user-identities-in-your-mobile-apps"></a>Incorporación de autenticación y administración de identidades de usuario en las aplicaciones móviles

Tener una vista de los usuarios y su comportamiento en la aplicación permite a los desarrolladores ofrecerles experiencias a medida para interaccionar mejor con ellos. Tanto si es un desarrollador de aplicaciones que crea una aplicación de colaboración para los usuarios de la organización como si está creando la siguiente plataforma de red social, necesita una manera de autenticar a los usuarios y administrar sus identidades. Un servicio de administración de identidades es una de las características más importantes de un servicio de back-end móvil.

Use los servicios siguientes para permitir la autenticación de usuarios en las aplicaciones móviles.

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