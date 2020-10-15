---
title: 'Tutorial: Creación de un segmento de red de NSX-T en Azure VMware Solution'
description: Obtenga información sobre cómo crear los segmentos de red de NSX-T que se usan para las VM en vCenter.
ms.topic: tutorial
ms.date: 09/21/2020
ms.openlocfilehash: 780cac15efc043b9ae44b77af1234adca3fec5a1
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91254540"
---
# <a name="tutorial-create-an-nsx-t-network-segment-in-azure-vmware-solution"></a>Tutorial: Creación de un segmento de red de NSX-T en Azure VMware Solution

Los segmentos de red creados en NSX-T Manager se usan como redes para máquinas virtuales (VM) en vCenter. Las máquinas virtuales creadas en vCenter se colocan en los segmentos de red creados en NSX-T y son visibles en vCenter.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Desplazarse en NSX-T Manager para agregar segmentos de red
> * Agregar un nuevo segmento de red
> * Observar el nuevo segmento de red en vCenter

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, se requiere una nube privada de Azure VMware Solution con acceso a las interfaces de administración de vCenter y NSX-T Manager. Para más información, consulte el tutorial [Configuración de redes para la nube privada de VMware en Azure](tutorial-configure-networking.md).

## <a name="provision-a-network-segment-in-nsx-t"></a>Aprovisionamiento de un segmento de red en NSX-T

1. En vCenter para la nube privada, seleccione **SDDC-Datacenter > Networks** (Redes ) y observe que todavía no hay redes.

   :::image type="content" source="media/nsxt/vcenter-without-ls01.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::.

1. En el administrador de NSX-T para su nube privada, seleccione **Networking** (Redes).

   :::image type="content" source="media/nsxt/nsxt-network-overview.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::.

1. Seleccione **Segments** (Segmentos).

   :::image type="content" source="media/nsxt/nsxt-select-segments.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::

1. En la página de información general de segmentos de NSX-T, seleccione **ADD SEGMENT** (AGREGAR SEGMENTO). Se crean tres segmentos como parte del aprovisionamiento de la nube privada y no se pueden usar para las máquinas virtuales.  Tendrá que agregar un nuevo segmento de red para este fin.

   :::image type="content" source="media/nsxt/nsxt-segments-overview.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::.

1. Asigne un nombre al segmento; en **Connected Gateway** (Puerta de enlace conectada), elija la puerta de enlace Tier1 preconfigurada (TNTxx-T1); deje **Type** (Tipo) como Flexible; en **Transport Zone** (zona de transporte), elija la superposición preconfigurada (TNTxx-OVERLAY-TZ); y, por último, seleccione Set Subnets (Establecer subredes). El resto de la configuración de esta sección así como **PORTS**  (PUERTOS) y **SEGMENT PROFILES** (PERFILES DE SEGMENTO) pueden permanecer en el valor predeterminado, al igual que la configuración.

   :::image type="content" source="media/nsxt/nsxt-create-segment-specs.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::

1. Establezca la dirección IP de la puerta de enlace para el nuevo segmento y, a continuación, seleccione **ADD** (AGREGAR). La dirección IP que use debe estar en un bloque de direcciones RFC1918 que no se superponga, lo que garantiza que pueda conectarse a las máquinas virtuales en el nuevo segmento.

   :::image type="content" source="media/nsxt/nsxt-create-segment-gateway.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::.

1. Aplique el nuevo segmento de red; para ello, seleccione **APPLY** (APLICAR) y, después, guarde la configuración con **SAVE** (GUARDAR).

   :::image type="content" source="media/nsxt/nsxt-create-segment-apply.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::

   :::image type="content" source="media/nsxt/nsxt-create-segment-save.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::

1. Ahora se ha creado el nuevo segmento de red y rechazará la opción para continuar configurando el segmento si selecciona **NO**.

   :::image type="content" source="media/nsxt/nsxt-create-segment-continue-no.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::

1. Confirme que el nuevo segmento de red está presente en NSX-T; para ello, seleccione **Networking > Segments** (Redes > Segmentos) y compruebe que el nuevo segmento se encuentra en la lista (en este caso, "ls01").

   :::image type="content" source="media/nsxt/nsxt-new-segment-overview-2.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::

1. Confirme que el nuevo segmento de red esté presente en vCenter; para ello, seleccione **Networking > SDDC-Datacenter** (Redes > SDDC-Datacenter) y observe que el nuevo segmento esté en la lista (en este caso, "ls01").

   :::image type="content" source="media/nsxt/vcenter-with-ls01-2.png" alt-text="En vCenter para la nube privada, seleccione SDDC-Datacenter > Networks (Redes ) y observe que todavía no hay redes":::

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado los segmentos de red de NSX-T que se usan para las máquinas virtuales en vCenter. Ahora puede [crear una biblioteca de contenido para implementar VM en Azure VMware Solution](deploy-vm-content-library.md) y aprovisionar una VM en la red que creó en este tutorial.

De lo contrario, pase al tutorial siguiente para obtener información sobre cómo crear el emparejamiento de Global Reach de ExpressRoute y una nube privada en Azure VMware Solution.

> [!div class="nextstepaction"]
> [Emparejamiento de entornos locales con una nube privada](tutorial-expressroute-global-reach-private-cloud.md)

<!-- LINKS - external-->

<!-- LINKS - internal -->
