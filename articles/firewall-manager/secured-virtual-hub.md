---
title: ¿Qué es un centro virtual protegido?
description: Información sobre centros virtuales protegidos
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 10/12/2020
ms.author: victorh
ms.openlocfilehash: c840bf9e82b8dcdb1fbf9b380ea847b3d1b08dd9
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2020
ms.locfileid: "91948075"
---
# <a name="what-is-a-secured-virtual-hub"></a>¿Qué es un centro virtual protegido?

Un centro virtual es una red virtual administrada por Microsoft que permite la conectividad desde otros recursos. Cuando se crea un centro virtual desde una WAN virtual en Azure Portal, se crean como componentes una red virtual del centro virtual y puertas de enlace (opcionales).

Un centro virtual *protegido* es un [centro de conectividad de Azure Virtual WAN](../virtual-wan/virtual-wan-about.md#resources) con directivas de enrutamiento y seguridad asociadas configuradas por Azure Firewall Manager. Use centros virtuales protegidos para crear fácilmente arquitecturas radiales y transitivas con servicios de seguridad nativos para la protección y la gobernanza del tráfico. 

Puede usar un centro virtual protegido para filtrar el tráfico entre redes virtuales (V2V), redes virtuales y sucursales (B2V) y el tráfico a Internet (B2I/V2I). Un centro virtual protegido proporciona enrutamiento automatizado. Por ello, no es necesario configurar sus propias UDR (rutas definidas por el usuario) para enrutar el tráfico a través del firewall.

Puede elegir los proveedores de seguridad necesarios para proteger y controlar el tráfico de red, incluido Azure Firewall, proveedores de seguridad como servicio (SECaaS) externos o ambos. Actualmente, un centro protegido no admite el filtrado de sucursal a sucursal (B2B) y el filtrado entre varios centros. Para obtener más información, vea [¿Qué es Azure Firewall Manager?](overview.md#known-issues). 

## <a name="create-a-secured-virtual-hub"></a>Crear un centro virtual protegido

Con Firewall Manager en Azure Portal, puede crear un nuevo centro virtual protegido o convertir un centro virtual existente creado anteriormente con Azure Virtual WAN.

## <a name="next-steps"></a>Pasos siguientes

Para crear un centro virtual protegido y usarlo para proteger y controlar una red radial, consulte [Tutorial: Proteja su red en la nube con la versión preliminar de Azure Firewall Manager en Azure Portal](secure-cloud-network.md).