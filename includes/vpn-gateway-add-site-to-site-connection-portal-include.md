---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 5149973fe63f867b49e55c970779c005e12536b9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "68780155"
---
1. Abra la página de la puerta de enlace de red virtual. Hay varias formas de navegar. Para navegar a la puerta de enlace, vaya a **Nombre de la red virtual -> Información general -> Dispositivos conectados -> Nombre de la puerta de enlace**.
2. En la página de la puerta de enlace, haga clic en **Conexiones**. En la parte superior de la página Conexiones, haga clic en **+Agregar** para abrir la página **Agregar conexión**.

   ![Creación de una conexión de sitio a sitio](./media/vpn-gateway-add-site-to-site-connection-portal-include/configure-site-to-site-connection.png)
3. En la página **Agregar conexión**, configure los valores de la conexión.

   - **Nombre**: asigne un nombre a la conexión.
   - **Tipo de conexión:** seleccione **Sitio a sitio (IPSec)** .
   - **Puerta de enlace de red virtual:**  el valor es fijo porque se conecta desde esta puerta de enlace.
   - **Puerta de enlace de red local:** haga clic en **Elegir una puerta de enlace de red local** y seleccione la puerta de enlace de red local que desea utilizar.
   - **Clave compartida:** este valor debe ser el mismo que el que usa para el dispositivo VPN local. En el ejemplo se usa "abc123", pero puede (y debería) utilizar algo más complejo. Lo importante es que el valor que especifique aquí debe ser el mismo que el que se especificó al configurar el dispositivo VPN.
   - Los restantes valores de **Suscripción**, **Grupo de recursos** y **Ubicación** son fijos.

4. Haga clic en **Aceptar** para crear la conexión. El mensaje *Creando la conexión* aparecerá de forma intermitente en la pantalla.
5. La conexión se puede ver en la página **Conexiones** de la puerta de enlace de red virtual. El valor de Estado pasará de *Desconocido* a *Conectando* y luego a *Correcto*.
