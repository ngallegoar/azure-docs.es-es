---
title: Problemas de reinicio o cambio de tamaño de las máquinas virtuales de Azure | Microsoft Docs
description: Solución de problemas de la implementación de Resource Manager con el reinicio o el cambio de tamaño de una máquina virtual existente en Azure
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85acd8e26ca10730638332047a37d281358d205f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "86526561"
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a>Solución de problemas de implementación con el reinicio o el cambio de tamaño de una máquina virtual Windows existente en Azure
Al intentar iniciar una máquina virtual de Azure detenida o cambiar el tamaño de una máquina virtual de Azure existente, es común encontrarse un error de asignación. Dicho error se produce cuando el clúster o la región no tienen recursos disponibles o no admiten el tamaño de máquina virtual solicitado.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a>Recopilación de registros de actividad
Para iniciar la solución de problemas, recopile los registros de actividad para identificar el error asociado con el problema. Los vínculos siguientes contienen información detallada sobre el proceso:

[Ver operaciones de implementación](../../azure-resource-manager/templates/deployment-history.md)

[Ver registros de actividad para administrar recursos de Azure](../../azure-resource-manager/management/view-activity-logs.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a>Problema: Error al iniciar una máquina virtual detenida
Intenta iniciar una máquina virtual detenida, pero obtiene un error de asignación.

### <a name="cause"></a>Causa
La solicitud de iniciar la máquina virtual detenida se debe intentar en el clúster original que hospeda el servicio en la nube. Sin embargo, el clúster no tiene espacio libre disponible para responder a la solicitud.

### <a name="resolution"></a>Solución
* Detenga todas las máquinas virtuales del conjunto de disponibilidad y reinicie las máquinas virtuales.
  
  1. Haga clic en **Grupos de recursos** > *su grupo de recursos* > **Recursos** > *su conjunto de disponibilidad* > **Virtual Machines** > *su máquina virtual* > **Detener**.
  2. Después de detener todas las máquinas virtuales, selecciónelas y haga clic en Iniciar.
* Vuelva a intentar solicitar el reinicio más tarde.

## <a name="issue-error-when-resizing-an-existing-vm"></a>Problema: error al reiniciar una máquina virtual existente
Intenta cambiar el tamaño de una máquina virtual existente, pero obtiene un error de asignación.

### <a name="cause"></a>Causa
La solicitud de cambiar el tamaño de la máquina virtual se debe realizar en el clúster original que alberga el servicio en la nube. Sin embargo, el clúster no admite el tamaño de máquina virtual solicitado.

### <a name="resolution"></a>Solución
* Vuelva a intentar la solicitud con un tamaño de máquina virtual menor.
* Si no se puede cambiar el tamaño de la máquina virtual solicitada:
  
  1. Detenga todas las máquinas virtuales en el conjunto de disponibilidad.
     
     * Haga clic en **Grupos de recursos** > *su grupo de recursos* > **Recursos** > *su conjunto de disponibilidad* > **Virtual Machines** > *su máquina virtual* > **Detener**.
  2. Después de detener todas las máquinas virtuales, aumente el tamaño de la máquina virtual que quiera.
  3. Seleccione la máquina virtual cuyo tamaño ha cambiado, haga clic en **Iniciar**e inicie las máquinas virtuales detenidas.

## <a name="next-steps"></a>Pasos siguientes
Si surgen problemas al crear una nueva máquina virtual Windows en Azure, consulte [Solución de problemas de implementación de Resource Manager con la creación de una máquina virtual de Windows en Azure](./troubleshoot-deployment-new-vm-windows.md).
