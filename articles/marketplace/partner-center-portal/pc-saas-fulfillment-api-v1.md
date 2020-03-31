---
title: API de suministro de SaaS v1 | Azure Marketplace
description: Explica cómo crear y administrar una oferta de SaaS en Azure Marketplace mediante las API de suministro v1 asociadas.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 05/23/2019
ms.author: dsindona
ROBOTS: NOINDEX
ms.openlocfilehash: 3ec8373288a2ea5809ee5d349c52c57051586035
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80288349"
---
# <a name="saas-fulfillment-apis-version-1-deprecated"></a>API de suministro de SaaS, versión 1 (en desuso)

En este artículo se explica cómo crear una oferta de SaaS con las API. Las API, que están compuestas de métodos y puntos de conexión de REST, son necesarias para permitir las suscripciones a la oferta de SaaS si tiene seleccionada la venta mediante Azure.  

> [!WARNING]
> Esta versión inicial de la API de suministro de SaaS está en desuso; en su lugar, use la [API de suministro de SaaS V2](./pc-saas-fulfillment-api-v2.md).  Esta versión inicial de la API se mantiene actualmente solo para ofrecer servicio a los editores existentes. 

Las API siguientes se proporcionan para ayudarle a integrar su servicio SaaS con Azure:

-   Resolver
-   Suscribirse
-   Convert
-   Cancelar suscripción


## <a name="api-methods-and-endpoints"></a>Métodos y puntos de conexión de la API

Las secciones siguientes describen los métodos de API y los puntos de conexión disponibles para habilitar las suscripciones para una oferta de SaaS.


### <a name="marketplace-api-endpoint-and-api-version"></a>Punto de conexión de la API de Marketplace y versión de la API

El punto de conexión para la API de Azure Marketplace es `https://marketplaceapi.microsoft.com`.

La versión actual de la API es `api-version=2017-04-15`.


### <a name="resolve-subscription"></a>Resolución de la suscripción

La acción POST en el punto de conexión resolve permite a los usuarios resolver un token de Marketplace a un identificador de recurso persistente.  El identificador de recurso es el identificador único para la suscripción de SAAS. 

Cuando se redirige a un usuario al sitio web del ISV, la dirección URL contiene un token en los parámetros de consulta. Se espera que el ISV use este token y haga una solicitud para resolverlo. La respuesta contiene el id. de suscripción de SAAS, el nombre, el id. de oferta y el plan exclusivos para el recurso. Este token es válido durante una hora únicamente.

*Solicitud*

**POST**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/resolve?api-version=2017-04-15**

|  **Nombre de parámetro** |     **Descripción**                                      |
|  ------------------ |     ---------------------------------------------------- |
|  api-version        |  Versión de la operación que se usará para esta solicitud.   |
|  |  |


*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                                                                                                                                                  |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | No           | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
| x-ms-correlationid | No           | Valor de cadena único para la operación en el cliente. Este campo pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| Content-type       | Sí          | `application/json`                                        |
| authorization      | Sí          | Token de portador JSON Web Token (JWT).                    |
| x-ms-marketplace-token| Sí| Parámetro de consulta del token de la dirección URL cuando se redirige al usuario al sitio web del ISV de SaaS desde Azure. **Nota:** Este token solo es válido durante 1 hora. Además, descodifique como dirección URL el valor del token del explorador antes de usarlo.|
|  |  |  |
  

*Cuerpo de la respuesta*

``` json
{
    "id": "",
    "subscriptionName": "",
    "offerId": "",
    "planId": "",
}
```

| **Nombre de parámetro** | **Tipo de datos** | **Descripción**                       |
|--------------------|---------------|---------------------------------------|
| id                 | String        | Identificador de la suscripción de SaaS.          |
| subscriptionName| String| Nombre de la suscripción de SaaS establecida por el usuario en Azure al suscribirse al servicio SaaS.|
| OfferId            | String        | Identificador de la oferta a la que se ha suscrito el usuario. |
| planId             | String        | Identificador del plan al que se ha suscrito el usuario.  |
|  |  |  |


*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                                         |
|----------------------|--------------------| --------------------------------------------------------------------------------------- |
| 200                  | `OK`                 | Token resuelto correctamente.                                                            |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o se ha especificado una versión de API incorrecta. No se pudo resolver el token porque tiene formato incorrecto o ha expirado (una vez generado, el token solo es válido durante 1 hora). |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                                 |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                                |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                                        |
|  |  |  |


*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sí          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | Sí          | Identificador de correlación si lo pasa el cliente; en caso contrario, este valor es el identificador de correlación del servidor.                   |
| x-ms-activityid    | Sí          | Valor de cadena único para el seguimiento de la solicitud del servicio. Este identificador se utiliza para las reconciliaciones. |
| Retry-After        | No           | Este valor se establece solo para una respuesta 429.                                                                   |
|  |  |  |


### <a name="subscribe"></a>Suscribirse

El punto de conexión de suscripción permite a los usuarios iniciar una suscripción a un servicio SaaS para un plan determinado y habilitar la facturación en el sistema de comercio.

**PUT**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{identificador_de_suscripción}* ?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Identificador único de la suscripción de SaaS que se obtiene después de resolver el token mediante la API de resolución.                              |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

|  **Clave del encabezado**        | **Obligatorio** |  **Descripción**                                                  |
| ------------------     | ------------ | --------------------------------------------------------------------------------------- |
| x-ms-requestid         |   No         | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| x-ms-correlationid     |   No         | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| If-Match/If-None-Match |   No         |   Valor ETag de validador seguro.                                                          |
| content-type           |   Sí        |    `application/json`                                                                   |
|  authorization         |   Sí        |    Token de portador JSON Web Token (JWT).                                               |
| x-ms-marketplace-session-mode| No | Marca para habilitar el modo de simulacro al suscribirse a una oferta de SaaS. Si se establece, no se cargará la suscripción. Esto es útil para escenarios de pruebas del ISV. Se debe establecer en **'dryrun'**|
|  |  |  |

*Cuerpo*

``` json
{
    "lanId": "",
}
```

| **Nombre del elemento** | **Tipo de datos** | **Descripción**                      |
|------------------|---------------|--------------------------------------|
| planId           | Cadena (obligatorio)        | Identificador del plan del servicio SaaS al que se está suscribiendo el usuario.  |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activación de la suscripción SaaS recibida para un plan determinado.                   |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o el cuerpo del elemento JSON tiene un formato incorrecto. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                   |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                  |
| 409                  | `Conflict`           | Otra operación está en curso en la suscripción.                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                  |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                          |
|  |  |  |

En el caso de una respuesta 202, realice un seguimiento del estado de la operación de solicitud en el encabezado "Operation-location". La autenticación es igual que en otras API de Marketplace.

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sí          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | Sí          | Identificador de correlación si lo pasa el cliente; en caso contrario, este valor es el identificador de correlación del servidor.                   |
| x-ms-activityid    | Sí          | Valor de cadena único para el seguimiento de la solicitud del servicio. Este valor se utiliza para las reconciliaciones. |
| Retry-After        | Sí          | Intervalo con el que el cliente puede comprobar el estado.                                                       |
| Operation-Location | Sí          | Vínculo a un recurso para obtener el estado de la operación.                                                        |
|  |  |  |

### <a name="change-plan-endpoint"></a>Punto de conexión de cambio de plan

El punto de conexión de cambio permite al usuario convertir el plan al que actualmente está suscrito a un nuevo plan.

**PATCH**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{identificador_de_suscripción}* ?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Identificador de la suscripción de SaaS.                              |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**          | **Obligatorio** | **Descripción**                                                                                                                                                                                                                  |
|-------------------------|--------------|---------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid          | No           | Valor de cadena único para el seguimiento de la solicitud del cliente. Se recomienda utilizar un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.   |
| x-ms-correlationid      | No           | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| If-Match /If-None-Match | No           | Valor ETag de validador seguro.                              |
| content-type            | Sí          | `application/json`                                        |
| authorization           | Sí          | Token de portador JSON Web Token (JWT).                    |
|  |  |  |

*Cuerpo*

```json
{
    "planId": ""
}
```

|  **Nombre del elemento** |  **Tipo de datos**  | **Descripción**                              |
|  ---------------- | -------------   | --------------------------------------       |
|  planId           |  Cadena (obligatorio)         | Identificador del plan del servicio SaaS al que se está suscribiendo el usuario.          |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activación de la suscripción SaaS recibida para un plan determinado.                   |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o el cuerpo del elemento JSON tiene un formato incorrecto. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                   |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                  |
| 409                  | `Conflict`           | Otra operación está en curso en la suscripción.                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                  |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                          |
|  |  |  |

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sí          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | Sí          | Identificador de correlación si lo pasa el cliente; en caso contrario, este valor es el identificador de correlación del servidor.                   |
| x-ms-activityid    | Sí          | Valor de cadena único para el seguimiento de la solicitud del servicio. Este valor se utiliza para las reconciliaciones. |
| Retry-After        | Sí          | Intervalo con el que el cliente puede comprobar el estado.                                                       |
| Operation-Location | Sí          | Vínculo a un recurso para obtener el estado de la operación.                                                        |
|  |  |  |

### <a name="delete-subscription"></a>Eliminar suscripción

La acción de eliminación en el punto de conexión de suscripción permite al usuario eliminar una suscripción con un identificador especificado.

*Solicitud*

**DELETE**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{identificador_de_suscripción}* ?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Identificador de la suscripción de SaaS.                              |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                                                                                                                                                  |
|--------------------|--------------| ----------------------------------------------------------|
| x-ms-requestid     | No           | Valor de cadena único para el seguimiento de la solicitud del cliente. Se recomienda utilizar un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.                                                           |
| x-ms-correlationid | No           | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| authorization      | Sí          | Token de portador JSON Web Token (JWT).                    |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                           |
|----------------------|--------------------|---------------------------------------------------------------------------|
| 202                  | `Accepted`           | Activación de la suscripción SaaS recibida para un plan determinado.                   |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o el cuerpo del elemento JSON tiene un formato incorrecto. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                   |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                  |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                  |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio. Inténtelo de nuevo más tarde.                          |
|  |  |  |

En el caso de una respuesta 202, realice un seguimiento del estado de la operación de solicitud en el encabezado "Operation-location". La autenticación es igual que en otras API de Marketplace.

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sí          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | Sí          | Identificador de correlación si lo pasa el cliente; en caso contrario, es el identificador de correlación del servidor.                   |
| x-ms-activityid    | Sí          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | Sí          | Intervalo con el que el cliente puede comprobar el estado.                                                       |
| Operation-Location | Sí          | Vínculo a un recurso para obtener el estado de la operación.                                                        |
|   |  |  |

### <a name="get-operation-status"></a>Obtención del estado de la operación

Este punto de conexión permite el usuario realizar un seguimiento del estado de una operación asincrónica desencadenada (Suscribirse / Cancelar suscripción / Cambiar plan).

*Solicitud*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/operations/ *{identificador_de_operación}* ?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| operationId         | Identificador único de la operación desencadenada.                |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                                                                                                                                                  |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | No           | Valor de cadena único para el seguimiento de la solicitud del cliente. Se recomienda utilizar un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.   |
| x-ms-correlationid | No           | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.  |
| authorization      | Sí          | Token de portador JSON Web Token (JWT).                    |
|  |  |  | 

*Cuerpo de la respuesta*

```json
{
    "id": "",
    "status": "",
    "resourceLocation": "",
    "created": "",
    "lastModified": ""
}
```

| **Nombre de parámetro** | **Tipo de datos** | **Descripción**                                                                                                                                               |
|--------------------|---------------|-------------------------------------------------------------------------------------------|
| id                 | String        | Identificador de la operación.                                                                      |
| status             | Enum          | Estado de la operación, uno de los siguientes: `In Progress`, `Succeeded` o `Failed`.          |
| resourceLocation   | String        | Vínculo a la suscripción que se ha creado o modificado. Sirve de ayuda al cliente para obtener el estado actualizado de la operación POST. No se establece este valor para las operaciones `Unsubscribe`. |
| created            | DateTime      | Hora de creación de la operación en formato UTC.                                                           |
| lastModified       | DateTime      | Última actualización de la operación en formato UTC.                                                      |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | La solicitud GET se ha resuelto correctamente y el cuerpo contiene la respuesta.    |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o se ha especificado una versión de API incorrecta. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                      |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado.                                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                     |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                             |
|  |  |  |

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sí          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | Sí          | Identificador de correlación si lo pasa el cliente; en caso contrario, es el identificador de correlación del servidor.                   |
| x-ms-activityid    | Sí          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | Sí          | Intervalo con el que el cliente puede comprobar el estado.                                                       |
|  |  |  |

### <a name="get-subscription"></a>Obtención de la suscripción

La acción Get en el punto de conexión de suscripción permite a un usuario recuperar una suscripción con un identificador de recurso determinado.

*Solicitud*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions/ *{identificador_de_suscripción}* ?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| subscriptionId      | Identificador de la suscripción de SaaS.                              |
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                           |
|--------------------|--------------|-----------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | No           | Valor de cadena único para el seguimiento de la solicitud del cliente, preferiblemente un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.                                                           |
| x-ms-correlationid | No           | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| authorization      | Sí          | Token de portador JSON Web Token (JWT).                                                                    |
|  |  |  |

*Cuerpo de la respuesta*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Nombre de parámetro**     | **Tipo de datos** | **Descripción**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Identificador del recurso de la suscripción de SaaS en Azure.    |
| offerId                | String        | Identificador de la oferta a la que se ha suscrito el usuario.         |
| planId                 | String        | Identificador del plan al que se ha suscrito el usuario.          |
| saasSubscriptionName   | String        | Nombre de la suscripción de SaaS.                |
| saasSubscriptionStatus | Enum          | Estado de la operación.  Uno de los siguientes:  <br/> - `Subscribed`: la suscripción está activa.  <br/> - `Pending`: el usuario ha creado el recurso, pero no ha sido activado por el ISV.   <br/> - `Unsubscribed`: el usuario ha cancelado la suscripción.   <br/> - `Suspended`: el usuario ha suspendido la suscripción.   <br/> - `Deactivated`:  la suscripción de Azure está suspendida.  |
| created                | DateTime      | Valor de la marca de tiempo de la creación de la suscripción en formato UTC. |
| lastModified           | DateTime      | Valor de la marca de tiempo de la modificación de la suscripción en formato UTC. |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | La solicitud GET se ha resuelto correctamente y el cuerpo contiene la respuesta.    |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o se ha especificado una versión de API incorrecta. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                      |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                     |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio, vuelva a intentarlo más tarde.                             |
|  |  |  |

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sí          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | Sí          | Identificador de correlación si lo pasa el cliente; en caso contrario, es el identificador de correlación del servidor.                   |
| x-ms-activityid    | Sí          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | No           | Intervalo con el que el cliente puede comprobar el estado.                                                       |
| eTag               | Sí          | Vínculo a un recurso para obtener el estado de la operación.                                                        |
|  |  |  |

### <a name="get-subscriptions"></a>Obtención de las suscripciones

La acción Get en el punto de conexión de las suscripciones permite a un usuario recuperar todas las suscripciones de todas las ofertas del ISV.

*Solicitud*

**GET**

**https://marketplaceapi.microsoft.com/api/saas/subscriptions?api-version=2017-04-15**

| **Nombre de parámetro**  | **Descripción**                                       |
|---------------------|-------------------------------------------------------|
| api-version         | Versión de la operación que se usará para esta solicitud. |
|  |  |

*Encabezados*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                           |
|--------------------|--------------|-----------------------------------------------------------|
| x-ms-requestid     | No           | Valor de cadena único para el seguimiento de la solicitud del cliente. Se recomienda utilizar un GUID. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta.             |
| x-ms-correlationid | No           | Valor de cadena único para la operación en el cliente. Este valor pone en correlación todos los eventos de la operación en el cliente con los eventos en el servidor. Si no se proporciona este valor, se generará uno y se proporcionará en los encabezados de respuesta. |
| authorization      | Sí          | Token de portador JSON Web Token (JWT).                    |
|  |  |  |

*Cuerpo de la respuesta*

```json
{
    "id": "",
    "saasSubscriptionName": "",
    "offerId": "",
    "planId": "",
    "saasSubscriptionStatus": "",
    "created": "",
    "lastModified": ""
}
```

| **Nombre de parámetro**     | **Tipo de datos** | **Descripción**                               |
|------------------------|---------------|-----------------------------------------------|
| id                     | String        | Identificador del recurso de la suscripción de SaaS en Azure.    |
| offerId                | String        | Identificador de la oferta a la que se ha suscrito el usuario.         |
| planId                 | String        | Identificador del plan al que se ha suscrito el usuario.          |
| saasSubscriptionName   | String        | Nombre de la suscripción de SaaS.                |
| saasSubscriptionStatus | Enum          | Estado de la operación.  Uno de los siguientes:  <br/> - `Subscribed`: la suscripción está activa.  <br/> - `Pending`: el usuario ha creado el recurso, pero no ha sido activado por el ISV.   <br/> - `Unsubscribed`: el usuario ha cancelado la suscripción.   <br/> - `Suspended`: el usuario ha suspendido la suscripción.   <br/> - `Deactivated`:  la suscripción de Azure está suspendida.  |
| created                | DateTime      | Valor de la marca de tiempo de la creación de la suscripción en formato UTC. |
| lastModified           | DateTime      | Valor de la marca de tiempo de la modificación de la suscripción en formato UTC. |
|  |  |  |

*Códigos de respuesta*

| **Código de estado HTTP** | **Código de error**     | **Descripción**                                                              |
|----------------------|--------------------|------------------------------------------------------------------------------|
| 200                  | `OK`                 | La solicitud GET se ha resuelto correctamente y el cuerpo contiene la respuesta.    |
| 400                  | `BadRequest`         | Faltan encabezados necesarios o se ha especificado una versión de API incorrecta. |
| 403                  | `Forbidden`          | El llamador no está autorizado para realizar esta operación.                      |
| 404                  | `NotFound`           | No se ha encontrado una suscripción con el identificador especificado                                     |
| 429                  | `RequestThrottleId`  | El servicio está ocupado procesando solicitudes, vuelva a intentarlo más tarde.                     |
| 503                  | `ServiceUnavailable` | El servicio está temporalmente fuera de servicio. Inténtelo de nuevo más tarde.                             |
|  |  |  |

*Encabezados de respuesta*

| **Clave del encabezado**     | **Obligatorio** | **Descripción**                                                                                        |
|--------------------|--------------|--------------------------------------------------------------------------------------------------------|
| x-ms-requestid     | Sí          | Identificador de solicitud recibido del cliente.                                                                   |
| x-ms-correlationid | Sí          | Identificador de correlación si lo pasa el cliente; en caso contrario, es el identificador de correlación del servidor.                   |
| x-ms-activityid    | Sí          | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones. |
| Retry-After        | No           | Intervalo con el que el cliente puede comprobar el estado.                                                       |
|  |  |  |

### <a name="saas-webhook"></a>Webhook de SaaS

Un webhook de SaaS se usa para notificar los cambios de forma proactiva al servicio SaaS. Se espera que esta API POST no esté autenticada y el servicio de Microsoft la llamará. Se espera que el servicio SaaS llame a la API de operaciones para validar y autorizar antes de realizar alguna acción en la notificación de webhook. 

*Cuerpo*

``` json
  {
    "id": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "activityId": "be750acb-00aa-4a02-86bc-476cbe66d7fa",
    "subscriptionId":"cd9c6a3a-7576-49f2-b27e-1e5136e57f45",
    "action": "Subscribe", // Subscribe/Unsubscribe/ChangePlan
    "operationRequestSource":"Azure",
    "timeStamp":"2018-12-01T00:00:00"
  }
```

| **Nombre de parámetro**     | **Tipo de datos** | **Descripción**                               |
|------------------------|---------------|-----------------------------------------------|
| id  | String       | Identificador único de la operación desencadenada.                |
| activityId   | String        | Valor de cadena único para el seguimiento de la solicitud del servicio. Se utiliza para las reconciliaciones.               |
| subscriptionId                     | String        | Identificador del recurso de la suscripción de SaaS en Azure.    |
| offerId                | String        | Identificador de la oferta a la que se ha suscrito el usuario. Se proporciona únicamente con la acción "Actualizar".        |
| publisherId                | String        | Id. de publicador de la oferta de SaaS         |
| planId                 | String        | Identificador del plan al que se ha suscrito el usuario. Se proporciona únicamente con la acción "Actualizar".          |
| action                 | String        | La acción que desencadena esta notificación. Valores posibles: Activate, Delete, Suspend, Reinstate, Update          |
| Marca de tiempo                 | String        | Valor de marca de tiempo en UTC cuando se desencadenó esta notificación.          |
|  |  |  |


## <a name="next-steps"></a>Pasos siguientes

Los desarrolladores también pueden recuperar y manipular mediante programación las ofertas de carga de trabajo y los perfiles del anunciante usando las [API de REST de Cloud Partner Portal](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview).
