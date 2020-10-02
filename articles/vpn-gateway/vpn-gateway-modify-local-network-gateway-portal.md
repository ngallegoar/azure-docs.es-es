---
title: 'VPN Gateway: modificar la configuración de la dirección IP de puerta de enlace: Portal de Azure'
description: Este artículo explica paso a paso cómo cambiar los prefijos de direcciones IP de la puerta de enlace de red local con Azure Portal.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/16/2020
ms.author: cherylmc
ms.openlocfilehash: af3513c4a4f3b3187e85c65de51ad2e6e2d7279c
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2020
ms.locfileid: "90983182"
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Modificación de la configuración de la puerta de enlace de red local mediante Azure Portal

A veces, cambia la configuración de la puerta de enlace de red local AddressPrefix o GatewayIPAddress. Este artículo muestra cómo modificar la configuración de puerta de enlace de red local. También puede modificar estas configuraciones mediante un método diferente, si selecciona una opción diferente de la lista siguiente:

Antes de eliminar la conexión, es posible que quiera descargar la configuración para los dispositivos de conexión con el fin de obtener la PSK definida. De este modo, no tendrá que volver a definirla en el otro lado.

> [!div class="op_single_selector"]
> * [Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [CLI de Azure](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="local-network-gateway-configuration"></a><a name="configure-lng"></a>Configuración de la puerta de enlace de red local

La siguiente captura de pantalla muestra la página **Configuración** de un recurso de puerta de enlace de red local mediante el punto de conexión de dirección IP pública:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/ip-address.png" alt-text="Configuración de una puerta de enlace de red local: dirección IP":::

Esta es la misma página de configuración con un punto de conexión de FQDN:

:::image type="content" source="./media/vpn-gateway-modify-local-network-gateway-portal/fqdn.png" alt-text="Configuración de una puerta de enlace de red local: dirección IP":::

## <a name="modify-the-gateway-ip-address"></a><a name="ip"></a>Modificación de la dirección IP de la puerta de enlace

Si el dispositivo VPN al que desea conectarse ha cambiado su dirección IP pública, debe modificar la puerta de enlace de red local para reflejar ese cambio.

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. En el cuadro **Dirección IP**, modifique la dirección IP.
3. Haga clic en **Guardar** para guardar la configuración.

## <a name="modify-the-gateway-fqdn"></a><a name="fqdn"></a>Modificación del FQDN de la puerta de enlace

Si el dispositivo VPN al que desea conectarse ha cambiado su nombre de dominio completo (FQDN), debe modificar la puerta de enlace de red local para reflejar ese cambio.

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. En el cuadro **FQDN**, modifique el nombre de dominio.
3. Haga clic en **Guardar** para guardar la configuración.

> No se puede cambiar una puerta de enlace de red local entre el punto de conexión FQDN y el punto de conexión de dirección IP. Debe eliminar todas las conexiones asociadas a esta puerta de enlace de red local, crear una nueva con el nuevo punto de conexión (dirección IP o FQDN) y, a continuación, volver a crear las conexiones.

## <a name="modify-ip-address-prefixes"></a><a name="ipaddprefix"></a>Modificación de los prefijos de direcciones IP

### <a name="to-add-additional-address-prefixes"></a>Para agregar prefijos de dirección adicionales:

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. Agregue el espacio de direcciones IP en el cuadro *Agregar otro intervalo de direcciones*.
3. Haga clic en **Guardar** para guardar la configuración.

### <a name="to-remove-address-prefixes"></a>Para quitar prefijos de dirección:

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. Haga clic en **"..."** en la línea que contiene el prefijo que desea quitar.
3. Haga clic en **Quitar**.
4. Haga clic en **Guardar** para guardar la configuración.

## <a name="modify-bgp-settings"></a><a name="bgp"></a>Modificación de la configuración de BGP

### <a name="to-add-or-update-bgp-settings"></a>Para agregar o actualizar la configuración de BGP:

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. Seleccione **"Configurar BGP"** para mostrar o actualizar las configuraciones de BGP para esta puerta de enlace de red local.
3. Agregue o actualice el número de sistema autónomo o la dirección IP del par BGP en los campos correspondientes.
4. Haga clic en **Guardar** para guardar la configuración.

### <a name="to-remove-bgp-settings"></a>Para quitar la configuración de BGP:

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. Anule la selección de la opción **Configurar BGP** para quitar el ASN de BGP existente y la dirección IP del par BGP.
3. Haga clic en **Guardar** para guardar la configuración.

## <a name="next-steps"></a>Pasos siguientes

Puede comprobar la conexión de la puerta de enlace. Consulte [Comprobación de una conexión de puerta de enlace](vpn-gateway-verify-connection-resource-manager.md).
