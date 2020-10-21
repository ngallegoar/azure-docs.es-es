---
title: Uso de la funcionalidad de dirección IP pública en Virtual WAN
description: En este artículo se explica cómo usar la funcionalidad de IP pública en Azure Virtual WAN.
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: 61ed6487bc000a35fd25cabde2b562b6eb08da46
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92048311"
---
# <a name="how-to-use-the-public-ip-functionality-in-azure-virtual-wan"></a>Uso de la funcionalidad de dirección IP pública en Azure Virtual WAN

La dirección IP pública es una característica nueva de la conectividad de Azure VMware Solution, gracias a la cual los clientes pueden habilitar el acceso público a Internet de dos maneras. 

- Las aplicaciones se pueden hospedar y publicar en el equilibrador de carga de Application Gateway para el tráfico HTTP/HTTPS.
- Publicado mediante las características de IP públicas de Azure Virtual WAN.

Como parte de la implementación de la nube privada de Azure VMware Solution, al habilitar la funcionalidad de IP pública, los componentes automatizados necesarios se crean y habilitan automáticamente:

-  Virtual WAN

-  Centro de Virtual WAN con conectividad a ExpressRoute

-  Servicios de Azure Firewall con IP pública

En este artículo se detalla cómo se puede usar la funcionalidad de IP pública en Virtual WAN para hacer que recursos como servidores web, máquinas virtuales (VM) y hosts, sean accesibles a través de una red pública.

## <a name="prerequisites"></a>Requisitos previos

- Entorno de Azure VMware Solution
- Un servidor web que se ejecute en el entorno de Azure VMware Solution.
- Un nuevo intervalo de direcciones IP no superpuestas para la implementación del concentrador de Virtual WAN, normalmente un valor `/24`.

## <a name="reference-architecture"></a>Arquitectura de referencia

:::image type="content" source="media/public-ip-usage/public-ip-architecture-diagram.png" alt-text="Diagrama de la arquitectura de la IP pública" border="false" lightbox="media/public-ip-usage/public-ip-architecture-diagram.png":::

El diagrama de arquitectura muestra un servidor web de cliente hospedado en el entorno de Azure VMware Solution que está configurado con direcciones IP privadas de tipo RFC1918.  Este servicio web está disponible en Internet a través de la funcionalidad de IP pública de Virtual WAN.  La dirección IP pública normalmente es una NAT de destino traducida en Azure Firewall. Con las reglas de DNAT, la directiva de firewall traduce las solicitudes de direcciones IP públicas a una dirección privada (webserver) con un puerto.

Las solicitudes de usuario llegan al firewall de una dirección IP pública que, a su vez, se traduce en una dirección IP privada mediante las reglas DNAT de Azure Firewall. El firewall comprueba la tabla NAT y, si la solicitud coincide con una entrada, reenvía el tráfico a la dirección y el puerto traducidos en el entorno de Azure VMware Solution.

El servidor web recibe la solicitud, responde con la información o la página solicitada al firewall y, a continuación, este reenvía la información al usuario en la dirección IP pública.

## <a name="test-case"></a>Caso de prueba
En este escenario, debe publicar el servidor web de IIS en Internet. Use la característica de IP pública en Azure VMware Solution para publicar el sitio web en una dirección IP pública.  Configuraremos las reglas NAT en el firewall y así podrá acceder a los recursos de Azure VMware Solution (VM con servidor web) con la dirección IP pública.

## <a name="deploy-virtual-wan"></a>Implementar Virtual WAN.

1. Inicie sesión en Azure Portal y, a continuación, busque y seleccione **Azure VMware Solution**.

1. Seleccione la nube privada de Azure VMware Solution.

   :::image type="content" source="media/public-ip-usage/avs-private-cloud-resource.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/avs-private-cloud-resource.png":::

1. En **Administrar**, seleccione **Conectividad**.

   :::image type="content" source="media/public-ip-usage/avs-private-cloud-manage-menu.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/avs-private-cloud-manage-menu.png":::

1. Seleccione la pestaña de **IP pública** y, a continuación, seleccione **Configurar**.

   :::image type="content" source="media/public-ip-usage/connectivity-public-ip-tab.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/connectivity-public-ip-tab.png":::

1. Acepte los valores predeterminados o cámbielos; a continuación, seleccione **Crear**.

   - Grupo de recursos de la red de área extensa virtual

   - Nombre de la red de área extensa virtual

   - Bloque de direcciones de concentrador virtual (con el nuevo intervalo de direcciones IP no superpuestas)

   - Número de IP públicas (1-100)

Se tarda aproximadamente una hora en completar la implementación de todos los componentes. Esta implementación solo tiene que producirse una vez para admitir todas las IP públicas futuras en este entorno de Azure VMware Solution.  

>[!TIP]
>Puede supervisar el estado en el área de **Notificación**. 

## <a name="view-and-add-public-ip-addresses"></a>Ver y agregar direcciones IP públicas

Para comprobar y agregar más direcciones IP públicas, siga los pasos que se indican a continuación.

1. En Azure Portal, busque y seleccione **Firewall**.

1. Seleccione un firewall implementado y, a continuación, seleccione **Visit Azure Firewall Manager to configure and manage this firewall** (Visite Azure Firewall Manager para configurar y administrar este firewall).

   :::image type="content" source="media/public-ip-usage/configure-manage-deployed-firewall.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/configure-manage-deployed-firewall.png":::

1. Seleccione la opción de los **Centros virtuales protegidos** y, en la lista, seleccione un centro virtual.

   :::image type="content" source="media/public-ip-usage/select-virtual-hub.png" alt-text="Diagrama de la arquitectura de la IP pública" lightbox="media/public-ip-usage/select-virtual-hub.png":::

1. En la página del centro virtual, seleccione la **Configuración de IP pública** y, para agregar más direcciones IP públicas, seleccione **Agregar**. 

   :::image type="content" source="media/public-ip-usage/virtual-hub-page-public-ip-configuration.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/virtual-hub-page-public-ip-configuration.png":::

1. Proporcione el número de direcciones IP necesarias y seleccione **Agregar**.

   :::image type="content" source="media/public-ip-usage/add-number-of-ip-addresses-required.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true":::


## <a name="create-firewall-policies"></a>Creación de directivas de firewall

Una vez que se implementan todos los componentes, puede verlos en el grupo de recursos agregado. El siguiente paso es agregar una directiva de firewall.

1. En Azure Portal, busque y seleccione **Firewall**.

1. Seleccione un firewall implementado y, a continuación, seleccione **Visit Azure Firewall Manager to configure and manage this firewall** (Visite Azure Firewall Manager para configurar y administrar este firewall).

   :::image type="content" source="media/public-ip-usage/configure-manage-deployed-firewall.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/configure-manage-deployed-firewall.png":::

1. Seleccione **Directivas de Azure Firewall** y, a continuación, seleccione **Crear una directiva de Azure Firewall**.

   :::image type="content" source="media/public-ip-usage/create-firewall-policy.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/create-firewall-policy.png":::

1. En la pestaña **Básico**, proporcione los detalles necesarios y seleccione **Siguiente: Configuración de DNS**. 

1. En la pestaña **DNS**, seleccione **Deshabilitar** y, a continuación, seleccione **Siguiente: Las reglas**.

1. Seleccione **Agregar una colección de reglas**, proporcione los detalles siguientes y seleccione **Agregar**; a continuación, seleccione **Siguiente: Inteligencia sobre amenazas**.

   -  Name
   -  Tipo de colección de reglas: DNAT
   -  Prioridad
   -  Acción de la colección de reglas: permitir
   -  Nombre de la regla
   -  Tipo de origen: **IPaddress**
   -  Origen: **\***
   -  Protocolo **TCP**
   -  Puerto de destino: **80**
   -  Tipo de destino: **Dirección IP**
   -  Destino: **Dirección IP pública**
   -  Dirección traducida: **dirección IP privada del servidor web de Azure VMware Solution**
   -  Puerto traducido: **puerto del servidor web de Azure VMware Solution**

1. Deje el valor predeterminado y seleccione **Siguiente: Centros de conectividad**.

1. Seleccione **Asociar centros virtuales**.

   :::image type="content" source="media/public-ip-usage/associate-virtual-hubs-azure-firewall-policy.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/associate-virtual-hubs-azure-firewall-policy.png":::

1. Seleccione un centro de la lista y seleccione **Agregar**.

   :::image type="content" source="media/public-ip-usage/secure-hubs-with-azure-firewall-polcy.png" alt-text="Diagrama de la arquitectura de la IP pública" border="true" lightbox="media/public-ip-usage/secure-hubs-with-azure-firewall-polcy.png":::.

1. Seleccione **Siguiente: Etiquetas**. 

1. (Opcional) Cree pares de nombre/valor para clasificar los recursos. 

1. Seleccione **Siguiente: revisar y crear** y, a continuación, seleccione **Crear**.

## <a name="limitations"></a>Limitaciones

Puede tener 100 direcciones IP públicas por SDDC.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el uso de direcciones IP públicas mediante [Azure Virtual WAN](../virtual-wan/virtual-wan-about.md).

