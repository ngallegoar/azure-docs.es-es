---
title: Autenticación basada en encabezados con Azure Active Directory
description: Guía arquitectónica para lograr este patrón de autenticación
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f364e4e14dd1b7c60cb81f06051d9dedd94396a
ms.sourcegitcommit: ae6e7057a00d95ed7b828fc8846e3a6281859d40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92114030"
---
# <a name="header-based-authentication-with-azure-active-directory"></a>Autenticación basada en encabezados con Azure Active Directory

Las aplicaciones heredadas suelen usar la autenticación basada en encabezados. En este escenario, un usuario (o remitente del mensaje) se autentica en una solución de identidad intermediaria. La solución intermediario autentica al usuario y propaga los encabezados del Protocolo de transferencia de hipertexto (HTTP) necesarios al servicio web de destino. Azure Active Directory (AD) admite este patrón a través del servicio Application Proxy y las integraciones con otras soluciones de controlador de red. 

En nuestra solución, Application Proxy proporciona acceso remoto a la aplicación, autentica al usuario y pasa los encabezados exigidos por la aplicación. 

## <a name="use-when"></a>Cuándo se utiliza

Los usuarios remotos necesitan un inicio de sesión único (SSO) de forma segura en las aplicaciones locales que requieren autenticación basada en encabezados.

![Imagen de la arquitectura de la autenticación basada en encabezados](./media/authentication-patterns/header-based-auth.png)

## <a name="components-of-system"></a>Componentes del sistema

* **Usuario** : Accede a las aplicaciones heredadas atendidas por Application Proxy.

* **Explorador web** : Componente con el que el usuario interactúa para acceder a la dirección URL externa de la aplicación.

* **Azure AD** : Autentica al usuario. 

* **Servicio Application Proxy** : Actúa como proxy inverso para enviar la solicitud del usuario a la aplicación local. Reside en Azure AD y también puede aplicar directivas de acceso condicional.

* **Conector de Application Proxy** : Se instala de forma local en servidores Windows para proporcionar conectividad a las aplicaciones. Solo usa conexiones salientes. Devuelve la respuesta a Azure AD.

* **Aplicaciones heredadas** : Aplicaciones que reciben solicitudes de usuario de Application Proxy. La aplicación heredada recibe los encabezados HTTP necesarios para configurar una sesión y devolver una respuesta. 

## <a name="implement-header-based-authentication-with-azure-ad"></a>Implementación de la autenticación basada en encabezados con Azure AD

* [Adición de una aplicación local para el acceso remoto mediante Application Proxy en Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)  

* [Autenticación basada en el encabezado para el inicio de sesión único con el proxy de aplicación y PingAccess](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-ping-access) 

* [Protección de aplicaciones heredadas con redes y controladores de entrega de aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/secure-hybrid-access)
