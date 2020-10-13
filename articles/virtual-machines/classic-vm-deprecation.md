---
title: Vamos a retirar las máquinas virtuales (clásicas) de Azure el 1 de marzo de 2023
description: En este artículo se proporciona información general de alto nivel sobre la retirada de máquinas virtuales creadas con el modelo de implementación clásica.
author: tanmaygore
manager: vashan
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 02/10/2020
ms.author: tagore
ms.openlocfilehash: c9277f510e71e4d6ecb6595aa2d67d16c2ac5695
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2020
ms.locfileid: "91326035"
---
# <a name="migrate-your-iaas-resources-to-azure-resource-manager-by-march-1-2023"></a>Migre los recursos de IaaS a Azure Resource Manager antes del 1 de marzo de 2023. 

En 2014, lanzamos la infraestructura como servicio (IaaS) en [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/). Desde entonces, hemos mejorado las funcionalidades. Dado que Azure Resource Manager tiene ahora funcionalidades completas de IaaS y otras mejoras, hemos puesto en desuso las máquinas virtuales (VM) de IaaS mediante [Azure Service Manager](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-faq#what-is-azure-service-manager-and-what-does-it-mean-by-classic) (ASM) el 28 de febrero de 2020. Esta funcionalidad se retirará por completo el 1 de marzo de 2023. 

En la actualidad, aproximadamente el 90 por ciento de las máquinas virtuales de IaaS usan Azure Resource Manager. Si usa recursos de IaaS a través de ASM, empiece a planear la migración ahora. Complétela antes del 1 de marzo de 2023 para aprovechar [Azure Resource Manager](../azure-resource-manager/management/index.yml).

Las máquinas virtuales creadas con el modelo de implementación clásica seguirán la [directiva de ciclo de vida moderno ](https://support.microsoft.com/help/30881/modern-lifecycle-policy) para su retirada.

## <a name="how-does-this-affect-me"></a>¿Cómo me afecta esto? 

- A partir del 28 de febrero de 2020, los clientes que no hayan usado máquinas virtuales de IaaS a través de ASM en el mes de febrero de 2020 ya no podrán crear máquinas virtuales (clásicas). 
- El 1 de marzo de 2023, los clientes ya no podrán iniciar máquinas virtuales de IaaS con ASM. Todo lo que todavía esté en ejecución o asignado se detendrá y desasignará. 
- El 1 de marzo de 2023, se informará a las suscripciones que no se migran a Azure Resource Manager acerca de nuestras escalas de tiempo para eliminar las máquinas virtuales (clásicas) restantes.  

Esta retirada *no* afecta a los siguientes servicios y funcionalidades de Azure: 
- Azure Cloud Services 
- Las máquinas virtuales (clásicas) *no* usan las cuentas de almacenamiento. 
- Las máquinas virtuales (clásicas) *no* usan las redes virtuales. 
- Otros recursos clásicos

## <a name="what-actions-should-i-take"></a>¿Qué medidas debo tomar? 

Empiece ya a planear la migración a Azure Resource Manager. 

1. Haga una lista de todas las máquinas virtuales afectadas: 

   - Las máquinas virtuales de tipo **máquinas virtuales (clásicas)** en el [panel Máquinas virtuales de Azure Portal](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ClassicCompute%2FVirtualMachines) son todas las máquinas virtuales afectadas dentro de la suscripción. 
   - También puede realizar consultas en Azure Resource Graph mediante el [portal](https://portal.azure.com/#blade/HubsExtension/ArgQueryBlade/query/resources%0A%7C%20where%20type%20%3D%3D%20%22microsoft.classiccompute%2Fvirtualmachines%22) o [PowerShell](https://docs.microsoft.com/azure/governance/resource-graph/concepts/work-with-data) para ver la lista de todas las máquinas virtuales (clásicas) marcadas y su información relacionada para las suscripciones seleccionadas. 
   - El 8 de febrero y el 2 de septiembre de 2020, enviamos correos electrónicos a los propietarios de suscripciones con una lista de todas las suscripciones que contienen estas máquinas virtuales (clásicas). Úselas para crear esta lista. 

1. [Más información](./windows/migration-classic-resource-manager-overview.md) sobre cómo migrar las máquinas virtuales (clásicas) [Linux](./linux/migration-classic-resource-manager-plan.md) y [Windows](./windows/migration-classic-resource-manager-plan.md) a Azure Resource Manager. Para más información, consulte [Preguntas más frecuentes sobre la migración del método clásico al de Azure Resource Manager](./migration-classic-resource-manager-faq.md).

1. Se recomienda iniciar el planeamiento con la [herramienta de migración de compatibilidad de la plataforma](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview) para migrar las máquinas virtuales existentes en tres sencillos pasos: validar, preparar y confirmar. La herramienta está diseñada para migrar las máquinas virtuales sin apenas tiempo de inactividad o ninguno en absoluto. 

   1. El primer paso, validación, no afecta de ningún modo a la implementación existente y proporciona una lista de todos los escenarios de migración no admitidos. 
   1. Examine la [lista de soluciones alternativas](https://docs.microsoft.com/azure/virtual-machines/windows/migration-classic-resource-manager-overview#unsupported-features-and-configurations) para corregir su implementación y prepararla para la migración. 
   1. Lo normal es que después de corregir todos los errores de validación no se encuentre con ningún problema durante los pasos de preparación y confirmación. Después de que la confirmación se realice correctamente, la implementación se migra en vivo a Azure Resource Manager y, a partir de entonces, se puede administrar con las nuevas API que Azure Resource Manager expone. 

   Si la herramienta de migración no es adecuada para su migración, puede explorar [otras ofertas de proceso](https://docs.microsoft.com/azure/architecture/guide/technology-choices/compute-decision-tree) para la migración. Dado que hay muchas ofertas de proceso de Azure, y son muy diferentes entre sí, no podemos proporcionar un método de migración admitido por la plataforma para estas ofertas.  

1. Para consultar preguntas técnicas y problemas, y obtener ayuda para agregar suscripciones a la lista de permitidos, [póngase en contacto con el soporte técnico](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"8a82f77d-c3ab-7b08-d915-776b4ff64ff4"}).

1. Complete la migración lo antes posible para evitar el impacto en el negocio y aproveche el rendimiento mejorado, la seguridad y las características nuevas de Azure Resource Manager. 

## <a name="what-resources-are-available-for-this-migration"></a>¿Qué recursos están disponibles para esta migración?

- [Microsoft Q&A](https://docs.microsoft.com/answers/topics/azure-virtual-machines-migration.html): Soporte técnico de Microsoft y de la comunidad para la migración

- [Soporte técnico para la migración de Azure](https://ms.portal.azure.com/#create/Microsoft.Support/Parameters/{"pesId":"6f16735c-b0ae-b275-ad3a-03479cfa1396","supportTopicId":"1135e3d0-20e2-aec5-4ef0-55fd3dae2d58"}): equipo de soporte técnico dedicado para brindar asistencia técnica durante la migración.

- [Microsoft Fast Track](https://www.microsoft.com/fasttrack): Fast Track puede ayudar a los clientes que reúnan los requisitos a planificar y ejecutar esta migración. [Nomínese](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fprograms%2Fazure-fasttrack%2F%23nomination&data=02%7C01%7CTanmay.Gore%40microsoft.com%7C3e75bbf3617944ec663a08d85c058340%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637360526032558561&sdata=CxWTVQQPVWNwEqDZKktXzNV74pX91uyJ8dY8YecIgGc%3D&reserved=0).  

- Si su empresa u organización se ha asociado con Microsoft o trabaja con un representante de Microsoft (como arquitecto de soluciones en la nube [CSA] o administradores técnicos de cuentas [TAM]), trabaje en conjunto con ellos para obtener recursos adicionales para la migración. 

