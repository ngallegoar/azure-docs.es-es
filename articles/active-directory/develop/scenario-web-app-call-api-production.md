---
title: 'Traslado a producción de una aplicación web que llama a API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo migrar a producción una aplicación web que llama a API web.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 6404d00b87b9ee745b9e3a92c646404e574417c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2020
ms.locfileid: "80881668"
---
# <a name="a-web-app-that-calls-web-apis-move-to-production"></a>Una aplicación web que llama a API web: Paso a producción

Ahora que sabe cómo adquirir un token para llamar a las API web, conozca cómo pasarlo a producción.

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Pasos siguientes

Para más información, pruebe el tutorial completo y progresivo de aplicaciones web de ASP.NET Core. Este tutorial:

- Muestra cómo iniciar la sesión de los usuarios en varias audiencias o nubes nacionales, o con identidades sociales.
- Llama a Microsoft Graph.
- Llama a varias API de Microsoft.
- Controla el consentimiento incremental.
- Llama a sus propias API web.

> [!div class="nextstepaction"]
> [Tutorial de una aplicación web de ASP.NET Core](https://github.com/Azure-Samples/ms-identity-aspnetcore-webapp-tutorial#scope-of-this-tutorial)

<!--- Removing this diagram as it's already shown from the next step linked tutorial

![Tutorial overview](media/scenarios/aspnetcore-webapp-tutorial.svg)

--->
