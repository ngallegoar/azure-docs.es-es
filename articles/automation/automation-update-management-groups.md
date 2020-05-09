---
title: Uso de grupos dinámicos con Update Management de Azure Automation
description: En este artículo se describe cómo funcionan los grupos dinámicos con Update Management de Azure Automation.
services: automation
ms.subservice: update-management
ms.date: 11/20/2019
ms.topic: conceptual
ms.openlocfilehash: bf4c156d2bf9c205bd7545a96b5314dd43b2d02c
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82690775"
---
# <a name="use-dynamic-groups-with-update-management"></a>Uso de grupos dinámicos con Update Management

Update Management permite tener como destino un grupo dinámico de VM de Azure (o de VM que no sean de Azure) para implementaciones de actualizaciones. El uso de un grupo dinámico evita que tenga que editar la implementación para actualizar las máquinas.

> [!NOTE]
> Los grupos dinámicos no funcionan con máquinas virtuales clásicas.

Puede definir grupos dinámicos para máquinas, sean o no de Azure, desde **Update Management** en Azure Portal. Consulte [Administración de actualizaciones para varias máquinas virtuales de Azure](manage-update-multi.md).

Un grupo dinámico se define mediante una consulta que Azure Automation evalúa en el momento de la implementación. Aunque una consulta de grupo dinámico recupere un gran número de máquinas, Azure Automation solo puede procesar un máximo de 1000 máquinas a la vez. Vea [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#update-management). 

> [!NOTE]
> Si espera actualizar más de 1000 máquinas, es recomendable que divida las actualizaciones entre varias programaciones de actualización. 

## <a name="define-dynamic-groups-for-azure-machines"></a>Definición de grupos dinámicos para máquinas de Azure

Al definir una consulta de grupo dinámico para las máquinas de Azure, puede usar los siguientes elementos para rellenar el grupo dinámico:

* Subscription
* Grupos de recursos
* Ubicaciones
* Etiquetas

![Selección de grupos](./media/automation-update-management/select-groups.png)

Para obtener una vista previa de los resultados de un grupo dinámico, haga clic en **Vista previa**. La versión preliminar muestra la pertenencia al grupo en el momento actual. En el ejemplo, buscamos máquinas que tengan la etiqueta `Role` para el grupo **BackendServer**. Si se ha agregado esta etiqueta a varias máquinas, se agregan a las implementaciones futuras en ese grupo.

![vista previa de grupos](./media/automation-update-management/preview-groups.png)

## <a name="define-dynamic-groups-for-non-azure-machines"></a>Definición de grupos dinámicos para máquinas que no son de Azure

Un grupo dinámico para máquinas que no son de Azure usa búsquedas guardadas, que también se denominan grupos de equipos. Para aprender a crear una búsqueda guardada, consulte [Creación de un grupo de equipos](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Una vez creada la búsqueda guardada, puede seleccionarla en la lista de búsquedas guardadas en **Update Management** en Azure Portal. Haga clic en **Vista previa** para obtener una vista previa de los equipos de la búsqueda guardada.

![Selección de grupos](./media/automation-update-management/select-groups-2.png)

## <a name="next-steps"></a>Pasos siguientes

Después de crear un grupo dinámico, puede [crear una implementación de actualizaciones](automation-tutorial-update-management.md).
