---
title: Enrutamiento del tráfico a través de NVA mediante una configuración personalizada
titleSuffix: Azure Virtual WAN
description: Este escenario le ayuda a enrutar el tráfico a través de las NVA mediante una NVA diferente para el tráfico vinculado a Internet.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 122e76e4bde96823ff18207bc24df4a8e91afb1c
ms.sourcegitcommit: 59f506857abb1ed3328fda34d37800b55159c91d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92517975"
---
# <a name="scenario-route-traffic-through-nvas-by-using-custom-settings"></a>Escenario: Enrutamiento del tráfico a través de NVA mediante una configuración personalizada

Al trabajar con el enrutamiento de centros virtuales de Azure Virtual WAN, dispone de varias opciones. Este artículo se centra en el enrutamiento del tráfico a través de una aplicación virtual de red (NVA) para la comunicación entre redes virtuales y ramas, y usar otra NVA para el tráfico vinculado a Internet. Para obtener más información, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="design"></a>Diseño

* **Radios** para redes virtuales conectadas al centro virtual. (Por ejemplo, VNet 1, VNet 2 y VNet 3 en el diagrama más adelante en este artículo).
* **Red virtual de servicio** para las redes virtuales en las que los usuarios han implementado una NVA para inspeccionar el tráfico que no procede de Internet, y posiblemente con servicios comunes a los que se accede mediante radios. (Por ejemplo, VNet 4 en el diagrama más adelante en este artículo). 
* **Red virtual perimetral** para las redes virtuales en las que los usuarios han implementado una NVA que se usará para inspeccionar el tráfico enlazado a Internet. (Por ejemplo, VNet 5 en el diagrama más adelante en este artículo).
* **Centros** para los centros de Virtual WAN administrados por Microsoft.

En la tabla siguiente se resumen las conexiones que se admiten en este escenario:

| De          | En|Radios|VNet de servicio|Ramas|Internet|
|---|---|:---:|:---:|:---:|:---:|:---:|
| **Radios**| ->| Directamente |Directamente | a través de la red virtual de servicio |a través de la red virtual perimetral |
| **VNet de servicio**| ->| Directamente |N/D| Directamente | |
| **Ramas** | ->| a través de la red virtual de servicio |Directamente| Directamente |  |

En cada una de las celdas de la matriz se describe si la conectividad fluye directamente a través de Virtual WAN o de una de las redes virtuales con una NVA. 

Tenga en cuenta los siguientes detalles:
* Radios:
  * los radios llegarán a otros radios directamente a través de los concentradores de Virtual WAN.
  * Los radios obtendrán conectividad a las ramas a través de una ruta estática que apunta a la VNet de servicio. No conocen los prefijos específicos de las ramas, porque son más específicos e invalidan el resumen.
  * Los radios enviarán el tráfico de Internet a la red virtual perimetral mediante un emparejamiento de VNet directo.
* las ramas llegarán a los radios a través de un enrutamiento estático que apunta a la VNet de servicio. No conocen los prefijos específicos de las redes virtuales que invalidan la ruta estática resumida.
* La red virtual de servicio será similar a una red virtual de servicios compartidos que debe ser accesible desde todas las redes virtuales y todas las ramas.
* No es necesario que la red virtual perimetral tenga conectividad a través de Virtual WAN, porque el único tráfico que admitirá proviene de emparejamientos de redes virtuales directos. Pero para simplificar la configuración, use el mismo modelo de conectividad que para la red virtual perimetral.

Hay tres patrones de conectividad distintos, lo que se traduce en tres tablas de rutas. Las asociaciones a las distintas redes virtuales son las siguientes:

* Radios:
  * Tabla de enrutamiento asociada: **RT_V2B**
  * Propagación a tablas de enrutamiento: **RT_V2B** y **RT_SHARED**
* Redes virtuales de NVA (red virtual de servicio y red virtual DMZ):
  * Tabla de enrutamiento asociada: **RT_SHARED**
  * Propagación a tablas de enrutamiento: **RT_SHARED**
* Ramas:
  * Tabla de enrutamiento asociada: **Valor predeterminado**
  * Propagación a tablas de enrutamiento: **RT_SHARED** y **Predeterminada**

Estas rutas estáticas garantizan que el tráfico hacia y desde la red virtual y la rama atraviesa la NVA en la red virtual de servicio (VNet 4):

| Descripción | Tabla de rutas | Ruta estática              |
| ----------- | ----------- | ------------------------- |
| Ramas    | RT_V2B      | 10.2.0.0/16-> vnet4conn  |
| Radios de NVA  | Valor predeterminado     | 10.1.0.0/16 -> vnet4conn  |

Ahora puede usar Virtual WAN para seleccionar la conexión correcta a la que enviar los paquetes. También debe usar Virtual WAN para seleccionar la acción correcta que se realizará al recibir esos paquetes. Para ello, use las tablas de rutas de conexión como se indica a continuación:

| Descripción | Conexión | Ruta estática            |
| ----------- | ---------- | ----------------------- |
| VNet2Branch | vnet4conn  | 10.2.0.0/16 -> 10.4.0.5 |
| Branch2VNet | vnet4conn  | 10.1.0.0/16 -> 10.4.0.5 |

Para obtener más información, consulte [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).

## <a name="architecture"></a>Architecture

Este es un diagrama de la arquitectura que se ha descrito anteriormente en el artículo.

Hay un centro, llamado **Centro 1**.

* El **Concentrador 1** está conectado directamente a las VNet de NVA **VNet 4** y **VNet 5**.

* Se espera que el tráfico entre las redes virtuales 1, 2 y 3, y las ramas pase por la **NVA VNet 4**  10.4.0.5.

* Se espera que todo el tráfico vinculado a Internet desde las redes virtuales 1, 2 y 3 pase por la **NVA VNet 5**  10.5.0.5.

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-1.png" alt-text="Diagrama de una arquitectura de red.":::

## <a name="workflow"></a>Flujo de trabajo

Para configurar el enrutamiento a través de NVA, estos son los pasos que debe tener en cuenta:

1. Para que el tráfico vinculado a Internet pase por la VNet 5, es necesario que las redes virtuales 1, 2 y 3 se conecten directamente a la 5 a través del emparejamiento de red virtual. También es necesario configurar una ruta definida por el usuario en las redes virtuales para 0.0.0.0/0 y el próximo salto 10.5.0.5. Actualmente, Virtual WAN no permite una NVA de próximo salto en el centro virtual para 0.0.0.0/0.

1. En Azure Portal, vaya al centro virtual y cree una tabla de rutas personalizada con el nombre **RT_Shared**. En esta tabla, las rutas se obtienen a través de la propagación desde todas las redes virtuales y las conexiones de rama. Puede ver esta tabla vacía en el diagrama siguiente.

   * **Rutas:** no es necesario agregar ninguna ruta estática.

   * **Asociación:** seleccione las redes virtuales 4 y 5, lo que significa que sus conexiones se asocian a la tabla de rutas **RT_Shared**.

   * **Propagación:** como quiere que todas las ramas y conexiones de red virtual propaguen sus rutas dinámicamente a esta tabla de rutas, seleccione las ramas y todas las redes virtuales.

1. Cree una tabla de rutas personalizada con el nombre **RT_V2B** para dirigir el tráfico desde las redes virtuales 1, 2 y 3 a las ramas.

   * **Rutas:** agregue una entrada de ruta estática agregada para las ramas, con el próximo salto como la conexión de la red virtual 4. Configure una ruta estática en la conexión de la red virtual 4 para los prefijos de rama, e indique que el próximo salto sea la dirección IP específica de la NVA en la red virtual 4.

   * **Asociación:** seleccione las VNet 1, 2 y 3. Esto hará que las conexiones de red virtual 1, 2 y 3 se asociarán a esta tabla de rutas y podrán aprender las rutas (estáticas y dinámicas mediante propagación) en esta tabla de rutas.

   * **Propagación:** Las conexiones propagan las rutas a las tablas de rutas. La selección de las redes virtuales 1, 2 y 3 habilita la propagación de rutas desde estas redes virtuales hasta esta tabla de rutas. No es necesario propagar las rutas desde las conexiones de rama a **RT_V2B** , ya que el tráfico de la red virtual de rama pasa por la NVA en la red virtual 4.
  
1. Edite la tabla de rutas predeterminada, **DefaultRouteTable**.

   Todas las conexiones VPN, Azure ExpressRoute y VPN de usuario están asociadas a la tabla de rutas predeterminada. Todas las conexiones VPN, ExpressRoute y VPN de usuario propagan rutas al mismo conjunto de tablas de rutas.

   * **Rutas:** agregue una entrada de ruta estática agregada para las redes virtuales 1, 2 y 3, con el próximo salto como la conexión de la red virtual 4. Configure una ruta estática en la conexión de la red virtual 4 para los prefijos agregados de las redes virtuales 1, 2 y 3, e indique que el próximo salto sea la dirección IP específica de la NVA en la red virtual 4.

   * **Asociación:** asegúrese de que la opción para las ramas (VPN/ER/P2S) esté seleccionada, y de que las conexiones de rama locales estén asociadas a la tabla de rutas predeterminada.

   * **Propagación:** asegúrese de que la opción para las ramas (VPN/ER/P2S) esté seleccionada, y de que las conexiones de rama locales estén asociadas a la tabla de rutas predeterminada.

:::image type="content" source="./media/routing-scenarios/nva-custom/figure-2.png" alt-text="Diagrama del flujo de trabajo." lightbox="./media/routing-scenarios/nva-custom/figure-2.png":::

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte las [preguntas más frecuentes](virtual-wan-faq.md).
* Para obtener más información sobre el enrutamiento de centros virtuales, vea [Acerca del enrutamiento de centros virtuales](about-virtual-hub-routing.md).
