---
title: Compatibilidad con trabajo remoto de Azure Firewall
description: En este artículo se muestra cómo Azure Firewall puede admitir los requisitos del personal laboral remoto.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: victorh
ms.openlocfilehash: 5abe9344b0512433c48df50335cce5cf1e3e3547
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80289409"
---
# <a name="azure-firewall-remote-work-support"></a>Compatibilidad con trabajo remoto de Azure Firewall

Azure Firewall es un servicio de seguridad de red administrado y basado en la nube que protege los recursos de red virtual de Azure. Se trata de un firewall como servicio con estado completo que incorpora alta disponibilidad y escalabilidad a la nube sin restricciones. 

## <a name="firewall-rules"></a>Reglas de firewall

Puede usar Azure Firewall para proteger el acceso RDP de entrada de la infraestructura de escritorio virtual (VDI) a la red virtual de Azure mediante las [reglas DNAT](rule-processing.md) de Azure Firewall. Windows Virtual Desktop (WVD) no requiere que abra ningún acceso de entrada a la red virtual. Sin embargo, debe permitir un conjunto de conexiones de red de salida para las máquinas virtuales WVD que se ejecutan en la red virtual. Para más información, consulte [¿Qué es Windows Virtual Desktop?](../virtual-desktop/overview.md#requirements)

Puede configurar este acceso de salida mediante reglas de aplicación de Azure Firewall. Para más información, consulte el [Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal](tutorial-firewall-deploy-portal.md).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [Windows Virtual Desktop](https://docs.microsoft.com/azure/virtual-desktop/).