---
title: 'Compilación de una aplicación de página única que llama a una API web: Plataforma de identidad de Microsoft | Azure'
description: Aprenda a compilar una aplicación de página única que llama a una API web.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.openlocfilehash: 3b71f66dfcbd33cdecc1a6fea46871ba0cda687d
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442895"
---
# <a name="single-page-application-call-a-web-api"></a>Aplicación de página única: Llamada a una API de web

Se recomienda que llame al método `acquireTokenSilent` para adquirir o renovar un token de acceso antes de llamar a una API web. Una vez que disponga de un token, puede llamar a una API web protegida.

## <a name="call-a-web-api"></a>Llamada a una API de web

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Use el token de acceso adquirido como portador en una solicitud HTTP para llamar a cualquier API web, como Microsoft Graph API. Por ejemplo:

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

# <a name="angular"></a>[Angular](#tab/angular)

El contenedor MSAL Angular aprovecha las ventajas del interceptor HTTP para adquirir de forma automática y silenciosa tokens de acceso y adjuntarlos a las solicitudes HTTP de las API. Para más información, consulte [Aplicación de página única: adquirir un token para llamar a una API](scenario-spa-acquire-token.md).

---

## <a name="next-steps"></a>Pasos siguientes

Avance al siguiente artículo de este escenario, [Paso a producción](scenario-spa-production.md).