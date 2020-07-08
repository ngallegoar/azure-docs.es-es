---
title: Información general sobre el control del mantenimiento de máquinas virtuales de Azure mediante Azure Portal
description: Obtenga información sobre cómo controlar cuándo se aplica mantenimiento a las máquinas virtuales de Azure mediante el control de mantenimiento.
author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 04/22/2020
ms.author: cynthn
ms.openlocfilehash: 950f4cfda73b40db0de8ba035868573cda1a5017
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "84675806"
---
# <a name="managing-platform-updates-with-maintenance-control"></a>Administración de las actualizaciones de la plataforma con el control de mantenimiento 

Administre las actualizaciones de la plataforma, que no requieren un reinicio, mediante el control de mantenimiento. Azure actualiza con frecuencia su infraestructura para mejorar la confiabilidad, el rendimiento y la seguridad o para poner en marcha nuevas características. La mayoría de las actualizaciones son transparentes para los usuarios. Algunas cargas de trabajo especialmente delicadas, como los juegos, el streaming multimedia o las transacciones financieras, no pueden tolerar ni siquiera unos segundos de bloqueo o desconexión por mantenimiento de una máquina virtual. El control de mantenimiento ofrece la opción de poner en espera las actualizaciones de la plataforma y aplicarlas en un período sucesivo de 35 días. 

El control de mantenimiento le permite decidir cuándo se aplican las actualizaciones a las máquinas virtuales aisladas y los hosts dedicados de Azure.

Con el control de mantenimiento, puede:
- Aplicar actualizaciones por lotes en un único paquete de actualización.
- Esperar hasta 35 días para aplicar las actualizaciones. 
- Automatizar las actualizaciones de la plataforma para la ventana de mantenimiento mediante Azure Functions.
- Mantener el trabajo de las configuraciones entre suscripciones y grupos de recursos. 

## <a name="limitations"></a>Limitaciones

- Las máquinas virtuales deben estar en un [host dedicado](./linux/dedicated-hosts.md) o bien crearse con un [tamaño de máquina virtual aislada](./linux/isolation.md).
- Después de 35 días, se aplicará automáticamente una actualización.
- El usuario debe tener acceso de **colaborador del recurso**.

## <a name="management-options"></a>Opciones de administración

Puede crear y administrar configuraciones de mantenimiento con cualquiera de estas opciones:

- [CLI de Azure](maintenance-control-cli.md)
- [Azure PowerShell](maintenance-control-powershell.md)
- [Azure Portal](maintenance-control-portal.md)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte [Mantenimiento y actualizaciones](maintenance-and-updates.md).
