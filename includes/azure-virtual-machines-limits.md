---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/09/2018
ms.author: cynthn
ms.openlocfilehash: 2cfd376f595ae70daf9ab468d464dd9c8ff13d74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335017"
---
| Resource | Límite |
| --- | --- |
| [Máquinas virtuales](../articles/virtual-machines/virtual-machines-linux-about.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) por servicio en la nube<sup>1</sup> |50 |
| Extremos de entrada por servicio en la nube<sup>2</sup> |150 |

<sup>1</sup>Las máquinas virtuales creadas mediante el modelo de implementación clásico, en lugar de Azure Resource Manager, se almacenan automáticamente en un servicio en la nube. Puede agregar más máquinas virtuales a ese servicio en la nube para usarlas para el equilibrio de carga y la disponibilidad. 

<sup>2</sup>Los extremos de entrada permiten establecer comunicación con una máquina virtual desde fuera del servicio en la nube de la máquina virtual. Las máquinas virtuales en el mismo servicio en la nube o red virtual pueden comunicarse automáticamente entre sí. Para más información, consulte [Configuración de puntos de conexión en una máquina virtual](../articles/virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). 
