---
title: Conexión de Azure VMware Solution al entorno local
description: Aprenda a conectar Azure VMware Solution al entorno local.
ms.topic: tutorial
ms.date: 10/02/2020
ms.openlocfilehash: 2a0cb641df00f3e580e87e38aff382d8e8101fc7
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578909"
---
# <a name="connect-azure-vmware-solution-to-your-on-premises-environment"></a>Conexión de Azure VMware Solution al entorno local

En este artículo, podrá seguir usando la información [recopilada durante la planeación](production-ready-deployment-steps.md) para conectar Azure VMware Solution al entorno local.

Antes de comenzar, tiene que cumplir dos requisitos previos para conectar Azure VMware Solution al entorno local:

- Tener un circuito de ExpressRoute desde su entorno local a Azure.
- Tener un bloque de direcciones de red de tipo /29 que no estén superpuestas para el emparejamiento de Global Reach de ExpressRoute, que definió como parte de la [fase de planeación](production-ready-deployment-steps.md).

>[!NOTE]
> Puede conectarse a través de VPN, pero ese tema no se detalla en este documento de inicio rápido.

## <a name="establish-an-expressroute-global-reach-connection"></a>Establecimiento de una conexión de Global Reach de ExpressRoute

Para establecer la conectividad local a la nube privada de Azure VMware Solution con Global Reach de ExpressRoute, siga los pasos del tutorial [Entornos locales del mismo nivel en una nube privada](tutorial-expressroute-global-reach-private-cloud.md).



## <a name="verify-on-premises-network-connectivity"></a>Comprobación de la conectividad de red local

Ahora debería ver en el **enrutador perimetral local** el lugar donde ExpressRoute conecta los segmentos de red NSX-T y los segmentos de administración de Azure VMware Solution.

>[!NOTE]
>Todos los usuarios tienen un entorno diferente, aunque algunos deberán permitir que estas rutas se propaguen de nuevo a la red local.  

Algunos entornos tendrán firewalls que protegen el circuito de ExpressRoute.  Si no hay ningún firewall y no se está produciendo ninguna eliminación de entradas, puede hacer ping en el servidor vCenter de Azure VMware Solution o en una [VM](deploy-azure-vmware-solution.md#add-a-vm-on-the-nsx-t-network-segment) en el segmento NSX-T desde el entorno local.

Igualmente, puede llegar a los recursos del entorno local desde la VM del segmento NSX-T.

## <a name="next-steps"></a>Pasos siguientes

Continúe con la siguiente sección para implementar y configurar VMware HCX.

> [!div class="nextstepaction"]
> [Implementación y configuración de VMware HCX](tutorial-deploy-vmware-hcx.md)