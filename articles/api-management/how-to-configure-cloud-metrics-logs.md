---
title: Configuración de los registros y las métricas en la nube para la puerta de enlace autohospedada de Azure API Management | Microsoft Docs
description: Aprenda a configurar los registros y las métricas en la nube para la puerta de enlace autohospedada de Azure API Management.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 04/30/2020
ms.author: apimpm
ms.openlocfilehash: 3bbfd167e89ae1b5f9b7de1df5fd1cb72c720cb6
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2020
ms.locfileid: "86254541"
---
# <a name="configure-cloud-metrics-and-logs-for-azure-api-management-self-hosted-gateway"></a>Configuración de los registros y las métricas en la nube para la puerta de enlace autohospedada de Azure API Management

En este artículo se proporcionan detalles para configurar los registros y las métricas en la nube para la [puerta de enlace autohospedada](./self-hosted-gateway-overview.md).

La puerta de enlace autohospedada debe estar asociada a un servicio API Management y requiere conectividad TCP/IP saliente a Azure en el puerto 443. La puerta de enlace aprovecha la conexión de salida para enviar telemetría a Azure, si se configura para ello. 

## <a name="metrics"></a>Métricas
De forma predeterminada, la puerta de enlace autohospedada emite un número de métricas a través de [Azure Monitor](https://azure.microsoft.com/services/monitor/), igual que la puerta de enlace administrada [en la nube](api-management-howto-use-azure-monitor.md). 

La característica se puede habilitar o deshabilitar con la clave `telemetry.metrics.cloud` en ConfigMap de la implementación de puerta de enlace. A continuación se muestra un desglose de las configuraciones disponibles:

| Campo  | Valor predeterminado | Descripción |
| ------------- | ------------- | ------------- |
| telemetry.metrics.cloud  | `true` | Habilita el registro mediante Azure Monitor. El valor puede ser `true` o `false`. |


A continuación se muestra una configuración de ejemplo:

```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
        name: contoso-gateway-environment
    data:
        config.service.endpoint: "<contoso-gateway-management-endpoint>"
        telemetry.metrics.cloud: "true"
```

La puerta de enlace autohospedada actualmente emite las siguientes métricas a través de Azure Monitor:

| Métrica  | Descripción |
| ------------- | ------------- |
| Requests  | Número de solicitudes de API en el período |
| Duración de las solicitudes de puerta de enlace | Número de milisegundos transcurridos desde el momento en que la puerta de enlace recibió la solicitud hasta que se envió toda la respuesta |
| Duración de las solicitudes de back-end | Número de milisegundos empleados en la E/S global de back-end (conexión, envío y recepción de bytes)  |

## <a name="logs"></a>Registros

Actualmente, la puerta de enlace autohospedada no envía [registros de diagnóstico](./api-management-howto-use-azure-monitor.md#activity-logs) a la nube. Sin embargo, es posible [configurar y conservar los registros localmente](how-to-configure-local-metrics-logs.md) donde se implementa la puerta de enlace autohospedada. 

Si una puerta de enlace está implementada en [Azure Kubernetes Service](https://azure.microsoft.com/services/kubernetes-service/), puede habilitar [Azure Monitor para contenedores](../azure-monitor/insights/container-insights-overview.md) para recopilar registros de los contenedores y verlos en Log Analytics. 


## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la puerta de enlace autohospedada, consulte [Introducción a la puerta de enlace autohospedada de Azure API Management](self-hosted-gateway-overview.md)
* Más información sobre la [configuración y conservación de registros localmente](how-to-configure-local-metrics-logs.md)
