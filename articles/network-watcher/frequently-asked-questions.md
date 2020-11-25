---
title: Preguntas más frecuentes (P+F) acerca de Azure Network Watcher | Microsoft Docs
description: Este artículo contiene respuestas a algunas preguntas frecuentes sobre el servicio Azure Network Watcher.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2019
ms.author: damendo
ms.openlocfilehash: 959062d493d9eb47204be2488f216b70804b3605
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2020
ms.locfileid: "94965772"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>Preguntas más frecuentes (P+F) sobre Azure Network Watcher
El servicio [Azure Network Watcher](./network-watcher-monitoring-overview.md) proporciona un conjunto de herramientas para supervisar, diagnosticar, ver las métricas y habilitar o deshabilitar registros de recursos en una red virtual de Azure. En este artículo se responde a preguntas habituales sobre el servicio.

## <a name="general"></a>General

### <a name="what-is-network-watcher"></a>¿Qué es Network Watcher?
Network Watcher está diseñado para supervisar y reparar el estado de la red de los componentes de IaaS (infraestructura como servicio), que incluye máquinas virtuales, redes virtuales, puertas de enlace de aplicaciones, equilibradores de carga y otros recursos de una red virtual de Azure. No es una solución para supervisar la infraestructura de PaaS (plataforma como servicio) u obtener análisis web/móviles.

### <a name="what-tools-does-network-watcher-provide"></a>¿Qué herramientas proporciona Network Watcher?
Network Watcher proporciona tres conjuntos principales de funcionalidades
* Supervisión
  * [Vista de la topología](./view-network-topology.md) muestra los recursos de la red virtual y las relaciones entre ellos.
  * [Monitor de conexión](./connection-monitor.md) permite supervisar la conectividad y la latencia entre una máquina virtual y otro recurso de red.
  * [Network Performance Monitor](../azure-monitor/insights/network-performance-monitor.md) permite supervisar la conectividad y las latencias en las arquitecturas de red híbrida, los circuitos de Express Route y los puntos de conexión de servicio/aplicación.  
* Diagnóstico
  * [Comprobación del flujo de IP](./network-watcher-ip-flow-verify-overview.md) permite detectar problemas de filtrado del tráfico a nivel de máquina virtual.
  * [Próximo salto](./network-watcher-next-hop-overview.md) ayuda a comprobar las rutas de tráfico y a detectar problemas de enrutamiento.
  * [Solución de problemas de conexión](./network-watcher-connectivity-portal.md) permite la supervisión puntual de la conectividad y la latencia entre una máquina virtual y otro recurso de red.
  * [Captura de paquetes](./network-watcher-packet-capture-overview.md) permite capturar todo el tráfico de una máquina virtual de la red virtual.
  * [Solución de problemas de VPN](./network-watcher-troubleshoot-overview.md) ejecuta varias comprobaciones de diagnóstico en las puertas de enlace de VPN y las conexiones para ayudar a depurar problemas.
* Registro
  * [Registro de flujo de NSG ](./network-watcher-nsg-flow-logging-overview.md) permite registrar todo el tráfico de los [grupos de seguridad de red](../virtual-network/network-security-groups-overview.md)
  * [Análisis de tráfico](./traffic-analytics.md) procesa los datos del registro de flujo del registro de flujo de NSG para que pueda visualizar, consultar, analizar y comprender el tráfico.


Para información más detallada, consulte la [página de información general de Network Watcher](./network-watcher-monitoring-overview.md).


### <a name="how-does-network-watcher-pricing-work"></a>¿Cómo son los precios de Network Watcher?
Visite la [página de precios](https://azure.microsoft.com/pricing/details/network-watcher/) para conocer los componentes de Network Watcher y sus precios.

### <a name="which-regions-is-network-watcher-supportedavailable-in"></a>¿Qué regiones Network Watcher admiten o están disponibles en?
Puede ver la última disponibilidad regional en la [página de disponibilidad del servicio de Azure](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher)

### <a name="which-permissions-are-needed-to-use-network-watcher"></a>¿Qué permisos se necesitan para usar Network Watcher?
Consulte la lista en [Permisos de control de acceso basado en roles necesarios para usar las funcionalidades de Network Watcher](./required-rbac-permissions.md). Para implementar recursos, necesita permisos de colaborador para NetworkWatcherRG (consulte a continuación).

### <a name="how-do-i-enable-network-watcher"></a>¿Cómo puedo habilitar Network Watcher?
El servicio Network Watcher se [habilita automáticamente](https://azure.microsoft.com/updates/azure-network-watcher-will-be-enabled-by-default-for-subscriptions-containing-virtual-networks/) para cada suscripción.

### <a name="what-is-the-network-watcher-deployment-model"></a>¿Qué es el modelo de implementación de Network Watcher?
El recurso primario de Network Watcher se implementa con una instancia única en todas las regiones. Formato de nombre: NetworkWatcher_NombreDeRegión. Ejemplo: NetworkWatcher_centralus es el recurso de Network Watcher para la región "Central US" (Centro de EE. UU.).

### <a name="what-is-the-networkwatcherrg"></a>¿Qué es NetworkWatcherRG?
Los recursos de Network Watcher se encuentran en el grupo de recursos oculto **NetworkWatcherRG** que se crea automáticamente. Por ejemplo, el recurso Registros de flujo de NSG es un recurso secundario de Network Watcher y está habilitado en NetworkWatcherRG.

### <a name="why-do-i-need-to-install-the-network-watcher-extension"></a>¿Por qué es necesario instalar la extensión Network Watcher? 
La extensión Network Watcher es necesaria para cualquier característica que necesite generar o interceptar el tráfico de una máquina virtual. 

### <a name="which-features-require-the-network-watcher-extension"></a>¿Qué características requiere la extensión Network Watcher?
La características de captura de paquetes, solución de problemas de conexión y monitor de conexión necesitan que la extensión Network Watcher esté presente.

### <a name="what-are-resource-limits-on-network-watcher"></a>¿Cuáles son los límites de recursos en Network Watcher?
Consulte la página de [límites de servicio](../azure-resource-manager/management/azure-subscription-service-limits.md#network-watcher-limits) para conocer todos los límites.  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>¿Por qué solo se permite una instancia Network Watcher por región? 
Network Watcher solo debe habilitarse una vez para que funcionen las características de la suscripción; no se trata de un límite de servicio.

### <a name="how-can-i-manage-the-network-watcher-resource"></a>¿Cómo puedo administrar el recurso de Network Watcher? 
El recurso de Network Watcher representa el servicio back-end para Network Watcher y está totalmente administrado por Azure. Los clientes no tienen que administrarlo. No se admiten operaciones como Move en el recurso. Sin embargo, [se puede eliminar el recurso](./network-watcher-create.md#delete-a-network-watcher-in-the-portal). 

## <a name="service-availability-and-redundancy"></a>Disponibilidad y redundancia del servicio 

### <a name="is-the-network-watcher-service-zone-resilient"></a>¿Es resistente a las zonas el servicio Network Watcher? 
Sí. El servicio Network Watcher es resistente a las zonas de manera predeterminada. 

### <a name="how-do-i-configure-the-network-watcher-service-to-be-zone-resilient"></a>¿Cómo se configura el servicio Network Watcher para que sea resistente a las zonas? 
No es necesaria ninguna configuración de parte del cliente para habilitar la resistencia de zonas. La resistencia de zonas para los recursos de Network Watcher está disponible de manera predeterminada y la administra el propio servicio. 

## <a name="nsg-flow-logs"></a>Registro de flujo de NSG

### <a name="what-does-nsg-flow-logs-do"></a>¿Qué hace Registro de flujo de NSG?
Los recursos de red de Azure se pueden combinar y administrar mediante [grupos de seguridad de red](../virtual-network/network-security-groups-overview.md). Registro de flujo de NSG permite registrar información de flujo de una tupla de 5 sobre todo el tráfico de los grupos de seguridad de red. Los registros de flujo sin procesar se escriben en una cuenta de Azure Storage desde donde se pueden procesar, analizar, consultar o exportar según sea necesario.

### <a name="how-do-i-use-nsg-flow-logs-with-a-storage-account-behind-a-firewall"></a>¿Cómo se usan los registros de flujo de grupo de seguridad de red en una cuenta de Storage con un firewall?

Para usar una cuenta de Storage con un firewall, debe especificar una excepción para que los Servicios de Microsoft de confianza accedan a la cuenta de almacenamiento:

* Para ir a la cuenta de almacenamiento, escriba el nombre de la misma en la búsqueda global del portal o en la [página de cuentas de almacenamiento](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts)
* En la sección **CONFIGURACIÓN**, seleccione **Firewalls y redes virtuales**.
* En "Permitir el acceso desde", seleccione **Redes seleccionadas**. Después, en **Excepciones**, active la casilla situada junto a **"Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento"** 
* Si ya está seleccionada, no hay que hacer ningún cambio.  
* Busque el grupo de seguridad de red de destino en la [página de introducción a los registros de flujo de NSG](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) y habilite los registros de flujo de NSG con la cuenta de almacenamiento anterior seleccionada.

Puede comprobar los registros de almacenamiento después de unos minutos; verá una marca de tiempo actualizada o un nuevo archivo JSON.

### <a name="how-do-i-use-nsg-flow-logs-with-a-storage-account-behind-a-service-endpoint"></a>¿Cómo se usan los registros de flujo de grupo de seguridad de red con una cuenta de Storage detrás de un punto de conexión de servicio?

Los registros de flujo de NSG son compatibles con los puntos de conexión de servicio sin necesidad de ninguna configuración adicional. Consulte [el tutorial de sobre cómo habilitar puntos de conexión de servicio](../virtual-network/tutorial-restrict-network-access-to-resources.md#enable-a-service-endpoint) en su red virtual.


### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>¿Cuál es la diferencia entre la versión 1 y la versión 2 de los registros de flujo?
La versión 2 de los registros de flujo presenta el concepto de *estado del flujo* y almacena información sobre los bytes y los paquetes transmitidos. [Más información](./network-watcher-nsg-flow-logging-overview.md#log-format).

## <a name="next-steps"></a>Pasos siguientes
 - Vaya a la [página de información general de documentación](./index.yml) para algunos tutoriales para empezar a trabajar con Network Watcher.