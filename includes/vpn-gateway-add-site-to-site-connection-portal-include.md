---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/22/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 48f33514510618abadf329a11a9ab71a020be0bd
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2020
ms.locfileid: "92479587"
---
1. Abra la página de la puerta de enlace de red virtual. Para desplazarse a la puerta de enlace, vaya a **Nombre de la red virtual -> Información general -> Dispositivos conectados -> Nombre de la puerta de enlace**, aunque hay otras formas de hacerlo.
1. En la página de la puerta de enlace, seleccione **Conexiones**. En la parte superior de la página Conexiones, seleccione **+Agregar** para abrir la página **Agregar conexión**.

   :::image type="content" source="./media/vpn-gateway-add-site-to-site-connection-portal-include/connection.png" alt-text="Conexión de sitio a sitio":::
1. En la página **Agregar conexión**, configure los valores de la conexión.

   * **Nombre:** asigne un nombre a la conexión.
   * **Tipo de conexión**: Seleccione **Sitio a sitio (IPSec)** .
   * **Puerta de enlace de red virtual:** el valor es fijo porque se conecta desde esta puerta de enlace.
   * **Puerta de enlace de red local:** Seleccione **Elegir una puerta de enlace de red local** y seleccione la puerta de enlace de red local que quiere utilizar.
   * **Clave compartida:** este valor debe ser el mismo que el que usa para el dispositivo VPN local. En el ejemplo se usa "abc123", pero puede (y debería) utilizar algo más complejo. Lo importante es que el valor que especifique aquí debe ser el mismo que el que se especificó al configurar el dispositivo VPN.
   * Deje la casilla **Usar la dirección IP privada de Azure** desactivada.
   * Deje la casilla **Habilitar BGP** desactivada.
   * Seleccione **IKEv2**.
   * Los restantes valores de **Suscripción**, **Grupo de recursos** y **Ubicación** son fijos.

1. Haga clic en **Aceptar** para crear la conexión. El mensaje *Creando la conexión* aparecerá de forma intermitente en la pantalla.
1. La conexión se puede ver en la página **Conexiones** de la puerta de enlace de red virtual. El valor de Estado pasará de *Desconocido* a *Conectando* y luego a *Correcto*.
