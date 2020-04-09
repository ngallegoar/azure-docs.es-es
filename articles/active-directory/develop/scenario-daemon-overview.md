---
title: 'Compilación de una aplicación demonio que llama a las API web: Plataforma de identidad de Microsoft | Azure'
description: Obtenga información sobre cómo compilar una aplicación de demonio que llama a las API web
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 01/31/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: df06c4c55941f4424d6b90d2846af17bf055b2e4
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885470"
---
# <a name="scenario-daemon-application-that-calls-web-apis"></a>Escenario: Aplicación de demonio que llama a las API web

Obtenga toda la información necesaria para compilar una aplicación de demonio que llama a las API web.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="overview"></a>Información general

La aplicación puede adquirir un token para llamar a una API web en nombre propio (no en el nombre de un usuario). Este escenario es útil para aplicaciones de demonio. Usa la concesión de [credenciales del cliente](v2-oauth2-client-creds-grant-flow.md) OAuth 2.0 estándar.

![Aplicaciones de demonio](./media/scenario-daemon-app/daemon-app.svg)

Estos son algunos ejemplos de casos de usos para aplicaciones de demonio:

- Aplicaciones web que se usan para aprovisionar o administrar usuarios, o llevar a cabo procesos por lotes en un directorio
- Aplicaciones de escritorio (como servicios de Windows en Windows o procesos de demonio en Linux) que realizan trabajos por lotes, o un servicio de sistema operativo que se ejecuta en segundo plano
- API Web que necesitan manipular directorios, no usuarios específicos

Hay otro caso habitual en el que las aplicaciones que no son demonios usan las credenciales del cliente: incluso cuando actúan en nombre de usuarios, necesitan acceder a una API web o a un recurso con su propia identidad por motivos técnicos. Un ejemplo es el acceso a los secretos de Azure Key Vault o una base de datos de Azure SQL de una caché.

Las aplicaciones que adquieren un token para sus propias identidades:

- Son aplicaciones cliente confidenciales. Estas aplicaciones, dado que acceden a recursos con independencia de los usuarios, deben demostrar su identidad. También son aplicaciones bastante confidenciales. Tienen que ser aprobadas por los administradores de inquilinos de Azure Active Directory (Azure AD).
- Han registrado un secreto (contraseña de la aplicación o certificado) en Azure AD. Este secreto se pasa durante la llamada a Azure AD para obtener un token.

## <a name="specifics"></a>Características específicas

> [!IMPORTANT]
>
> - Los usuarios no pueden interactuar con una aplicación de demonio. Una aplicación de demonio requiere su propia identidad. Este tipo de aplicación solicita un token de acceso mediante su identidad de aplicación y presenta el identificador de aplicación, las credenciales (contraseña o certificado) y el URI del identificador de aplicación a Azure AD. Tras una autenticación correcta, el demonio recibe un token de acceso (y un token de actualización) del punto de conexión de la plataforma de identidad de Microsoft. Este token se usa luego para llamar a la API web (y se actualiza según sea necesario).
> - Dado que los usuarios no pueden interactuar con aplicaciones de demonio, no es posible el consentimiento incremental. Todos los permisos de API necesarios deben configurarse en el registro de aplicación. El código de la aplicación simplemente solicita permisos definidos de forma estática. Esto también significa que las aplicaciones de demonio no admiten el consentimiento incremental.

Para los desarrolladores, la experiencia total de este escenario tiene los siguientes aspectos:

- Las aplicaciones de demonio solo pueden funcionar en inquilinos de Azure AD. No tendría sentido crear una aplicación de demonio que intentara manipular cuentas personales de Microsoft. Si es desarrollador de aplicaciones de línea de negocio (LOB), creará la aplicación de demonio en el inquilino. Si es fabricante de software independiente, es posible que quiera crear una aplicación de demonio de varios inquilinos. Cada administrador de inquilinos debe dar su consentimiento.
- Durante el [registro de aplicación](./scenario-daemon-app-registration.md), no se necesita el URI de respuesta. Debe compartir secretos o certificados, o instrucciones de aserción firmadas, con Azure AD. También debe solicitar permisos de aplicación y otorgar consentimiento del administrador para usar esos permisos de aplicación.
- La [configuración de la aplicación](./scenario-daemon-app-configuration.md) tiene que proporcionar las credenciales de cliente como compartidas con Azure AD durante el registro de la aplicación.
- El [ámbito](scenario-daemon-acquire-token.md#scopes-to-request) usado para adquirir un token con el flujo de credenciales del cliente debe ser un ámbito estático.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: registro de la aplicación](./scenario-daemon-app-registration.md)
