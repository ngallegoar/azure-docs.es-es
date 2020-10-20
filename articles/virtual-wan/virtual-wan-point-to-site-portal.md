---
title: 'Tutorial: Uso de Azure Virtual WAN para crear una conexión de punto a sitio a Azure'
description: En este tutorial, aprenda a usar Azure Virtual WAN para crear una conexión VPN de punto a sitio a Azure.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: tutorial
ms.date: 10/06/2020
ms.author: cherylmc
ms.openlocfilehash: 3d03d0267ff4fb16042d5cc2016e87139b88281a
ms.sourcegitcommit: 1b47921ae4298e7992c856b82cb8263470e9e6f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/14/2020
ms.locfileid: "92056589"
---
# <a name="tutorial-create-a-user-vpn-connection-using-azure-virtual-wan"></a>Tutorial: Creación de una conexión VPN de usuario mediante Azure Virtual WAN

Este tutorial muestra cómo usar Virtual WAN para conectarse a los recursos de Azure a través de una conexión VPN de OpenVPN o IPsec/IKE (IKEv2). Este tipo de conexión requiere que se configure un cliente en el equipo cliente. Para más información sobre Virtual WAN, consulte la [Introducción a Virtual WAN](virtual-wan-about.md)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una red de área extensa (WAN)
> * Creación de una configuración de P2S
> * Crear un concentrador
> * Especificación de los servidores DNS
> * Descarga de un perfil de cliente VPN
> * Visualizar la instancia de Virtual WAN

![Diagrama de Virtual WAN](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Before beginning](../../includes/virtual-wan-before-include.md)]

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Creación de una instancia de Virtual WAN

[!INCLUDE [Create a virtual WAN](../../includes/virtual-wan-create-vwan-include.md)]

## <a name="create-a-p2s-configuration"></a><a name="p2sconfig"></a>Creación de una configuración de P2S

Una configuración de punto a sitio (P2S) define los parámetros para conectar clientes remotos.

[!INCLUDE [Create client profiles](../../includes/virtual-wan-p2s-configuration-include.md)]

## <a name="create-hub-with-point-to-site-gateway"></a><a name="hub"></a>Creación de un centro de conectividad con puerta de enlace de punto a sitio

[!INCLUDE [Create hub](../../includes/virtual-wan-p2s-hub-include.md)]

## <a name="specify-dns-server"></a><a name="dns"></a>Especificación del servidor DNS

Las puertas de enlace VPN del usuario de la red WAN virtual permiten especificar hasta 5 servidores DNS. Puede configurar estos durante el proceso de creación del centro de conectividad o realizar modificaciones posteriormente. Para ello, busque el centro de conectividad virtual. En **User VPN (point to site)** (VPN de usuario [punto a sitio]), seleccione **configure** (configurar) y escriba las direcciones IP del servidor de DNS en los cuadros de texto **Custom DNS Servers** (Servidores DNS personalizados).

   :::image type="content" source="media/virtual-wan-point-to-site-portal/custom-dns.png" alt-text="DNS personalizado" lightbox="media/virtual-wan-point-to-site-portal/custom-dns-expand.png":::

## <a name="download-vpn-profile"></a><a name="download"></a>Descarga del perfil de VPN

Use el perfil de VPN para configurar los clientes.

[!INCLUDE [Download profile](../../includes/virtual-wan-p2s-download-profile-include.md)]

### <a name="configure-user-vpn-clients"></a>Configuración de clientes VPN de usuario

Use el perfil descargado para configurar los clientes de acceso remoto. El procedimiento de cada sistema operativo es diferente, siga las instrucciones apropiadas para su sistema.

[!INCLUDE [Configure clients](../../includes/virtual-wan-p2s-configure-clients-include.md)]

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Visualización de la instancia de Virtual WAN

1. Vaya a la instancia de Virtual WAN.
1. En la página **Información general**, cada punto del mapa representa un centro de conectividad.
1. En la sección de **centros de conectividad y conexiones**, puede ver estado del centro de conectividad, sitio, región, estado de la conexión VPN y bytes de entrada y salida.

## <a name="clean-up-resources"></a><a name="cleanup"></a>Limpieza de recursos

Cuando ya no necesite estos recursos, puede usar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene. Reemplace "myResourceGroup" con el nombre del grupo de recursos y ejecute el siguiente comando de PowerShell:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre Virtual WAN en:

> [!div class="nextstepaction"]
> * [Preguntas más frecuentes sobre Virtual WAN](virtual-wan-faq.md)
