---
title: Autenticación saliente
description: Aprenda a configurar o quitar la autenticación de salida de Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/15/2016
ms.openlocfilehash: bcd14e618323aec1c7ce47fcebb25099fa96be81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898508"
---
# <a name="outbound-authentication-for-azure-scheduler"></a>Autenticación de salida en Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) reemplaza a Azure Scheduler, que se [va a retirar](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para seguir utilizando los trabajos configurados en Scheduler, [migre a Azure Logic Apps](../scheduler/migrate-from-scheduler-to-logic-apps.md) cuanto antes. 
>
> Scheduler ya no está disponible en Azure portal, pero la [API REST](/rest/api/scheduler) y los [cmdlets de PowerShell para Azure Scheduler](scheduler-powershell-reference.md) siguen disponibles en la actualidad para que pueda administrar los trabajos y las colecciones de trabajos.

Es posible que los trabajos de Azure Scheduler tengan que llamar a servicios que requieran autenticación, como otros servicios de Azure, Salesforce.com, Facebook y sitios web personalizados seguros. El servicio llamado puede determinar si el trabajo del Programador puede acceder a los recursos solicitados. 

El Programador admite estos modelos de autenticación: 

* Autenticación de *certificado de cliente* cuando se usan certificados de cliente SSL/TLS
* Autenticación *básica*
* Autenticación de *Active Directory OAuth*

## <a name="add-or-remove-authentication"></a>Adición o eliminación de la autenticación

* Para agregar autenticación a un trabajo del Programador, al crear o actualizar el trabajo, agregue el `authentication`elemento secundario de notación de objetos JavaScript (JSON) al elemento `request`. 

  Las respuestas nunca devuelven secretos que se pasen al servicio Scheduler mediante una solicitud PUT, PATCH o POST, en el objeto `authentication`. 
  Las respuestas establecen la información del secreto como nula o pueden usar un token público que representa la entidad autenticada. 

* Para quitar la autenticación de un trabajo del Programador, ejecute explícitamente una solicitud PUT o PATCH en el trabajo establezca el objeto `authentication` como nulo. La respuesta no contendrá propiedades de autenticación.

## <a name="client-certificate"></a>Certificado de cliente

### <a name="request-body---client-certificate"></a>Cuerpo de solicitud - Certificado de cliente

Al agregar autenticación mediante el modelo `ClientCertificate`, especifique estos elementos adicionales en el cuerpo de la solicitud.  

| Elemento | Obligatorio | Descripción |
|---------|----------|-------------|
| **autenticación** (elemento principal) | El objeto de autenticación para usar un certificado de cliente SSL |
| **type** | Sí | El tipo de autenticación. Para los certificados de cliente SSL, el valor es `ClientCertificate`. |
| **pfx** | Sí | El contenido con codificación base64 del archivo PFX |
| **password** | Sí | La contraseña para acceder al archivo PFX |
||| 

### <a name="response-body---client-certificate"></a>Cuerpo de respuesta: Certificado de cliente 

Cuando se envía una solicitud con información de autenticación, la respuesta contiene estos elementos de autenticación.

| Elemento | Descripción | 
|---------|-------------| 
| **autenticación** (elemento principal) | El objeto de autenticación para usar un certificado de cliente SSL |
| **type** | El tipo de autenticación. Para los certificados de cliente SSL, el valor es `ClientCertificate`. |
| **certificateThumbprint** |La huella digital del certificado |
| **certificateSubjectName** |El nombre distintivo del sujeto del certificado |
| **certificateExpiration** | La fecha de expiración del certificado |
||| 

### <a name="sample-rest-request---client-certificate"></a>Solicitud de REST de ejemplo - Certificado de cliente

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "clientcertificate",
          "password": "password",
          "pfx": "pfx key"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled"
  }
}
```

### <a name="sample-rest-response---client-certificate"></a>Respuesta de REST de ejemplo - Certificado de cliente

```json
HTTP/1.1 200 OKCache-Control: no-cache
Pragma: no-cache
Content-Length: 858
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 56c7b40e-721a-437e-88e6-f68562a73aa8
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 1075219e-e879-4030-bc81-094e54fbabce
x-ms-routing-request-id: WESTUS:20160316T190424Z:1075219e-e879-4030-bc81-094e54fbabce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:04:23 GMT

{
  "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
  "type": "Microsoft.Scheduler/jobCollections/jobs",
  "name": "southeastasiajc/httpjob",
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "certificateThumbprint": "88105CG9DF9ADE75B835711D899296CB217D7055",
          "certificateExpiration": "2021-01-01T07:00:00Z",
          "certificateSubjectName": "CN=Scheduler Mgmt",
          "type": "ClientCertificate"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled",
    "status": {
      "nextExecutionTime": "2016-03-16T19:05:00Z",
      "executionCount": 0,
      "failureCount": 0,
      "faultedCount": 0
    }
  }
}
```

## <a name="basic"></a>Básica

### <a name="request-body---basic"></a>Cuerpo de la solicitud - Básico

Al agregar autenticación mediante el modelo `Basic`, especifique estos elementos adicionales en el cuerpo de la solicitud.

| Elemento | Obligatorio | Descripción |
|---------|----------|-------------|
| **autenticación** (elemento principal) | El objeto de autenticación para usar la autenticación básica | 
| **type** | Sí | El tipo de autenticación. En autenticación básica, el valor es `Basic`. | 
| **username** | Sí | El nombre de usuario que se autentica | 
| **password** | Sí | La contraseña que se autentica |
|||| 

### <a name="response-body---basic"></a>Cuerpo de respuesta: Básico

Cuando se envía una solicitud con información de autenticación, la respuesta contiene estos elementos de autenticación.

| Elemento | Descripción | 
|---------|-------------|
| **autenticación** (elemento principal) | El objeto de autenticación para usar la autenticación básica |
| **type** | El tipo de autenticación. En el caso de la autenticación básica, el valor es `Basic`. |
| **username** | El nombre del usuario autenticado |
||| 

### <a name="sample-rest-request---basic"></a>Solicitud de REST de ejemplo - Básico

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 562
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "type": "basic",
          "username": "user",
          "password": "password"
        }
      },
      "type": "http"
    },
    "recurrence": {
      "frequency": "minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "enabled"
  }
}
```

### <a name="sample-rest-response---basic"></a>Respuesta de REST de ejemplo - Básico

```json
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 701
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: a2dcb9cd-1aea-4887-8893-d81273a8cf04
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 7816f222-6ea7-468d-b919-e6ddebbd7e95
x-ms-routing-request-id: WESTUS:20160316T190506Z:7816f222-6ea7-468d-b919-e6ddebbd7e95
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:05:06 GMT

{  
   "id":"/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type":"Microsoft.Scheduler/jobCollections/jobs",
   "name":"southeastasiajc/httpjob",
   "properties":{  
      "startTime":"2015-05-14T14:10:00Z",
      "action":{  
         "request":{  
            "uri":"https://mywebserviceendpoint.com",
            "method":"GET",
            "headers":{  
               "x-ms-version":"2013-03-01"
            },
            "authentication":{  
               "username":"user1",
               "type":"Basic"
            }
         },
         "type":"Http"
      },
      "recurrence":{  
         "frequency":"Minute",
         "endTime":"2016-04-10T08:00:00Z",
         "interval":1
      },
      "state":"Enabled",
      "status":{  
         "nextExecutionTime":"2016-03-16T19:06:00Z",
         "executionCount":0,
         "failureCount":0,
         "faultedCount":0
      }
   }
}
```

## <a name="active-directory-oauth"></a>Active Directory OAuth

### <a name="request-body---active-directory-oauth"></a>Cuerpo de la solicitud - Active Directory OAuth 

Al agregar autenticación mediante el modelo `ActiveDirectoryOAuth`, especifique estos elementos adicionales en el cuerpo de la solicitud.

| Elemento | Obligatorio | Descripción |
|---------|----------|-------------|
| **autenticación** (elemento principal) | Sí | El objeto de autenticación para usar la autenticación de ActiveDirectoryOAuth |
| **type** | Sí | El tipo de autenticación. En autenticación ActiveDirectoryOAuth, el valor es `ActiveDirectoryOAuth`. |
| **tenant** | Sí | Identificador del inquilino de Azure AD. Para encontrar el identificador del inquilino de Azure AD, ejecute `Get-AzureAccount` en Azure PowerShell. |
| **audience** | Sí | Este valor se establece en `https://management.core.windows.net/`. | 
| **clientId** | Sí | Identificador de cliente para la aplicación de Azure AD | 
| **secret** | Sí | El secreto del cliente que solicita el token | 
|||| 

### <a name="response-body---active-directory-oauth"></a>Cuerpo de respuesta: Active Directory OAuth

Cuando se envía una solicitud con información de autenticación, la respuesta contiene estos elementos de autenticación.

| Elemento | Descripción |
|---------|-------------|
| **autenticación** (elemento principal) | El objeto de autenticación para usar la autenticación de ActiveDirectoryOAuth |
| **type** | El tipo de autenticación. En autenticación ActiveDirectoryOAuth, el valor es `ActiveDirectoryOAuth`. | 
| **tenant** | Identificador del inquilino de Azure AD. |
| **audience** | Este valor se establece en `https://management.core.windows.net/`. |
| **clientId** | Identificador de cliente para la aplicación de Azure AD |
||| 

### <a name="sample-rest-request---active-directory-oauth"></a>Solicitud de REST de ejemplo - Active Directory OAuth

```json
PUT https://management.azure.com/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobcollections/southeastasiajc/jobs/httpjob?api-version=2016-01-01 HTTP/1.1
User-Agent: Fiddler
Host: management.azure.com
Authorization: Bearer sometoken
Content-Length: 757
Content-Type: application/json; charset=utf-8

{
  "properties": {
    "startTime": "2015-05-14T14:10:00Z",
    "action": {
      "request": {
        "uri": "https://mywebserviceendpoint.com",
        "method": "GET",
        "headers": {
          "x-ms-version": "2013-03-01"
        },
        "authentication": {
          "tenant":"microsoft.onmicrosoft.com",
          "audience":"https://management.core.windows.net/",
          "clientId":"dc23e764-9be6-4a33-9b9a-c46e36f0c137",
          "secret": "G6u071r8Gjw4V4KSibnb+VK4+tX399hkHaj7LOyHuj5=",
          "type":"ActiveDirectoryOAuth"
        }
      },
      "type": "Http"
    },
    "recurrence": {
      "frequency": "Minute",
      "endTime": "2016-04-10T08:00:00Z",
      "interval": 1
    },
    "state": "Enabled"
  }
}
```

### <a name="sample-rest-response---active-directory-oauth"></a>Respuesta de REST de ejemplo - Active Directory OAuth

```json
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 885
Content-Type: application/json; charset=utf-8
Expires: -1
x-ms-request-id: 86d8e9fd-ac0d-4bed-9420-9baba1af3251
Server: Microsoft-IIS/8.5
X-AspNet-Version: 4.0.30319
X-Powered-By: ASP.NET
x-ms-ratelimit-remaining-subscription-resource-requests: 599
x-ms-correlation-request-id: 5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
x-ms-routing-request-id: WESTUS:20160316T191003Z:5183bbf4-9fa1-44bb-98c6-6872e3f2e7ce
Strict-Transport-Security: max-age=31536000; includeSubDomains
Date: Wed, 16 Mar 2016 19:10:02 GMT

{
   "id": "/subscriptions/<Azure-subscription-ID>/resourceGroups/CS-SoutheastAsia-scheduler/providers/Microsoft.Scheduler/jobCollections/southeastasiajc/jobs/httpjob",
   "type": "Microsoft.Scheduler/jobCollections/jobs",
   "name": "southeastasiajc/httpjob",
   "properties": {
      "startTime": "2015-05-14T14:10:00Z",
      "action": {  
         "request": {
            "uri": "https://mywebserviceendpoint.com",
            "method": "GET",
            "headers": {  
               "x-ms-version": "2013-03-01"
            },
            "authentication": {  
               "tenant": "microsoft.onmicrosoft.com",
               "audience": "https://management.core.windows.net/",
               "clientId": "dc23e764-9be6-4a33-9b9a-c46e36f0c137",
               "type": "ActiveDirectoryOAuth"
            }
         },
         "type": "Http"
      },
      "recurrence": {  
         "frequency": "minute",
         "endTime": "2016-04-10T08:00:00Z",
         "interval": 1
      },
      "state": "Enabled",
      "status": {  
         "lastExecutionTime": "2016-03-16T19:10:00.3762123Z",
         "nextExecutionTime": "2016-03-16T19:11:00Z",
         "executionCount": 5,
         "failureCount": 5,
         "faultedCount": 1
      }
   }
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Conceptos, terminología y jerarquía de entidades de Azure Scheduler](scheduler-concepts-terms.md)
* [Límites, valores predeterminados y códigos de error de Azure Scheduler](scheduler-limits-defaults-errors.md)
* [Referencia de API de REST de Azure Scheduler](/rest/api/scheduler)
* [Referencia de cmdlets de PowerShell de Azure Scheduler](scheduler-powershell-reference.md)