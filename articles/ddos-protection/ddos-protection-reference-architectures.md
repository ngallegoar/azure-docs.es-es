---
title: Arquitecturas de referencia de Azure DDoS Protection
description: Aprenda las arquitecturas de referencia de Azure DDoS Protection.
services: ddos-protection
documentationcenter: na
author: yitoh
ms.service: ddos-protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/08/2020
ms.author: yitoh
ms.openlocfilehash: 3371b9cc0848e387c0150ca9aa7e7a971cecba1a
ms.sourcegitcommit: 693df7d78dfd5393a28bf1508e3e7487e2132293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92904982"
---
# <a name="ddos-protection-reference-architectures"></a>Arquitecturas de referencia de DDoS Protection

DDoS Protection Standard se ha diseñado [para los servicios que se implementan en una red virtual](/azure/virtual-network/virtual-network-for-azure-services). Para otros servicios se aplica el servicio predeterminado DDoS Protection Basic. Las siguientes arquitecturas de referencia se organizan por escenarios, y los patrones de la arquitectura se agrupan juntos.

## <a name="virtual-machine-windowslinux-workloads"></a>Cargas de trabajo de máquina virtual (Windows o Linux)

### <a name="application-running-on-load-balanced-vms"></a>Aplicación que se ejecuta en máquinas virtuales con equilibrio de carga

En esta arquitectura de referencia se muestra un conjunto de prácticas demostradas para ejecutar varias máquinas virtuales con Windows en un conjunto de escalado detrás de un equilibrador de carga, para mejorar la disponibilidad y escalabilidad. Esta arquitectura puede usarse para cargas de trabajo sin estado, como un servidor web.

![Diagrama de la arquitectura de referencia para una aplicación que se ejecuta en máquinas virtuales con equilibrio de carga](./media/ddos-best-practices/image-9.png)

En esta arquitectura, una carga de trabajo se distribuye entre varias instancias de máquina virtual. Hay una única dirección IP pública, y el tráfico de Internet se distribuye a las máquinas virtuales a través de un equilibrador de carga. DDoS Protection Standard está habilitado en la red virtual del equilibrador de carga de Azure (Internet) que tiene asociada la dirección IP pública.

El equilibrador de carga distribuye las solicitudes entrantes de Internet a las instancias de máquina virtual. Los conjuntos de escalado de máquinas virtuales permiten reducir o escalar horizontalmente el número de máquinas virtuales de forma manual, o bien automáticamente en función de reglas predefinidas. Esto es importante si el recurso está sufriendo un ataque de DDoS. Consulte [este artículo](/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm) para más información acerca de esta arquitectura de referencia.

### <a name="application-running-on-windows-n-tier"></a>Aplicación que se ejecuta en una arquitectura de n niveles de Windows

Hay muchas maneras de implementar una arquitectura de n niveles. El siguiente diagrama muestra una aplicación web típica de tres niveles. Esta arquitectura se basa en el artículo [Ejecución de máquinas virtuales de carga equilibrada para escalabilidad y disponibilidad](/azure/architecture/reference-architectures/virtual-machines-windows/multi-vm). Los niveles Web y Business usan máquinas virtuales de carga equilibrada.

![Diagrama de la arquitectura de referencia para una aplicación que se ejecuta en una arquitectura de n niveles de Windows](./media/ddos-best-practices/image-10.png)

En esta arquitectura, DDoS Protection Standard está habilitado en la red virtual. Todas las direcciones IP públicas de la red virtual obtendrán protección contra DDoS en los niveles 3 y 4. Para la protección del nivel 7, implemente Application Gateway en la SKU de WAF. Consulte [este artículo](/azure/architecture/reference-architectures/virtual-machines-windows/n-tier) para más información acerca de esta arquitectura de referencia.

> [!NOTE]
> No se admiten los escenarios en los que una sola máquina virtual se ejecuta detrás de una dirección IP pública.

### <a name="paas-web-application"></a>Aplicación web PaaS

Esta arquitectura de referencia muestra la ejecución de una aplicación de Azure App Service en una única región. Esta arquitectura muestra un conjunto de prácticas demostradas para una aplicación web que usa [Azure App Service](https://azure.microsoft.com/documentation/services/app-service/) y [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/).
Se configura una región en espera para escenarios de conmutación por error.

![Diagrama de la arquitectura de referencia para una aplicación web PaaS](./media/ddos-best-practices/image-11.png)

Azure Traffic Manager enruta las solicitudes entrantes a Application Gateway en una de las regiones. Durante las operaciones normales, enruta las solicitudes a Application Gateway en la región activa. Si esa región deja de estar disponible, Traffic Manager conmuta por error a Application Gateway en la región en espera.

Todo el tráfico de Internet con destino a la aplicación web se enruta a la [dirección IP pública de Application Gateway](/azure/application-gateway/application-gateway-web-app-overview) a través de Traffic Manager. En este escenario, App Service (Web Apps) no es directamente accesible desde el exterior y está protegido por Application Gateway. 

Se recomienda configurar la SKU de WAF de Application Gateway (modo de prevención) para protegerse contra ataques de nivel 7 (WebSocket, HTTP o HTTPS). Además, las aplicaciones web están configuradas para [aceptar tráfico solo desde la dirección IP de Application Gateway](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/).

Consulte [este artículo](/azure/architecture/reference-architectures/app-service-web-app/multi-region) para más información acerca de esta arquitectura de referencia.

## <a name="mitigation-for-non-web-paas-services"></a>Mitigación para servicios de PaaS que no son web

### <a name="hdinsight-on-azure"></a>HDInsight en Azure

Esta arquitectura de referencia muestra cómo configurar DDoS Protection Standard para un [clúster de Azure HDInsight](/azure/hdinsight/). Asegúrese de que el clúster de HDInsight esté vinculado a una red virtual y que DDoS Protection esté habilitado en esa red virtual.

![Paneles "HDInsight" y "Configuración avanzada", con la configuración de la red virtual](./media/ddos-best-practices/image-12.png)

![Selección para habilitar DDoS Protection](./media/ddos-best-practices/image-13.png)

En esta arquitectura, el tráfico de Internet con destino al clúster de HDInsight se enruta a la dirección IP pública asociada con el equilibrador de carga de la puerta de enlace de HDInsight. El equilibrador de carga de la puerta de enlace envía el tráfico directamente a los nodos principales o los nodos de trabajo. Como DDoS Protection Standard está habilitado en la red virtual de HDInsight, todas las direcciones IP públicas de la red virtual obtienen protección contra DDoS en los niveles 3 y 4. Esta arquitectura de referencia puede combinarse con arquitecturas de referencia de n niveles o múltiples regiones.

Para más información acerca de esta arquitectura de referencia, consulte [Extensión de HDInsight con Azure Virtual Network](/azure/hdinsight/hdinsight-extend-hadoop-virtual-network?toc=%2fazure%2fvirtual-network%2ftoc.json).


> [!NOTE]
> Azure App Service Environment para PowerApps o API Management en redes con direcciones IP públicas no se admiten de forma nativa.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [crear un plan de protección contra DDoS](manage-ddos-protection.md).