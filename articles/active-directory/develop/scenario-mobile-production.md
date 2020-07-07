---
title: Preparación de la aplicación móvil que llama a las API web para producción | Azure
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo compilar una aplicación móvil que llama a las API web. (Preparación de aplicaciones para producción).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 28ace84f9a80b71209d7963d02b66317292b151b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "80882546"
---
# <a name="prepare-mobile-apps-for-production"></a>Preparación de aplicaciones móviles para producción

En este artículo se proporcionan detalles sobre cómo mejorar la calidad y la confiabilidad de la aplicación móvil antes de moverla a producción.

## <a name="handle-errors"></a>errores

Al preparar una aplicación móvil para producción, pueden surgir varias condiciones de error. Los principales casos que va a administrar serán errores silenciosos y retrocesos de interacción. Otras condiciones que debe tener en cuenta incluyen situaciones sin red, interrupciones de servicio, requisitos para el consentimiento del administrador y otros casos específicos del escenario.

Para cada tipo de Biblioteca de autenticación de Microsoft (MSAL), puede encontrar código de ejemplo y contenido de wiki que describe cómo administrar las condiciones de error:

- [MSAL Android wiki](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigate-and-investigate-issues"></a>Mitigación e investigación de problemas

Para diagnosticar mejor los problemas en su aplicación, realice una recopilación de datos. Para más información sobre los tipos de datos que puede recopilar, consulte [Inicio de sesión en aplicaciones de MSAL](https://docs.microsoft.com/azure/active-directory/develop/msal-logging).

Estas son algunas sugerencias para la recopilación de datos:

- Los usuarios pueden pedir ayuda cuando tengan problemas. Un procedimiento recomendado es capturar y almacenar temporalmente los registros. Proporcione una ubicación donde los usuarios puedan cargar los registros. MSAL proporciona extensiones de registro para capturar información detallada acerca de la autenticación.

- Si hay telemetría disponible, habilítela mediante MSAL para recopilar datos acerca de cómo inician sesión en la aplicación los usuarios.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [Common steps to move to production](../../../includes/active-directory-develop-scenarios-production.md)]

Para probar ejemplos adicionales, consulte [Aplicaciones de escritorio y móviles de cliente público](sample-v2-code.md#desktop-and-mobile-public-client-apps).
