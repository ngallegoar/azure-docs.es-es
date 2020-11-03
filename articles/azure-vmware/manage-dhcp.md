---
title: Creación y administración de DHCP
description: En este artículo se explica cómo administrar DHCP en Azure VMware Solution.
ms.topic: conceptual
ms.date: 10/22/2020
ms.openlocfilehash: cb74ed4474cc14081e59142f2f60915fccd47559
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2020
ms.locfileid: "92461063"
---
# <a name="how-to-create-and-manage-dhcp-in-azure-vmware-solution"></a>Creación y administración de DHCP en Azure VMware Solution

NSX-T le proporciona la capacidad de configurar DHCP para su nube privada. Si usa NSX-T para hospedar el servidor DHCP, consulte [Creación de un servidor DHCP](#create-dhcp-server). En caso contrario, si tiene un servidor DHCP externo de terceros en la red, consulte [Creación de un servicio de retransmisión DHCP](#create-dhcp-relay-service).

## <a name="create-dhcp-server"></a>Creación de un servidor DHCP

Para configurar un servidor DHCP en NSX-T, siga los pasos indicados a continuación.

1. En el administrador de NSX, vaya a la pestaña **Networking** (Redes) y seleccione **DHCP**. 
1. Seleccione **ADD SERVER** (AGREGAR SERVIDOR) y proporcione el nombre del servidor y la dirección IP. 
1. Seleccione **Guardar**.

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="agregar un servidor DHCP" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>Conecte el servidor DHCP a la puerta de enlace de nivel 1.

1. Seleccione **Tier 1 Gateways** (Puertas de enlace de nivel 1) en la lista, luego, la puerta de enlace y, a continuación, **Edit** (Editar).

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="seleccionar la puerta de enlace que se va a usar" border="true":::

1. Para agregar una subred, seleccione **No IP Allocation Set** (No hay conjuntos de asignación de direcciones IP).

   :::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="agregar una subred" border="true":::

1. Seleccione **DHCP Local Server** (Servidor local de DHCP) como **Type** (Tipo). 
1. Seleccione **Default DHCP** (DHCP predeterminado) como **DHCP Server** (Servidor DHCP) y, luego, seleccione **Save** (Guardar).


1. En la ventana **Tier -1 Gateway** (Puerta de enlace de nivel 1), seleccione **Save** (Guardar). 
1. Seleccione **Close Editing** (Cerrar edición) para finalizar.

### <a name="add-a-network-segment"></a>Incorporación de un segmento de red

Una vez que haya creado el servidor DHCP, deberá agregarle segmentos de red.

1. En NSX-T, seleccione la pestaña **Networking** (Redes) y, luego, **Segments** (Segmentos) en **Connectivity** (Conectividad). 
1. Seleccione **ADD SEGMENT** (AGREGAR SEGMENTO) y asigne un nombre al segmento y a la conexión en la puerta de enlace de nivel 1. 
1. Seleccione **Set Subnets** (Establecer subredes) para configurar una nueva subred. 

   :::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="agregar nuevo segmento de red" border="true":::

1. En la ventana **Set Subnets** (Establecer subredes), seleccione **ADD SUBNET** (AGREGAR SUBRED). 
1. Escriba la dirección IP de la puerta de enlace y el intervalo de DHCP, y seleccione **Add** (Agregar) y, después, **APPLY** (APLICAR).

1. Seleccione **Save** (Guardar) para agregar el nuevo segmento de red.

## <a name="create-dhcp-relay-service"></a>Creación de un servicio de retransmisión DHCP

1. Seleccione la pestaña **Networking** (Redes) y, en **IP Management** (Administración de direcciones IP), elija **DHCP**. 
1. Seleccione **ADD SERVER** (AGREGAR SERVIDOR). 
1. Seleccione DHCP Relay (Retransmisión DHCP) para **Server Type** (Tipo de servidor) y escriba el nombre del servidor y la dirección IP del servidor de retransmisión. 
1. Seleccione **Guardar**.

   :::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="crear servidor de retransmisión DHCP" border="true":::

1. Seleccione **Tier-1 Gateways** (Puertas de enlace de nivel 1) en **Connectivity** (Conectividad). 
1. Seleccione los puntos suspensivos verticales en la puerta de enlace de nivel 1 y seleccione **Edit** (Editar).

   :::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="editar puerta de enlace de nivel 1" border="true":::

1. Seleccione **No IP Allocation Set** (No hay conjuntos de asignación de direcciones IP) para definir la asignación de direcciones IP.

   :::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="editar asignación de direcciones IP" border="true":::

1. Seleccione **DHCP Relay Server** (Servidor de retransmisión DHCP) para **Type** (Tipo).
1. Seleccione el servidor de retransmisión DHCP para **DHCP Relay** (Retransmisión DHCP). 
1. Seleccione **Guardar**.


## <a name="specify-a-dhcp-range-ip-on-a-segment"></a>Definición de una dirección IP del intervalo de DHCP en el segmento

> [!NOTE]
> Esta configuración es necesaria para realizar la funcionalidad de retransmisión DHCP en el segmento del cliente DHCP. 

1. En **Connectivity** (Conectividad), seleccione **Segments** (Segmentos). 
1. Seleccione los puntos suspensivos verticales y, luego, **Edit** (Editar). 

   >[!TIP]
   >Si desea agregar un nuevo segmento, seleccione **Add Segment** (Agregar segmento).

1. Agregue detalles sobre el segmento. 
1. Seleccione el valor en **Set Subnets** (Establecer subredes) para agregar o modificar la subred.

   :::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="segmentos de red" border="true":::

1. Seleccione los puntos suspensivos verticales y elija **Edit** (Editar). Si necesita crear una nueva subred, seleccione **Add Subnet** (Agregar subred) para crear una puerta de enlace y configurar un intervalo de DHCP. 
1. Proporcione el intervalo del grupo de direcciones IP y seleccione **Apply** (Aplicar) y, después, **Save** (Guardar).

   :::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="editar subredes" border="true":::

   Se asigna un grupo de servidores DHCP al segmento.

   :::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="grupo de servidores DHCP asignado al segmento" border="true":::
