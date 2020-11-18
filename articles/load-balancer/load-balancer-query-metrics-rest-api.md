---
title: Recuperación de métricas con la API de REST
titleSuffix: Azure Load Balancer
description: En este artículo, empezará a usar las API de REST de Azure para recopilar métricas de mantenimiento y uso de Azure Load Balancer.
services: sql-database
author: asudbring
manager: KumudD
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: how-to
ms.date: 11/19/2019
ms.author: allensu
ms.openlocfilehash: eeca560989c174bcb654116eb13da40d6ec79e60
ms.sourcegitcommit: e2dc549424fb2c10fcbb92b499b960677d67a8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94700518"
---
# <a name="get-load-balancer-usage-metrics-using-the-rest-api"></a>Obtención de métricas de utilización de Load Balancer con la API de REST

Recopile el número de bytes procesados por una instancia de [Standard Load Balancer](./load-balancer-overview.md) durante un intervalo de tiempo mediante la [API de REST de Azure](/rest/api/azure/).

La documentación de referencia completa y ejemplos adicionales para la API de REST están disponibles en [Azure Monitor REST reference](/rest/api/monitor) (Referencia de REST de Azure Monitor). 

## <a name="build-the-request"></a>Compilar la solicitud

Utilice la siguiente solicitud GET para recopilar la [métrica de ByteCount](./load-balancer-standard-diagnostics.md#multi-dimensional-metrics) desde un servicio Standard Load Balancer. 

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
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
| subscriptionId | El id. de suscripción que identifica una suscripción de Azure. Si tiene varias suscripciones, consulte [Trabajo con varias suscripciones](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| resourceGroupName | Nombre del grupo de recursos que contiene el recurso. Puede obtener este valor en la API de Azure Resource Manager, la CLI o en Azure Portal. |
| loadBalancerName | El nombre de Azure Load Balancer. |
| metric names | Lista separada por comas de [métricas válidas de Load Balancer](./load-balancer-standard-diagnostics.md). |
| api-version | La versión de API que se usará para la solicitud.<br /><br /> En este documento se describe la versión `2018-01-01` de la API que se incluye en la dirección URL anterior.  |
| timespan | El intervalo de tiempo de la consulta. Es una cadena con el siguiente formato `startDateTime_ISO/endDateTime_ISO`. Este parámetro opcional se establece para devolver los datos recopilados durante un día en el ejemplo. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>Cuerpo de la solicitud

No se necesita ningún cuerpo de solicitud para esta operación.

## <a name="handle-the-response"></a>Control de la respuesta

Cuando la lista de valores de métricas se devuelve correctamente, se devuelve el código de estado 200. Una lista completa de códigos de error está disponible en la [documentación de referencia](/rest/api/monitor/metrics/list#errorresponse).

## <a name="example-response"></a>Respuesta de ejemplo 

```json
{
    "cost": 0,
    "timespan": "2018-06-05T03:00:00Z/2018-06-07T03:00:00Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/Microsoft.Insights/metrics/ByteCount",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "ByteCount",
                "localizedValue": "Byte Count"
            },
            "unit": "Count",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-06T17:24:00Z",
                            "total": 1067921034.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:25:00Z",
                            "total": 0.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:26:00Z",
                            "total": 3781344.0
                        },
                    ]
                }
            ]
        }
    ],
    "namespace": "Microsoft.Network/loadBalancers",
    "resourceregion": "eastus"
}
```