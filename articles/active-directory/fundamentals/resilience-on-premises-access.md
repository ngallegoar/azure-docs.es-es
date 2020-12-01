---
title: Aumento de la resistencia en el acceso a las aplicaciones con Application Proxy
description: Guía para arquitectos y administradores de TI sobre el uso de Application Proxy para el acceso resistente a aplicaciones locales
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 11/30/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8bfc3fb239f30911eddf0aa27496a465e36c486
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919126"
---
# <a name="build-resilience-in-application-access-with-application-proxy"></a>Aumento de la resistencia en el acceso a las aplicaciones con Application Proxy

Application Proxy es una característica de Azure AD que permite a los usuarios acceder a aplicaciones web locales desde un cliente remoto. Application Proxy incluye el servicio Application Proxy en la nube y los conectores de Application Proxy, que se ejecutan en un servidor local. 

Los usuarios acceden a los recursos locales mediante una dirección URL publicada a través de Application Proxy. Se les redirige a la página de inicio de sesión de Azure AD. A continuación, el servicio Application Proxy de Azure AD envía un token al conector de Application Proxy en la red corporativa, que pasa el token a Active Directory local. El usuario autenticado puede acceder al recurso local. En el diagrama siguiente, se muestran [conectores](../manage-apps/application-proxy-connectors.md) en un [grupo de conectores](../manage-apps/application-proxy-connector-groups.md).

> [!IMPORTANT]
> Al publicar las aplicaciones a través de Application Proxy, debe implementar [planeamiento de capacidad y la redundancia adecuada para los conectores de Application Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors#capacity-planning).

![Diagrama de la arquitectura de la aplicación](./media/resilience-on-prem-access/admin-resilience-app-proxy.png))

## <a name="how-do-i-implement-application-proxy"></a>¿Cómo se implementa Application Proxy?

Para implementar el acceso remoto con Application Proxy de Azure AD, consulte los siguientes recursos.

* [Planeación de la implementación de un proxy de aplicación](../manage-apps/application-proxy-deployment-plan.md)

* [Procedimientos recomendados de alta disponibilidad y equilibrio de carga](../manage-apps/application-proxy-high-availability-load-balancing.md)

* [Configuración de los servidores proxy](../manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)

* [Diseño de una estrategia de control de acceso resistente](../authentication/concept-resilient-controls.md)

## <a name="next-steps"></a>Pasos siguientes
Recursos de resistencia para administradores y arquitectos
 
* [Aumento de la resistencia con la administración de credenciales](resilience-in-credentials.md)

* [Aumento de la resistencia con estados de dispositivos](resilience-with-device-states.md)

* [Aumento de la resistencia mediante la evaluación continua de acceso (CAE)](resilience-with-continuous-access-evaluation.md)

* [Aumento de la resistencia en la autenticación de usuario externo](resilience-b2b-authentication.md)

* [Aumento de la resistencia en la autenticación híbrida](resilience-in-hybrid.md)

Recursos de resistencia para desarrolladores

* [Aumento de la resistencia IAM en las aplicaciones](resilience-app-development-overview.md)

* [Aumento de la resistencia en los sistemas CIAM](resilience-b2c.md)
