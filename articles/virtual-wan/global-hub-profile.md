---
title: Descarga de perfiles de VPN basados en centros de conectividad o globales de Azure Virtual WAN | Microsoft Docs
description: Obtenga información acerca de la conectividad entre ramas escalable y automatizada de rama a rama de Virtual WAN, las regiones en que está disponible y sus asociados.
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 4/20/2020
ms.author: alzam
ms.openlocfilehash: b63bb861f4df087f852bb1bf599d32100d063f7e
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/21/2020
ms.locfileid: "81733183"
---
# <a name="download-a-global-or-hub-based-profile-for-user-vpn-clients"></a>Descarga de un perfil global o basado en centros de conectividad para clientes VPN de usuario

Azure Virtual WAN ofrece dos tipos de conectividad para los usuarios remotos: global y basada en centros de conectividad. Use las secciones siguientes para obtener información sobre un perfil y descargarlo. 

> [!IMPORTANT]
> La autenticación RADIUS solo admite el perfil basado en centro de conectividad.

## <a name="global-profile"></a>Perfil global

El perfil apunta a un equilibrador de carga que incluye todos los centros de conectividad VPN de usuario activos. El usuario se dirige al centro de conectividad más cercano a la ubicación geográfica del usuario. Este tipo de conectividad es útil cuando los usuarios viajan a diferentes ubicaciones con frecuencia. Para descargar el perfil **global**:

1. Vaya a la instancia de Virtual WAN.
2. Haga clic en **Configuración de VPN de usuario**.
3. Resalte la configuración para la que quiere descargar el perfil.
4. Haga clic en **Descargar perfil de VPN de usuario de WAN virtual**.

   ![Perfil global](./media/global-hub-profile/global1.png)

## <a name="hub-based-profile"></a>Perfil basado en centro de conectividad

El perfil apunta a un solo centro de conectividad. El usuario solo puede conectarse a un centro de conectividad concreto mediante este perfil. Para descargar el perfil **basado en centro de conectividad**:

1. Vaya a la instancia de Virtual WAN.
2. En la página Información general, haga clic en **Centro**.

    ![Perfil 1 del centro de conectividad](./media/global-hub-profile/hub1.png)
3. Haga clic en **VPN de usuario (punto a sitio)** .
4. Haga clic en **Descargar el perfil de VPN de usuario de centro virtual**.

   ![Perfil 2 del centro de conectividad](./media/global-hub-profile/hub2.png)
5. Compruebe **EAPTLS**.
6. Haga clic en **Generar y descargar perfil**.

   ![Perfil 3 del centro de conectividad](./media/global-hub-profile/download.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).
