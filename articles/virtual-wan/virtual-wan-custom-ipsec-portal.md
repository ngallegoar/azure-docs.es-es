---
title: 'Configurar una directiva IPsec personalizada para Azure Virtual WAN: Portal | Microsoft Docs'
description: Obtenga información sobre cómo configurar una directiva IPsec personalizada para Azure Virtual WAN en el portal.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 0ea4523d1558f6887e1aef344198026591dac617
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84752610"
---
# <a name="configure-a-custom-ipsec-policy-for-virtual-wan-using-the-portal"></a>Configuración de una directiva IPsec personalizada para Azure Virtual WAN en el portal

Puede configurar una directiva IPsec personalizada para Azure Virtual WAN en el Azure Portal. Las directivas personalizadas son útiles cuando se desea que ambos lados (en el entorno local y en la puerta de enlace Azure VPN) usen la misma configuración para Fase IKE 1 y Fase IKE 2.

## <a name="working-with-custom-policies"></a>Trabajar con directivas personalizadas

[!INCLUDE [IPsec](../../includes/virtual-wan-ipsec-custom-include.md)]

## <a name="configure-a-policy"></a>Configuración de una directiva

1. **Busque el centro de conectividad virtual**. Desde un explorador, navegue al [Portal de Azure](https://aka.ms/azurevirtualwanpreviewfeatures) e inicie sesión con su cuenta de Azure. Busque el centro de conectividad virtual de su sitio.
2. **Seleccione el sitio VPN**. En la página del centro de conectividad, seleccione el sitio VPN para el que desea configurar una directiva personalizada.

   ![select](./media/virtual-wan-custom-ipsec-portal/locate.png)
3. **Edición de la conexión VPN**. Desde el **menú contextual** **...** , seleccione **Edición de conexión VPN**.

   ![edición](./media/virtual-wan-custom-ipsec-portal/contextmenu.png)
4. **Configurar los parámetros**. En la página **Edición de conexión VPN**, configure los ajustes de los parámetros. Haga clic en **Guardar** para guardar la configuración.

   ![configurar y guardar](./media/virtual-wan-custom-ipsec-portal/edit.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).