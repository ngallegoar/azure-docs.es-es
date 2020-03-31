---
title: 'Registro de una aplicación web que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a registrar una aplicación web que llama a las API web.
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
ms.openlocfilehash: 5a57fcef3569734964bf6e8a41faa49800798f9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759064"
---
# <a name="a-web-app-that-calls-web-apis-app-registration"></a>Aplicación web que llama a las API web: Registro de aplicación

Una aplicación web que llama a las API web que tiene el mismo registro que una aplicación web que permite iniciar sesión a los usuarios. Por lo tanto, siga las instrucciones de [Aplicación web que permite iniciar sesión a los usuarios: registro de aplicación](scenario-web-app-sign-user-app-registration.md).

Sin embargo, dado que la aplicación web ahora llama también a las API web, se convierte en una aplicación cliente confidencial. Este es el motivo por el que se requiere algún registro adicional. La aplicación debe compartir las credenciales del cliente o los *secretos*, con la Plataforma de identidad de Microsoft.

[!INCLUDE [Registration of client secrets](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>Permisos de API

Las aplicaciones web llaman a las API en nombre del usuario con sesión iniciada. Para ello, deben solicitar *permisos delegados*. Para más detalles, consulte [Adición de permisos para acceder a las API web](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación web que llama a las API web: configuración del código](scenario-web-app-call-api-app-configuration.md)
