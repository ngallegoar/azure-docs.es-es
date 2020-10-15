---
title: Registro de aplicaciones de demonio que llaman a las API web - Plataforma de identidad de Microsoft | Azure
description: 'Obtenga información sobre cómo compilar una aplicación de demonio que llame a las API web: registro de la aplicación'
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/15/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 508101ad615dd96559b1c68a61be7c08772545db
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "80885487"
---
# <a name="daemon-app-that-calls-web-apis---app-registration"></a>Aplicación de demonio que llama a las API web: registro de la aplicación

Para una aplicación de demonio, esto es lo que tiene que saber cuando registre la aplicación.

## <a name="supported-account-types"></a>Tipos de cuenta admitidos

Las aplicaciones de demonio solo tienen sentido en inquilinos de Azure AD. Por lo tanto, cuando cree la aplicación, debe elegir una de las siguientes opciones:

- **Solo las cuentas de este directorio organizativo**. Esta opción es la más común, ya que normalmente son desarrolladores de línea de negocio (LOB) quienes escriben aplicaciones de demonio.
- **Cuentas en cualquier directorio organizativo**. Elegirá esta opción si es un ISV que proporciona una herramienta de utilidad a sus clientes. Necesitará que los administradores de inquilinos de sus clientes la aprueben.

## <a name="authentication---no-reply-uri-needed"></a>Autenticación: sin necesidad de URI de respuesta

En caso de que la aplicación cliente confidencial use *solo* el flujo de credenciales del cliente, no es necesario registrar el URI de respuesta. No se necesita para la configuración ni la construcción de la aplicación. El flujo de credenciales de cliente no la usa.

## <a name="api-permissions---app-permissions-and-admin-consent"></a>Permisos de API: permisos de aplicación y consentimiento del administrador

Una aplicación de demonio solo puede solicitar permisos de aplicación a las API (no permisos delegados). En la página **Permisos de API** del registro de la aplicación, después de haber seleccionado **Agregar un permiso** y elegido la familia de la API, elija **Permisos de la aplicación** y, luego, seleccione los permisos.

![Permisos de aplicación y consentimiento del administrador](media/scenario-daemon-app/app-permissions-and-admin-consent.png)

> [!NOTE]
> La API web a la que desea llamar debe definir los *permisos de aplicación (roles de aplicación)* , no los permisos delegados. Para más información sobre cómo exponer una API de este tipo, consulte [API web protegida: Registro de aplicación: si una aplicación de demonio llama a la API web](scenario-protected-web-api-app-registration.md#if-your-web-api-is-called-by-a-daemon-app).

Las aplicaciones de demonio requieren un consentimiento previo del administrador de inquilinos para la aplicación que llama a la API web. Los administradores de inquilinos proporcionan este consentimiento en la misma página **Permiso de API**, mediante la selección de **Conceda consentimiento del administrador a *nuestra organización*** .

Si es un ISV que compila una aplicación de varios inquilinos, debe leer la sección [Implementación: caso de aplicaciones de demonio de varios inquilinos](scenario-daemon-production.md#deployment---multitenant-daemon-apps).

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: configuración del código de aplicación](./scenario-daemon-app-configuration.md)
