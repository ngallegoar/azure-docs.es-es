---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 02/10/2020
ms.author: cynthn
ms.openlocfilehash: 90dc5a067c05cadb3d5e102435b2e3d3de803e28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334603"
---
| Resource | Límite |
| --- | --- |
| Máquinas virtuales por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |25 000<sup>1</sup> por región. |
| Total de núcleos de máquina virtual por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> por región. Póngase en contacto con el soporte técnico para aumentar el límite. |
| Total de núcleos de máquina virtual de Azure Spot por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> por región. Póngase en contacto con el soporte técnico para aumentar el límite. |
| Máquina virtual por serie, como Dv2 y F, núcleos por [suscripción](../articles/billing-buy-sign-up-azure-subscription.md) |20<sup>1</sup> por región. Póngase en contacto con el soporte técnico para aumentar el límite. |
| [Conjuntos de disponibilidad](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) por suscripción |2000 por región |
| Máquinas virtuales por conjunto de disponibilidad | 200 |
| Certificados por suscripción |Ilimitado<sup>2</sup> |

<sup>1</sup>Los límites predeterminados varían según el tipo de categoría de la oferta, por ejemplo, evaluación gratuita, pago por uso y por serie, como Dv2, F y G. Por ejemplo, el valor predeterminado de la suscripción Contrato Enterprise es 350.

<sup>2</sup>Con Azure Resource Manager, los certificados se almacenan en Azure Key Vault. El número de certificados para una suscripción es ilimitado. Hay un límite de 1 MB de certificados por implementación, que consta de una sola máquina virtual o un conjunto de disponibilidad.

> [!NOTE]
> Los núcleos de máquina virtual tienen un límite total regional. También tienen un límite para la serie regional por tamaño, como Dv2 y F. Estos límites se aplican por separado. Por ejemplo, considere una suscripción con un límite total de núcleos de máquinas virtuales de Este de EE. UU. de 30, un límite de núcleos de serie A de 30 y un límite de núcleos de serie D de 30. Esta suscripción puede implementar 30 máquinas virtuales A1 o 30 máquinas virtuales D1, o una combinación de ambas, con tal de que no supere un total de 30 núcleos. Ejemplo de una combinación es 10 máquinas virtuales A1 y 20 máquinas virtuales D1.  
> <!-- -->
> 
