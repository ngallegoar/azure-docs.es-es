---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/29/2020
ms.author: cherylmc
ms.openlocfilehash: 15c29648e42ba190991d51188489883e29bee165
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93041555"
---
>[!NOTE]
>Debe tener derechos de administrador en el equipo cliente de Windows desde el que se va a conectar.
>

1. Para conectarse a su red virtual, en el equipo cliente, vaya a la configuración de VPN y localice la conexión VPN que creó. Tiene el mismo nombre que su red virtual. Seleccione **Conectar**. Es posible que aparezca un mensaje emergente que haga referencia al uso del certificado. Seleccione **Continuar** para usar privilegios elevados.

1. En la página de estado **Conexión** , seleccione **Conectar** para iniciar la conexión. Si ve una pantalla para **Seleccionar certificado** , compruebe que el certificado de cliente que se muestra es el que desea utilizar para conectarse. Si no es así, use la flecha de lista desplegable para seleccionar el certificado correcto y, luego, seleccione **Aceptar**.

   :::image type="content" source="./media/vpn-gateway-p2s-connect-windows-client/connection-status.png" alt-text="Conectarse desde un equipo Windows":::

1. Se ha establecido la conexión.

   :::image type="content" source="./media/vpn-gateway-p2s-connect-windows-client/connected.png" alt-text="Conexión de un equipo a una red virtual de Azure: diagrama de conexión de punto a sitio":::
