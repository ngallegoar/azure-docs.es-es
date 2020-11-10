---
title: Redes y conectividad de Azure VMware Solution
description: Descripción de redes y conectividad de Azure VMware Solution.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 47b5fab8430f4e462f1e4a0e8cb6ddb69ec38c02
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2020
ms.locfileid: "92925004"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware Solution ofrece un entorno de nube privada accesible desde recursos o entornos locales y basados en Azure. Servicios como Azure ExpressRoute y las conexiones VPN proporcionan la conectividad. Estos servicios requieren intervalos de direcciones de red y puertos de firewall específicos para habilitar los servicios.

Cuando se implementa una nube privada, se crean redes privadas para la administración, el aprovisionamiento y vMotion. Use estas redes privadas para acceder a vCenter y NSX-T Manager, así como para la implementación o vMotion de máquinas virtuales.  Global Reach de ExpressRoute se usa para conectar las nubes privadas a entornos locales. La conexión requiere una red virtual con un circuito ExpressRoute en su suscripción.

Los recursos como, por ejemplo, los servidores web y las máquinas virtuales, son accesibles a través de Internet mediante la funcionalidad de IP pública de Azure Virtual WAN.  De forma predeterminada, el acceso a Internet está deshabilitado para las nuevas nubes privadas. Para más información, consulte [Uso de la funcionalidad de IP pública en Azure VMware Solution](../public-ip-usage.md).