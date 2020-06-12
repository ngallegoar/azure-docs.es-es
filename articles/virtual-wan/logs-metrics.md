---
title: Selección de ruta de acceso de Azure en varios vínculos de ISP
titleSuffix: Azure Virtual WAN
description: Obtenga información sobre la selección de ruta de acceso de Azure y Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 06/01/2020
ms.author: cherylmc
ms.openlocfilehash: 73249b2c66c7e064aa5c07ae1b78d819f4f44c23
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2020
ms.locfileid: "84302367"
---
# <a name="azure-virtual-wan-logs-and-metrics"></a>Registros y métricas de Azure Virtual WAN

Puede supervisar Azure Virtual WAN mediante Azure Monitor. Virtual WAN es un servicio de red que aporta muchas funciones de red, seguridad y enrutamiento para proporcionar una única interfaz operativa. Las puertas de enlace de VPN de Virtual WAN, las puertas de enlace de ExpressRoute y Azure Firewall tienen registros y métricas disponibles a través de Azure Monitor. Para obtener información de Azure Firewall, consulte [Métricas y registros de Azure Firewall](../firewall/logs-and-metrics.md).

En este artículo se describen las métricas y los diagnósticos que están disponibles a través del portal. Las métricas son ligeras y pueden admitir escenarios casi en tiempo real, lo que las hace útiles para alertas y detección rápida de problemas.

## <a name="metrics"></a>Métricas

Las métricas de Azure Monitor son valores numéricos que describen algunos aspectos de un sistema en un momento dado. Las métricas se recopilan cada minuto y son útiles para las alertas porque se pueden muestrear con frecuencia. Una alerta puede activarse rápidamente con una lógica relativamente simple.

### <a name="site-to-site-vpn-gateways"></a>Puertas de enlace de VPN de sitio a sitio

Las siguientes métricas están disponibles para las puertas de enlace de VPN de sitio a sitio de Azure:

* **Ancho de banda de puerta de enlace**: media de ancho de banda agregado de sitio a sitio de una puerta de enlace en bytes por segundo.
* **Ancho de banda de túnel**: media de ancho de banda de un túnel en bytes por segundo.
* **Bytes de salida de túnel**: bytes de salida de un túnel. 
* **Paquetes de salida de túnel**: recuento de paquetes de salida de un túnel. 
* **Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de salida**: recuento de colocación de paquetes de salida con error de coincidencia del selector de tráfico de un túnel. 
* **Bytes de entrada de un túnel**: bytes de entrada de un túnel. 
* **Paquete de entrada de un túnel**: recuento de paquetes de entrada de un túnel. 
* **Colocación de paquetes con error de coincidencia del selector de tráfico de túnel de entrada**: recuento de colocación de paquetes de entrada con error de coincidencia del selector de tráfico de un túnel. 

### <a name="point-to-site-vpn-gateways"></a>Puertas de enlace de VPN de punto a sitio

Las siguientes métricas están disponibles para puertas de enlace de VPN de punto a sitio de Azure:

* **Ancho de banda de punto a sitio de puerta de enlace**: media de ancho de banda agregado de punto a sitio de una puerta de enlace en bytes por segundo.
* **Recuento de conexiones de punto a sitio**: recuento de conexiones de punto a sitio de una puerta de enlace.

### <a name="azure-expressroute-gateways"></a>Puertas de enlace de Azure ExpressRoute

Las siguientes métricas están disponibles para las puertas de enlace de Azure ExpressRoute:

* **BitsInPerSecond**: bits que ingresan a Azure por segundo.
* **BitsOutPerSecond**: bits que egresan de Azure por segundo.

### <a name="view-gateway-metrics"></a><a name="metrics-steps"></a>Visualización de métricas de puerta de enlace

Los siguientes pasos le ayudarán a ubicar y ver las métricas:

1. En el portal, navegue hasta el centro virtual que tiene la puerta de enlace.

2. Seleccione **VPN (sitio a sitio)** para buscar una puerta de enlace de sitio a sitio, **ExpressRoute** para buscar una puerta de enlace de ExpressRoute o **VPN de usuario (punto a sitio)** para buscar una puerta de enlace de punto a sitio. En la página, puede ver la información de la puerta de enlace. Copie esta información. La usará más adelante para ver los diagnósticos mediante Azure Monitor.

3. Seleccione **Métricas**.

   :::image type="content" source="./media/logs-metrics/metrics.png" alt-text="métricas":::

4. En la página **Métricas**, puede ver las métricas que le interesen.

   :::image type="content" source="./media/logs-metrics/metrics-page.png" alt-text="página Métricas":::

## <a name="diagnostic-logs"></a><a name="diagnostic"></a>Registros de diagnóstico

### <a name="site-to-site-vpn-gateways"></a>Puertas de enlace de VPN de sitio a sitio

Los siguientes diagnósticos están disponibles para las puertas de enlace de VPN de sitio a sitio de Azure:

* **Registros de diagnóstico de puerta de enlace**: diagnósticos específicos de la puerta de enlace, como el estado, la configuración o las actualizaciones del servicio, así como diagnósticos adicionales.
* **Registros de diagnóstico de túnel**: se trata de registros relacionados con el túnel de IPsec como los eventos de conexión y desconexión para un túnel de IPsec de sitio a sitio, SA negociados, motivos de desconexión y diagnósticos adicionales.
* **Registros de diagnóstico de ruta**: se trata de registros relacionados con eventos para rutas estáticas, BGP, actualizaciones de ruta y diagnósticos adicionales.
* **Registros de diagnóstico de IKE**: diagnósticos específicos de IKE para conexiones de IPsec.

### <a name="point-to-site-vpn-gateways"></a>Puertas de enlace de VPN de punto a sitio

Los siguientes diagnósticos están disponibles para puertas de enlace de VPN de punto a sitio de Azure:

* **Registros de diagnóstico de puerta de enlace**: diagnósticos específicos de la puerta de enlace, como el estado, la configuración o las actualizaciones del servicio, así como otros diagnósticos.
* **Registros de diagnóstico de IKE**: diagnósticos específicos de IKE para conexiones de IPsec.
* **Registros de diagnóstico de punto a sitio**: se trata de la configuración de punto a sitio y eventos de cliente de la VPN del usuario. Incluyen la conexión/desconexión del cliente y la asignación de direcciones de cliente de VPN, así como otros diagnósticos.

### <a name="view-diagnostic-logs"></a><a name="diagnostic-steps"></a>Visualización de los registros de diagnóstico

Los siguientes pasos le ayudarán a localizar y ver los diagnósticos:

1. En el portal, navegue hasta el recurso de Virtual WAN. En la sección **Información general** de la página Virtual WAN del portal, seleccione **Información esencial** para expandir la vista y obtener la información del grupo de recursos. Copie la información del grupo de recursos.

   :::image type="content" source="./media/logs-metrics/3.png" alt-text="página Métricas":::

2. En la sección Supervisión, navegue hasta el grupo de recursos. Seleccione **Configuración de diagnóstico** y, a continuación, especifique la información de recursos. Esta es la información de recursos que copió en el paso 2 de la sección [Visualización de métricas de puerta de enlace](#metrics-steps) anteriormente en este artículo.

   :::image type="content" source="./media/logs-metrics/4.png" alt-text="página Métricas":::

3. En la página de resultados, seleccione **+ Agregar configuración de diagnóstico** y, después, seleccione una opción. Puede optar por enviar a Log Analytics, transmitir a un centro de eventos o simplemente archivar en una cuenta de almacenamiento.

   :::image type="content" source="./media/logs-metrics/5.png" alt-text="página Métricas":::

### <a name="log-analytics-sample-query"></a><a name="sample-query"></a>Consulta de ejemplo de Log Analytics

Los registros se encuentran en el **área de trabajo de Azure Log Analytics**. Puede configurar una consulta en Log Analytics. El ejemplo siguiente contiene una consulta para obtener los diagnósticos de ruta de sitio a sitio.

```AzureDiagnostics | where Category == "RouteDiagnosticLog"```

Reemplace los valores siguientes, después de **= =** , según sea necesario.

* "GatewayDiagnosticLog"
* "IKEDiagnosticLog"
* "P2SDiagnosticLog”
* "TunnelDiagnosticLog"
* "RouteDiagnosticLog"

## <a name="activity-logs"></a><a name="activity-logs"></a>Registros de actividad

Las entradas del **registro de actividad** están habilitadas de forma predeterminada y se pueden ver en Azure Portal. Puede usar los registros de actividad de Azure (anteriormente conocidos como *registros operativos* y *registros de auditoría*) para ver todas las operaciones enviadas a su suscripción de Azure.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo supervisar las métricas y los registros de Azure Firewall, consulte [Tutorial: Supervisión de los registros de Azure Firewall](../firewall/tutorial-diagnostics.md).
* Para más información sobre las métricas en Azure Monitor, consulte [Métricas en Azure monitor](../azure-monitor/platform/data-platform-metrics.md).
