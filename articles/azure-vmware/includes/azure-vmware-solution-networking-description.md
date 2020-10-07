---
title: Redes y conectividad de Azure VMWare Solution
description: Descripción de redes y conectividad de Azure VMWare Solution.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 1d43a8a49fc56a6a5b534a89fd1dfc2f5ae2d478
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2020
ms.locfileid: "91574470"
---
<!-- Used in introduction.md and concepts-networking.md -->

Azure VMware Solution ofrece un entorno de nube privada accesible desde recursos o entornos locales y basados en Azure. Servicios como Azure ExpressRoute y las conexiones VPN proporcionan la conectividad. Estos servicios requieren intervalos de direcciones de red y puertos de firewall específicos para habilitar los servicios.

Cuando se implementa una nube privada, se crean redes privadas para la administración, el aprovisionamiento y vMotion. Use estas redes privadas para acceder a vCenter y NSX-T Manager, así como para la implementación o vMotion de máquinas virtuales.  Global Reach de ExpressRoute se usa para conectar las nubes privadas a entornos locales. La conexión requiere una red virtual con un circuito ExpressRoute en su suscripción.



>[!NOTE]
>El acceso a Internet y los servicios de Azure se aprovisionan y proporcionan para que consuman máquinas virtuales de las redes de producción al implementar una nube privada.  El acceso a Internet está deshabilitado de forma predeterminada para las nuevas nubes privadas, pero puede habilitarse o deshabilitarse en cualquier momento.