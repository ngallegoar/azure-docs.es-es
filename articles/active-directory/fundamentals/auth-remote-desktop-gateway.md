---
title: Servicios de puerta de enlace de Escritorio remoto con Azure Active Directory
description: Guía de arquitectura para lograr servicios de puerta de enlace de Escritorio remoto con Azure Active Directory.
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
ms.openlocfilehash: 4baaf2de6fbe4a56f64d449644b8594217dc432c
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2020
ms.locfileid: "96172744"
---
# <a name="remote-desktop-gateway-services"></a>Servicios de puerta de enlace de Escritorio remoto

Una implementación estándar de Servicios de Escritorio remoto (RDS) estándar incluye diversos [servicios de rol de Escritorio remoto](/windows-server/remote/remote-desktop-services/Desktop-hosting-logical-architecture) que se ejecutan en Windows Server. La implementación de RDS con Application Proxy de Azure Active Directory (Azure AD) tiene una conexión de salida permanente desde el servidor que ejecuta el servicio de conexión. Otras implementaciones dejan conexiones entrantes abiertas a través de un equilibrador de carga. Este patrón de autenticación le permite ofrecer más tipos de aplicaciones mediante la publicación de aplicaciones locales mediante Servicios de Escritorio remoto. También reduce la superficie de ataque de su implementación mediante el uso de Application Proxy de Azure AD.

## <a name="use-when"></a>Cuándo se utiliza

Debe proporcionar acceso remoto y proteger la implementación de Servicios de Escritorio remoto con autenticación previa.

![Diagrama de arquitectura](./media/authentication-patterns/rdp-auth.png)

## <a name="components-of-system"></a>Componentes del sistema

* **Usuario**: Accede a RDS atendido por Application Proxy.

* **Explorador web**: Componente con el que el usuario interactúa para acceder a la dirección URL externa de la aplicación.

* **Azure AD**: Autentica el usuario. 

* **Servicio Application Proxy**: Actúa como proxy inverso para reenviar la solicitud del usuario a RDS. Application Proxy también puede aplicar directivas de acceso condicional. 

* **Servicios de Escritorio remoto**: Actúa como plataforma para las aplicaciones virtualizadas individuales, lo que proporciona un acceso seguro al escritorio remoto y móvil, y proporciona a los usuarios finales la capacidad de ejecutar sus aplicaciones y equipos de escritorio desde la nube. 

## <a name="implement-remote-desktop-gateway-services-with-azure-ad"></a>Implementación de servicios de puerta de enlace de Escritorio remoto con Azure AD

* [Publicación de Escritorio Remoto con Application Proxy de Azure AD](../manage-apps/application-proxy-integrate-with-remote-desktop-services.md) 

* [Adición de una aplicación local para el acceso remoto mediante Application Proxy en Azure AD](../manage-apps/application-proxy-add-on-premises-application.md)

