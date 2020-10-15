---
author: rothja
ms.service: cloud-services
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 390460cf6a22c164ee41a9b6679c7b7f2979d8ec
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "85838870"
---
| Resource | Límite |
| --- | --- |
| [Roles web o de trabajo por implementación](../articles/cloud-services/cloud-services-choose-me.md)<sup>1</sup> |25 |
| [Puntos de conexión de entrada de instancia](/previous-versions/azure/reference/gg557552(v=azure.100)#instanceinputendpoint) por implementación |25 |
| [Puntos de conexión de entrada](/previous-versions/azure/reference/gg557552(v=azure.100)#inputendpoint) por implementación |25 |
| [Puntos de conexión internos](/previous-versions/azure/reference/gg557552(v=azure.100)#internalendpoint) por implementación |25 |
| [Certificados de servicio hospedados](../articles/cloud-services/cloud-services-certs-create.md#what-are-service-certificates) por implementación |199 |

<sup>1</sup>Cada servicio en la nube de Azure con roles web o de trabajo puede tener dos implementaciones, una para producción y otra para ensayo. Este límite se refiere al número de roles diferentes, es decir, la configuración. No hace referencia al número de instancias por rol, es decir, el escalado.

