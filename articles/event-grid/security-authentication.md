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
ms.openlocfilehash: 0b7c5b42ac6291c6687337ba8d6a9d35830b9bda
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79236252"
---
# <a name="authenticating-access-to-event-grid-resources"></a>Autenticación de acceso a recursos de Event Grid

Azure Event Grid tiene tres tipos de autenticación:

* Entrega de eventos de WebHook
* Suscripciones a eventos
* Publicación de temas personalizados

## <a name="webhook-event-delivery"></a>Entrega de eventos de WebHook

Los webhooks son una de las muchas maneras de recibir eventos de Azure Event Grid. Cuando un nuevo evento está preparado, el servicio EventGrid publica una solicitud HTTP en el punto de conexión configurado con el evento en el cuerpo de la solicitud.

Al igual que muchos otros servicios que admiten webhooks, EventGrid requiere que demuestre la propiedad de su punto de conexión de Webhook antes de que comience a entregar eventos a ese punto de conexión. Este requisito evita que un usuario malintencionado sature el punto de conexión con eventos. Cuando utiliza cualquiera de los tres servicios de Azure enumerados a continuación, la infraestructura de Azure controla automáticamente esta validación:

* Azure Logic Apps con el [conector de Event Grid](https://docs.microsoft.com/connectors/azureeventgrid/)
* Azure Automation a través de [webhook](../event-grid/ensure-tags-exists-on-new-virtual-machines.md)
* Azure Functions con el [desencadenador de Event Grid](../azure-functions/functions-bindings-event-grid.md)

Si utiliza cualquier otro tipo de punto de conexión, como una función de Azure basada en un desencadenador HTTP, el código del punto de conexión debe participar en un protocolo de enlace de validación con EventGrid. Event Grid admite dos formas de validar la suscripción.

1. **Protocolo de enlace ValidationCode (mediante programación)** : Si se controla el código fuente para el punto de conexión, se recomienda este método. En el momento de crear la suscripción a eventos, Event Grid envía un evento de validación de suscripción en su punto de conexión. El esquema de este evento es similar a cualquier otro evento de Event Grid. La parte de datos de este evento incluye una propiedad `validationCode`. La aplicación comprueba que la solicitud de validación es para una suscripción a un evento esperado, y devuelve el código de validación a Event Grid. Este mecanismo del protocolo de enlace se admite en todas las versiones de EventGrid.

2. **Protocolo de enlace ValidationURL (manual)** : En algunos casos, no puede acceder al código fuente del punto de conexión para implementar el protocolo de enlace ValidationCode. Por ejemplo, si usa un servicio de terceros (como [Zapier](https://zapier.com) o [IFTTT](https://ifttt.com/)), no puede responder con el código de validación mediante programación.

   A partir de la versión 2018-05-01-preview, EventGrid admite un protocolo de enlace de validación manual. Si va a crear una suscripción de eventos mediante el SDK o la herramienta que usa la versión de API 2018-05-01-preview, EventGrid envía una propiedad `validationUrl` en los datos del evento de validación de suscripción. Para completar el protocolo de enlace, busque esa dirección URL en los datos del evento y envíele manualmente una solicitud GET. Puede usar un cliente de REST o el explorador web.

   La dirección URL proporcionada es válida durante 5 minutos. Durante ese tiempo, el estado de aprovisionamiento de la suscripción al eventos es `AwaitingManualAction`. Si no ha completado la validación manual en 5 minutos, el estado de aprovisionamiento se establece en `Failed`. Tendrá que crear la suscripción de eventos de nuevo antes de intentar la validación manual.

    Este mecanismo de autenticación también requiere el punto de conexión de webhook para devolver un código de estado HTTP de 200 para que sepa que se aceptó el objeto POST para el evento de validación antes de se pueda colocar en el modo de validación manual. Es decir, si el punto de conexión devuelve 200, pero no devuelve una respuesta de validación mediante programación, el modo se cambia al modo de validación manual. Si hay una operación GET en la dirección URL de validación dentro de un plazo de cinco minutos, se considera el protocolo de enlace de validación se realizó correctamente.

> [!NOTE]
> Para la validación no se admite el uso de certificados autofirmados. En su lugar, use un certificado firmado de una entidad de certificación (CA).

### <a name="validation-details"></a>Detalles de la validación

* En el momento de crear o actualizar la suscripción a eventos, Event Grid publica un evento de validación de suscripción en el punto de conexión de destino. 
* El evento contiene un valor de encabezado "aeg-event-type: SubscriptionValidation".
* El cuerpo del evento tiene el mismo esquema que otros eventos de Event Grid.
* La propiedad eventType del evento es `Microsoft.EventGrid.SubscriptionValidationEvent`.
* La propiedad de datos del evento incluye una propiedad `validationCode` con una cadena generada aleatoriamente. Por ejemplo, "validationCode: acb13…".
* Los datos del evento también incluyen una propiedad `validationUrl` con una dirección URL para validar manualmente la suscripción.
* La matriz contiene solo el evento de validación. Otros eventos se envían en una solicitud aparte después de devolver el código de validación.
* Los SDK de EventGrid DataPlane tienen clases que corresponden a los datos de evento de validación de suscripción y a la respuesta de validación de suscripción.

En el siguiente ejemplo se muestra un evento SubscriptionValidationEvent de ejemplo:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6",
    "validationUrl": "https://rp-eastus2.eventgrid.azure.net:553/eventsubscriptions/estest/validate?id=512d38b6-c7b8-40c8-89fe-f46f9e9622b6&t=2018-04-26T20:30:54.4538837Z&apiVersion=2018-05-01-preview&token=1A1A1A1A"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Para comprobar la propiedad del punto de conexión, devuelva el código de validación en la propiedad validationResponse, como se muestra en el siguiente ejemplo:

```json
{
  "validationResponse": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
}
```

Debe devolver un código de estado de respuesta HTTP 200 OK. HTTP 202 Aceptado no se reconoce como una respuesta válida de validación de suscripción a Event Grid. La solicitud http debe completarse en 30 segundos. Si la operación no finaliza en 30 segundos, se cancelará y puede que se vuelva a intentar pasados cinco segundos. Si se producen errores en todos los intentos, se tratará como un error de protocolo de enlace de validación.

O bien, puede enviar una solicitud GET a la dirección URL de validación para validar la suscripción manualmente. La suscripción a eventos permanece en estado pendiente hasta que se valida. La dirección URL de validación usa el puerto 553. Si las reglas de firewall bloquean el puerto 553, puede que sea necesario actualizarlas para aplicar el protocolo de enlace manual de forma satisfactoria.

Para ver un ejemplo del tratamiento del protocolo de enlace de validación de suscripción, consulte un [ejemplo de C#](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/blob/master/EventGridConsumer/EventGridConsumer/Function1.cs).

### <a name="checklist"></a>Lista de comprobación

Durante la creación de la suscripción a eventos, si ve un mensaje de error, parecido a "Error al intentar validar el punto de conexión proporcionado https:\//your-endpoint-here failed. Para más información, visite https:\//aka.ms/esvalidation", indica que hay un error en el protocolo de enlace de validación. Para resolver este error, compruebe los siguientes aspectos:

* ¿Controla el código de aplicación que se ejecuta en el punto de conexión de destino? Por ejemplo, si está escribiendo un desencadenador HTTP basado en Azure Function, ¿tiene acceso al código de aplicación para realizar cambios en él?
* Si tiene acceso al código de aplicación, implemente el mecanismo del protocolo de enlace basado en ValidationCode como se muestra en el ejemplo anterior.

* Si no tiene acceso al código de aplicación (por ejemplo, si usa un servicio de terceros que admita webhooks), puede usar el mecanismo del protocolo de enlace manual. Asegúrese de que usa la versión de la API 2018-05-01-preview o posterior (instale la extensión de la CLI de Azure Event Grid) para recibir la propiedad validationUrl en el evento de validación. Para completar el protocolo de enlace de validación manual, obtenga el valor de la propiedad `validationUrl` y visite esa dirección URL en el explorador web. Si la validación es correcta, verá un mensaje en el explorador web que lo indica. Verá que el valor provisioningState de la suscripción del evento es "Succeeded". 

### <a name="event-delivery-security"></a>Seguridad de entrega de eventos

#### <a name="azure-ad"></a>Azure AD

Puede proteger el punto de conexión de webhook mediante Azure Active Directory para autenticar y autorizar a Event Grid a que publique eventos en los puntos de conexión. Tendrá que crear una aplicación de Azure Active Directory, crear un rol y una entidad de servicio en la aplicación que autorice a Event Grid, y configurar la suscripción de eventos para usar la aplicación de Azure AD. [Obtenga información sobre cómo configurar AAD con Event Grid](secure-webhook-delivery.md).

#### <a name="query-parameters"></a>Parámetros de consulta
Puede proteger el punto de conexión del webhook mediante la incorporación de parámetros de consulta a la URL del webhook al crear una suscripción a eventos. Establezca uno de estos parámetros de consulta para que sea un secreto, como un [token de acceso](https://en.wikipedia.org/wiki/Access_token). El webhook puede usar el secreto para reconocer que el evento procede de Event Grid con permisos válidos. Event Grid incluye estos parámetros de consulta en cada entrega de eventos al webhook.

Al editar la suscripción al evento, los parámetros de consulta no se muestran ni se devuelven, a menos que el parámetro [--include-full-endpoint-url](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-show) se utilice en la [CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) de Azure.

Por último, es importante tener en cuenta que Azure Event Grid solo admite puntos de conexión de webhook HTTPS.

## <a name="event-subscription"></a>Suscripción a eventos

Para suscribirse a un evento, debe probar que tiene acceso al origen del evento y al controlador. La demostración de que posee un WebHook se ha tratado en la sección anterior. Si usa un controlador de eventos que no sea un WebHook (como un almacenamiento de cola o un centro de eventos), necesita acceso de escritura a ese recurso. Esta comprobación de permisos impide que un usuario no autorizado envíe eventos al recurso.

Debe tener el permiso **Microsoft.EventGrid/EventSubscriptions/Write** en el recurso que constituya el origen del evento. Necesita este permiso porque está escribiendo una nueva suscripción en el ámbito del recurso. El recurso requerido difiere en función de si se suscribe a un tema del sistema o a un tema personalizado. Ambos tipos se describen en esta sección.

### <a name="system-topics-azure-service-publishers"></a>Temas del sistema (editores del servicio de Azure)

Para temas del sistema, necesita permiso para escribir una nueva suscripción a eventos en el ámbito del recurso que publica el evento. El formato del recurso es: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{resource-provider}/{resource-type}/{resource-name}`

Por ejemplo, para suscribirse a un evento en una cuenta de almacenamiento denominada **myacct**, necesita el permiso Microsoft.EventGrid/EventSubscriptions/Write en:`/subscriptions/####/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myacct`

### <a name="custom-topics"></a>Temas personalizados

Para temas personalizados, necesita permiso para escribir una nueva suscripción a eventos en el ámbito del tema de Event Grid. El formato del recurso es: `/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.EventGrid/topics/{topic-name}`

Por ejemplo, para suscribirse a un tema personalizado denominado **mytopic**, necesita el permiso Microsoft.EventGrid/EventSubscriptions/Write en: `/subscriptions/####/resourceGroups/testrg/providers/Microsoft.EventGrid/topics/mytopic`

## <a name="custom-topic-publishing"></a>Publicación de temas personalizados

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

El recurso es la ruta de acceso del tema de Event Grid al que envía los eventos. Por ejemplo, una ruta de acceso de recurso válida es: `https://<yourtopic>.<region>.eventgrid.azure.net/eventGrid/api/events`

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

## <a name="next-steps"></a>Pasos siguientes

* Para obtener una introducción a Event Grid, vea [About Event Grid](overview.md) (Acerca de Event Grid).
