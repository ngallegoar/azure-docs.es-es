---
title: Supervisión y registro del firewall de aplicaciones web de Azure
description: Información sobre la supervisión y el registro del firewall de aplicaciones web (WAF) con FrontDoor
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 06/09/2020
ms.author: victorh
ms.openlocfilehash: 596374d4f3f188e08a10bd25b36b178cc79a6e57
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "84808950"
---
# <a name="azure-web-application-firewall-monitoring-and-logging"></a>Supervisión y registro del firewall de aplicaciones web de Azure

La supervisión y el registro del firewall de aplicaciones web de Azure (WAF) se proporcionan mediante el registro y la integración con Azure Monitor y con los registros de Azure Monitor.

## <a name="azure-monitor"></a>Azure Monitor

WAF con el registro de FrontDoor se integra con [Azure Monitor](../../azure-monitor/overview.md). Azure Monitor permite realizar un seguimiento de la información de diagnóstico incluidos los registros y las alertas de WAF. Puede configurar la supervisión de WAF dentro del recurso de Front Door en el portal, en la pestaña **Diagnóstico** o mediante el servicio Azure Monitor directamente.

Desde Azure Portal, vaya al tipo de recurso Front Door. En la pestaña **Supervisión**/**Métricas** a la izquierda, puede agregar **WebApplicationFirewallRequestCount** para realizar un seguimiento del número de solicitudes que coinciden con las reglas de WAF. Se pueden crear filtros personalizados en función de los tipos de acción y los nombres de las reglas.

:::image type="content" source="../media/waf-frontdoor-monitor/waf-frontdoor-metrics.png" alt-text="WAFMetrics ":::

## <a name="logs-and-diagnostics"></a>Registros y diagnósticos

WAF con Front Door ofrece informes detallados sobre cada amenaza que detecta. El registro se integra con los registros de Diagnósticos de Azure y las alertas se registran en formato json. Estos registros pueden integrarse con los [registros de Azure Monitor](../../azure-monitor/insights/azure-networking-analytics.md).

![WAFDiag](../media/waf-frontdoor-monitor/waf-frontdoor-diagnostics.png)

[FrontdoorAccessLog](../../frontdoor/front-door-diagnostics.md) registra todas las solicitudes. FrontdoorWebApplicationFirewallLog registra cualquier solicitud que coincida con una regla de WAF y que tenga el siguiente esquema:

| Propiedad  | Descripción |
| ------------- | ------------- |
|Acción|Acción realizada en la solicitud|
| ClientIp | Dirección IP del cliente que realizó la solicitud. Si hubiera un encabezado X-Forwarded-For en la solicitud, la dirección IP del cliente se seleccionaría del mismo campo de encabezado. |
| ClientPort | Puerto IP del cliente que realizó la solicitud. |
| Detalles|Detalles adicionales sobre la solicitud coincidente |
|| matchVariableName: el nombre del parámetro http de la solicitud coincidente, por ejemplo, los nombres de encabezado|
|| matchVariableValue: valores que desencadenaron la coincidencia|
| Host | El encabezado de host de la solicitud coincidente |
| Directiva | El nombre de la directiva WAF que coincidió con la solicitud. |
| PolicyMode | Modo de operaciones de la directiva WAF. Los valores posibles son: "Prevention" y "Detection" |
| RequestUri | URI completo de la solicitud coincidente. |
| RuleName | El nombre de la regla de WAF que coincidió con la solicitud. |
| SocketIp | La dirección IP de origen que WAF detectó. Esta dirección IP se basa en la sesión de TCP, independientemente de los encabezados de solicitud.|
| TrackingReference | La cadena de referencia exclusiva que identifica una solicitud atendida por Front Door, que también se envía como encabezado X-Azure-Ref al cliente. Se requiere para buscar los detalles en los registros de acceso para una solicitud específica. |

La siguiente consulta de ejemplo devuelve los registros de WAF en las solicitudes bloqueadas:

``` WAFlogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

```

Este es un ejemplo de una solicitud registrada en el registro de WAF:

``` WAFlogQuerySample
{
    "time":  "2020-06-09T22:32:17.8376810Z",
    "category": "FrontdoorWebApplicationFirewallLog",
    "operationName": "Microsoft.Network/FrontDoorWebApplicationFirewallLog/Write",
    "properties":
    {
        "clientIP":"xxx.xxx.xxx.xxx",
        "clientPort":"52097",
        "socketIP":"xxx.xxx.xxx.xxx",
        "requestUri":"https://wafdemofrontdoorwebapp.azurefd.net:443/?q=%27%20or%201=1",
        "ruleName":"Microsoft_DefaultRuleSet-1.1-SQLI-942100",
        "policy":"WafDemoCustomPolicy",
        "action":"Block",
        "host":"wafdemofrontdoorwebapp.azurefd.net",
        "trackingReference":"08Q3gXgAAAAAe0s71BET/QYwmqtpHO7uAU0pDRURHRTA1MDgANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
        "policyMode":"prevention",
        "details":
            {
            "matches":
                [{
                "matchVariableName":"QueryParamValue:q",
                "matchVariableValue":"' or 1=1"
                }]
            }
     }
}
```

La siguiente consulta de ejemplo devuelve las entradas de AccessLogs:

``` AccessLogQuery
AzureDiagnostics
| where ResourceType == "FRONTDOORS" and Category == "FrontdoorAccessLog"

```

Este es un ejemplo de una solicitud registrada en el registro de acceso:

``` AccessLogSample
{
"time": "2020-06-09T22:32:17.8383427Z",
"category": "FrontdoorAccessLog",
"operationName": "Microsoft.Network/FrontDoor/AccessLog/Write",
 "properties":
    {
    "trackingReference":"08Q3gXgAAAAAe0s71BET/QYwmqtpHO7uAU0pDRURHRTA1MDgANjMxNTAwZDAtOTRiNS00YzIwLTljY2YtNjFhNzMyOWQyYTgy",
    "httpMethod":"GET",
    "httpVersion":"2.0",
    "requestUri":"https://wafdemofrontdoorwebapp.azurefd.net:443/?q=%27%20or%201=1",
    "requestBytes":"715",
    "responseBytes":"380",
    "userAgent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4157.0 Safari/537.36 Edg/85.0.531.1",
    "clientIp":"xxx.xxx.xxx.xxx",
    "socketIp":"xxx.xxx.xxx.xxx",
    "clientPort":"52097",
    "timeTaken":"0.003",
    "securityProtocol":"TLS 1.2",
    "routingRuleName":"WAFdemoWebAppRouting",
    "rulesEngineMatchNames":[],
    "backendHostname":"wafdemowebappuscentral.azurewebsites.net:443",
    "sentToOriginShield":false,
    "httpStatusCode":"403",
    "httpStatusDetails":"403",
    "pop":"SJC",
    "cacheStatus":"CONFIG_NOCACHE"
    }
}

```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [Front Door](../../frontdoor/front-door-overview.md).
