---
title: Problemas conocidos del explorador de Safari (MSAL.js) | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información acerca de los problemas conocidos al usar la Biblioteca de autenticación de Microsoft para JavaScript (MSAL.js) con el explorador Safari.
services: active-directory
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: troubleshooting
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: edb995e31c2872c1541e29fee09dd66aafc8f9e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76696119"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Problemas conocidos en el explorador Safari con MSAL.js 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Renovación de token silenciosa en Safari 12 e ITP 2.0

Los sistemas operativos Apple iOS 12 y MacOS 10.14 incluyen la versión [12 del explorador Safari](https://developer.apple.com/safari/whats-new/). Para fines de seguridad y privacidad, Safari 12 incluye[Intelligent Tracking Prevention 2.0](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/). Gracias a este sistema, explorador elimina las cookies de terceros que se establecen en el mismo. ITP 2.0 también trata las cookies que establecen los proveedores de identidades como cookies de terceros.

### <a name="impact-on-msaljs"></a>Impacto en MSAL.js

MSAL.js usa un Iframe oculto para realizar la adquisición y renovación silenciosa de los token como parte de las llamadas `acquireTokenSilent`. Las solicitudes de token silenciosas dependen de que Iframe obtenga acceso a la sesión de usuario autenticado que representan las cookies que estableció Azure AD. Como ITP 2.0 impide el acceso a estas cookies, MSAL.js no puede adquirir y renovar token de forma silenciosa, lo que provoca errores de tipo `acquireTokenSilent`.

Como en este momento no existe una solución para este problema, estamos evaluando las opciones con la comunidad de estándares.

### <a name="work-around"></a>Solución

De forma predeterminada, la configuración de ITP está habilitada en el explorador Safari. Puede deshabilitar esta configuración si va a **Preferences** -> **Privacy** (Preferencias > Privacidad) y desactivando la opción **Prevent cross-site tracking** (Evitar el seguimiento entre sitios).

![configuración de Safari](./media/msal-js-known-issue-safari-browser/safari.png)

Deberá controlar los errores de tipo `acquireTokenSilent` con una llamada interactiva de token de adquisición, que le solicitará al usuario que inicie sesión.
Para evitar inicios de sesión repetidos, una solución puede usar es controlar el error de tipo `acquireTokenSilent` y proporcionar al usuario una opción para deshabilitar la configuración de ITP en Safari antes de continuar con la llamada interactiva. Una vez deshabilitada la configuración, las renovaciones de token silenciosas siguientes deberían realizarse correctamente.
