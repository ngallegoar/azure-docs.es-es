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
ms.openlocfilehash: 9c18a2c74d03a636a0865f3008eb421ab8d7412d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2020
ms.locfileid: "68781492"
---
1. En el portal, haga clic en **+Crear un recurso**.
2. En el cuadro de búsqueda, escriba **puerta de enlace de red local**, a continuación, presione **Entrar** para buscar. Se devolverá una lista de resultados. Haga clic en **Puerta de enlace de red local** y haga clic en el botón **Crear** para abrir la página **Crear puerta de enlace de red local**.

   ![Creación de la puerta de enlace de red local](./media/vpn-gateway-add-lng-rm-portal-include/local-network-gateway.png "Creación de la puerta de enlace de red local")

3. En la página **Crear puerta de enlace de red local**, especifique los valores de la puerta de enlace de red local.

   - **Nombre:** especifique el nombre del objeto de puerta de enlace de red local.
   - **Dirección IP:** es la dirección IP pública del dispositivo VPN al que desea que Azure se conecte. Especifique una dirección IP pública válida. Si no tiene la dirección IP en este momento, puede usar los valores que se muestran en el ejemplo, pero deberá volver y reemplazar la dirección IP del marcador de posición por la dirección IP pública de su dispositivo VPN. Si no lo hace, Azure no podrá conectarse.
   - **Espacio de direcciones** hace referencia a los intervalos de direcciones de la red que representa esta red local. Puede agregar varios intervalos de espacios de direcciones. Asegúrese de que los intervalos que especifique aquí no se superpongan con los de otras redes a las que quiera conectarse. Azure enrutará el intervalo de direcciones que especifique a la dirección IP del dispositivo VPN local. *Use sus propios valores aquí, y no los mostrados en el ejemplo, si quiere conectarse a su sitio local*.
   - **Configurar BGP:** solo se debe usar al configurar BGP. En caso contrario, no seleccione esta opción.
   - **Suscripción:** compruebe que se muestra la suscripción correcta.
   - **Grupo de recursos:** seleccione el grupo de recursos que desea utilizar. Puede crear un grupo de recursos nuevo o seleccionar uno ya creado.
   - **Ubicación:** seleccione la ubicación en la que se creará este objeto. Puede seleccionar la misma ubicación en la que reside la red, pero no es obligatorio.

4. Cuando haya terminado de especificar los valores, haga clic en el botón **Crear** para crear la puerta de enlace de red local.
