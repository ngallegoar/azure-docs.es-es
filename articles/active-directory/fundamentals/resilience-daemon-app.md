---
title: Aumento de la resistencia de la autenticación y la autorización en las aplicaciones demonio que se desarrollan
titleSuffix: Microsoft identity platform
description: Guía para aumentar la resistencia de la autenticación y la autorización en una aplicación demonio mediante la Plataforma de identidad de Microsoft
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: how-to
author: knicholasa
ms.author: nichola
manager: martinco
ms.date: 11/23/2020
ms.openlocfilehash: 74bfc9eeeb8375fca2c88a3fd3c31f17e130fc99
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "95919219"
---
# <a name="increase-the-resilience-of-authentication-and-authorization-in-daemon-applications-you-develop"></a>Aumento de la resistencia de la autenticación y la autorización en las aplicaciones demonio que se desarrollan

En este artículo se proporcionan instrucciones sobre cómo pueden los desarrolladores utilizar la Plataforma de identidad de Microsoft y Azure Active Directory para aumentar la resistencia de las aplicaciones demonio. Esto incluye procesos en segundo plano, servicios, aplicaciones de servidor a servidor y aplicaciones sin usuarios.

![Aplicación demonio que realiza una llamada a Microsoft Identity](media/resilience-daemon-app/calling-microsoft-identity.png)

## <a name="use-managed-identities-for-azure-resources"></a>Uso de identidades administradas para Azure Resources

Los desarrolladores que crean aplicaciones demonio en Microsoft Azure pueden utilizar [identidades administradas para Azure Resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Las identidades administradas eliminan la necesidad de que los desarrolladores administren secretos y credenciales. La característica mejora la resistencia y evita los errores relacionados con la expiración del certificado, los errores de rotación o la confianza. También tiene varias características integradas diseñadas específicamente para aumentar la resistencia.

Las identidades administradas utilizan tokens de acceso de larga duración e información de Microsoft Identity para adquirir nuevos tokens de forma proactiva dentro de un amplio período de tiempo antes de que expire el token existente. La aplicación puede continuar ejecutándose mientras intenta adquirir un nuevo token.

Las identidades administradas también utilizan puntos de conexión regionales para mejorar el rendimiento y la resistencia frente a los errores de fuera de la región. El uso de un punto de conexión regional ayuda a mantener todo el tráfico dentro de un área geográfica. Por ejemplo, si el recurso de Azure se encuentra en WestUS2, todo el tráfico, incluido el tráfico generado por Microsoft Identity, debe permanecer en WestUS2. Se eliminan así los posibles puntos de error mediante la consolidación de las dependencias de su servicio.

## <a name="use-the-microsoft-authentication-library"></a>Uso de la biblioteca de autenticación de Microsoft

Los desarrolladores de aplicaciones demonio que no utilizan identidades administradas pueden usar la [Biblioteca de autenticación de Microsoft (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview), que facilita la implementación de la autenticación y la autorización y aplica automáticamente los procedimientos recomendados para la resistencia. MSAL facilita el proceso para proporcionar las credenciales de cliente necesarias. Por ejemplo, la aplicación no necesita implementar la creación y la firma de aserciones de JSON Web Token cuando se utilizan credenciales basadas en certificados.

### <a name="use-microsoftidentityweb-for-net-developers"></a>Uso de Microsoft.Identity.Web para desarrolladores de .NET

Los desarrolladores que crean aplicaciones demonio en ASP.NET Core pueden utilizar la biblioteca [Microsoft.Identity.Web](https://docs.microsoft.com/azure/active-directory/develop/microsoft-identity-web). Esta biblioteca se basa en MSAL, lo que facilita aún más la implementación de la autorización para las aplicaciones de ASP.NET Core. Incluye varias estrategias de [caché de tokens distribuidos](https://github.com/AzureAD/microsoft-identity-web/wiki/token-cache-serialization#distributed-token-cache) para las aplicaciones distribuidas que se pueden ejecutar en varias regiones.

## <a name="cache-and-store-tokens"></a>Almacenamiento y almacenamiento en caché de tokens

Si no utiliza MSAL para implementar la autenticación y la autorización, puede implementar algunos procedimientos recomendados para el almacenamiento y el almacenamiento en caché de tokens. MSAL implementa y sigue automáticamente estos procedimientos recomendados.

Una aplicación adquiere tokens de un proveedor de identidades para autorizar a la aplicación a llamar a API protegidas. Cuando la aplicación recibe tokens, la respuesta que contiene los tokens también contiene una propiedad "expires\_in" que indica a la aplicación durante cuánto tiempo se debe almacenar en caché, y volver a usar, el token. Es importante que las aplicaciones usen la propiedad "expires\_in" para determinar la duración del token. La aplicación nunca debe intentar descodificar un token de acceso de API. El uso del token almacenado en caché evita el tráfico innecesario entre la aplicación y Microsoft Identity. El usuario puede mantener la sesión iniciada en la aplicación a lo largo de toda la duración de este token.

## <a name="properly-handle-service-responses"></a>Administración adecuada de las respuestas del servicio

Por último, aunque las aplicaciones deben administrar todas las respuestas de error, hay algunas que pueden afectar a la resistencia. Si la aplicación recibe un código de respuesta HTTP 429, Demasiadas solicitudes, Microsoft Identity está limitando las solicitudes. Si la aplicación continúa realizando demasiadas solicitudes, se sigue limitando, lo que impide que reciba tokens. La aplicación no debe intentar adquirir un token de nuevo hasta que haya pasado el tiempo, en segundos, del campo de respuesta "Retry-After". La recepción de una respuesta 429 suele indicar que la aplicación no está almacenando en caché ni reutilizando los tokens correctamente. Los desarrolladores deben revisar el modo en que los tokens se almacenan en caché y se reutilizan en la aplicación.

Cuando una aplicación recibe un código de respuesta HTTP 5xx, no debe entrar en un bucle de reintentos rápidos. Cuando está presente, la aplicación debe respetar la misma administración de "Retry-After" que para una respuesta 429. Si la respuesta no proporciona ningún encabezado "Retry-After", se recomienda implementar un reintento de retroceso exponencial con el primer reintento al menos 5 segundos después de la respuesta.

Cuando una solicitud agota el tiempo de espera, las aplicaciones no deben volver a intentarlo de inmediato. Implemente un reintento de retroceso exponencial con el primer reintento al menos 5 segundos después de la respuesta.

## <a name="next-steps"></a>Pasos siguientes

- [Aumento de la resistencia en aplicaciones en las que inician sesión los usuarios](resilience-client-app.md)
- [Aumento de la resistencia en la infraestructura de administración de identidades y accesos](resilience-in-infrastructure.md)
- [Aumento de la resistencia en los sistemas CIAM](resilience-b2c.md)
