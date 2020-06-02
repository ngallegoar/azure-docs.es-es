---
title: API de suministro de SaaS v2 en el marketplace comercial de Microsoft
description: Aprenda a crear y administrar una oferta de SaaS en Microsoft AppSource y Azure Marketplace mediante las API de suministro v2.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 10/18/2019
ms.author: dsindona
ms.openlocfilehash: 8f452349c0d636bc74a3722e2526623c955093f7
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2020
ms.locfileid: "82854192"
---
# <a name="saas-fulfillment-apis-version-2-in-microsoft-commercial-marketplace"></a>API de suministro de SaaS v2 en el marketplace comercial de Microsoft

En este artículo se describen las API de suministro que permiten a los partners vender sus aplicaciones de SaaS en Microsoft AppSource y Azure Marketplace. Estas API son necesarias para las ofertas de SaaS comercializables.

## <a name="managing-the-saas-subscription-life-cycle"></a>Administrar el ciclo de vida de la suscripción a SaaS

SaaS de Azure administra todo el ciclo de vida de una compra de suscripción de SaaS. Usa las API de suministro como mecanismo para controlar el suministro real, los cambios en los planes y la eliminación de la suscripción al partner. La factura que recibe el cliente se basa en el estado de la suscripción a SaaS que mantiene Microsoft. En el siguiente diagrama se muestran los estados y las operaciones que controlan los cambios entre estados.

![Estados del ciclo de vida de la suscripción a SaaS](./media/saas-subscription-lifecycle-api-v2.png)


### <a name="states-of-a-saas-subscription"></a>Estados de una suscripción a SaaS

En la tabla siguiente se enumeran los estados de aprovisionamiento de una suscripción a SaaS, e incluye una descripción y un diagrama de secuencias para cada uno (si procede). 

#### <a name="provisioning"></a>Aprovisionamiento

Cuando un cliente inicia una compra, el partner recibe esta información en un código de autorización de una página web interactiva del cliente que usa un parámetro de URL. Un ejemplo es `https://contoso.com/signup?token=..`, donde la dirección URL de la página de aterrizaje en el Centro de partners es `https://contoso.com/signup`. El código de autorización se puede validar e intercambiar por los detalles del servicio de aprovisionamiento mediante una llamada a la API de resolución.  Cuando un servicio de SaaS finaliza el aprovisionamiento, envía una llamada de activación para indicar que el procesamiento ha finalizado y que se puede facturar al cliente. 

En el diagrama siguiente se muestra la secuencia de llamadas API en un escenario de aprovisionamiento.  

![Llamadas API para aprovisionar un servicio de SaaS](./media/saas-post-provisioning-api-v2-calls.png)

#### <a name="provisioned"></a>aprovisionado

Es el estado estable de un servicio de aprovisionamiento.

##### <a name="provisioning-for-update"></a>Aprovisionamiento para actualizar 

Este estado indica que un servicio existente tiene una actualización pendiente. El cliente puede iniciar esta actualización en Marketplace o en el servicio de SaaS (solo para transacciones directas al cliente).

##### <a name="provisioning-for-update-when-its-initiated-from-the-marketplace"></a>Aprovisionamiento para actualizar (cuando se inicia desde Marketplace)

En el siguiente diagrama se muestra la secuencia de las acciones cuando se inicia una actualización desde Marketplace.

![Llamadas API cuando se inicia una actualización desde Marketplace](./media/saas-update-api-v2-calls-from-marketplace-a.png)

##### <a name="provisioning-for-update-when-its-initiated-from-the-saas-service"></a>Aprovisionamiento para actualizar (cuando se inicia desde el servicio de SaaS)

En el siguiente diagrama se muestran las acciones cuando se inicia una actualización desde el servicio de SaaS. (La llamada de webhook se sustituye por una actualización de la suscripción que inicia el servicio de SaaS). 

![Llamadas API cuando se inicia una actualización desde el servicio de SaaS](./media/saas-update-api-v2-calls-from-saas-service-a.png) 

#### <a name="suspended"></a>Suspended

Este estado indica que no se ha recibido el pago de un cliente. En cumplimiento de la directiva, proporcionaremos al cliente un período de gracia antes de cancelar la suscripción. Cuando una suscripción está en este estado: 

- Como asociado, puede optar por degradar o bloquear el acceso del usuario al servicio.
- La suscripción debe permanecer en un estado recuperable que pueda restaurar la funcionalidad completa sin que se pierdan datos ni la configuración. 
- Espere recibir una solicitud de restablecimiento para esta suscripción a través de la API de suministro o una solicitud de desaprovisionamiento al finalizar el período de gracia. 

#### <a name="unsubscribed"></a>Suscripción cancelada 

Las suscripciones adquieren este estado en respuesta a una solicitud explícita del cliente o ante un impago de cuotas. Se espera que el asociado conserve los datos del cliente durante un cierto número de días para llevar a cabo una recuperación si se solicita y que después los elimine. 


## <a name="api-reference"></a>Referencia de API

En esta sección se documentan la *API de suscripción* a SaaS y la *API de operaciones*.  El valor del parámetro `api-version` para la versión 2 de las API es `2018-08-31`.  


### <a name="parameter-and-entity-definitions"></a>Definiciones de parámetros y entidades

En la tabla siguiente se recogen las definiciones de entidades y parámetros comunes usados por las API de suministro.

|     Entidad/parámetro     |     Definición                         |
|     ----------------     |     ----------                         |
| `subscriptionId`         | Identificador GUID para un recurso de SaaS.  |
| `name`                   | Nombre descriptivo proporcionado por el cliente para este recurso. |
| `publisherId`            | Identificador de cadenas único para cada anunciante (por ejemplo, "contoso"). |
| `offerId`                | Identificador de cadenas único para cada oferta (por ejemplo, "oferta1").  |
| `planId`                 | Identificador de cadenas único para cada plan/SKU (por ejemplo, "plata"). |
| `operationId`            | Identificador GUID para una operación determinada.  |
|  `action`                | La acción que se va a realizar en un recurso, ya sea `Unsubscribe`, `Suspend`, `Reinstate` o `ChangePlan`, `ChangeQuantity`, `Transfer`. |
|   |   |

Los identificadores únicos globales ([GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)) son números de 128 bits (hexadecimal de 32) que normalmente se generan automáticamente. 

#### <a name="resolve-a-subscription"></a>Resolver una suscripción 

El punto de conexión de resolución permite al anunciante resolver un token de Marketplace a un identificador de recursos persistente. El identificador de recurso es el identificador único para la suscripción de SaaS. Cuando se redirige a un usuario al sitio web de un asociado, la dirección URL contiene un token en los parámetros de consulta. Se espera que el partner use este token y haga una solicitud para resolverlo. La respuesta contiene el id. de suscripción de SaaS, el nombre, el id. de oferta y el plan exclusivos para el recurso. Este token es válido durante una hora únicamente. 

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsresolveapi-versionapiversion"></a>Post<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |

*Encabezados de la solicitud*:
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |  Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). Por ejemplo, "`Bearer <access_token>`". |
|  x-ms-marketplace-token  |  Parámetro de consulta de token de la dirección URL cuando se redirige al usuario al sitio web del asociado de SaaS desde Azure (por ejemplo: `https://contoso.com/signup?token=..`). *Nota:* La URL descodifica el valor del token del explorador antes de usarlo.  |

*Códigos de respuesta*:

Código: 200<br>
Resuelve el token opaco en una suscripción de SaaS. Cuerpo de la respuesta:
 

```json
{
    "id": "<guid>",  
    "subscriptionName": "Contoso Cloud Solution",
    "offerId": "offer1",
    "planId": "silver",
    "quantity": "20" 
}
```

Código: 400<br>
Solicitud incorrecta. Falta el token x-ms-marketplace-token, tiene un formato incorrecto o ha caducado.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.

Código: 404<br>
Not found.

Código: 500<br>
Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

### <a name="subscription-api"></a>API de suscripción

La API de suscripción admite las siguientes operaciones de HTTPS: **Get**, **Post**, **Patch** y **Delete**.


#### <a name="list-subscriptions"></a>Lista de suscripciones

Se enumeran todas las suscripciones a SaaS para un anunciante.

##### <a name="getbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionsapi-versionapiversion"></a>Obtener<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=<ApiVersion>`

*Parámetros de consulta*:

|             |                   |
|  --------   |  ---------------  |
| ApiVersion  |  Versión de la operación que se usará para esta solicitud.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
| Content-Type       |  `application/json`  |
| x-ms-requestid     |  Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
| x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
| authorization      |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). Por ejemplo, "`Bearer <access_token>`".  |

*Códigos de respuesta*:

Código: 200 <br/>
Obtiene el anunciante y las suscripciones correspondientes para todas las ofertas del anunciante, según el token de autenticación.

>[!Note]
>Las [API simuladas](#mock-apis) se usan cuando se desarrolla por primera vez la oferta, mientras que las API reales deben usarse cuando se publica realmente la oferta.  Las API reales y las API simuladas difieren en la primera línea del código.  En la API real hay una sección `subscription`, mientras que esta sección no existe en la API simulada.

Carga de respuesta de la API simulada:<br>

```json
{
  [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
            "term": {
                "startDate": "2019-05-31",
                "endDate": "2019-06-29",
                "termUnit": "P1M"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "isFreeTrial": "true", // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial.
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "continuationToken": ""
}
```
Y para la API real: <br>

```json
{
  "subscriptions": [
      {
          "id": "<guid>",
          "name": "Contoso Cloud Solution",
          "publisherId": "contoso",
          "offerId": "offer1",
          "planId": "silver",
          "quantity": "10",
          "beneficiary": { // Tenant, object id and email address for which SaaS subscription is purchased.
              "emailId": "<email>",
              "objectId": "<guid>",                     
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant, object id and email address that purchased the SaaS subscription. These could be different for reseller scenario
              "emailId": "<email>",
              "objectId": "<guid>",                      
              "tenantId": "<guid>"
          },
            "term": {
                "startDate": "2019-05-31",
                "endDate": "2019-06-29",
                "termUnit": "P1M"
          },
          "allowedCustomerOperations": [
              "Read" // Possible Values: Read, Update, Delete.
          ], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
          "sessionMode": "None", // Possible Values: None, DryRun (Dry Run indicates all transactions run as Test-Mode in the commerce stack)
          "isFreeTrial": true, // true - the customer subscription is currently in free trial, false - the customer subscription is not currently in free trial.(optional field - default false)
          "isTest": false, //indicating whether the current subscription is a test asset
          "sandboxType": "None", // Possible Values: None, Csp (Csp sandbox purchase)
          "saasSubscriptionStatus": "Subscribed" // Indicates the status of the operation: [NotStarted, PendingFulfillmentStart, Subscribed, Suspended, Unsubscribed]
      }
  ],
  "@nextLink": ""
}
```
El token de continuación estará presente solo si hay más “páginas” de planes por recuperar. 

Código: 403 <br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual. 

Código: 500<br>
Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="get-subscription"></a>Obtener una suscripción

Obtiene la suscripción de SaaS especificada. Use esta llamada para obtener información sobre la licencia y sobre el plan.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Obtener<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
| subscriptionId     |   Identificador único de la suscripción de SaaS que se obtiene después de resolver el token mediante la API de resolución.   |
|  ApiVersion        |   Versión de la operación que se usará para esta solicitud.   |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`  |
|  x-ms-requestid    |  Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). Por ejemplo, "`Bearer <access_token>`".  |

*Códigos de respuesta*:

Código: 200<br>
Obtiene la suscripción a SaaS del identificador. Carga de respuesta:<br>

```json
Response Body:
{ 
        "id":"",
        "name":"Contoso Cloud Solution",
        "publisherId": "contoso",
        "offerId": "offer1",
        "planId": "silver",
        "quantity": "10",
          "beneficiary": { // Tenant for which SaaS subscription is purchased.
              "tenantId": "<guid>"
          },
          "purchaser": { // Tenant that purchased the SaaS subscription. These could be different for reseller scenario
              "tenantId": "<guid>"
          },
        "allowedCustomerOperations": ["Read"], // Indicates operations allowed on the SaaS subscription. For CSP-initiated purchases, this will always be Read.
        "sessionMode": "None", // Dry Run indicates all transactions run as Test-Mode in the commerce stack
        "isFreeTrial": "true", // true - customer subscription is currently in free trial, false - customer subscription is not currently in free trial.
        "status": "Subscribed", // Indicates the status of the operation.
          "term": { //This gives the free trial term start and end date
            "startDate": "2019-05-31",
            "endDate": "2019-06-29",
            "termUnit": "P1M" //where P1M: Monthly, P1Y: Yearly 
        },
}
```

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.

Código: 404<br>
Not found.<br> 

Código: 500<br>
Error interno del servidor<br>

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }  
```

#### <a name="list-available-plans"></a>Lista de planes disponibles

Use esta llamada para averiguar si hay alguna oferta pública o privada para el anunciante actual.

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidlistavailableplansapi-versionapiversion"></a>Obtener<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/listAvailablePlans?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |   Versión de la operación que se usará para esta solicitud.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|   x-ms-requestid   |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
|  x-ms-correlationid  | Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Por ejemplo, "`Bearer <access_token>`". |

*Códigos de respuesta*:

Código: 200<br>
Obtiene una lista de los planes disponibles para un cliente. Cuerpo de la respuesta:

```json
{
    "plans": [{
        "planId": "Platinum001",
        "displayName": "Private platinum plan for Contoso",
        "isPrivate": true
    }]
}
```

Código: 404<br>
Not found.<br> 

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual. <br> 

Código: 500<br>
Error interno del servidor<br>

```json
{ 
    "error": { 
      "code": "UnexpectedError", 
      "message": "An unexpected error has occurred." 
    } 
```

#### <a name="activate-a-subscription"></a>Activar una suscripción

##### <a name="postbrhttpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidactivateapi-versionapiversion"></a>Post<br>`https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/activate?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de SaaS que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:
 
|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json`  |
|  x-ms-requestid    | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid  | Valor de cadena único para la operación en el cliente. Esta cadena pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Por ejemplo, "`Bearer <access_token>`". |

*Carga de la solicitud*:

```json
{
    "planId": "gold",
    "quantity": ""
}
```

*Códigos de respuesta*:

Código: 200<br>
Activa la suscripción.<br>

Código: 400<br>
Solicitud incorrecta: errores de validación.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.

Código: 404<br>
Not found.

Código: 500<br>
Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

#### <a name="change-the-plan-on-the-subscription"></a>Cambiar el plan de la suscripción

Actualizar el plan de la suscripción

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Revisión<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de SaaS que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid  |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.    |
| authorization      |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Por ejemplo, "`Bearer <access_token>`".  |

*Carga de la solicitud*:

```json
Request Body:
{
    "planId": "gold"
}
```

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Vínculo a un recurso para obtener el estado de la operación.   |

*Códigos de respuesta*:

Código: 202<br>
Se ha aceptado la solicitud para cambiar el plan. Se espera que el partner sondee la operación-ubicación para determinar si se realiza correctamente o no. <br>

Código: 400<br>
Solicitud incorrecta: errores de validación.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.

Código: 404<br>
Not found.

Código: 500<br>
Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Solo se puede revisar un plan o cantidad a la vez, pero no ambos. Las ediciones en una suscripción con **Update** no se encuentran en `allowedCustomerOperations`.

#### <a name="change-the-quantity-on-the-subscription"></a>Cambiar la cantidad de la suscripción

Actualizar la cantidad de la suscripción

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Patch:<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de SaaS que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      | `application/json` |
|  x-ms-requestid    |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid  |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.    |
| authorization      |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Por ejemplo, "`Bearer <access_token>`".  |

*Carga de la solicitud*:

```json
Request Body:
{
    "quantity": 5
}
```

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
| Operation-Location | Vínculo a un recurso para obtener el estado de la operación.   |

*Códigos de respuesta*:

Código: 202<br>
Se ha aceptado la solicitud para cambiar la cantidad. Se espera que el partner sondee la operación-ubicación para determinar si se realiza correctamente o no. <br>

Código: 400<br>
Solicitud incorrecta: errores de validación.


Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.

Código: 404<br>
Not found.

Código: 500<br>
Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```

>[!Note]
>Solo se puede revisar un plan o cantidad a la vez, pero no ambos. Las ediciones en una suscripción con **Update** no se encuentran en `allowedCustomerOperations`.

#### <a name="delete-a-subscription"></a>Eliminación de una suscripción

Cancele la suscripción y elimine la suscripción especificada.

##### <a name="deletebr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidapi-versionapiversion"></a>Eliminar<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de SaaS que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.   |
|  x-ms-correlationid  |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.   |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Por ejemplo, "`Bearer <access_token>`".  |

*Códigos de respuesta*:

Código: 202<br>
El partner inició una llamada para cancelar la suscripción a una suscripción de SaaS.<br>

Código: 400<br>
Eliminar en una suscripción cuando **Delete** no está en `allowedCustomerOperations`.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.

Código: 404<br>
Not found.

Código: 500<br>
Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}
```


### <a name="operations-api"></a>API de operaciones

La API de operaciones admite las siguientes operaciones Patch y Get.

#### <a name="list-outstanding-operations"></a>Lista de operaciones pendientes 

Se enumeran las operaciones pendientes para el anunciante actual. 

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsapi-versionapiversion"></a>Obtener<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations?api-version=<ApiVersion>`

*Parámetros de consulta*:

|             |        |
|  ---------------   |  ---------------  |
|    ApiVersion                |   Versión de la operación que se usará para esta solicitud.                |
| subscriptionId     | Identificador único de la suscripción de SaaS que se obtiene después de resolver el token mediante la API de resolución.  |

*Encabezados de la solicitud*:
 
|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     |  `application/json` |
|  x-ms-requestid    |  Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Por ejemplo, "`Bearer <access_token>`".  |

*Códigos de respuesta*:

Código: 200<br> Obtiene la lista de operaciones pendientes en una suscripción. Carga de respuesta:

```json
[{
    "id": "<guid>",  
    "activityId": "<guid>",
    "subscriptionId": "<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",  
    "status": "NotStarted"  
}]
```


Código: 400<br>
Solicitud incorrecta: errores de validación.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.

Código: 404<br>
Not found.

Código: 500<br>
Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

#### <a name="get-operation-status"></a>Obtener el estado de la operación

Permite al anunciante hacer un seguimiento del estado de la operación asincrónica desencadenada que se ha especificado (como `Subscribe`, `Unsubscribe`, `ChangePlan` o `ChangeQuantity`).

##### <a name="getbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Obtener<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|  ApiVersion        |  Versión de la operación que se usará para esta solicitud.  |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|  Content-Type      |  `application/json`   |
|  x-ms-requestid    |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta.  |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app). Por ejemplo, "`Bearer <access_token>`".  |

*Códigos de respuesta*:<br>

Código: 200<br> Obtiene la operación de SaaS pendiente especificada. Carga de respuesta:

```json
Response body:
{
    "id  ": "<guid>",
    "activityId": "<guid>",
    "subscriptionId":"<guid>",
    "offerId": "offer1",
    "publisherId": "contoso",  
    "planId": "silver",
    "quantity": "20",
    "action": "Convert",
    "timeStamp": "2018-12-01T00:00:00",
    "status": "NotStarted"
}

```

Código: 400<br>
Solicitud incorrecta: errores de validación.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.
 
Código: 404<br>
Not found.

Código: 500<br> Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```
#### <a name="update-the-status-of-an-operation"></a>Actualizar el estado de una operación

Actualiza el estado de una operación para indicar si se ha realizado correctamente o no con los valores proporcionados.

##### <a name="patchbr-httpsmarketplaceapimicrosoftcomapisaassubscriptionssubscriptionidoperationsoperationidapi-versionapiversion"></a>Revisión<br> `https://marketplaceapi.microsoft.com/api/saas/subscriptions/<subscriptionId>/operations/<operationId>?api-version=<ApiVersion>`

*Parámetros de consulta*:

|                    |                   |
|  ---------------   |  ---------------  |
|   ApiVersion       |  Versión de la operación que se usará para esta solicitud.  |
| subscriptionId     | Identificador único de la suscripción de SaaS que se obtiene después de resolver el token mediante la API de resolución.  |
|  operationId       | La operación que se está completando. |

*Encabezados de la solicitud*:

|                    |                   |
|  ---------------   |  ---------------  |
|   Content-Type     | `application/json`   |
|   x-ms-requestid   |   Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
|  x-ms-correlationid |  Valor de cadena único para la operación en el cliente. Este parámetro pone en correlación todos los eventos de la operación del cliente con los eventos del servidor. Si este valor no se proporciona, se generará uno y se proporcionará en los encabezados de respuesta. |
|  authorization     |  [Obtener un token de portador JSON Web Token (JWT)](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/saas-app/cpp-saas-registration#get-a-token-based-on-the-azure-ad-app).  Por ejemplo, "`Bearer <access_token>`".  |

*Carga de la solicitud*:

```json
{
    "planId": "offer1",
    "quantity": "44",
    "status": "Success"    // Allowed Values: Success/Failure. Indicates the status of the operation.
}

```

*Códigos de respuesta*:

Código: 200<br> Llamada para informar de la finalización de una operación por parte del partner. Por ejemplo, esta respuesta podría indicar el cambio de puestos o planes.

Código: 400<br>
Solicitud incorrecta: errores de validación.

Código: 403<br>
No autorizado. No se ha proporcionado el token de autenticación, no es válido o la solicitud intenta acceder a una adquisición que no pertenece al anunciante actual.

Código: 404<br>
Not found.

Código: 409<br>
Conflicto. Por ejemplo, ya se suministra una transacción más reciente.

Código: 500<br> Error interno del servidor

```json
{
    "error": {
      "code": "UnexpectedError",
      "message": "An unexpected error has occurred."
    }
}

```

## <a name="implementing-a-webhook-on-the-saas-service"></a>Implementación de un webhook en el servicio de SaaS

El anunciante debe implementar un webhook en este servicio de SaaS para informar de forma proactiva a los usuarios de los cambios en su servicio. Se espera que el servicio de SaaS llame a la API de operaciones para validar y autorizar antes de realizar una acción en la notificación de webhook.


```json
{
  "id": "<this is a GUID operation id, you can call operations API with this to get status>",
  "activityId": "<this is a Guid correlation id>",
  "subscriptionId": "<Guid to uniquely identify this resource>",
  "publisherId": "<this is the publisher's name>",
  "offerId": "<this is the offer name>",
  "planId": "<this is the plan id>",
  "quantity": "<the number of seats, will be null if not per-seat saas offer>",
  "timeStamp": "2019-04-15T20:17:31.7350641Z",
  "action": "Unsubscribe",
  "status": "NotStarted"  

}
```
Donde la acción puede ser una de las siguientes: 
- `Unsubscribe` (cuando se ha eliminado el recurso)
- `ChangePlan` (cuando se ha completado la operación de cambio de plan)
- `ChangeQuantity` (cuando se ha completado la operación de cambio de cantidad)
- `Suspend` (cuando se ha suspendido el recurso)
- `Reinstate` (cuando se ha recuperado el recurso tras la suspensión)

Donde el estado puede ser uno de los siguientes: 
- **NotStarted** <br>
 - **InProgress** <br>
- **Correcto** <br>
- **Erróneo** <br>
- **Conflict** <br>

En una notificación de webhook, los estados que requieren acción son **Succeeded** y **Failed**. El ciclo de vida de una operación va de **NotStarted** a un estado final, como **Succeeded**, **Failed** o **Conflict**. Si el estado es **NotStarted** o **InProgress**, antes de realizar cualquier acción, siga solicitando el estado a través de la API de la operación GET hasta que la operación alcance un estado final. 

## <a name="mock-apis"></a>API simuladas

Puede usar nuestras API simuladas para empezar a desarrollar, especialmente para crear prototipos, así como para probar proyectos. 

Punto de conexión de host: `https://marketplaceapi.microsoft.com/api` (no se requiere autenticación)<br/>
Versión de API: `2018-09-15`<br/>
URI de ejemplo: `https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2018-09-15` <br/>

Las rutas de acceso del punto de conexión de la API son las mismas tanto en las API simuladas como en las API reales, pero las versiones de API son diferentes. La versión es `2018-09-15` para la versión simulada, y `2018-08-31` para la versión de producción. 

Todas las llamadas de API de este artículo se pueden realizar en el punto de conexión de host simulado. En general, puede esperar obtener datos simulados como respuesta. Las llamadas a los métodos de actualización de la suscripción en la API simulada siempre devuelven 500. 

## <a name="next-steps"></a>Pasos siguientes

Los desarrolladores también pueden recuperar y manipular mediante programación las cargas de trabajo, las ofertas y los perfiles de anunciante usando las [API de REST de Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
