---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: include
ms.date: 11/05/2020
ms.author: kkrishna
ms.openlocfilehash: 174946667885debc348370ef2c6f93206890e9c1
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "94628019"
---
Puede usar la implementación de la caché de tokens de MSAL para permitir que las aplicaciones, las API y los servicios en segundo plano usen la caché de tokens de acceso para continuar actuando en nombre de los usuarios en su ausencia. Hacerlo resulta especialmente útil si las aplicaciones y los servicios en segundo plano deben seguir trabajando en nombre del usuario después de que haya salido de la aplicación web de front-end.

En la actualidad, la mayoría de los procesos en segundo plano usan [permisos de aplicación](/graph/auth/auth-concepts#microsoft-graph-permissions) cuando necesitan trabajar con los datos de un usuario sin que este esté presente para autenticarse o volver a autenticarse. Dado que los permisos de aplicación suelen requerir el consentimiento del administrador, lo que requiere la elevación de privilegios, se genera una fricción innecesaria porque el desarrollador no tenía la intención de obtener permiso más allá del que el usuario dio su consentimiento originalmente para su aplicación.

En este ejemplo de código en GitHub se muestra cómo evitar esta fricción innecesaria mediante el acceso a la caché de tokens de MSAL desde las aplicaciones en segundo plano:

 [Acceso a la memoria caché de tokens del usuario que ha iniciado sesión desde aplicaciones en segundo plano, API y servicios](https://github.com/Azure-Samples/ms-identity-dotnet-advanced-token-cache)