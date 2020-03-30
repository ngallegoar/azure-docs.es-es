---
title: Red virtual para servicios de Azure
titlesuffix: Azure Virtual Network
description: Obtenga información sobre las ventajas de la implementación de recursos en una red virtual. Los recursos de redes virtuales pueden comunicarse entre sí, y con recursos locales, sin tráfico que recorra Internet.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: malop
ms.reviewer: kumud
ms.openlocfilehash: 24bcc7e698527cd39958c53b48a0b36404c36bb4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79235836"
---
# <a name="virtual-network-integration-for-azure-services"></a>Integración de red virtual para los servicios de Azure

La integración de servicios de Azure en una red virtual de Azure permite el acceso privado al servicio desde las máquinas virtuales o los recursos de proceso de la red virtual.
Puede integrar los servicios de Azure en la red virtual con las siguientes opciones:
- Implementando instancias dedicadas del servicio en una red virtual. Luego se puede acceder de forma privada a los servicios dentro de la red virtual y desde redes locales.
- Usando [Private Link](../private-link/private-link-overview.md) para acceder de forma privada a una instancia específica del servicio desde la red virtual y desde las redes locales.

También puede acceder al servicio mediante puntos de conexión públicos si amplía una red virtual al servicio a través de los [puntos de conexión de servicio](virtual-network-service-endpoints-overview.md). Los puntos de conexión de servicio permiten que los recursos de servicio se protejan en la red virtual.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Implementación de servicios de Azure en las redes virtuales

Al implementar servicios de Azure dedicados en una [red virtual](virtual-networks-overview.md), puede comunicarse con los recursos de los servicios de forma privada mediante direcciones IP privadas.

![Servicios implementados en una red virtual](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

La implementación de servicios dentro de una red virtual ofrece las siguientes funcionalidades:

- Los recursos dentro de la red virtual pueden comunicarse entre sí de forma privada a través de direcciones IP privadas. Ejemplo: transferencia directa de datos entre HDInsight y SQL Server que se ejecutan en una máquina virtual, en la red virtual.
- Los recursos locales pueden acceder a recursos de una red virtual mediante direcciones IP privadas a través de [VPN de sitio a sitio (VPN Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) o [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Las redes virtuales pueden estar [emparejadas](virtual-network-peering-overview.md) para habilitar que los recursos de las redes virtuales se comuniquen entre sí mediante direcciones IP privadas.
- Normalmente, las instancias de servicio en una red virtual están totalmente administradas por el servicio de Azure. Esto incluye la supervisión del estado de los recursos y el escalado con carga.
- Las instancias de los servicios se implementan en una subred de una red virtual. El acceso de red entrante y saliente para la subred se debe abrir a través de [grupos de seguridad de red](security-overview.md#network-security-groups), siguiendo las instrucciones proporcionadas por el servicio.
- Ciertos servicios también imponen restricciones en la subred donde se implementan, los cuales limitan la aplicación de directivas, las rutas o la combinación de máquinas virtuales y recursos de servicio dentro de la misma subred. Compruebe las restricciones específicas de cada servicio porque podrían cambiar con el tiempo. Algunos ejemplos de estos servicios son Azure NetApp Files, Dedicated HSM, Azure Container Instances y App Service. 
- Opcionalmente, los servicios pueden requerir una [subred delegada](virtual-network-manage-subnet.md#add-a-subnet) como un identificador explícito de que una subred puede hospedar un servicio determinado. Mediante la delegación, los servicios obtienen permisos explícitos para crear recursos específicos del servicio en la subred delegada.
- Vea un ejemplo de una respuesta de API REST en una [red virtual con una subred delegada](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/get#get-virtual-network-with-a-delegated-subnet). Puede ver una lista completa de los servicios que usan el modelo de subred delegada a través de la API de [Available Delegations](https://docs.microsoft.com/rest/api/virtualnetwork/availabledelegations/list).

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Servicios que pueden implementarse en una red virtual

|Category|Servicio| Subred dedicada¹
|-|-|-|
| Proceso | Máquinas virtuales: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) o [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Conjuntos de escalado de máquinas virtuales](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Servicio en la nube](https://msdn.microsoft.com/library/azure/jj156091): solo red virtual (clásica)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)| No <br/> No <br/> No <br/> No²
| Red | [Application Gateway - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Firewall](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Aplicaciones virtuales de red](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) | Sí <br/> Sí <br/> Sí <br/> No
|data|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Instancia administrada de Azure SQL Database](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)| Sí <br/> Sí <br/> 
|Análisis | [HDInsight de Azure](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |No² <br/> No² <br/> 
| Identidad | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |No <br/>
| Contenedores | [Azure Kubernetes Service (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Instancia de Azure Container (ACI)](https://www.aka.ms/acivnet)<br/>[Motor de Azure Container Service](https://github.com/Azure/acs-engine) con el [complemento](https://github.com/Azure/acs-engine/tree/master/examples/vnet) CNI de Azure Virtual Network|No²<br/> Sí <br/><br/> No
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[entorno de App Service](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Sí <br/> Sí <br/> Sí
| Hospedada | [Azure Dedicated HSM](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Sí <br/> Sí <br/>
| | |

¹ "Dedicada" significa que en esta subred solo se pueden implementar los recursos específicos del servicio y no se pueden combinar con VM/VMSS del cliente. <br/> ² Se recomienda que estos servicios estén en una subred dedicada, pero no un requisito obligatorio impuesto por el servicio.
