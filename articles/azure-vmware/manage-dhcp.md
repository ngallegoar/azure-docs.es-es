---
title: Administración de DHCP para Azure VMware Solution
description: Aprenda a crear y administrar DHCP para la nube privada de Azure VMware Solution.
ms.topic: how-to
ms.date: 11/09/2020
ms.openlocfilehash: 9143a8544fe1b98262c3e990ccdf56f5d5f65957
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2020
ms.locfileid: "94335995"
---
# <a name="manage-dhcp-for-azure-vmware-solution"></a>Administración de DHCP para Azure VMware Solution

Las aplicaciones y cargas de trabajo que se ejecutan en un entorno de nube privada requieren servicios de DHCP para las asignaciones de direcciones IP.  En este artículo se muestra cómo crear y administrar DHCP en Azure VMware Solution de dos maneras:

- Si utiliza NSX-T para hospedar el servidor DHCP, deberá [crear un servidor DHCP](#create-a-dhcp-server) y [retransmitir a ese servidor](#create-dhcp-relay-service). Al crear el servidor DHCP, también agregará un segmento de red y especificará el intervalo de direcciones IP de DHCP.   

- Si usa un servidor DHCP externo de terceros en la red, tendrá que [crear un servicio de retransmisión DHCP](#create-dhcp-relay-service). Al crear una retransmisión a un servidor DHCP, ya sea mediante NSX-T o un tercero para hospedar el servidor DHCP, deberá especificar el intervalo de direcciones IP de DHCP.

>[!IMPORTANT]
>DHCP no funciona para las máquinas virtuales (VM) de la red elástica L2 de VMware HCX cuando el servidor DHCP está en el centro de datos local.  De forma predeterminada, NSX impide que todas las solicitudes DHCP atraviesen la red elástica L2. Para la solución, consulte el procedimiento [Envío de solicitudes DHCP al servidor DHCP local](#send-dhcp-requests-to-the-on-premises-dhcp-server).


## <a name="create-a-dhcp-server"></a>Creación de un servidor DHCP

Si desea usar NSX-T para hospedar el servidor DHCP, tendrá que crear un servidor DHCP. A continuación, agregará un segmento de red y especificará el intervalo de direcciones IP de DHCP.

1. En NSX-T Manager, seleccione **Networking** > **DHCP** (Redes > DHCP) y, a continuación, seleccione **Add Server** (Agregar servidor).

1. Seleccione **DHCP** para **Server Type** (Tipo de servidor), proporcione el nombre del servidor y la dirección IP y, a continuación, seleccione **Save** (Guardar).

   :::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="agregar un servidor DHCP" border="true":::

1. Seleccione **Tier 1 Gateways** (Puertas de enlace de nivel 1), seleccione los puntos suspensivos verticales en la puerta de enlace de nivel 1 y seleccione **Edit** (Editar).

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="seleccionar la puerta de enlace que se va a usar" border="true":::

1. Para agregar una subred, seleccione **No IP Allocation Set** (No hay conjuntos de asignación de direcciones IP).

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="agregar una subred" border="true":::

1. En **Type** (Tipo), seleccione **DHCP Local Server** (Servidor local DHCP). 
   
1. Para **DHCP Server** (Servidor DHCP), seleccione **Default DHCP** (DHCP predeterminado) y luego **Save** (Guardar).

1. Seleccione **Save** (Guardar) de nuevo y luego **Close Editing** (Cerrar edición).

### <a name="add-a-network-segment"></a>Incorporación de un segmento de red

[!INCLUDE [add-network-segment-steps](includes/add-network-segment-steps.md)]


## <a name="create-dhcp-relay-service"></a>Creación de un servicio de retransmisión DHCP

Si desea usar un servidor DHCP externo de terceros, deberá crear un servicio de retransmisión DHCP. También especificará el intervalo de direcciones IP de DHCP en NSX-T Manager. 

1. En NSX-T Manager, seleccione **Networking** > **DHCP** (Redes > DHCP) y, a continuación, seleccione **Add Server** (Agregar servidor).

1. Seleccione **DHCP Relay** (Retransmisión de DHCP) para **Server Type** (Tipo de servidor), proporcione el nombre del servidor y la dirección IP y, a continuación, seleccione **Save** (Guardar).

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="creación de un servicio de retransmisión DHCP" border="true":::

1. Seleccione **Tier 1 Gateways** (Puertas de enlace de nivel 1), seleccione los puntos suspensivos verticales en la puerta de enlace de nivel 1 y seleccione **Edit** (Editar).

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="editar puerta de enlace de nivel 1" border="true":::

1. Seleccione **No IP Allocation Set** (No hay conjuntos de asignación de direcciones IP) para definir la asignación de direcciones IP.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="editar asignación de direcciones IP" border="true":::

1. En **Type** (Tipo), seleccione **DHCP Server** (Servidor DHCP). 
   
1. Para **DHCP Server** (Servidor DHCP), seleccione **DHCP Relay** (Retransmisión DHCP) y luego **Save** (Guardar).

1. Seleccione **Save** (Guardar) de nuevo y luego **Close Editing** (Cerrar edición).


## <a name="specify-the-dhcp-ip-address-range"></a>Especifique el intervalo de direcciones IP de DHCP.

1. En NSX-T Manager, seleccione **Networking** > **Segments** (Redes > Segmentos). 
   
1. Seleccione los puntos suspensivos verticales en el nombre del segmento y seleccione **Edit** (Editar).
   
1. Seleccione **Set Subnets** (Establecer subredes) para especificar la dirección IP de DHCP para la subred. 
   
   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="segmentos de red" border="true":::
      
1. Modifique la dirección IP de puerta de enlace si es necesario y escriba la dirección IP del intervalo de DHCP. 
      
   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="editar subredes" border="true":::
      
1. Seleccione **Apply** (Aplicar) y, a continuación, **Save** (Guardar). Se asigna un grupo de servidores DHCP al segmento.
      
   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="grupo de servidores DHCP asignado al segmento" border="true":::


## <a name="send-dhcp-requests-to-the-on-premises-dhcp-server"></a>Envío de solicitudes DHCP al servidor DHCP local

Si desea enviar solicitudes DHCP desde las máquinas virtuales de Azure VMware Solution en el segmento extendido de L2 al servidor DHCP local, creará un perfil de segmento de seguridad. 

1. Inicie sesión en la instancia local de vCenter y en **Inicio**, seleccione **HCX**.

1. Seleccione **Network Extension** (Extensión de red) en **Services** (Servicios).

1. Seleccione la extensión de red que desea que admita las solicitudes DHCP de Azure VMware Solution en el entorno local. 

1. Tome nota del nombre de la red de destino.  

   :::image type="content" source="media/manage-dhcp/hcx-find-destination-network.png" alt-text="Captura de pantalla de una extensión de red en el cliente VMware vSphere" lightbox="media/manage-dhcp/hcx-find-destination-network.png":::

1. En NSX-T Manager de Azure VMware Solution, seleccione **Networking** > **Segments** > **Segment Profiles** (Redes > Segmentos > Perfiles de segmento). 

1. Seleccione **Add Segment Profile** (Agregar perfil de segmento) y luego **Segment Security** (Seguridad de segmento).

   :::image type="content" source="media/manage-dhcp/add-segment-profile.png" alt-text="Captura de pantalla de cómo agregar un perfil de segmento en NSX-T" lightbox="media/manage-dhcp/add-segment-profile.png":::

1. Proporcione un nombre y una etiqueta y, a continuación, establezca el conmutador **BPDU Filter** (Filtro BPDU) en ON (Activado) y todos los conmutadores de DHCP en OFF (Desactivado).

   :::image type="content" source="media/manage-dhcp/add-segment-profile-bpdu-filter-dhcp-options.png" alt-text="Captura de pantalla que muestra el conmutador de filtro BPDU activado y los conmutadores de DHCP desactivados" lightbox="media/manage-dhcp/add-segment-profile-bpdu-filter-dhcp-options.png":::

1. Quite todas las direcciones MAC, si las hay, en **BPDU Filter Allow List** (Lista de permitidos del filtro de BPDU).  Después, seleccione **Guardar**.

   :::image type="content" source="media/manage-dhcp/add-segment-profile-bpdu-filter-allow-list.png" alt-text="Captura de pantalla que muestra las direcciones MAC en la lista de permitidos del filtro de BPDU":::

1. En **Networking** > **Segments** > **Segments**, (Redes > Segmentos > Segmentos), en el área de búsqueda, escriba el nombre de la red de definición.

   :::image type="content" source="media/manage-dhcp/networking-segments-search.png" alt-text="Captura de pantalla del campo de filtro de Redes > Segmentos":::

1. Seleccione los puntos suspensivos verticales en el nombre del segmento y seleccione **Edit** (Editar).

   :::image type="content" source="media/manage-dhcp/edit-network-segment.png" alt-text="Captura de pantalla del botón Editar del segmento" lightbox="media/manage-dhcp/edit-network-segment.png":::

1. Cambie **Segment Security** (Seguridad del segmento) al perfil de segmento que creó anteriormente.

   :::image type="content" source="media/manage-dhcp/edit-segment-security.png" alt-text="Captura de pantalla del campo Seguridad del segmento" lightbox="media/manage-dhcp/edit-segment-security.png":::

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el [Mantenimiento y administración del ciclo de vida de hosts](concepts-private-clouds-clusters.md#host-maintenance-and-lifecycle-management).