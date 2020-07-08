---
title: Diferencias entre MSAL en iOS y macOS | Azure
titleSuffix: Microsoft identity platform
description: Describe las diferencias de uso de la Biblioteca de autenticación de Microsoft (MSAL) entre iOS y macOS.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: how-to
ms.workload: identity
ms.date: 08/28/2019
ms.author: marsma
ms.reviewer: oldalton
ms.custom: aaddev
ms.openlocfilehash: 41389bc5ed8580cd80dbc40e771c7f15241f5ae7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85479409"
---
# <a name="microsoft-authentication-library-for-ios-and-macos-differences"></a>Diferencias de la biblioteca de autenticación de Microsoft entre iOS y macOS

En este artículo se explican las diferencias de funcionalidad de la Biblioteca de autenticación de Microsoft (MSAL) entre iOS y macOS.

> [!NOTE]
> En el equipo Mac, MSAL solo admite aplicaciones macOS.

## <a name="general-differences"></a>Diferencias generales

MSAL para macOS es un subconjunto de la funcionalidad disponible para iOS.

MSAL para macOS no es compatible con:

- tipos de explorador distintos, como `ASWebAuthenticationSession`, `SFAuthenticationSession` o `SFSafariViewController`.
- la autenticación con agente mediante la aplicación Microsoft Authenticator no es compatible con macOS.

El uso compartido de llaveros entre aplicaciones del mismo editor es más limitado en macOS 10.14 y versiones anteriores. Use las [listas de control de acceso](https://developer.apple.com/documentation/security/keychain_services/access_control_lists?language=objc) para especificar las rutas de acceso a las aplicaciones que deben compartir el llavero. El usuario puede ver mensajes adicionales del llavero.

En macOS 10.15+, el comportamiento de MSAL es el mismo en iOS y macOS. MSAL usa los [grupos de acceso al llavero](https://developer.apple.com/documentation/security/keychain_services/keychain_items/sharing_access_to_keychain_items_among_a_collection_of_apps?language=objc) para el uso compartido del llavero. 

### <a name="conditional-access-authentication-differences"></a>Diferencias de autenticación del acceso condicional

En los escenarios de acceso condicional, habrá menos mensajes de usuario al usar MSAL para iOS. Esto se debe a que iOS usa la aplicación de agente (Microsoft Authenticator), que evita necesidad de preguntar al usuario en algunos casos.

### <a name="project-setup-differences"></a>Diferencias en la configuración del proyecto

**macOS**

- Al configurar el proyecto en macOS, asegúrese de que la aplicación está firmada con un certificado de desarrollo o de producción válido. MSAL sigue funcionando en el modo sin firma, pero se comportará de forma distinta con respecto a la persistencia en la memoria caché. La aplicación solo se debe ejecutar sin firma con fines de depuración. Si distribuye la aplicación sin firma, hará lo siguiente:
1. En la versión 10.14 y versiones anteriores, MSAL solicitará al usuario la contraseña del llavero cada vez que reinicie la aplicación.
2. En 10.15+, MSAL solicitará al usuario las credenciales para cada adquisición de token. 

- Las aplicaciones de macOS no necesitan implementar la llamada AppDelegate.

**iOS**

- Hay pasos adicionales para configurar el proyecto de forma que admita el flujo del agente de autenticación. Los pasos se indican en el tutorial.
- Los proyectos de iOS deben registrar esquemas personalizados en info.plist. Esto no es necesario en macOS.
