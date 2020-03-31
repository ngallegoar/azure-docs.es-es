---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 02/13/2019
ms.author: danlep
ms.openlocfilehash: 33a82a55b03cde79d2d80826041ee6d43565476a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334819"
---
| Resource | Límite |
| --- | :--- |
| Grupos de contenedores de SKU estándar por región por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
| Grupos de contenedores de SKU dedicado por región por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) | 0<sup>1</sup> |
| Número de contenedores por grupo de contenedor | 60 |
| Número de volúmenes por grupo de contenedor | 20 |
| Puertos por IP | 5 |
| Tamaño del registro de instancia de contenedor: instancia en ejecución | 4 MB |
| Tamaño del registro de instancia de contenedor: instancia detenida | 16 KB o 1000 líneas |
| Creaciones de contenedores cada hora |300<sup>1</sup> |
| Creaciones de contenedores cada 5 minutos | 100<sup>1</sup> |
| Eliminaciones de contenedores cada hora | 300<sup>1</sup> |
| Eliminaciones de contenedores cada 5 minutos | 100<sup>1</sup> |


<sup>1</sup>Para solicitar un aumento del límite, cree una [solicitud de soporte técnico de Azure][azure-support].<br />

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
