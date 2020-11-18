---
title: Creación de un túnel IPSec en Azure VMware Solution
description: Obtenga más información sobre cómo crear un centro de conectividad de Virtual WAN para establecer un túnel IPSec en instancias de Azure VMware Solution.
ms.topic: how-to
ms.date: 10/02/2020
ms.openlocfilehash: 9f869f04bf165f4791f13c626b63257ea98a7ca9
ms.sourcegitcommit: 4bee52a3601b226cfc4e6eac71c1cb3b4b0eafe2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/11/2020
ms.locfileid: "94506454"
---
# <a name="create-an-ipsec-tunnel-into-azure-vmware-solution"></a>Creación de un túnel IPSec en Azure VMware Solution

En este artículo analizaremos los pasos necesarios para establecer un túnel VPN (IPsec IKEv1 e IKEv2) de sitio a sitio que termine en el centro de Microsoft Azure Virtual WAN. Vamos a crear un centro de Azure Virtual WAN y una puerta de enlace de VPN con una dirección IP pública asociada. Luego, crearemos una puerta de enlace de Azure ExpressRoute y estableceremos un punto de conexión de Azure VMware Solution. También veremos los detalles de la habilitación de una instalación local de VPN basada en directivas. 

## <a name="topology"></a>Topología

![Diagrama que muestra la arquitectura de un túnel VPN de sitio a sitio.](media/create-ipsec-tunnel/vpn-s2s-tunnel-architecture.png)

El centro de Azure Virtual WAN contiene la puerta de enlace de ExpressRoute de Azure VMware Solution y la puerta de enlace de VPN de sitio a sitio. Conecta un dispositivo VPN local con un punto de conexión de Azure VMware Solution.

## <a name="before-you-begin"></a>Antes de empezar

Para crear el túnel VPN de sitio a sitio, deberá crear una dirección IP de acceso público que termine en un dispositivo VPN local.

## <a name="create-a-virtual-wan-hub"></a>Creación de un centro de conectividad de Virtual WAN

1. En el Azure Portal, busque **Redes WAN virtuales**. Seleccione **+Agregar**. Se abre la página Crear una red WAN.  

2. Escriba los campos obligatorios en la página **Crear una red WAN** y, luego, seleccione **Revisar y crear**.
   
   | Campo | Value |
   | --- | --- |
   | **Suscripción** | El valor se rellena previamente con la suscripción a la que pertenece el grupo de recursos. |
   | **Grupos de recursos** | Virtual WAN es un recurso global y no se limita a una región específica.  |
   | **Ubicación del grupo de recursos** | Para crear el centro de Virtual WAN, debe establecer una ubicación para el grupo de recursos.  |
   | **Nombre** |   |
   | **Tipo** | Seleccione **Estándar**, lo que permitirá algo más que el tráfico de la puerta de enlace de VPN.  |


    :::image type="content" source="media/create-ipsec-tunnel/create-wan.png" alt-text="Captura de pantalla que muestra la página de creación de una instancia de Virtual WAN en Azure Portal.":::

3. En Azure Portal, seleccione la instancia de Virtual WAN que creó en el paso anterior, elija **Crear centro de conectividad virtual**, escriba los campos obligatorios y seleccione **Siguiente: Sitio a sitio**. 

   | Campo | Value |
   | --- | --- |
   | **Región** | Desde el punto de vista administrativo, es necesario seleccionar una región.  |
   | **Nombre** |    |
   | **Espacio de direcciones privado del centro** | Escriba la subred con un valor `/24` (mínimo).  |

    :::image type="content" source="media/create-ipsec-tunnel/create-virtual-hub.png" alt-text="Captura de pantalla que muestra la página de creación de un centro virtual.":::

4. En la pestaña **Sitio a sitio**, defina la puerta de enlace de sitio a sitio estableciendo el rendimiento agregado desde la lista desplegable **Unidades de escalado de puerta de enlace**. 

   >[!TIP]
   >Una unidad de escalado equivale a 500 Mbps. Las unidades de escalado están en pares con fines de redundancia, y cada una admite 500 Mbps.
  
5. En la pestaña **ExpressRoute**, cree una puerta de enlace de ExpressRoute. 

   >[!TIP]
   >Una unidad de escalado equivale a 2 Gbps. 

    Se tardan 30 minutos aproximadamente en crear cada centro. 

## <a name="create-a-vpn-site"></a>Creación de un sitio de VPN 

1. En **Recursos recientes**, en Azure Portal, seleccione la instancia de Virtual WAN que creó en la sección anterior.

2. En la página **Información general** del centro virtual, seleccione **Conectividad** > **VPN (De sitio a sitio)** y, luego, **Crear un nuevo sitio de VPN**.


    :::image type="content" source="media/create-ipsec-tunnel/create-vpn-site-basics.png" alt-text="Captura de pantalla de la página de información general del centro virtual, con las opciones VPN (De sitio a sitio) y Crear un nuevo sitio de VPN seleccionadas.":::  
 
3. En la pestaña **Aspectos básicos**, escriba los campos obligatorios y seleccione **Siguiente: Vínculos**. 

   | Campo | Value |
   | --- | --- |
   | **Región** | Indique la misma región que especificó en la sección anterior.  |
   | **Nombre** |  |
   | **Proveedor del dispositivo** |  |
   | **Protocolo de puerta de enlace de borde** | Establezca este campo en **Habilitar** para asegurarse de que tanto Azure VMware Solution como los servidores locales anuncien sus rutas a través del túnel. Si esta opción está deshabilitada, las subredes que tienen que anunciarse deberán mantenerse manualmente. Si faltan subredes, HCX no podrá formar la malla del servicio. Para obtener más información, consulte [Acerca de BGP con Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md). |
   | **Espacio de direcciones privado**  | Especifique el bloque CIDR local.  Se usa para enrutar todo el tráfico enlazado en el entorno local a través del túnel.  El bloque CIDR solo es necesario si no se habilita BGP. |
   | **Conectar a** |   |

4. En la pestaña **Vínculos**, rellene los campos obligatorios y seleccione **Revisar y crear**. La especificación de los nombres del proveedor y el vínculo le permite distinguir entre cualquier número de puertas de enlace que puedan crearse en algún momento como parte del centro. El BGP y el número de sistema autónomo (ASN) deben ser únicos dentro de su organización.
 
## <a name="optional-defining-a-vpn-site-for-policy-based-vpn-site-to-site-tunnels"></a>(Opcional) Definición de un sitio de VPN para túneles VPN de sitio a sitio según directivas

Esta sección se aplica únicamente a las VPN basadas en directivas. En la mayoría de los casos, la configuración de las VPN basadas en directivas (o estáticas basadas en rutas) está determinada por las funcionalidades de los dispositivos VPN locales. Es necesario especificar redes locales y de Azure VMware Solution. En el caso de Azure VMware Solution con un centro de Azure Virtual WAN, no puede seleccionar *cualquier* red. En su lugar, tiene que especificar todos los intervalos pertinentes del entorno local y el centro de Virtual WAN de Azure VMware Solution. Dichos intervalos del centro se usan para especificar el dominio de cifrado del punto de conexión local del túnel VPN basado en directivas. El lado de Azure VMware Solution solo requiere que se habilite el indicador del selector de tráfico basado en directivas. 

1. En Azure Portal, vaya al sitio del centro de Virtual WAN; en **Conectividad**, seleccione **VPN (De sitio a sitio)** .

2. Seleccione el nombre del sitio de VPN, el botón de puntos suspensivos (...) situado en el extremo derecho y, luego, **Editar la conexión de VPN a este concentrador**.
 
    :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png" alt-text="Captura de pantalla de la página de Azure para el sitio del centro de WAN virtual que muestra un botón de puntos suspensivos seleccionado para acceder a la opción Editar la conexión de VPN a este concentrador." lightbox="media/create-ipsec-tunnel/edit-vpn-section-to-this-hub.png":::

3. Edite la conexión entre el sitio de VPN y el centro y, luego, seleccione **Guardar**.
   - Para el protocolo de seguridad de Internet (IPSec), seleccione **Personalizado**.
   - Para la opción Usar el selector de tráfico basado en directivas, seleccione **Habilitar**.
   - Especifique los detalles de **Fase 1 IKE** y **Fase 2 IKE (IPsec)** . 
 
    :::image type="content" source="media/create-ipsec-tunnel/edit-vpn-connection.png" alt-text="Captura de pantalla de la página Editar conexión VPN."::: 
 
    Los selectores de tráfico o las subredes que forman parte del dominio de cifrado basado en directivas deben ser los siguientes:
    
    - Centro de Virtual WAN /24
    - Nube privada de Azure VMware Solution /22
    - Red virtual de Azure conectada (si existe)

## <a name="connect-your-vpn-site-to-the-hub"></a>Conexión del sitio de VPN con el centro

1. Marque la casilla situada junto al nombre del sitio de VPN (consulte la captura de pantalla anterior, **VPN (De sitio a sitio)** ) y, luego, seleccione **Conectar sitios de VPN**. En el campo **Clave precompartida**, escriba la clave definida previamente para el punto de conexión local. Si no tiene una clave definida previamente, puede dejar este campo en blanco; se generará una clave automáticamente. 
 
    Habilite solo **Propagar la ruta predeterminada** si va a implementar un firewall en el centro y se trata del próximo salto para las conexiones a través de ese túnel.

    Seleccione **Conectar**. Se mostrará una pantalla sobre el estado de las conexiones con el estado de la creación del túnel.

2. Vaya a la página de información general de Virtual WAN. Abra la página del sitio de VPN y descargue el archivo de configuración de VPN para aplicarlo al punto de conexión local.  

3. Ahora aplicaremos una revisión de ExpressRoute de Azure VMware Solution en el centro de Virtual WAN. (Este paso requiere crear primero la nube privada).

    Vaya a la sección **Conectividad** de la nube privada de Azure VMware Solution. En la pestaña **ExpressRoute**, seleccione **+ Solicitar una clave de autorización**. Asígnele un nombre y seleccione **Crear**. (La clave puede tardar unos 30 segundos en crearse). Copie la clave de autorización y el identificador de ExpressRoute. 

    :::image type="content" source="media/create-ipsec-tunnel/express-route-connectivity.png" alt-text="Captura de pantalla de la página Conectividad de la nube privada, con la opción Solicitar una clave de autorización seleccionada en la pestaña ExpressRoute.":::

    > [!NOTE]
    > La clave de autorización desaparecerá tras un tiempo, por lo que debe copiarla en cuanto aparezca.

4. Seguidamente, vincularemos Azure VMware Solution y la puerta de enlace de VPN en el centro de Virtual WAN. En Azure Portal, abra la instancia de Virtual WAN que creó anteriormente. Seleccione el centro de Virtual WAN creado y **ExpressRoute** en el panel izquierdo. Seleccione **+ Canjear la clave de autorización**.

    :::image type="content" source="media/create-ipsec-tunnel/redeem-authorization-key.png" alt-text="Captura de pantalla que muestra la página de ExpressRoute de la nube privada con la opción Canjear la clave de autorización seleccionada.":::

    Pegue la clave de autorización en el campo Clave de autorización y el identificador de ExpressRoute en el campo **URI de circuito del mismo nivel**. Asegúrese de seleccionar **Asocie automáticamente este circuito de ExpressRoute con el centro**. Seleccione **Agregar** para establecer el vínculo. 

5. Para probar la conexión, cree un segmento NSX-T y aprovisione una máquina virtual en la red. Para obtener más información al respecto, consulte [Tutorial: Creación de un segmento de red de NSX-T en Azure VMware Solution](./tutorial-nsx-t-network-segment.md). En lo que respecta a la prueba, haga ping a los puntos de conexión locales y a los de Azure VMware Solution.
