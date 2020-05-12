---
title: Seguridad y autenticación de Azure Event Grid
description: En este artículo se describen las distintas formas de autenticar el acceso a los recursos de Event Grid (webhook, suscripciones, temas personalizados).
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: babanisa
ms.openlocfilehash: 528c3613549ee49009f99d45e5bd9c2cf1745d78
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780001"
---
# <a name="authenticating-access-to-azure-event-grid-resources"></a>Autenticación de acceso a recursos de Azure Event Grid
En este artículo se proporciona información sobre los siguientes escenarios:  

- Autentique a los clientes que publican eventos en temas de Azure Event Grid mediante la clave o la firma de acceso compartido (SAS) o la clave. 
- Proteja el punto de conexión de webhook mediante Azure Active Directory (Azure AD) para autenticar Event Grid para **entregar** eventos al punto de conexión.

## <a name="authenticate-publishing-clients-using-sas-or-key"></a>Autenticación de clientes de publicación mediante SAS o una clave
Los temas personalizados utilizan la Firma de acceso compartido (SAS) o la autenticación de clave. Se recomienda el uso de SAS, pero la autenticación de clave proporciona programación simple y es compatible con muchos editores de webhook existentes.

Incluya el valor de autenticación en el encabezado HTTP. Para SAS, use **aeg-sas-token** para el valor del encabezado. Para autenticación de clave, utilice **aeg-sas-key** para el valor del encabezado.

### <a name="key-authentication"></a>Autenticación de clave

La autenticación de clave es la forma más sencilla de autenticación. Utilice el siguiente formato: `aeg-sas-key: <your key>`

Por ejemplo, pasa una clave con:

```
aeg-sas-key: VXbGWce53249Mt8wuotr0GPmyJ/nDT4hgdEj9DpBeRr38arnnm5OFg==
```

### <a name="sas-tokens"></a>Tokens de SAS

Los tokens de SAS para Event Grid incluyen el recurso, un tiempo de expiración y una firma. El formato del token de SAS es: `r={resource}&e={expiration}&s={signature}`.

El recurso es la ruta de acceso del tema de Event Grid al que envía los eventos. Por ejemplo, una ruta de acceso de recurso válida es: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events?api-version=2019-06-01`. Para ver todas las versiones de API compatibles, vea [Tipos de recursos de Microsoft.EventGrid](https://docs.microsoft.com/azure/templates/microsoft.eventgrid/allversions). 

Genere la firma a partir de una clave.

Por ejemplo, un valor **aeg-sas-token** válido es:

```http
aeg-sas-token: r=https%3a%2f%2fmytopic.eventgrid.azure.net%2feventGrid%2fapi%2fevent&e=6%2f15%2f2017+6%3a20%3a15+PM&s=a4oNHpRZygINC%2fBPjdDLOrc6THPy3tDcGHw1zP4OajQ%3d
```

En el ejemplo siguiente se crea un token de SAS para su uso con Event Grid:

```cs
static string BuildSharedAccessSignature(string resource, DateTime expirationUtc, string key)
{
    const char Resource = 'r';
    const char Expiration = 'e';
    const char Signature = 's';

    string encodedResource = HttpUtility.UrlEncode(resource);
    var culture = CultureInfo.CreateSpecificCulture("en-US");
    var encodedExpirationUtc = HttpUtility.UrlEncode(expirationUtc.ToString(culture));

    string unsignedSas = $"{Resource}={encodedResource}&{Expiration}={encodedExpirationUtc}";
    using (var hmac = new HMACSHA256(Convert.FromBase64String(key)))
    {
        string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(unsignedSas)));
        string encodedSignature = HttpUtility.UrlEncode(signature);
        string signedSas = $"{unsignedSas}&{Signature}={encodedSignature}";

        return signedSas;
    }
}
```

### <a name="encryption-at-rest"></a>Cifrado en reposo

Todos los eventos o datos escritos en el disco por el servicio Event Grid se cifran mediante una clave administrada por Microsoft, lo que garantiza que se cifran en reposo. Además, el período máximo que se conservan los eventos o los datos es de 24 horas, conforme a la [directiva de reintentos de Event Grid](delivery-and-retry.md). Event Grid elimina automáticamente todos los eventos o datos tras 24 horas, o el período de vida del evento, lo que sea menor.

## <a name="authenticate-event-delivery-to-webhook-endpoints"></a>Autenticación de la entrega de eventos a los puntos de conexión de webhook
En las secciones siguientes se describe cómo autenticar la entrega de eventos a los puntos de conexión de webhook. Debe usar un mecanismo de protocolo de enlace de validación con independencia del método que use. Consulte [Entrega de eventos de webhook](webhook-event-delivery.md) para detalles. 

### <a name="using-azure-active-directory-azure-ad"></a>Uso de Azure Active Directory (Azure AD)
Puede proteger el punto de conexión de webhook mediante Azure Active Directory (Azure AD) para autenticar y autorizar a Event Grid a que entregue eventos en los puntos de conexión. Tendrá que crear una aplicación de Azure AD, crear un rol y una entidad de servicio en la aplicación que autorice a Event Grid, y configurar la suscripción de eventos para usar la aplicación de Azure AD. [Obtenga información sobre cómo configurar Azure Active Directory con Event Grid](secure-webhook-delivery.md).

### <a name="using-client-secret-as-a-query-parameter"></a>Uso del secreto de cliente como parámetro de consulta
Puede proteger el punto de conexión del webhook mediante la incorporación de parámetros de consulta a la URL del webhook al crear una suscripción a eventos. Establezca uno de estos parámetros de consulta para que sea un secreto de cliente, como un [token de acceso](https://en.wikipedia.org/wiki/Access_token) o un secreto compartido. El webhook puede usar el secreto para reconocer que el evento procede de Event Grid con permisos válidos. Event Grid incluye estos parámetros de consulta en cada entrega de eventos al webhook. Si se actualiza el secreto de cliente, también debe actualizarse la suscripción de eventos. Para evitar errores de entrega durante la rotación del secreto, haga que el webhook acepte tanto los secretos antiguos como los nuevos durante una duración limitada. 

Como los parámetros de consulta pueden contener secretos de cliente, se controlan con más cuidado. Se almacenan como cifrados y no son accesibles para los operadores de servicio. No se registran como parte de los registros o seguimientos de servicio. Al editar la suscripción al evento, los parámetros de consulta no se muestran ni se devuelven, a menos que el parámetro [--include-full-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) se utilice en la [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) de Azure.

Para más información sobre cómo entregar eventos a webhooks, consulte [Entrega de eventos de webhook](webhook-event-delivery.md).

> [!IMPORTANT]
Azure Event Grid solo admite puntos de conexión de webhook **HTTPS**. 

## <a name="next-steps"></a>Pasos siguientes

- Para obtener una introducción a Event Grid, vea [About Event Grid](overview.md) (Acerca de Event Grid).
