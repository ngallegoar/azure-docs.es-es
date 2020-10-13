---
title: Actualización de Bing Spell Check API de v5 a v7
titleSuffix: Azure Cognitive Services
description: Se identifican las partes de la aplicación de Bing Spell Check que se deben actualizar para usar la versión 7.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: scottwhi
ms.openlocfilehash: 642aa23bc74f1f3f44deb6e489e6f7aafe7ff237
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91254132"
---
# <a name="spell-check-api-upgrade-guide"></a>Guía de actualización de Spell Check API

Esta guía de actualización identifica los cambios entre las versiones 5 y 7 de Bing Spell Check API. Utilice esta guía para identificar las partes de la aplicación que se deben actualizar para usar la versión 7.

## <a name="breaking-changes"></a>Últimos cambios

### <a name="endpoints"></a>Puntos de conexión

- El número de versión del punto de conexión ha cambiado de v5 a v7. Por ejemplo, `https://api.cognitive.microsoft.com/bing/v7.0/spellcheck`.

### <a name="error-response-objects-and-error-codes"></a>Objetos de la respuesta de error y códigos de error

- Todas las solicitudes con error deben incluir ahora un objeto `ErrorResponse` en el cuerpo de la respuesta.

- Se han agregado los campos siguientes al objeto `Error`.  
  - `subCode`&mdash;Divide el código de error en cubos discretos, si es posible.
  - `moreDetails`&mdash;Información adicional sobre el error descrito en el campo `message`.
   

- Se han reemplazado los códigos de error v5 por los posibles valores de `code` y `subCode` siguientes.  
  
|Código|Subcódigo|Descripción
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|Bing devuelve ServerError cada vez que se produce alguna de las condiciones del subcódigo. La respuesta incluye estos errores si el código de estado HTTP es 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqueado|Bing devuelve InvalidRequest siempre que alguna parte de la solicitud no es válida. Por ejemplo, si falta un parámetro necesario o un valor de parámetro no es válido.<br/><br/>Si el error es ParameterMissing o ParameterInvalidValue, el código de estado HTTP es 400.<br/><br/>Si el error es HttpNotAllowed, el código de estado HTTP es 410.
|RateLimitExceeded||Bing devuelve RateLimitExceeded cada vez que se supera la cuota de consultas por segundo (QPS) o de consultas por mes (QPM).<br/><br/>Bing devuelve el código de estado HTTP 429 si supera la cuota QPS y 403 si supera la cuota QPM.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing devuelve InvalidAuthorization cuando no puede autenticar al autor de la llamada. Por ejemplo, si falta el encabezado `Ocp-Apim-Subscription-Key` o la clave de suscripción no es válida.<br/><br/>Se produce redundancia si especifica más de un método de autenticación.<br/><br/>Si el error es InvalidAuthorization, el código de estado HTTP es 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing devuelve InsufficientAuthorization cuando el autor de la llamada no tiene permisos para acceder al recurso. Esto puede ocurrir si la clave de suscripción se ha deshabilitado o ha expirado. <br/><br/>Si el error es InsufficientAuthorization, el código de estado HTTP es 403.

- A continuación se asignan los códigos de error anteriores a los nuevos códigos. Si tiene una dependencia de los códigos de error v5, actualice el código en consecuencia.  
  
|Código de la versión 5|Código.subcódigo de la versión 7
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Disabled|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
NotImplemented|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Bloqueado|InvalidRequest.Blocked

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Requisitos de uso y visualización](./UseAndDisplayRequirements.md)
