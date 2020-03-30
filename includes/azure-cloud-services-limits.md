---
author: rothja
ms.service: cost-management-billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 3295c7e677e4c9bf7bff27614fe5fa4fa3200570
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334886"
---
| Resource | Límite |
| --- | --- |
| [Roles web o de trabajo por implementación](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |
| [Puntos de conexión de entrada de instancia](/previous-versions/azure/reference/gg557552(v=azure.100)#instanceinputendpoint) por implementación |25 |
| [Puntos de conexión de entrada](/previous-versions/azure/reference/gg557552(v=azure.100)#inputendpoint) por implementación |25 |
| [Puntos de conexión internos](/previous-versions/azure/reference/gg557552(v=azure.100)#internalendpoint) por implementación |25 |
| [Certificados de servicio hospedados](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) por implementación |199 |

<sup>1</sup>Cada servicio en la nube de Azure con roles web o de trabajo puede tener dos implementaciones, una para producción y otra para ensayo. Este límite se refiere al número de roles diferentes, es decir, la configuración. No hace referencia al número de instancias por rol, es decir, el escalado.

