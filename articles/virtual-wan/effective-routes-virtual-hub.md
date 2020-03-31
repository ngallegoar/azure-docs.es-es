---
title: 'Vista de las rutas eficaces de un centro de conectividad virtual: Azure Virtual WAN | Microsoft Docs'
description: Vista de las rutas eficaces para un centro de conectividad virtual en Azure Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: cherylmc
ms.openlocfilehash: 1173da81736661048d1e4e12d9919bc2aadf73ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "73511213"
---
# <a name="view-effective-routes-of-a-virtual-hub"></a>Vista de las rutas eficaces de un centro de conectividad virtual

Puede ver todas las rutas del centro de conectividad virtual WAN en Azure Portal. Para ver las rutas, vaya al centro de conectividad virtual y, luego seleccione **Cálculo de ruta -> Ver rutas eficaces**.

## <a name="understanding-routes"></a><a name="understand"></a>Reconocimiento de rutas

El ejemplo siguiente puede ayudarle a reconocer mejor cómo aparece el cálculo de ruta del Virtual WAN.

En este ejemplo, tenemos un virtual WAN con tres centros de conectividad. El primer centro de conectividad está en la región Este de EE. UU., el segundo en la región del Oeste de Europa y el tercero en la región Oeste de EE. UU. En una virtual WAN, todos los centros de conectividad están interconectados. En este ejemplo, se supone que los centros de conectividad del Este de EE. UU. y el Oeste de Europa tienen conexiones de ramas locales (radios) y redes virtuales de Azure (radios).

Un radio de red virtual de Azure (10.4.0.0/16) con una aplicación virtual de red (10.4.0.6) se empareja más a una red virtual (10.5.0.0/16). Vea [información adicional](#abouthubroute) más adelante en este artículo para conocer más acerca de la tabla de rutas del centro de conectividad.

En este ejemplo, también se supone que la rama 1 del Oeste de Europa está conectada al centro de conectividad del Este de EE. UU., así como al centro de conectividad del Oeste de Europa. Un circuito ExpressRoute del Este de EE. UU. conecta la rama 2 al centro de conectividad del Este de EE. UU.

![diagrama](./media/effective-routes-virtual-hub/diagram.png)

## <a name="view-effective-routes"></a><a name="view"></a>Vista de las rutas eficaces

Al seleccionar "Ver rutas eficaces" en el portal, se genera el resultado que se muestra en la [tabla de rutas del centro de conectividad](#routetable) para el Este de EE. UU.

Para ponerlo en perspectiva, la primera línea implica que el centro de conectividad del Este de EE. UU. haya aprendido la ruta de 10.20.1.0/24 (rama 1) debido a la conexión VPN *Tipo del próximo salto* ("Próximo salto" puerta de enlace VPN Instancia0 IP 10.1.0.6, Instancia1 IP 10.1.0.7). *Origen de ruta* señala el identificador de recurso. *La ruta de acceso del sistema autónomo (AS)* indica la ruta del sistema autónomo (AS) para la rama 1.

### <a name="hub-route-table"></a><a name="routetable"></a>Tabla de rutas del centro de conectividad

Use la barra de desplazamiento de la parte inferior de la tabla para ver la "ruta de acceso del sistema autónomo (AS)".

| **Prefijo** |  **Tipo del próximo salto** | **Próximo salto** |  **Origen de la ruta** |**Ruta de acceso del sistema autónomo (AS)** |
| ---        | ---                | ---          | ---               | ---         |
| 10.20.1.0/24|VPN |10.1.0.6, 10.1.0.7| /subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-gw| 20000|
|10.21.1.0/24 |ExpressRoute|10.1.0.10, 10.1.0.11|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/expressRouteGateways/4444a6ac74e4d85555-eastus-gw|21000|
|10.23.1.0/24| VPN |10.1.0.6, 10.1.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-gw|23000|
|10.4.0.0/16|Conexión de Virtual Network| Vínculo en línea |  |  |
|10.5.0.0/16| Dirección IP| 10.4.0.6|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|0.0.0.0/0| Dirección IP| `<Azure Firewall IP>` |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|10.22.1.0/16| Centro de conectividad remoto|10.8.0.6, 10.8.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/westhub_| 4848-22000 |
|10.9.0.0/16| Centro de conectividad remoto|  Vínculo en línea |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/westhub_1| |

>[!NOTE]
> Si los centros de conectividad del Este de EE. UU. y el Oeste de Europa no se comunican entre sí en la topología de ejemplo, la ruta aprendida (10.9.0.0/16) no existirá. Los centros de conectividad solo anuncian las redes que están conectadas directamente a ellos.
>

## <a name="additional-information"></a><a name="additional"></a>Información adicional

### <a name="about-the-hub-route-table"></a><a name="abouthubroute"></a>Sobre las tablas de rutas del centro de conectividad

puede crear una ruta del centro virtual y aplicarla a la tabla de rutas del centro virtual. Puede aplicar varias rutas a la tabla de rutas del concentrador virtual. Esto le permite establecer una ruta para la red virtual destino a través de una dirección IP (normalmente, la aplicación virtual de red (NVA) en una red virtual de radios). Para más información sobre NVA, consulte [Enrutamiento del tráfico desde un centro de conectividad virtual hacia un NVA](virtual-wan-route-table-portal.md).

### <a name="about-default-route-00000"></a><a name="aboutdefaultroute"></a> Sobre la ruta predeterminada (0.0.0.0/0)

Un centro de conectividad virtual tiene la capacidad de propagar una ruta predeterminada aprendida en una red virtual, una VPN de sitio a sitio y una conexión de ExpressRoute si la marca está "habilitada" en la conexión. Esta marca está visible cuando se edita una conexión de red virtual, una conexión VPN o una conexión ExpressRoute. 'EnableInternetSecurity' siempre es falso de forma predeterminada en las conexiones de red virtual del centro de conectividad, ExpressRoute y VPN.

La ruta predeterminada no se origina en el centro de conectividad virtual WAN. La ruta predeterminada se propaga si ya la ha aprendido el centro de conectividad virtual WAN como resultado de la implementación de un firewall en el centro de conectividad o en caso de que otro sitio conectado tenga habilitada la tunelización forzada.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la [Introducción a Virtual WAN](virtual-wan-about.md).