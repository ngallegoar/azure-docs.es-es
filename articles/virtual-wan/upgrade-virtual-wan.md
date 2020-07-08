---
title: Actualización de una Azure Virtual WAN de nivel Básico al Estándar - Azure Portal | Microsoft Docs
description: Puede actualizar el tipo de virtual WAN para obtener una mayor funcionalidad.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 769aa6c0d546b7a9bcbe355136bad811fb4f47b3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753305"
---
# <a name="upgrade-a-virtual-wan-from-basic-to-standard"></a>Actualización de una virtual WAN de Básica a Estándar

Este artículo le ayuda a actualizar una WAN Básica a una WAN Estándar. Un tipo de WAN "Básico" crea todos los centros de conectividad dentro de sí como centros SKU básicos. Los centros de conectividad Básicos se limitan solamente a la funcionalidad de VPN de sitio a sitio. Un tipo de WAN "Estándar" crea todos los centros de conectividad dentro de sí como centros SKU estándar. Al usar los centros de conectividad Estándar, puede habilitar Microsoft Azure ExpressRoute, VPN de usuario (punto a sitio), un centro de conectividad de malla completa y el tránsito de VNet a VNet a través de los centros de Azure.

En la tabla siguiente se muestran las configuraciones disponibles para cada tipo de WAN:

[!INCLUDE [Basic and Standard SKUs](../../includes/virtual-wan-standard-basic-include.md)]

## <a name="to-change-the-virtual-wan-type"></a>Para cambiar el tipo de virtual WAN

1. En la página de la virtual WAN, seleccione **Configuración** para abrir la página de configuración.

   ![Diagrama de Virtual WAN](./media/upgrade-virtual-wan/1.png)
2. Para el tipo de Virtual WAN, seleccione **Estándar** de la lista desplegable.

   ![Diagrama de Virtual WAN](./media/upgrade-virtual-wan/2.png)
3. Tenga en cuenta que si actualiza a una Virtual WAN Estándar, no podrá volver a una Virtual WAN básica. Seleccione **Confirmar** si desea actualizar.

   ![Diagrama de Virtual WAN](./media/upgrade-virtual-wan/4.png)
4. Una vez guardado el cambio, la página de virtual WAN tendrá un aspecto similar al de este ejemplo.

   ![Diagrama de Virtual WAN](./media/upgrade-virtual-wan/5.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte la página [Introducción a Virtual WAN](virtual-wan-about.md).