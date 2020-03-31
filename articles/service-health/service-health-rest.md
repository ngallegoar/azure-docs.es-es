---
title: Obtención de eventos de estado de recursos con la API de REST | Microsoft Docs
description: Use las API de REST de Azure para obtener los eventos de estado de los recursos de Azure.
ms.topic: conceptual
ms.date: 06/06/2017
ms.openlocfilehash: 6964a6c4e85c38d532b12e730a02c4df73be76e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654008"
---
# <a name="get-resource-health-using-the-rest-api"></a>Obtención de eventos de estado de recursos con la API de REST 

En este artículo de ejemplo se muestra cómo recuperar una lista de eventos de estado de los recursos de Azure de su suscripción mediante la [API de REST de Azure](/rest/api/azure/).

La documentación de referencia completa y ejemplos adicionales para la API de REST están disponibles en [Azure Monitor REST reference](/rest/api/monitor) (Referencia de REST de Azure Monitor). 

## <a name="build-the-request"></a>Compilar la solicitud

Use la siguiente solicitud HTTP `GET` para enumerar los eventos de estado de la suscripción que se han producido en el intervalo de tiempo transcurrido entre `2018-05-16` y `2018-06-20`.

```http
https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&%24filter=eventTimestamp%20ge%20'2018-05-16T04%3A36%3A37.6407898Z'%20and%20eventTimestamp%20le%20'2018-06-20T04%3A36%3A37.6407898Z'
```

### <a name="request-headers"></a>Encabezados de solicitud

Los siguientes encabezados son obligatorios: 

|Encabezado de solicitud|Descripción|  
|--------------------|-----------------|  
|*Content-Type:*|Necesario. Establézcalo en `application/json`.|  
|*Authorization:*|Necesario. Establézcalo en un [token de acceso](/rest/api/azure/#authorization-code-grant-interactive-clients) `Bearer` válido. |  

### <a name="uri-parameters"></a>Parámetros del identificador URI

| Nombre | Descripción |
| :--- | :---------- |
| subscriptionId | El id. de suscripción que identifica una suscripción de Azure. Si tiene varias suscripciones, consulte [Trabajo con varias suscripciones](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| api-version | La versión de API que se usará para la solicitud.<br /><br /> En este documento se describe la versión `2015-04-01` de la API que se incluye en la dirección URL anterior.  |
| $filter | La opción de filtrado usada para reducir el conjunto de resultados devueltos. Los patrones disponibles para este parámetro se encuentran [en la referencia de la operación de Registros de actividad](/rest/api/monitor/activitylogs/list#uri-parameters). El ejemplo mostrado captura todos los eventos en un intervalo de tiempo entre el 16-05-2018 y 20-06-2018. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>Cuerpo de la solicitud

No se necesita ningún cuerpo de solicitud para esta operación.

## <a name="handle-the-response"></a>Control de la respuesta

Se devuelve el código de estado 200 y una lista de valores de evento de estado correspondientes al parámetro de filtro, junto con un URI de `nextlink` para recuperar la página de resultados siguiente.

## <a name="example-response"></a>Respuesta de ejemplo 

```json
{
  "value": [
    {
      "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
      "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
      },
      "id": "/subscriptions/{subscription-id}/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
      "resourceGroupName": "MSSupportGroup",
      "resourceProviderName": {
        "value": "microsoft.support",
        "localizedValue": "microsoft.support"
      },
      "operationName": {
        "value": "microsoft.support/supporttickets/write",
        "localizedValue": "microsoft.support/supporttickets/write"
      },
      "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
      },
      "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
      "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
      "level": "Informational"
    }
  ],
  "nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```
