---
title: Compatibilidad con trabajo remoto de Azure Firewall
description: En este artículo se muestra cómo Azure Firewall puede admitir los requisitos del personal laboral remoto.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: victorh
ms.openlocfilehash: 3c0e2033ee559af38a6816bdfa611eea86b14dea
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658322"
---
# <a name="azure-firewall-remote-work-support"></a>Compatibilidad con trabajo remoto de Azure Firewall

Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de red virtual de Azure. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad a la nube sin restricciones.

## <a name="virtual-desktop-infrastructure-vdi-deployment-support"></a>Compatibilidad con la implementación de Infraestructura de escritorio virtual (VDI)

Las directivas de trabajo desde casa requieren que muchas organizaciones de TI aborden cambios fundamentales en relación con la capacidad, la red, la seguridad y la gobernanza. Los empleados no están protegidos por directivas de seguridad por niveles asociadas a servicios locales cuando trabajan desde casa. Las implementaciones de Infraestructura de escritorio virtual (VDI) en Azure pueden ayudar a las organizaciones a responder rápidamente a este entorno cambiante. Sin embargo, necesita una forma de proteger el acceso a Internet entrante o saliente a y desde estas implementaciones de VDI. Puede usar las [reglas DNAT](rule-processing.md) de Azure Firewall junto con sus capacidades de filtrado basadas en la [inteligencia sobre amenazas](threat-intel.md) para proteger las implementaciones de VDI.

## <a name="azure-windows-virtual-desktop-support"></a>Compatibilidad con Windows Virtual Desktop de Azure

Windows Virtual Desktop es un servicio completo de virtualización de escritorio y de aplicaciones que se ejecuta en Azure. Es la única infraestructura de escritorio virtual (VDI) que ofrece administración simplificada, Windows 10 para sesiones múltiples, optimizaciones para aplicaciones de Microsoft 365 para empresas y compatibilidad con entornos de Servicios de Escritorio remoto (RDS). Puede implementar y escalar las aplicaciones y los escritorios de Windows en Azure en unos minutos y obtener características de cumplimiento y seguridad integradas. Windows Virtual Desktop no requiere que abra ningún acceso de entrada a la red virtual. Sin embargo, debe permitir un conjunto de conexiones de red de salida para las máquinas virtuales con Windows Virtual Desktop que se ejecutan en la red virtual. Para más información, consulte [Uso de Azure Firewall para proteger las implementaciones de Windows Virtual Desktop](protect-windows-virtual-desktop.md).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [Windows Virtual Desktop](../virtual-desktop/index.yml).