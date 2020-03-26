---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 11/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4bcee1097010bb8746b11185a470ca2584485c3f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "73488973"
---
1. En la página del portal de la WAN virtual, en la sección **Conectividad**, seleccione **Sitios VPN** para abrir la página de sitios VPN.
2. En la página **Sitios de VPN**, haga clic en **+Crear sitio**.

   ![Conceptos básicos](./media/virtual-wan-tutorial-site-include/basics.png "Aspectos básicos")
3. Dentro de la página **Crear un sitio VPN**, en la pestaña **Aspectos básicos** rellene los campos siguientes:

    * **Región**: anteriormente se conocía como ubicación. Se trata de la ubicación en la que desea crear este recurso de sitio.
    * **Nombre**: el nombre que desee usar para hacer referencia a su sitio local.
    * **Proveedor del dispositivos**: el nombre del proveedor del dispositivo VPN (por ejemplo: Citrix, Cisco, Barracuda). Esto puede ayudar al equipo de Azure a comprender mejor su entorno para agregar las posibilidades de optimización adicionales en el futuro, así como para ayudarle a solucionar problemas.
    * **Border Gateway Protocol**: su habilitación implica que todas las conexiones del sitio estarán habilitadas para BGP. En su momento configurará la información de BGP para cada vínculo desde el sitio VPN en la sección Vínculos. La configuración de BGP en una instancia de Virtual WAN es equivalente a la configuración de BGP en una VPN de puerta de enlace de red virtual de Azure. Su dirección del par BGP local no puede ser la misma que la dirección IP pública del dispositivo VPN, ni que el espacio de direcciones de red virtual del sitio VPN. Use otra dirección IP en el dispositivo VPN para la dirección IP del par BGP. Puede ser una dirección asignada a la interfaz de bucle invertido en el dispositivo. Sin embargo, no puede ser una dirección APIPA (169.254.x.x). Especifique esta dirección en el sitio VPN correspondiente que representa la ubicación. Para conocer los requisitos previos de BGP, consulte [Acerca de BGP con Azure VPN Gateway](../articles/vpn-gateway/vpn-gateway-bgp-overview.md). Siempre tiene la opción de editar una conexión VPN para actualizar sus parámetros BGP (IP de emparejamiento en el vínculo y el número de espacio de direcciones) una vez que esté habilitado el valor de BGP del sitio VPN.
    * **Espacio de direcciones privadas**: es el espacio de direcciones IP que se encuentra en el sitio local. El tráfico destinado a este espacio de direcciones se enruta al sitio local. Esto es necesario cuando no se habilita BGP para el sitio.
    * **Centros de conectividad**: el centro de conectividad al que desea que se conecte el sitio. Un sitio solo se puede conectar a los centros de conectividad que tienen una instancia de VPN Gateway. Si no puede visualizar un centro de conectividad, cree primero una puerta de enlace de VPN en ese centro.
4. Seleccione **Vínculos** para agregar información sobre los vínculos físicos en la rama. Si tiene un dispositivo CPE de asociado de Virtual WAN, consulte con su asociado para ver si esta información se intercambia con Azure como parte de la configuración de carga de información de la rama desde sus sistemas.

   ![vínculos](./media/virtual-wan-tutorial-site-include/links.png "Vínculos")

    * **Nombre del vínculo**: el nombre que quiera proporcionar para el vínculo físico en el sitio VPN. Ejemplo: mylink1.
    * **Nombre de proveedor**: el nombre del vínculo físico en el sitio VPN. Ejemplo: ATT, Verizon.
    * **Velocidad**: esta es la velocidad del dispositivo VPN en la ubicación de la rama. Ejemplo: 50, que significa que 50 Mbps es la velocidad del dispositivo VPN en el sitio de la rama.
    * **Dirección IP**: dirección IP pública del dispositivo local utilizado en este vínculo. Opcionalmente, puede proporcionar la dirección IP privada del dispositivo VPN local que está detrás de ExpressRoute.
5. Puede usar la casilla para eliminar o agregar vínculos adicionales. Se admiten cuatro vínculos por cada sitio VPN. Por ejemplo, si tiene cuatro ISP (proveedor de servicios de Internet) en la ubicación de la rama, puede crear cuatro vínculos. uno por cada ISP y proporcionar la información de cada vínculo.
6. Una vez que haya terminado de rellenar los campos, seleccione **Revisar y crear** para comprobar y crear el sitio.
7. Vea el estado en la página de sitios de VPN. El sitio irá a **Es necesaria una conexión** porque el sitio aún no se ha conectado al centro de conectividad.