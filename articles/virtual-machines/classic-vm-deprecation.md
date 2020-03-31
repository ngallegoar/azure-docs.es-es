---
title: Vamos a retirar las máquinas virtuales clásicas de Azure el 1 de marzo de 2023
description: El artículo proporciona información general de alto nivel sobre la retirada de las máquinas virtuales clásicas.
services: virtual-machines
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: 764567bffd2a08ebb5beb17e3063998848b3f110
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127339"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>Migre los recursos de IaaS a Azure Resource Manager antes del 1 de marzo de 2023. 

En 2014, se inició IaaS en Azure Resource Manager y se han mejorado las funcionalidades desde entonces. Dado que [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/) tiene ahora funcionalidades completas de IaaS y otras mejoras, hemos puesto en desuso las máquinas virtuales de IaaS mediante Azure Service Manager el 28 de febrero de 2020 y esta funcionalidad se retirará por completo el 1 de marzo de 2023. 

En la actualidad, aproximadamente el 90 % de las máquinas virtuales de IaaS usan Azure Resource Manager. Si va a utilizar recursos de IaaS a través de Azure Service Manager (ASM), empiece a planear la migración ahora y complétela antes del 1 de marzo de 2023 para aprovechar [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/management/).

Las máquinas virtuales clásicas seguirán la [directiva de ciclo de vida actual](https://support.microsoft.com/help/30881/modern-lifecycle-policy) para su retirada.

## <a name="how-does-this-affect-me"></a>¿Cómo me afecta esto? 

1) A partir del 28 de febrero de 2020, los clientes que no han usado las máquinas virtuales de IaaS a través de Azure Service Manager (ASM) en febrero de 2020 no podrán crear máquinas virtuales clásicas. 
2) El 1 de marzo de 2023 los clientes ya no podrán iniciar máquinas virtuales de IaaS mediante Azure Service Manager y aquellas que estén en ejecución o asignadas se detendrán y desasignarán. 
2) El 1 de marzo de 2023, se informará a las suscripciones que no se hayan migrado a Azure Resource Manager acerca de nuestras escalas de tiempo para eliminar las máquinas virtuales clásicas restantes.  

Los siguientes servicios y funcionalidades de Azure **NO** se verán afectados por esta retirada: 
- Cloud Services 
- Las cuentas de almacenamiento **no** se usan en máquinas virtuales clásicas. 
- Las redes virtuales **no** se usan en máquinas virtuales clásicas. 
- Otros recursos clásicos

## <a name="what-actions-should-i-take"></a>¿Qué medidas debo tomar? 

- Empiece ya a planear la migración a Azure Resource Manager. 

- [Más información](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview) sobre cómo migrar las máquinas virtuales [Linux](./linux/migration-classic-resource-manager-plan.md) y [Windows](./windows/migration-classic-resource-manager-plan.md) clásicas a Azure Resource Manager.

- Para más información, consulte las [preguntas más frecuentes sobre la migración del método clásico al de Azure Resource Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq).

- Para preguntas y problemas técnicos, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

- Para otras preguntas que no formen parte de las preguntas más frecuentes y comentarios, envíe un comentario a continuación.
