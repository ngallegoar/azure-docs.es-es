---
title: Información general sobre el control de mantenimiento para las actualizaciones de imágenes del sistema operativo en conjuntos de escalado de máquinas virtuales de Azure
description: Aprenda a controlar cuándo se implementan las actualizaciones automáticas de las imágenes del sistema operativo en los conjuntos de escalado de máquinas virtuales de Azure mediante el control de mantenimiento.
author: ju-shim
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 09/11/2020
ms.author: jushiman
ms.openlocfilehash: 55cbc5db534dd1b05f91a24e0104b1f2dc110547
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "90532993"
---
# <a name="preview-maintenance-control-for-azure-virtual-machine-scale-sets"></a>Vista previa: Controles de seguridad para conjuntos de escalado de máquinas virtuales de Azure 

Administre [actualizaciones automáticas de imágenes del sistema operativo](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md) para los conjuntos de escalado de máquinas virtuales mediante el control de mantenimiento.

El control de mantenimiento le permite decidir cuándo se aplican las actualizaciones a los discos del sistema operativo en los conjuntos de escalado de máquinas virtuales gracias a una experiencia más sencilla y predecible. 

Mantener el trabajo de las configuraciones entre suscripciones y grupos de recursos.

Todo el flujo de trabajo se reduce a estos pasos: 
- Crear una configuración de mantenimiento.
- Asociar un conjunto de escalado de máquinas virtuales a una configuración de mantenimiento.
- Habilitar las actualizaciones automáticas del sistema operativo.

> [!IMPORTANT]
> El control de mantenimiento para las actualizaciones de imágenes del sistema operativo en conjuntos de escalado de máquinas virtuales de Azure se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="limitations"></a>Limitaciones

- Las máquinas virtuales deben estar en un conjunto de escalado.
- El usuario debe tener acceso de **colaborador del recurso**.
- La duración del mantenimiento debe ser de cinco horas o más en la configuración de mantenimiento.
- La periodicidad de mantenimiento debe establecerse en "días" en la configuración de mantenimiento


## <a name="management-options"></a>Opciones de administración

Puede crear y administrar configuraciones de mantenimiento con cualquiera de estas opciones:

- [Azure PowerShell](virtual-machine-scale-sets-maintenance-control-powershell.md)


## <a name="next-steps"></a>Pasos siguientes

Aprenda a controlar cuándo se aplica el mantenimiento a las máquinas virtuales de Azure mediante el control de mantenimiento y PowerShell.

> [!div class="nextstepaction"]
> [Control de mantenimiento del conjunto de escalado de máquinas virtuales mediante PowerShell](virtual-machine-scale-sets-maintenance-control-powershell.md)
