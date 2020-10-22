---
title: Supervisión de Virtual WAN con conclusiones de Azure Monitor
description: En este artículo, obtendrá información sobre la supervisión de Azure Virtual WAN mediante conclusiones de Azure Monitor.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 052fd0ea7619d566e78806580ee7b39e49cc85d2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91448607"
---
# <a name="azure-monitor-insights-for-virtual-wan-preview"></a>Azure Monitor Insights para Virtual WAN (versión preliminar)

Las [conclusiones de Azure Monitor](../azure-monitor/insights/network-insights-overview.md) para Azure Virtual WAN ofrecen a los usuarios y operadores la posibilidad de ver el estado de una red WAN virtual, presentado mediante un mapa topológico de detección automática. El estado de los recursos se superpone en el mapa para proporcionar una vista de instantánea del estado general de la red WAN virtual. Puede ir a los recursos que aparecen en el mapa mediante el acceso con un clic a las páginas de configuración de recursos del portal de Virtual WAN.

Las métricas de nivel de recurso de Virtual WAN se recopilan y presentan mediante un libro de métricas de Virtual WAN empaquetado previamente. El libro muestra las métricas en los niveles de red WAN virtual, concentrador, puerta de enlace y conexión. Este artículo le guiará por los pasos para usar Azure Monitor Insights para Virtual WAN, a fin de ver la topología de Virtual WAN y las métricas en un solo lugar.

> [!NOTE]
> La opción de menú **Conclusiones** del portal de Virtual WAN está en proceso de implementación. Mientras se implementa este menú, puede acceder a la topología de Virtual WAN y al libro de métricas mediante Azure Monitor para redes. Para más información, consulte [Azure Monitor para redes](../azure-monitor/insights/network-insights-overview.md). 
>

## <a name="before-you-begin"></a>Antes de empezar

Para completar los pasos de este artículo, debe tener una red WAN virtual con uno o más concentradores. Para crear una red WAN virtual y un concentrador, siga los pasos descritos en estos artículos:

* [Creación de una instancia de Virtual WAN](virtual-wan-site-to-site-portal.md#openvwan)
* [Creación de un centro de conectividad](virtual-wan-site-to-site-portal.md#hub)

## <a name="view-vwan-topology"></a><a name="topology"></a>Visualización de la topología de VWAN

Vaya a **Azure Portal** > **Virtual WAN**. En el menú **Supervisión** del panel izquierdo, seleccione **Conclusiones (versión preliminar)** . Aparece la vista **Conclusiones**. Muestra el mapa de dependencias de Virtual WAN y el mini libro de **métricas** de alto nivel.

**Figura 1: menú Supervisión > Conclusiones**

:::image type="content" source="./media/azure-monitor-insights/monitor-menu.png" alt-text="Captura de pantalla que muestra la vista Conclusiones (versión preliminar)." lightbox="./media/azure-monitor-insights/monitor-menu.png":::

En la vista **Conclusiones** puede ver los recursos de Virtual WAN detectados automáticamente. Estos recursos incluyen concentradores, puertas de enlace, firewalls, conexiones y redes virtuales de los radios, NVA de terceros y ramas de una red WAN virtual de un extremo a otro. Para ver un ejemplo, consulte la **Figura 2**.

El estado de los recursos y el estado general están codificados por colores y superpuestos en los iconos de los recursos del mapa. Las métricas de Virtual WAN de alto nivel, como la capacidad del concentrador y el uso de la puerta de enlace, aparecen en el lado derecho de la ventana en un mini libro.

**Figura 2: vista Insights**

:::image type="content" source="./media/azure-monitor-insights/insights-view.png" alt-text="Captura de pantalla que muestra la vista Conclusiones (versión preliminar)." lightbox="./media/azure-monitor-insights/insights-view.png":::

## <a name="dependency-view"></a><a name="dependency"></a>Vista de dependencias

La vista **Dependencias** de Virtual WAN ayuda a visualizar la vista interconectada de todos los recursos de Virtual WAN organizados principalmente en una arquitectura de concentrador y radio.

**Figura 3: vista de dependencias de VWAN**

:::image type="content" source="./media/azure-monitor-insights/dependency-map.png" alt-text="Captura de pantalla que muestra la vista Conclusiones (versión preliminar)." lightbox="./media/azure-monitor-insights/dependency-map.png":::

En el mapa de la vista **Dependencias** se muestran los siguientes recursos como un gráfico conectado:

* Centros de Virtual WAN en varias regiones de Azure.
* Redes virtuales radiales que están conectadas directamente al concentrador.
* Sitios de ramas de VPN y Azure ExpressRoute y usuarios de P2S que están conectados a cada concentrador mediante sus respectivas conexiones de ExpressRoute, S2S y P2S y puertas de enlace de red virtual.
* Firewalls de Azure (incluidos los servidores proxy de firewall de terceros) implementados en un concentrador (concentrador protegido).
* NVA de terceros (aplicaciones virtuales de red) que se han implementado en las redes virtuales radiales.

El mapa de dependencias también muestra las redes virtuales conectadas indirectamente (redes virtuales que están emparejadas con redes virtuales radiales de Virtual WAN).

El mapa de dependencias facilita la exploración de las opciones de configuración de cada recurso. Por ejemplo, puede mantener el ratón sobre el recurso del concentrador para ver la configuración básica del recurso, como la región y el prefijo del concentrador. Haga clic con el botón derecho para acceder a la página de Azure Portal del recurso del centro.

**Ilustración 4: Navegar a la información específica del recurso**

:::image type="content" source="./media/azure-monitor-insights/resource-information.png" alt-text="Captura de pantalla que muestra la vista Conclusiones (versión preliminar).":::

La barra de búsqueda y filtrado de la vista **Dependencias** ofrece una forma sencilla de buscar en el gráfico. Los diferentes filtros proporcionan ayuda para restringir la búsqueda a una ruta y un estado específicos.

**Ilustración 5: Búsqueda y filtrado**

:::image type="content" source="./media/azure-monitor-insights/search-filter.png" alt-text="Captura de pantalla que muestra la vista Conclusiones (versión preliminar)." lightbox="./media/azure-monitor-insights/search-filter.png":::

## <a name="detailed-metrics"></a><a name="detailed"></a>Métricas detalladas

Puede seleccionar **Ver métricas detalladas** para acceder a la página de **Métricas** detalladas. La página **Métricas** es un panel que está preconfigurado con pestañas independientes. Estas pestañas proporcionan información sobre la capacidad, el rendimiento y la utilización de los recursos de la red WAN virtual en el nivel de red WAN virtual, de concentrador y de conexiones individuales.

**Ilustración 6: Panel de métricas detalladas**

:::image type="content" source="./media/azure-monitor-insights/detailed-metrics.png" alt-text="Captura de pantalla que muestra la vista Conclusiones (versión preliminar)." lightbox="./media/azure-monitor-insights/detailed-metrics.png":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información, consulte [Métricas en Azure Monitor](../azure-monitor/platform/data-platform-metrics.md).
* Para obtener una descripción completa de todas las métricas de Virtual WAN, vea [Registros y métricas de Azure Virtual WAN](logs-metrics.md).
