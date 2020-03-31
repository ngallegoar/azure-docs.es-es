---
title: 'Registro de una API web que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una API web que llame a API web de bajada (registro de la aplicación).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: bafd71f34602535bb6193a8d8114a1182e4e8f40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76701797"
---
# <a name="a-web-api-that-calls-web-apis-app-registration"></a>API web que llama a API web: Registro de aplicación

Una API web que llama a API web de bajada tiene el mismo registro que una API web protegida. Por lo tanto, deberá seguir las instrucciones de [API web protegida: registro de aplicación](scenario-protected-web-api-app-registration.md).

Dado que la aplicación web ahora llama a las API web, se convierte en una aplicación cliente confidencial. Por eso se exige información de registro adicional: la aplicación tiene que compartir secretos (credenciales de cliente) con la Plataforma de identidad de Microsoft.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Permisos de API

Las aplicaciones web llaman a las API en nombre de los usuarios para los que se ha recibido el token de portador. Tienen que solicitar permisos delegados. Para más información, consulte [Adición de permisos para acceder a las API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [API web que llama a las API web: configuración del código](scenario-web-api-call-api-app-configuration.md)
