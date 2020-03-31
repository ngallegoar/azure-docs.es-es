---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9648f4c16e8d266bbdd504d4a7599b67a5c846b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "73523802"
---
1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso**. 

   ![Crear un recurso en Azure Portal](./media/vpn-gateway-add-local-network-gateway-portal-include/azure-portal-create-resource.png)
2. En el campo **Buscar en Marketplace**, escriba **Puerta de enlace de red local** y presione **Entrar** para comenzar la búsqueda. Se devolverá una lista de resultados. Haga clic en **Puerta de enlace de red local** y haga clic en el botón **Crear** para abrir la página **Crear puerta de enlace de red local**.

   ![Creación de la puerta de enlace de red local](./media/vpn-gateway-add-local-network-gateway-portal-include/create-local-network-gateway.png "Creación de la puerta de enlace de red local")

3. En la página **Crear puerta de enlace de red local**, especifique los valores de la puerta de enlace de red local.

   - **Nombre:** especifique el nombre del objeto de puerta de enlace de red local.
   - **Dirección IP**: es la dirección IP pública del dispositivo VPN al que desea que Azure se conecte. Especifique una dirección IP pública válida. Si no tiene la dirección IP en este momento, puede usar los valores que se muestran en el ejemplo, pero deberá volver y reemplazar la dirección IP del marcador de posición por la dirección IP pública de su dispositivo VPN. Si no lo hace, Azure no podrá conectarse.
   - **Espacio de direcciones** hace referencia a los intervalos de direcciones de la red que representa esta red local. Puede agregar varios intervalos de espacios de direcciones. Asegúrese de que los intervalos que especifique aquí no se superpongan con los de otras redes a las que quiera conectarse. Azure enrutará el intervalo de direcciones que especifique a la dirección IP del dispositivo VPN local. *Use sus propios valores aquí, y no los mostrados en el ejemplo, si quiere conectarse a su sitio local*.
   - **Configurar BGP:** usar solo al configurar BGP. En caso contrario, no seleccione esta opción.
   - **Subscription** (Suscripción): compruebe que se muestra la suscripción correcta.
   - **Grupos de recursos:** seleccione el grupo de recursos que quiere usar. Puede crear un grupo de recursos nuevo o seleccionar uno ya creado.
   - **Ubicación:** La ubicación es la misma que **Región** en otros valores. seleccione la ubicación en la que se creará este objeto. Puede seleccionar la misma ubicación en la que reside la red, pero no es obligatorio.

4. Cuando haya terminado de especificar los valores, haga clic en el botón **Crear** en la parte inferior de la página para crear la puerta de enlace de red local.
