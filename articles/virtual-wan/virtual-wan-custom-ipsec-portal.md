---
title: 'Configurar una directiva IPsec personalizada para Azure Virtual WAN: Portal | Microsoft Docs'
description: Obtenga información sobre cómo configurar una directiva IPsec personalizada para Azure Virtual WAN en el portal.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 10/14/2019
ms.author: cherylmc
ms.openlocfilehash: 07eae453441ea8bff81d7cdb60f9c46c08a22829
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91851187"
---
# <a name="configure-a-custom-ipsec-policy-for-virtual-wan-using-the-portal"></a>Configuración de una directiva IPsec personalizada para Azure Virtual WAN en el portal

Puede configurar una directiva IPsec personalizada para una conexión VPN de Virtual WAN en el Azure Portal. Las directivas personalizadas son útiles cuando se desea que ambos lados (en el entorno local y en la puerta de enlace Azure VPN) usen la misma configuración para Fase IKE 1 y Fase IKE 2.

## <a name="working-with-custom-policies"></a>Trabajar con directivas personalizadas

[!INCLUDE [IPsec](../../includes/virtual-wan-ipsec-custom-include.md)]

## <a name="configure-a-policy"></a>Configuración de una directiva

1. **Busque el centro de conectividad virtual**. Desde un explorador, navegue al [Portal de Azure](https://aka.ms/azurevirtualwanpreviewfeatures) e inicie sesión con su cuenta de Azure. Navegue hasta el recurso de Virtual WAN y busque el concentrador virtual al que está conectado el sitio VPN.
2. **Seleccione el sitio VPN**. En la página de información general del concentrador, haga clic en **VPN (sitio a sitio)** y seleccione el sitio VPN para el que desea configurar una directiva IPsec personalizada.

   ![select](./media/virtual-wan-custom-ipsec-portal/locate.png)
3. **Edición de la conexión VPN**. Desde el **menú contextual** **...** , seleccione **Edición de conexión VPN**.

   ![edición](./media/virtual-wan-custom-ipsec-portal/contextmenu.png)
4. **Configurar los parámetros**. En la página **Editar conexión VPN**, cambie la configuración de IPsec de predeterminada a personalizada y personalice la directiva IPsec. Haga clic en **Guardar** para guardar la configuración.

   ![configurar y guardar](./media/virtual-wan-custom-ipsec-portal/edit.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).