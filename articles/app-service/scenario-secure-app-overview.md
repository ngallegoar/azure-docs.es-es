---
title: 'Tutorial: Creación de una aplicación web protegida en Azure App Service | Azure'
description: En este tutorial aprenderá a crear una aplicación web con Azure App Service, habilitar la autenticación, llamar a Azure Storage y llamar a Microsoft Graph.
services: active-directory, app-service-web, storage, microsoft-graph
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 11/09/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: fcddf99c3a4c53fe25db1ed653983e8ddac0edb7
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428434"
---
# <a name="tutorial-enable-authentication-in-app-service-and-access-storage-and-microsoft-graph"></a>Tutorial: Habilitación de la autenticación en App Service y acceso a Storage y a Microsoft Graph

En este tutorial se describe un escenario de aplicación común en el que aprenderá a:

- [(A) Configurar la autenticación para una aplicación web](scenario-secure-app-authentication-app-service.md) y limitar el acceso a los usuarios de su organización.
- [(B) Acceder de forma segura a Azure Storage](scenario-secure-app-access-storage.md) en nombre de la aplicación web mediante identidades administradas.
- (C) Acceder a datos en Microsoft Graph [en nombre del usuario con la sesión iniciada](scenario-secure-app-access-microsoft-graph-as-user.md) o [en nombre de la aplicación web](scenario-secure-app-access-microsoft-graph-as-app.md) mediante identidades administradas.
- [Limpie los recursos](scenario-secure-app-clean-up-resources.md) que creó para este tutorial.

:::image type="content" source="./media/scenario-secure-app-overview/web-app.svg" alt-text="Escenarios de aplicaciones en la Plataforma de identidad de Microsoft" border="false":::

Para empezar, obtenga información sobre cómo habilitar la autenticación para una aplicación web.

> [!div class="nextstepaction"]
> [Configuración de la autenticación de una aplicación web](scenario-secure-app-authentication-app-service.md)
