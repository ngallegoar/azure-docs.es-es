---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1199819d274590cc81d0234680f8765f9cc36c0a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "67186151"
---
### <a name="to-modify-local-network-gateway-ip-address-prefixes---no-gateway-connection"></a><a name="noconnection"></a>Para modificar los prefijos de dirección IP de la puerta de enlace de red local (sin conexión de puerta de enlace)

#### <a name="to-add-additional-address-prefixes"></a>Para agregar prefijos de dirección adicionales:

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. Agregue el espacio de direcciones IP en el cuadro *Agregar otro intervalo de direcciones*.
3. Haga clic en **Guardar** para guardar la configuración.

#### <a name="to-remove-address-prefixes"></a>Para quitar prefijos de dirección:

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. Haga clic en **"..."** en la línea que contiene el prefijo que desea quitar.
3. Haga clic en **Quitar**.
4. Haga clic en **Guardar** para guardar la configuración.

### <a name="to-modify-local-network-gateway-ip-address-prefixes---existing-gateway-connection"></a><a name="withconnection"></a>Para modificar los prefijos de dirección IP de la puerta de enlace de red local (conexión de puerta de enlace existente)

Si tiene una conexión de puerta de enlace y desea agregar o quitar los prefijos de dirección IP contenidos en la puerta de enlace de red local, tendrá que realizar los pasos siguientes en orden. Esto tendrá como resultado un tiempo de inactividad para la conexión VPN. Al modificar los prefijos de dirección IP, no es necesario eliminar la puerta de enlace VPN. Basta con quitar la conexión.

#### <a name="1-remove-the-connection"></a>1. Cierre la conexión.

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Conexiones**.
2. Haga clic en **...** en la línea de cada conexión y, después, haga clic en **Eliminar**.
3. Haga clic en **Guardar** para guardar la configuración.

#### <a name="2-modify-the-address-prefixes"></a>2. Modifique los prefijos de las direcciones.

Para agregar prefijos de dirección adicionales:

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. Agregue el espacio de direcciones IP.
3. Haga clic en **Guardar** para guardar la configuración.

Para quitar prefijos de dirección:

1. En el recurso de la puerta de enlace de red local, en la sección **Configuración**, haga clic en **Configuración**.
2. Haga clic en **...** en la línea que contiene el prefijo que desea quitar.
3. Haga clic en **Quitar**.
4. Haga clic en **Guardar** para guardar la configuración.

#### <a name="3-recreate-the-connection"></a>3. Vuelva a crear la conexión.

1. Navegue hasta la puerta de enlace de red virtual de la red virtual. (no la puerta de enlace de red local).
2. En la puerta de enlace de red virtual, en la sección **Configuración**, haga clic en **Conexiones**.
3. Haga clic en **+ Agregar** para abrir la hoja **Agregar conexión**.
4. Vuelva a crear la conexión.
5. Haga clic en **Aceptar** para crear la conexión.