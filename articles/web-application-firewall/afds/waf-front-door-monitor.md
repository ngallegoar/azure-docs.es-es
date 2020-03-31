---
title: Supervisión y registro del firewall de aplicaciones web de Azure
description: Información sobre la supervisión y el registro del firewall de aplicaciones web (WAF) con FrontDoor
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 4488fadf5db3b32049b5dce4bbee1fa76c320e96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80284150"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Supervisión y registro del firewall de aplicaciones web de Azure 

La supervisión y el registro del firewall de aplicaciones web de Azure (WAF) se proporcionan mediante el registro y la integración con Azure Monitor y con los registros de Azure Monitor.

## <a name="azure-monitor"></a>Azure Monitor

WAF con el registro de FrontDoor se integra con [Azure Monitor](../../azure-monitor/overview.md). Azure Monitor permite realizar un seguimiento de la información de diagnóstico incluidos los registros y las alertas de WAF. Puede configurar la supervisión de WAF dentro del recurso de Front Door en el portal, en la pestaña **Diagnóstico** o mediante el servicio Azure Monitor directamente.

Desde Azure Portal, vaya al tipo de recurso Front Door. En la pestaña **Supervisión**/**Métricas** a la izquierda, puede agregar **WebApplicationFirewallRequestCount** para realizar un seguimiento del número de solicitudes que coinciden con las reglas de WAF. Se pueden crear filtros personalizados en función de los tipos de acción y los nombres de las reglas.

![WAFMetrics](../media/waf-frontdoor-monitor/waf-frontdoor-metrics.png)

## <a name="logs-and-diagnostics"></a>Registros y diagnósticos

WAF con Front Door ofrece informes detallados sobre cada amenaza que detecta. El registro se integra con los registros de Diagnósticos de Azure y las alertas se registran en formato json. Estos registros pueden integrarse con los [registros de Azure Monitor](../../azure-monitor/insights/azure-networking-analytics.md).

![WAFDiag](../media/waf-frontdoor-monitor/waf-frontdoor-diagnostics.png)

FrontdoorAccessLog registra todas las solicitudes que se reenvían a los back-ends del cliente. FrontdoorWebApplicationFirewallLog registra cualquier solicitud que coincida con una regla de WAF.

La siguiente consulta de ejemplo obtiene los registros de WAF en las solicitudes bloqueadas:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_name_s == "Block"

```

Este es un ejemplo de una solicitud registrada en el registro de WAF:

``` WAFlogQuerySample
{
    "PreciseTimeStamp": "2020-01-25T00:11:19.3866091Z",
    "time": "2020-01-25T00:11:19.3866091Z",
    "category": "FrontdoorWebApplicationFirewallLog",
    "operationName": "Microsoft.Network/FrontDoor/WebApplicationFirewallLog/Write",
    "properties": {
        "clientIP": "xx.xx.xxx.xxx",
        "socketIP": "xx.xx.xxx.xxx",
        "requestUri": "https://wafdemofrontdoorwebapp.azurefd.net:443/?q=../../x",
        "ruleName": "Microsoft_DefaultRuleSet-1.1-LFI-930100",
        "policy": "WafDemoCustomPolicy",
        "action": "Block",
        "host": "wafdemofrontdoorwebapp.azurefd.net",
        "refString": "0p4crXgAAAABgMq5aIpu0T6AUfCYOroltV1NURURHRTA2MTMANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "policyMode": "prevention"
    }
}

``` 

La siguiente consulta de ejemplo obtiene las entradas de AccessLogs:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"

```

Este es un ejemplo de una solicitud registrada en el registro de acceso:

``` AccessLogSample
{
    "PreciseTimeStamp": "2020-01-25T00:11:12.0160150Z",
    "time": "2020-01-25T00:11:12.0160150Z",
    "category": "FrontdoorAccessLog",
    "operationName": "Microsoft.Network/FrontDoor/AccessLog/Write",
    "properties": {
        "trackingReference": "0n4crXgAAAACnRKbdALbyToAqNfSHssDvV1NURURHRTA2MTMANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "httpMethod": "GET",
        "httpVersion": "2.0",
        "requestUri": "https://wafdemofrontdoorwebapp.azurefd.net:443/",
        "requestBytes": "710",
        "responseBytes": "3116",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4017.0 Safari/537.36 Edg/81.0.389.2",
        "clientIp": "xx.xx.xxx.xxx",
        "timeTaken": "0.598",
        "securityProtocol": "TLS 1.2",
        "routingRuleName": "WAFdemoWebAppRouting",
        "backendHostname": "wafdemouksouth.azurewebsites.net:443",
        "sentToOriginShield": false,
        "httpStatusCode": "200",
        "httpStatusDetails": "200"
    }
}

```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Front Door](../../frontdoor/front-door-overview.md).
